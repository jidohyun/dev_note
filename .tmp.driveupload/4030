
---

### js 프로토타입?

클래스 기반의 언어에선 클래스 내부에 모든 속성과 메소드가 정의되어있다. 해당 클래스를 기반으로 한 객체가 인스턴스로 생성되면 이 객체는 클래스 내부에 정의되어있는 속성과 메소드에 접근하여 사용할 수 있는 형태이다.

`프로토타입`은 이런 클래스와 아주 유사하며 `javascript` 의 모든 객체 프로토타입은 값을 할당하는 시점에 결정된다. 

### 프로토타입 기반 언어

>
>   JavaScript는 흔히 프로토타입 기반 언어라고 불린다.
>

모든 객체들이 메소드와 속성들을 상속받기 위한 명세로 프로토타입 객체를 가진다는 의미이다.

클래스 처럼 객체의 인스턴스를 위한 명세와 같은 역할을 하는데 객체 본인만이 가진 속성과 메소드에도 접근할 수 있으면서 프로토타입의 것들에도 접근할 수 있다.

javascript에서 함수를 생성할 때 프로토타입 속성이 함수에 붙여진다.
예를 들어, `new`로 함수들을 호출 할 때마다 생성되는 인스턴스 함수 프로토타입의 모든 속성을 상속한다.

```javascript
const Hello = function(name) {
  this.name = name;
}
```

![](https://i.imgur.com/4Vmvwvv.png)

속성과 메소드들은 각 객체 인스턴스가 아니라 객체 생성자의 `prototype` 속성에 정의되어 있다.

`Hello` 에 정의한 `name` 멤버 외에 프로토타입 객체인 `object`의 다른 멤버들도 존재함을 알 수 있다.

**이렇게 자바스크립트의 모드 객체는 자신의 부모 역할을 하는 객체와 연결되어있고 이 부모 객체를 `프로토타입` 이라고 한다.**

이런 방식으로 클래스를 상속하여 사용하는 것 같이 객체 지향 프로그래밍 방식을 사용할 수 있다.

```js
hello.toString();
```

생성되는 모든 객체는 이런 프로토타입 객체에 접근할 수 있고 동적으로 런타임 시에 멤버를 추가할 수도 있다.

![](https://i.imgur.com/HkBDeov.png)

### prototype에 접근하기

프로토타입에 접근하기 위해 `__proto__` (deprecated)를 인스턴스에 사용하거나 
`Object.getPrototypeOf(instance)` 를 사용할 수 있다.

```javascript
function Hello(name) {
  this.name = name;
}
const hello = new Hello('hello');
Object.getPrototypeOf(hello) === Hello.prototype; // true
```

### 네이티브 객체 프로토타입

자바스크립트 `Array`, `String` 등의 내장 객체 역시 자신의 프로토타입을 가지고 있다.

배열을 선언해 늘 사용하던 `map`, `filter`등을 사용해 조작하고 문자열을 선언해 `split` 등의 연산을 할 수 있게 해주는 이유다.

![](https://i.imgur.com/AGX6B6N.png)

```javascript
const arr = [1,2,3,4,5];
console.log(Object.getPrototypeOf(arr))  //Array prototype
const str = "Hello world!";
console.log(Object.getPrototypeOf(str)) //String prototype
const date = new Date();
console.log(Object.getPrototypeOf(date)) //Date prototype
```

### 프로토타입 체인

![](https://i.imgur.com/AlqOREY.png)

모든 객체들은 메소드와 속성을 상속받기 위한 명세로 프로토타입 객체를 가진다고 했다.

이는 프로토타입 객체도 또 다시 상위 프로토타입 객체로써 상속받을 수 있고 그 상위도 마찬가지인데 이를 `프로토타입 체인` 이라고 한다.

다른 객체에 정의된 메소드와 속성을 한 객체에서 사용할 수 있게 해준다.

객체 자신의 것 뿐 아니라 `[prototype]` 이 가리키는 링크를 따라 부모 역할을 하는 모든 프로토타입 객체의 속성이나 메소드에 접근할 수 있다.

```javascript
const obj = {hello: 'world'};
const str = 'hello'
Object.prototype.hi = function() {console.log('hi')};
obj.hi(); // hi
str.hi(); // hi
```

```javascript
Object.getPrototypeOf(str);
```

```null
String {'', constructor: ƒ, anchor: ƒ, at: ƒ, big: ƒ, …}
anchor
: 
ƒ anchor() at: 
ƒ at() big: 
....
[[Prototype]]: Object
  - hi: f(),
  - constructor: f Object()
  - ...
```

특정 객체에서 특정 속성이나 메소드에 접근할 때 자바스크립트 엔진에서 먼저 객체 본인이 가진 것인지 파악하고 없는 경우 프로토타입 체이닝이 일어나며
부모 역할이 되는 상위 객체를 향해 속성이나 메소드를 탐색해 나가고 존재하는지 찾다가 마지막에는
`Object.Prototype`에서도 찾지 못하게 된다면 `undefined` 를 리턴한다.

```javascript
const num = 55;
num.push // undefined;
num.push() // Uncaught TypeError: num.push is not a function
Object.prototype.push = function() {return this + 1};
num.push() // 56;
```

### 모든 속성, 메소드가 상속되지 않는 이유

상속받는 맴버들은 `prototype` 속성에 정의되어있고 이들만 상속된다.

![](https://i.imgur.com/5Iyelhh.png)

`Object.prototype.` 로 시작하는 속성들을 말하며 생성자로 생성되는 인스턴스 뿐 아니라 
`Object.prototype` 을 상속받는 객체라면 접근할 수 있다.

![](https://i.imgur.com/GjC1Ru0.png)

배열을 선언하고 `push`, `pop`등의 메서드를 사용할 수 있다.
이들은 `Array.prototype` 의 메서드이기 때문이다.

```javascript
const a = [];
a.isArray(); // Uncaught TypeError: a.isArray is not a function
```

프로토타입에 정의되지 않는 멤버들은 상속되지 않기 때문에 직접 사용할 수 없으며 `Array` 전역 객체를
이용해 직접 접근할 수 밖에 없다.

### 그래서 결론지어보는 사용되는 이유

#### 메모리 효율성

`Javascript` 에서 모든 객체는 **프로토타입을 공유**한다. 
객체 자체가 스스로 메소드와 속성을 모두 가지는 대신 여러 객체가 동일한 프로토타입을 공유하도록하고 이를 사용하면 메모리를 효율적으로 사용할 수 있다.

#### 객체지향스러움

프로토타입을 통해 클래스에서 `상속`해 사용하는 것 처럼 다른 객체로부터의 속성과 메소드를 사용할 수 있다. 프로토타입 체인을 통해 가능한 것이며 이를 통해 코드 재사용이 가능해진다.

#### 생산성

프로토타입을 통해 동일한 속성, 동작이 필요한 여러 객체들마다 이를 직접 선언하고 개발할 필요 없이 프로토타입을 이용해 관리할 수 있다. 


### 요약

- `prototype`은 객체 생성자 함수에 의해 생성되는 객체들이 공유하는 속성과 메소드를 저장하는 특수 객체이다.
- 모든 javascript 객체는 선언 할당하면 해당 생성자 함수의 `prototype` 객체와 연결되며 프로토타입 체이닝을 통해 해당 객체의 속성, 메소드에 접근 가능하다.

