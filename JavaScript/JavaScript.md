
# JavaScript

<img src="/path/to/img.jpg" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="https://user-images.githubusercontent.com/61407010/91863660-4f76c280-ecaa-11ea-8668-cee239d24b93.jpgg"></img><br/>
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
  
