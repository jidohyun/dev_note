
---

### 쿠버네티스?
- 구글에서 개발한 오픈소스 프로젝트
- 컨테이너 오케스트레이션 프레임워크
- 도커 컨테이너나 다른 기술을 통해 컨테이너를 관리함
- 물리적 머신, 가상 머신, 클라우드 환경, 하이브리드 배포 환경같은 다양한 환경에서 애플리케이션을 관리함

### 실제 작업
- 마이크로 서비스가 늘어나며 컨테이너 사용이 증가했다.
- 컨테이너는 실제로 마이크로서비스와 같은 소규모 독립 애플리케이션을 위한 완벽한 호스트를 제공한다.
- 컨테이너와 마이크로 서비스 기술의 등장으로 실제로 애플리케이션이 수백 개, 때로는 수천 개의 컨테이너로 구성되었다.
- 스크립트와 자체 제작 도구를 사용하여 여러 환경에 걸쳐 이러한 컨테이너 부하를 관리하는 것은 매우 복잡하고 때로는 불가능할 수 있다.
- 따라서 이러한 특정 시나리오가 실제로 컨테이너 오케스트레이션을 초래했다.

### 보장하는 기능

1. **고가용성** 즉, 애플리케이션에 다운타임 없이 사용자가 항상 엑세스 할 수 있음을 의미함.
2. **확장성** 즉, 애플리케이션에 부하가 걸리고 더 많은 사용자가 엑세스하려고 할 때 애플리케이션을 빠르게 확장할 수 있음을 의미하며, 마찬가지로 부하가 줄어들면 쉽게 축소할 수 있으므로 애플리케이션이 부하 증가 또는 감소에 맞게 조정할 수 있도록 더 유연해진다.
3. **재해 복구** 즉, 기본적으로 인프라에 데이터 손실, 서버 폭발 또는 서비스 센터에 문제가 발생하는 것과 같은 문제가 발생하면 인프라에는 데이터를 백업할 수 있는 일종의 메커니즘이 있어야 한다. 최신상태로 복구하여 애플리케이션이 실제로 데이터를 잃지 않고 컨테이너화된 애플리케이션이 복구 후 최신 상태에서 실행될 수 있도록 한다.
이러한 모든 기능은 쿠버네티스와 같은 컨테이너 오케스트레이션 기술이 제공하는 기능이다.

### k8s 기본 아키텍쳐

k8s의 클러스터는 최소 하나의 마스터 노드로 구성되고 여기에 연결된다.
각 노드에는 **큐브릿** 프로세스가 실행되는 여러 개의 워커 노드가 있다.
(큐브릿은 실제로 클러스터가 서로 통신하고 해당 노드에서 일부 작업을 실행할 수 있도록 하는 쿠버네티스 프로세스이다.)

각 워커 노드에는 다양한 애플리케이션의 컨테이너가 있으므로 작업 부하가 분산되는 방식에 따라 워커 노드에서 실행되는 도커 컨테이너 수가 달라진다.

워커 노드는 실제 작업이 발생하는 위치이다. 따라서 마스터노드에서 무엇이 실행되고 있는지가 문제이다. 마스터노드는 실제로 클러스터를 제대로 실행하고 관리하는 데 절대적으로 필요한 여러 쿠버네티스 프로세스를 실행한다.

이러한 프로세스 중 하나는 컨테이너이기도 한 API 서버이다.

실제로 k8s의 클러스터 진입점이므로 k8s 대시보드를 사용하는 경우 UI 스크립트 및 자동화 기술을 사용하는 경우 API, 명령줄 도구와 같이 다른 k8s 클라이언트가 통신하는 프로세스이다. 따라서 이 모든 것이 API 서버와 통신한다. 마스터 노드에서 실행되는 또 다른 프로세스는 컨트롤러 관리자로 기본적으로 클러스터에서 발생하는 상황을 개괄적으로 파악한다. 무언가를 수리해야 하는지 또는 컨테이너가 죽어서 다시 시작해야 하는지 등이다. 또 다른 프로세스는 스케줄러로 기본적으로 각 노드의 작업 부하와 사용 가능한 서버 리소스를 기반으로 여러 노드에 컨테이너를 스케줄링하는 역할을 한다. 따라서 해당 워커 노드의 사용 가능한 리소스와 해당 컨테이너에 필요한 부하를 기반으로 다음 컨테이너를 어느 워커 노드에 스케줄링해야 하는지 결정하는 지능형 프로세스이다. 전체 클러스터의 또 다른 매우 중요한 구성 요소는 실제로 etcd 키 값 저장소로 기본적으로 언제든지 k8s 클러스터의 현재 상태를 보관하므로 모든 구성 데이터와 각 노드의 모든 상태 데이터, 해당 노드 내부의 각 컨테이너, 백업이 있다. 이전에 언급한 복원은 실제로 이러한 etcd 스냅샷에서 이루어지기 때문에 해당 etcd 스냅샷을 사용하여 전체 클러스터 상태를 복구할 수 있으며 마지막으로 쿠버네티스의 매우 중요한 구성 요소인 노드 워커 노드 마스터노드가 서로 통신할 수 있도록 하는 것은 클러스터의 일부인 모든 노드를 포괄하는 가상 네트워크이며 간단히 말해서 가상 네트워크는 실제로 클러스터 내부의 모든 노드를 개별 노드의 모든 리소스의 합계를 가진 하나의 강력한 머신으로 전환한다.

![](https://i.imgur.com/Mioz054.png)
