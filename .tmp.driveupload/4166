
# Fiber 노드 구조 분석

React의 Fiber 노드는 React 컴포넌트 트리의 각 노드를 나타내는 핵심 데이터 구조입니다. Fiber 노드는 컴포넌트의 상태, 속성, 그리고 렌더링 작업을 관리하는 모든 정보를 포함합니다 [ReactInternalTypes.js:86-88](https://github.com/facebook/react/blob/26e87b5f15d80fd4aaf9909f90de0857e54c1129/packages/react-reconciler/src/ReactInternalTypes.js#L88)

## Fiber 노드의 핵심 구조

### 1. 기본 식별 정보

Fiber 노드는 컴포넌트를 식별하기 위한 기본 정보를 포함합니다 [ReactInternalTypes.js:99-113](https://github.com/facebook/react/blob/26e87b5f15d80fd4aaf9909f90de0857e54c1129/packages/react-reconciler/src/ReactInternalTypes.js#L99C1-L113C18) :

- `tag`: 컴포넌트 타입을 나타내는 WorkTag (FunctionComponent, ClassComponent 등)
- `key`: React 엘리먼트의 고유 식별자
- `elementType`: 원본 엘리먼트 타입
- `type`: 해결된 함수/클래스 타입
- `stateNode`: 실제 DOM 노드나 클래스 인스턴스

### 2. 트리 구조 관리

Fiber는 단일 연결 리스트 트리 구조로 구성됩니다 [ReactInternalTypes.js:121-130](https://github.com/facebook/react/blob/26e87b5f15d80fd4aaf9909f90de0857e54c1129/packages/react-reconciler/src/ReactInternalTypes.js#L121C3-L130C17) :

- `return`: 부모 Fiber (스택 프레임의 반환 주소와 유사)
- `child`: 첫 번째 자식 Fiber
- `sibling`: 다음 형제 Fiber
- `index`: 형제들 중의 위치

### 3. Props와 State 관리

컴포넌트의 데이터를 관리하는 필드들입니다 [ReactInternalTypes.js:141-152](https://github.com/facebook/react/blob/26e87b5f15d80fd4aaf9909f90de0857e54c1129/packages/react-reconciler/src/ReactInternalTypes.js#L141C3-L152C37) :

- `pendingProps`: 새로 들어오는 props
- `memoizedProps`: 이전 렌더링에서 사용된 props
- `updateQueue`: 상태 업데이트와 콜백 큐
- `memoizedState`: 현재 상태 (Hook 체인의 시작점)
- `dependencies`: 컨텍스트 의존성

### 4. 스케줄링과 우선순위

React의 동시성 기능을 위한 스케줄링 정보입니다 [ReactInternalTypes.js:167-168](https://github.com/facebook/react/blob/26e87b5f15d80fd4aaf9909f90de0857e54c1129/packages/react-reconciler/src/ReactInternalTypes.js#L141C3-L152C37) :

- `lanes`: 현재 Fiber의 우선순위 레인
- `childLanes`: 자식들의 우선순위 레인

## Fiber 노드 생성 과정

### FiberNode 생성자

Fiber 노드는 `FiberNode` 생성자를 통해 생성됩니다 [ReactFiber.js:136-209](https://github.com/facebook/react/blob/26e87b5f15d80fd4aaf9909f90de0857e54c1129/packages/react-reconciler/src/ReactFiber.js#L136C1-L209C2) . 이 생성자는 모든 필드를 초기화하고, 개발 모드에서는 디버깅 정보도 추가합니다.

### createFiber 함수

실제로는 `createFiber` 함수가 사용되며, 이는 feature flag에 따라 클래스 기반 또는 객체 리터럴 기반 구현을 선택합니다 [ReactFiber.js:301-303](https://github.com/facebook/react/blob/26e87b5f15d80fd4aaf9909f90de0857e54c1129/packages/react-reconciler/src/ReactFiber.js#L301C1-L303C26) :

```js
const createFiber = enableObjectFiber  
  ? createFiberImplObject  
  : createFiberImplClass;
```

### Double Buffering 시스템

React는 `createWorkInProgress` 함수를 통해 이중 버퍼링을 구현합니다 [ReactFiber.js:325-354](https://github.com/facebook/react/blob/26e87b5f15d80fd4aaf9909f90de0857e54c1129/packages/react-reconciler/src/ReactFiber.js#L310C1-L354C11) . 이는 현재 트리와 작업 중인 트리를 분리하여 효율적인 업데이트를 가능하게 합니다:

- `current.alternate`이 null이면 새로운 workInProgress Fiber 생성
- 이미 존재하면 기존 alternate 재사용하여 성능 최적화

## 컴포넌트 타입별 Fiber 생성

### createFiberFromTypeAndProps

다양한 컴포넌트 타입에 따라 적절한 WorkTag를 결정합니다 [ReactFiber.js:546-586](https://github.com/facebook/react/blob/26e87b5f15d80fd4aaf9909f90de0857e54c1129/packages/react-reconciler/src/ReactFiber.js#L546C1-L586C11) :

- 함수형 컴포넌트: `FunctionComponent` 또는 `ClassComponent`
- 문자열 타입: `HostComponent`, `HostHoistable`, `HostSingleton`
- 특수 타입들: Fragment, Suspense, Context 등

### 특수 컴포넌트 생성 함수들

각 특수 컴포넌트 타입마다 전용 생성 함수가 있습니다 [ReactFiber.js:746-755](https://github.com/facebook/react/blob/26e87b5f15d80fd4aaf9909f90de0857e54c1129/packages/react-reconciler/src/ReactFiber.js#L746C1-L755C2) :

- `createFiberFromFragment`: Fragment 컴포넌트
- `createFiberFromText`: 텍스트 노드
- `createFiberFromPortal`: Portal 컴포넌트

## 실제 사용 예시

### 자식 재조정에서의 활용

`ReactChildFiber.js`에서 Fiber 노드가 어떻게 생성되고 재사용되는지 볼 수 있습니다 [ReactChildFiber.js:664-676](https://github.com/facebook/react/blob/26e87b5f15d80fd4aaf9909f90de0857e54c1129/packages/react-reconciler/src/ReactChildFiber.js#L664C11-L676C26) . 새로운 엘리먼트가 들어오면 `createFiberFromElement`를 호출하여 새 Fiber를 생성하고, 부모-자식 관계를 설정합니다.

## Notes

Fiber 노드 구조는 React의 핵심 아키텍처로, 가상 DOM의 각 노드를 표현하면서 동시에 작업 단위(unit of work)로도 기능합니다. 이 구조를 통해 React는 렌더링을 중단하고 재개할 수 있는 동시성 기능을 구현할 수 있습니다. 개발 모드에서는 디버깅을 위한 추가 필드들(`_debugOwner`, `_debugHookTypes` 등)이 포함되어 개발자 도구와의 연동을 지원합니다.