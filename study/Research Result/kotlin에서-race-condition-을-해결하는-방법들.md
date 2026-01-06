---
title: "Kotlin에서 Race Condition 을 해결하는 방법들"
source: "https://velog.io/@mraz3068/How-To-Solve-Race-Condition-In-Kotlin"
created: 2026-01-05T00:58:51.885+09:00
type: article
tags: [reading, summary]
---

# 핵심 요약
- 멀티 스레드 환경에서 공유 자원 접근 시 발생하는 Race Condition을 해결하기 위해 Synchronized, AtomicInteger, Mutex 등을 활용할 수 있습니다.
- 단순한 단일 변수 연산에는 CPU 수준의 CAS 연산을 사용하는 AtomicInteger가 가장 빠르고 효율적입니다.
- 코루틴 환경에서는 스레드를 블로킹하지 않는 Mutex를 사용하는 것이 성능과 자원 효율성 측면에서 유리합니다.
- Semaphore는 Race Condition 해결보다는 동시 실행 개수를 제한하는 리소스 관리(Rate Limiting) 목적으로 사용해야 합니다.

# 상세 내용

### 1. Synchronized (동기화)
- 모니터 락(Monitor Lock)을 사용하여 한 번에 하나의 스레드만 임계 영역에 접근하도록 보장합니다.
- @Synchronized 어노테이션: 메서드 전체에 락을 걸며, 락 객체가 해당 인스턴스(this)로 고정되어 유연성이 낮습니다.
- Synchronized 블록: 필요한 코드 영역만 락을 걸 수 있어 성능 최적화가 가능하며, 특정 객체를 락으로 선택할 수 있습니다.
- 특징: OS 레벨의 락을 사용하므로 커널 모드 전환 비용이 발생하여 상대적으로 무겁습니다.

### 2. AtomicInteger (원자적 변수)
- 락을 사용하지 않고 CPU의 CAS(Compare-And-Swap) 연산을 통해 원자성을 보장합니다.
- 특징: OS 레벨의 락보다 훨씬 빠르며, 단일 변수의 증감이나 플래그 설정 등에 적합합니다.
- Kotlin 2.1부터는 멀티플랫폼 환경을 지원하는 전용 Atomic API(AtomicInt, Long 등)가 추가되었습니다.

### 3. Mutex (상호 배제)
- 코루틴 전용 락으로, suspend 함수 내에서 사용하도록 설계되었습니다.
- 특징: Java의 락과 달리 대기 중인 스레드를 블로킹하지 않고 코루틴만 일시 중단(suspend)시킵니다. 따라서 스레드 자원을 다른 코루틴이 사용할 수 있게 해줍니다.
- 멀티플랫폼 환경에서 공통 코드로 작성하기에 적합합니다.

### 4. Semaphore & limitedParallelism
- Semaphore: N개의 허가증(permit)을 관리하여 동시에 실행되는 코루틴의 개수를 제한합니다. 상호 배제가 목적이 아니므로 Race Condition 해결에는 적합하지 않습니다.
- limitedParallelism: 디스패처가 사용하는 스레드 풀의 크기를 제한하는 방식으로, 특정 작업이 시스템 자원을 독점하지 않도록 제어할 때 사용합니다.

# 인사이트 / 할 일
- 단순 카운터나 상태 플래그를 관리할 때는 복잡한 락 대신 AtomicInteger를 우선적으로 고려하여 성능을 확보하자.
- 코루틴 내부에서 Java의 synchronized를 사용하면 스레드가 차단되어 코루틴의 장점이 사라지므로, 반드시 kotlinx.coroutines의 Mutex를 사용하자.
- @Volatile은 변수의 가시성(메모리 동기화)만 보장할 뿐, 복합 연산(count++)의 원자성은 보장하지 않는다는 점을 명확히 인지하고 사용하자.
- 프로젝트의 Kotlin 버전을 확인하고, 2.1 이상이라면 플랫폼 독립적인 Atomic API 도입을 검토해 보자.