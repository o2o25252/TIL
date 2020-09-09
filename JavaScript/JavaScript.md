
# JavaScript

<img src="https://user-images.githubusercontent.com/61407010/91863660-4f76c280-ecaa-11ea-8668-cee239d24b93.jpg" width="30%"></img><br/>
## 화살표 함수 (arrow function)

보통 함수 표현식을 축약한 형태로 표시됩니다.

함수 표현식
```
const add = function (x,y){
    return x+y
}
```
arrow function
```
const add =(x,y) => x+y

const add = (x,y) => {x + y} // 🙅🏻, 에러 발생
const add = (x,y) => (x + y) 🙆🏻 
```
함수 내의 표현식이 두 줄 이상일 경우에는 , return 을 생략하기 보다는 중괄호와 return 을  명시적으로 쓰는 것이 좋습니다.

##### 화살표 함수는 클로저를 표현할 때 더욱 강력하다.
함수 표현식
```
const adder = function(x){
return function(y){
    return x + y
    }
}
adder(5)(7) // 12
```
arrow function

1. function 키워드를 없애자
```
const adder = (x) => {
return (y) => {
return x + y
    }
}
}
```
2. 가장 안쪼 return 없애보자.
        - return 생략시에는 중괄호를 사용하지 않는 점을 기억하지 않는다 
        - 파라미터가 단 한개라면, 소괄호를 생략할 수 있다.
        - 즉,인수가 하나밖에 없다면 인수를 감싸는 괄호를 생략할 수 있습니다
```
const adder = x => {
    return y => x + y
    }
```
3.   return 을 완전히 없애보자
```
const adder = x => y => x + y
```
 **이와 같이 클로저는 연속된 여러 개의 화살표로 표시할 수 있습니다.**
 return 없어도 결과를 반환. 
### 화살표 함수의 특징
    - call,apply,bind를 사용할 수 없습니다.
    - **this 를 가지지 않습니다.** this는 화살표 함수를 감싸고 있는 스코프의 실행 컨텍스트에 의해 결정됩니다.
```
let foo = {
  bar: function() {
    // 메소드 호출시 this === foo 
    
    const arrowfn = () => {
      console.log(this) // arrowfn을 감싸고 있는 함수 스코프의 실행 컨텍스트에 의해 결정됩니다
    }
    
    arrowfn()
  }
}

foo.bar() // {bar: ƒ}
```
**본문이 여러 줄인 화살표 함수**
중괄호 안에 평가해야 할 코드를 넣어주어야 하며 retrun 지시자를 사용해 명시적으로 결과값을 반환해 주어야 합니다.
```
let sum = (a, b) => {  // 중괄호는 본문 여러 줄로 구성되어 있음을 알려줍니다.
  let result = a + b;
  return result; // 중괄호를 사용했다면, return 지시자로 결괏값을 반환해주어야 합니다.
};

alert( sum(1, 2) ); // 3
```
#### 화살표 함수에는 *'this'* 가 없습니다.

화살표 함수엔  `this`가 없다는건 알 것이다.
`this` 에 접근 하면 , 외부에서 값을 가져온다.

```
let group = {
  title: "1모둠",
  students: ["보라", "호진", "지민"],

  showList() {
    this.students.forEach(
      student => alert(this.title + ': ' + student)
    );
  }
};

group.showList();
```
예시의 `forEach`에서 화살표 함수를 사용했기 때문에 화살표 함수 본문에 있는 `this.title`은 화살표 함수 바깥에 있는 메서드인 `showList`가 가리키는 대상과 동일해집니다. 즉 `this.title`은 `group.title`과 같습니다.

위 예시에서 화살표 함수 대신 ‘일반’ 함수를 사용했다면 에러가 발생했을 겁니다     
**화살표 함수는 `new`와 함께 실행할 수 없다.**
`this`가 없기 떄문에 화살표 함수는 생성자 함수로 사용할 수 없다는 제약이 있다.

**화살표 함수 vs bind**
화살표 함수와 일반 함수를 `.bind(this)`를 사용해서 호출하는 것 사이에는 미묘한 차이가 있다.
        -`.bind(this)`는 함수의 '한정된 버전'을 만듭니다.
        -화살표 함수는 어떤 것도 바인딩시키지 않는다. 화살표 함수엔 단지 `this`가 없을 뿐이다. 사용시에는 `this`의 값을 외부 렉시컬 환경에서 찾는다.
        
**'arguments'가없다**
화살표 함수는 일반 함수와는 다르게 모든 인수에 접근할 수 있게 해주는 유사 배열 객체 arguments를 지원하지 않습니다.

이런 특징은 현재 `this` 값과 `arguments` 정보를 함께 실어 호출을 포워딩해 주는 데코레이터를 만들 때 유용하게 사용됩니다.

아래 예시에서 데코레이터 defer(f, ms)는 함수를 인자로 받고 이 함수를 래퍼로 감싸 반환하는데, 함수 f는 ms 밀리초 후에 호출됩니다.

```
function defer(f, ms) {
  return function() {
    setTimeout(() => f.apply(this, arguments), ms)
  };
}

function sayHi(who) {
  alert('안녕, ' + who);
}

let sayHiDeferred = defer(sayHi, 2000);
sayHiDeferred("철수"); // 2초 후 "안녕, 철수"가 출력됩니다.
```
일반 함수로 구현
```
function defer(f, ms) {
  return function(...args) {
    let ctx = this;
    setTimeout(function() {
      return f.apply(ctx, args);
    }, ms);
  };
}
//일반 함수에선 setTimeout에 넘겨주는 콜백 함수에서 사용할 변수 ctx와 //args를 반드시 만들어줘야 합니다.
```
#### 예제
등장 하는 단어의 갯수를 값으로 가진 객체를 리턴 하라
```
let word = 'I felt happy because I saw the others were happy and because I knew I should feel happy but I was not really happy'

function ex1(str) {


  let a =str.split(' ');//쪼개기 [abc,'',a,'',b,obj]
 
  let word = a.filter( w => w !=='');
    
  return word.reduce((a,c) => {
       if( c in a){
            console.log(c);
           a[c]++
            
        }else{            
            a[c] = 1;
            //console.log(a[c]);
        }return a;
    
  },{})
}
ex1(word); // {I: 5, felt: 1, happy: 4, because: 2, saw: 1, …}
```
     
<hr>

## 'this'

'함수 선언식 호출'
```
function foo() {
  return this
}
```
'함수 표현식 호출'
```
const foo = function () {
  return this
}
```
둘다 window를 나타낸다.

'화살표 함수 호출'
```
const foo = () => {
    return this
  }
  foo() //window
```
외부에 값을 가져오기도 한다. 
        -화살표 함수에는 *'this'* 가 없습니다.참조 할것

'메소드 호출시 this'
```
  const counter = {
    value: 0,
    increse: function () {
      this.value++
    },
    decrease: function () {
      this.value--
    },
    getValue: function () {
      return this.value
    }
  }

  counter.increse()
  counter.increse()
  counter.decrease()
  counter.getValue() //1
})
```
'화살표 함수 호출시 this'
쓰지마세요 ^^

**메서드 내부에서 `this` 키워드를 사용하면 객체에 접근할 수 있습니다.**
이떄 **점 앞** 의 `this`는 객체를 나타냅니다.정확히는 메서드를 호출할 떄 사용된 객체를 나타낸다.

```
let user = {
  name: "John",
  age: 30,

  sayHi() {
    // 'this'는 '현재 객체'를 나타냅니다.
    alert(this.name);
  }

};

user.sayHi(); // John
```
`user.sayHi()`가 실행되는 동안에 `this` 는 `user`를 나타냅니다.
`this`를 사용하지 않고 외부 변수를 참조해 객체에 접근하는 것도 가능하다.

```
let user = {
  name: "John",
  age: 30,

  sayHi() {
    alert(user.name); // 'this' 대신 'user'를 이용함
  }

};

```
그런데 외부 변수를 사용해 객체를 참조하면 예상치 못한 에러가 발생할 수 있다.

`user`를 복사해 다른 변수에 할당`(admin = user)`하고, `user`는 전혀 다른 값으로 덮어썼다고 가정해 봅시다. sayHi()는 원치 않는 값(null)을 참조할 겁니다.


this 값은 런타임에 결정됩니다. 컨텍스트에 따라 달라지죠.
동일한 함수라도 다른 객체에서 호출했다면 'this’가 참조하는 값이 달라집니다.
규칙은 간단합니다. obj.f()를 호출했다면 this는 f를 호출하는 동안의 obj입니다

## 디스트럭처링/스프레드,레스트

**'rest/spread' 문법을 배열 분해에 적용 해보자**

```
const array = ['code', 'states', 'im', 'course']
const [start, ...rest] = arra

start// 'code'
rest// ['states', 'im', 'course']
```
객체의 단축(shorthand)

알아서 들어간다. (신기)
```
const name = '김코딩'
   const age = 28
   
   const person = {
     name,
     age,
     level: 'Junior',
   }
 person  //{name: "김코딩", age: 28, level: "Junior"}
```

객체 분해
```
const student = { name: '박해커', major: '물리학과' }

const { name } = student
name // "박해커"
```
rest/spread 문법을 객체 분해에 적용
```
const student = { name: '최초보', major: '물리학과' }
const { name, ...args } = student
name // "최초보"
args // {major: "물리학과"
```

```
const user = {
  name: '김코딩',
  company: {
    name: 'Code States',
    department: 'Development',
    role: {
      name: 'Software Engineer'
    }
  },
  age: 35
}

//스프레드 앞과 뒤의 차이 
const changedUser = {
  ...user,
  name: '박해커',
  age: 20
}
//자, 앞에 먼저 김코딩 과 에이지 35 가 있고 거기에 박해커 와 20 이 덮어 씌워진다.
const overwriteChanges = {
  name: '박해커',
  age: 20,
  ...user
}
//자, 이미 박해커와 20 이 있다 여기에 김코딩과 35가 덮어진다. 
```
## call,apply,bind

call,apply, bind 의 첫번째 인자는 `this` 다
 call,apply 는 바로 실행하지만 
 bind 는 바로 실행하지 않는다. 

 #### bind
 
  bind 는 this 자리에 오는 값을 고정하는 것 뿐만 아니라 함수의 인수도 고장해준다.
 
 ```
 function mul(a, b) {
   return a * b;
 }

 let double = mul.bind(null, 2);

 alert( double(3) ); // = mul(2, 3) = 6
 alert( double(4) ); // = mul(2, 4) = 8
 alert( double(5) ); // = mul(2, 5) = 10
 ```
 이런걸  부분 적용(partial application) 이라고 한다. 
 
 위 예시에선 this를 사용하지 않았다는 점에 주목하시기 바랍니다. bind엔 컨텍스트를 항상 넘겨줘야 하므로 null을 사용했습니다.
 
  **부분 함수는 왜 만드는 걸까?**
        -가독성이 좋은 이름(double, triple)을 가진 독립 함수를 만들 수 있다는 이점 때문
        -bind를 사용해 첫 번째 인수를 고정할 수 있기 때문에 매번 인수를 전달할 필요도 없다.

#### 컨텍스트 없는 부분 적용
인수만 바인딩해주는 헬퍼 함수 `partial`를 구현 해보자 

 ```
 function partial(func, ...argsBound) {
   return function(...args) { // (*)
     return func.call(this, ...argsBound, ...args);
   }
 }

 // 사용법:
 let user = {
   firstName: "John",
   say(time, phrase) {
     alert(`[${time}] ${this.firstName}: ${phrase}!`);
   }
 };

 // 시간을 고정한 부분 메서드를 추가함
 user.sayNow = partial(user.say, new Date().getHours() + ':' + new Date().getMinutes());

 user.sayNow("Hello");
 // 출력값 예시:
 // [10:00] John: Hello!
 ```
  
  ## OOP(Object Oriented Programming)가 무엇인지?
	OOP
   
면접관이 물어 봤다 "OOP 가 무엇인가요?"
나는 뭉뚱그려 설명 할 수 밖에 없었다.. 

프로그램을 현실 세계의 사물처럼 바바보고 만드는 프로그램의 하나의 방법 이라고 

표정을 보니 면접관은 맞다면 맞지만 그닥 원하는 대답이 아니였던거 같다. 

아직도 나는 내가말한게 맞지 않을까? 하고 생각중이다 . 한번 자세히 훑어보자

### 장점
	-유지 보수 성
	-직관적인 코드 분석

흔히 oop 는 *강한 응집력* 과 *약한 결합력* 을 지향한다. 

하나의 문제 해결을 위해 데이터를 모아 놓은 객체를 활용한 프로그래밍을 지향하므로 응집력을 강화하면,클래스 간에 독립적으로 디자인함으로써 결합력을 약하게 할 수 있다. 

### 요소

	-클래스
    	속성 과 기능들을 정의해 놓은 것
    -객체
    	클래스의 인스턴스 / 개별적인 속성과 기능 또한 가지고 있다.
    -메서드
		클래스로부터 생성된 객체를 다루는 방법 객체의 속성을 조작

### 특성
	-캡슐화
    	캡슐화는 객체의 데이터를 외부에서 직접 접근하지 못하게 막으며 함수를 통해서만 조작이 가능하게 하는 작업
    -추상화
    	추상화는 객체들이 가진 공통의 특성들을 파악하고 불필요한 특성들을 제거하는 과정을 말한다.객체들이 가진 동작들을 기준으로 이용자들이 동작만 쉽게 구동할 수 있도록 한다.
    -재사용성
    	한번 작성된 걸 활용하여 동일한 객체를 각각의 객체로 만들 수있고, 기능을 추가하여 같지만 다른 객체를 만들 수 있다.
	
## JavaScript에서 Object를 생성하는 여러가지 방법들

Functional
```js
let Car = function() {
  let someInstance = {};
  someInstance.position = 0;
  someInstance.move = function() {
    this.position ++;
  }
  return someInstance;
}

let car1 = Car();
car1.move()
car1.position // 1
```

```js
let CarType2 = function(position) {
  let someInstance = {};
  someInstance.position = position;
  someInstance.move = function() {
    console.log(this); 
    this.position ++;
  }
 
  return someInstance;
}

let car2 = CarType2(10);
car2.move() // {position: 10, move: ƒ}
```
Functional 방식은 인스턴스를 생성할 때마다 모든 메소드를 someInstance에 할당해서 각각의 인스턴스들에 해당하는 메소드의수 만큼의 메모리를 더 차지하게 된다

Functional Shared
```js
let extend = function(to, from) {
  for(let key in from) {
    to[key] = from[key];
  }
}

let someMethods = {};
someMethods.move = function() {
  this.position ++;
}

let Car = function(position) {
  let someInstance = {
    position : position,
  }
  
  extend(someInstance, someMethods);
  return someInstance;
}

let car1 = Car(10);
car1.move();
car1 // {position: 11, move: ƒ}
```
Functional Shared 방식을 사용하면 someMethods 라는 객체에 있는 메소드들의 메모리 주소만을 참조하기 때문에 메모리 효율이 좋아진다

Prototypal
```js
let someMethods = {};
someMethods.move = function() {
  this.position ++;
}

let Car = function(position) {
  let someInstance = Object.create(someMethods);

  someInstance.position = position;
  return someInstance;
}

let car1 = Car(10);
car1.move();
car1 // {position: 11}
```
Object.create() 메서드는 지정된 프로토타입 객체 및 속성(property)을 갖는 새 객체를 만들어준다

Pseudoclassical
```js
let Car = function(position) {
  this.position = position;
}

Car.prototype.move = function() {
  this.position ++;
}

let car = new Car(5);
```

[출처](https://velog.io/@yhe228/JavaScript%EC%97%90%EC%84%9C-Object%EB%A5%BC-%EC%83%9D%EC%84%B1%ED%95%98%EB%8A%94-%EC%97%AC%EB%9F%AC%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95%EB%93%A4)
[출처](https://urclass.codestates.com/feb3313b-77f0-4fc7-a150-4d96b586dd9d?playlist=255)
	
## JavaScript에서 Prototype은 무엇이고 왜 사용해야 하는지?

[출처](https://velog.io/@adam2/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-Prototype-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%A6%AC)
자바스크립트 객체는 `prototype` 이라는 내부 프로퍼티가 존재한다. 
이것을 왜 사용하는가 간단히 설명 하자면 
``` js
function.man
(
	this.eyes =2
  	this.nose =1
) 
let hee = new man()
let chan = new mna()
```
둘은 공통적으로 속성 두개를 가지고 있는데 메모리에는 eyes와 nose가 두개씩 총 4개 가 할당된다
이게 여러개면 여러개의 메모리를 차지하게 된다. 이문제를 프로토탑으로 해결 할 수 있다. 
```js
function man () {}
man.prototype.eyes =2
man.prototype.nose =1

let hee = new man()
let chan = new mna()
```
man.prototype이라는 빈 `Object`가 어딘가에 존재하고, man 함수로부터 생성된 객체 (hee,chan) 은 어딘가에 존재하는 오브젝트 에 들어있는 값을 모두 갖다쓸 수 있다. 
즉 어딘가에 있는 빈공간에 넣어놓고 두 객체는 공유해서 사용하는 거다. 

---



[출처](https://jsdev.kr/t/javascript-prototype/2853)
	자바스크립트 에서 함수를 생성하면 동시에 생성되는 프로토타입을 new 키워드로 함수에서 찍어낸 모든 객체들이 이 프로토타입을 참조한다. 
    먼저 함수를 생성하면 단순히 함수객체만 생성되는 것이 아니라 `prototype` 객체가 동시에 생성이 됩니다. 
    ![](https://images.velog.io/images/pen9508901/post/715829dc-38ec-4e1e-94b3-590836ca0827/proto.jpg)
    
   함수를 선언하면 위와 같은 구조의 2개의 객체가 생성이 된다. `function` 객체 , `prototype`객체 .
   2개의 객체가 생성된 것만 아니라 두 객체는 서로 이어져있는데. 함수에서는 프로토타입 객체에 `prototype`이라는 내부변수로 접근 할 수 있다 프로토타입에서는 `constructor`라는 변수로 함수에 접근 할 수 있게 됩니다. 서로를 참조하는 래퍼런스 변수를 통해 두 객체는 접근 뿐만 아니라 변경도 가능하다 .

``` js
function Foo (){}
Foo.prototype.proto_val = '원형 값';

Foo.prototype.constructor.construct_val = "생성자 값";

console.log(Foo.prototype.proto_val); //원형 값 을 출력
console.log(Foo.construct_val); //생성자 값 을 출력
console.log(Foo.proto_val); //?! undefined를 출력
```
위의 예제외 같이 프로토타입에 새로운 속성을 추가할 수도 있고, 반대로 프로토탑입에 접근하여 
constructor 참조변수로 함수에 접근하여 속성을 추가할 수 도 있다. 하지만 맨마지막, Foo에서 직접 프로토타입에 설정한 속성에 접근하려고 하니깐 접근할 수 없어 'undefined'로 출력 된 것을 볼 수 있습니다. *이 것으로 보아 함수는 프로토타입을 생성하지만 함수 스스로가 프로토타입으로부터 값을 얻지 못함을 알 수 있다. * 이렇게 설정을 해놓고 쓸 수 없다면 의미가 없다. 그렇다면 프로토타입이 *원형* 이라는 자신의 이름값을 하기 위해서는 다른 방식접근해야 하는데 그방법은 바로 *인스턴스를 생성*하는 것이다 .
![](https://images.velog.io/images/pen9508901/post/a7322e4f-d675-415e-9931-a70dbe98d21e/new%E1%84%8F%E1%85%B5%E1%84%8B%E1%85%AF%E1%84%83%E1%85%B3.jpg)

```js
function Foo(){
    //var this = {}; this가 생성이 됩니다.  
    //이 this는 dunder proto라는(__proto__)라는 참조변수로 프로토타입 객체를 참조합니다.
    //return this; this가 반환됩니다.
}
var foo_instance = new Foo();
```
new를 할 경우 this 객체가 생성되며, 또한 반환되어 foo_instance에서 해당 객체를 사용할 수있게 됩니다. *주목해야 할점은 this가 가지고 있는 내부 변수*은 *proto* 가 무엇을 가르키고 있느냐입니다.
*this는 Foo의 프로토타입*을 가르키게 되고 , Foo 함수 객체와는 다르게 *Foo의 함수에 직접 접근 할 수 있는 권한*을 가지게 됩니다. 

```js
function Foo (){} //함수 선언
Foo.prototype.proto_val = "접근 할 수있다!!"; //Foo의 프로토타입을 설정

var foo_instance = new Foo();  //Foo의 인스턴스를 생성

console.log(foo_instance.proto_val); //접근 할 수있다!! 를 출력
```
접근 할 수 있다는 의미보다는 프로토타입에 있는 값을 고스란히 복제해서 가져온다는 의미

```js
function Foo(){}

Foo.prototype.proto_val = 100;    //프로토타입 속성을 설정합니다.

var foo_instance = new Foo();  //Foo의 인스턴스를 생성합니다.

console.log(foo_instance.proto_val); //프로토타입값 100을 출력을 합니다.

foo_instance.proto_val -= 1;  // 인스턴스에서 proto_val을 1줄여봅니다.

console.log(foo_instance.__proto__.proto_val); //프로토타입의 값은 그대로입니다.
console.log(foo_instance.proto_val);  //하지만 인스턴스는 1이 줄은 99가 출력이 됩니다
```

프로토타입 속성 proto_val을 선언하고 100을 저장한 이후에 Foo로 인스턴스를 만들어내면

해당 인스턴스에서 Foo의 prototype에 있는 값을 사용 할 수 있게 됩니다.

그렇지만 prototype에 있는 속성을 직접 바꿀 수 있는 것은 아닙니다.

foo_instance에서 proto_val을 1줄였는데

아래 출력 값에 보면 프로토타입에 있는 값은 그대로인데

foo_instance에서 접근한 proto_val은 99가되어 있는 것을 보실 수 있습니다.

즉, prototype이 왜 원형이란 의미를 가지는지가 바로 여기서 나오는 것 같습니다.

비록 prototype에서 값을 가져오지만 어디까지나 프로토타입은 인스턴스를 찍어낼 때 참조 가능한 기본값을 가지고

각각의 인스턴스는 프로토타입으로 받은 값을 개별로 사용이 가능하게 된다는 것입니다.

그래고 한가지 더 참고를 하자면 언제 프로토타입에 있는 값을 인스턴스가 자신의 메모리로 가져오냐 입니다.
```js
function Foo(){}

Foo.prototype.proto_val = 100;

var foo_instance = new Foo();

console.log(foo_instance.proto_val === Foo.prototype.proto_val); //true를 출력합니다.

foo_instance.proto_val = foo_instance.prototype.proto_val -1;

console.log(foo_instance.proto_val === Foo.prototype.proto_val); //false를 출력합니다.

```

처음에는 인스턴스에서 호출한 proto_val과 프로토타입의 proto_val값이 같다고 나옵니다.

하지만 인스턴스의 proto_val을 1 줄이자 이번에는 똑같은 표현이 false로 나옵니다.

이를 통해 어떤 방식으로 프로토타입과 객체의 메모리가 관리가 되어지고 참조가 되어지는지 유추 할 수 있습니다.

프로토타입의 속성을 인스턴스에서 접근만 한다면 프로토타입에 값이 있는지 확인한 뒤에 참조만 하지만

인스턴스에서 프로토타입 속성으로 연산을 하려고 할 경우, 인스턴스에 메모리가 주어지고, 프로토타입의 속성을

호출하여 받은 값을 연산하여 할당된 메모리에 저장이 되어집니다.

이렇게 할당받은 메모리로 인스턴스는 고유의 속성을 가지게 되는겁니다.

간단하게 말해, 인스턴스에 메모리가 프로토타입으로부터 주어지는 경우는 인스턴스에서 프로토타입의 속성을 변경하려고

할 때라고 말할 수도 있을 것 같습니다.

물론 저 표현이 애매하다고 생각 할 수도 있을 것 같습니다. 왜냐하면 인스턴스 자체에서도 프로토타입에 직접 접근하여

프로토타입에 있는 기본값을 instance에서 바꿔버릴 수도 있기 때문입니다.

```js
function Foo(){}

Foo.prototype.proto_val = 100;

var foo_instance = new Foo();

console.log(foo_instance.proto_val); //100을 출력합니다.

foo_instance.proto_val -= 1; //1을 빼봅니다.

foo_instance.__proto__.proto_val = 50; // 원형 값을 바꿀 수 있습니다.

var foo_instance2 = new Foo();  //바꾼 뒤에 Foo로 부터 생성된 객체는 다른 기본 값을 가지게 됩니다.

console.log(foo_instance2.proto_val);  // 50을 출력합니다.

console.log(foo_instance.proto_val); //여전히 99를 출력합니다
```
foo_instance.proto.proto_val을 50으로 설정하자 다음 생성된 객체 foo_instance2.proto_val이 50을 가르키는 것을

볼 수 있습니다.

객체가 자신의 기본값을 바꿔버린거죠.
~~(어찌보면 편리하다고 생각할 수 있는 기능일 수도 있겠으나 한번더 생각해 보면 무시무시한 기능 같습니다.

다른 언어 같은 경우 class를 걸어 넣고 객체를 생성할 경우 객체에서는 클래스를 건드릴 수 없습니다.

그렇기 때문에 각각의 객체들이 원형이 변경될 위험으로부터 보호받을 수 있지만

자바스크립트에서는 생성자인 함수뿐만 아니라 생성자로부터 만들어진 객체까지 원형에 손을 댈 수있고,

그 과정이 컴파일 과정도 아니고 런타임에서 일어난다는 것은 매우 위험해 보입니다.

그래서 더욱 자바스크립트의 프로토타입 개념을 사용 할 때는 조심을 기해야 겠다는 생각이 듭니다)~~


## __proto__, constructor, prototype

prototype =>  *Prototype Link* 와 *Prototype Object*라는 것이 존재합니다. 그리고 이 둘을 통틀어 Prototype이라고 부릅니다. 
	*Prototype Object* 객체는 언제나 함수로 생성됩니다. 
	```js
    	function Person() {} // => 함수
		var personObject = new Person(); // => 함수로 객체를 생성
	    ```
	personObject 객체는 Person이라는 함수로 생성된 객체입니다. 이렇듯 언제나 객체는 함수에서 시작됩니다
    ```js
    	var obj = {}
    ```
    위코드와 아래 코드는 같다
    
    ```js
    var obj = new Object()
    ```
    
   Object가 자바스크립트에서 기본적으로 제공하는 함수입니다
   
   Object와 마찬가지로 Function, Array도 모두 함수로 정의되어 있습니다. 이것이 첫 번째 포인트입니다.
그렇다면 이것이 Prototype Object랑 무슨 상관이있느냐? 함수가 정의될 때는 2가지 일이 동시에 이루어집니다.

*1.해당 함수에 Constructor 자격 부여*
	Constructor 자격이 부여되면 new를 통해 객체를 만들어 낼 수 있게 됩니다. 이것이 함수만 new 키워드를 사용할 수 있는 이유입니다.
    
*2.해당 함수의 Prototype Object 생성 및 연결*
함수를 정의하면 함수만 생성되는 것이 아니라 Prototype Object도 같이 생성이 됩니다.
![](https://images.velog.io/images/pen9508901/post/b15fe038-4083-45db-9dab-04cb47d67ed0/%E1%84%92%E1%85%A1%E1%86%B7%E1%84%89%E1%85%AE%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%B4.png)

그리고 생성된 함수는 prototype이라는 속성을 통해 Prototype Object에 접근할 수 있습니다. Prototype Object는 일반적인 객체와 같으며 기본적인 속성으로 constructor와 __proto__를 가지고 있습니다.

constructor는 Prototype Object와 같이 생성되었던 함수를 가리키고 있습니다.

__proto__는 Prototype Link입니다.
![](https://images.velog.io/images/pen9508901/post/4dc28851-301c-4c84-a4ca-b3ee42de83aa/%E1%84%8B%E1%85%A8%E1%84%8C%E1%85%A6.png)

kim에는 eyes라는 속성이 없는데도 kim.eyes를 실행하면 2라는 값을 참조하는 것을 볼 수 있습니다. 위에서 설명했듯이 Prototype Object에 존재하는 eyes 속성을 참조한 것인데요, 이게 어떻게 가능한걸까요??

바로 kim이 가지고 있는 딱 하나의 속성 __proto__가 그것을 가능하게 해주는 열쇠입니다.
prototype 속성은 함수만 가지고 있던 것과는 달리

### __proto__속성은 모든 객체가 빠짐없이 가지고 있는 속성입니다.

__proto__는 객체가 생성될 때 조상이었던 함수의 Prototype Object를 가리킵니다. kim객체는 Person함수로부터 생성되었으니 Person 함수의 Prototype Object를 가리키고 있는 것이죠.

![](https://images.velog.io/images/pen9508901/post/ba64a9c2-ef40-45da-8470-c43574331a37/%E1%84%8B%E1%85%A6%E1%84%8C%E1%85%A62.png)

__proto__를 까보니 역시 Person 함수의 Prototype Object를 가리키고 있었습니다.

![](https://images.velog.io/images/pen9508901/post/44c1a95f-583b-4e47-a3ac-e74ea99bd4cd/%E1%84%8B%E1%85%A8%E1%84%8C%E1%85%A6%203.png)
[참조](https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67)

kim객체가 eyes를 직접 가지고 있지 않기 때문에 eyes 속성을 찾을 때 까지 상위 프로토타입을 탐색합니다. 최상위인 Object의 Prototype Object까지 도달했는데도 못찾았을 경우 undefined를 리턴합니다. 이렇게 __proto__속성을 통해 상위 프로토타입과 연결되어있는 형태를 프로토타입 체인(Chain)이라고 합니다.

![](https://images.velog.io/images/pen9508901/post/1cfcbd27-93dc-4269-bfbb-aefb2ac2ba5a/%E1%84%8B%E1%85%A8%E1%84%8C%E1%85%A64.png)

## Object.create
자바스크립트 언어는 상속을 통해 부모 객체의 기능을 물려받고,
본인만의 새로운 기능을 추가할 수 있다.
이러한 상속의 기능을 제대로 구현하기 위해서는 Object.create()를 활용 해야 한다!

## class 키워드 및 super 키워드 이용 방법

클레스는 
```js
class Parent {
    constructor (name) {
    	this.name = name;
    }
}
```
이렇게 구성지게 사용 
super 키워드는 자식 클래스의 생성자에는 반드시 super가 있어야한다. 이떄 슈퍼는 메소드 처럼 사용하는 안에 파라미터로 호출하고 싶은 부모 클래스의 클래스필드를 적어줍니다.

```js
class Child extends Parent {
	constructor (name, address) {
    	super(name);
        this.address = address;
    }
}
```

Child 클래스를 인스턴스화할 때 첫번째 파라미터인 name값은 부모클래스의 name을 호출하여 사용하게 됩니다


```js
class Parent {
  constructor (name) {
  	this.name = name;
  }
  introduce () {
  	return 'My name is ' + this.name;
  }
}
class Child extends Parent {
  constructor (name, address) {
  	super(name);
  	this.address = address;
  }
}
const me = new Child('cocoder', 'Seoul');
console.log(me.introduce()); // My name is cocoder
```
이렇게 부모 클래스의 클래스필드 값을 자식 클래스의 인스턴스의 파라미터에서 정의하고 그 값을 부모 클래스의 메소드를 호출할 때 사용할 수 있습니다.


[출처](https://cocoder.tistory.com/143)
