
# 질문!
<hr>

```js
class DancerClass {
  // your code here
  constructor(top, left, timeBetweenSteps) {
    this.top = top;
    this.left = left;
    this.timeBetweenSteps = timeBetweenSteps;

    this.dancer = {};
    this.$node = this.createDancerElement();
    this.step();
    this.setPosition(top, left)
  }

  createDancerElement() {
    let elDancer = document.createElement('span');
    elDancer.className = 'dancer';
    return elDancer;
  }
  step() { //앞의 this는 DancerClass 바인드의 this 는 step 이라는 함수가 실행후 몇초가 지나서 실행되기에 그때에 this를 모른다. 고로 현재 this(callback함수) 를 고정시킨다.
    setTimeout(this.step.bind(this), this.timeBetweenSteps);
  }
  setPosition(top, left) {
    Object.assign(this.$node.style, {
      top: `${top}px`,
      left: `${left}px`
    });
  }
}
```

`DancerClass`에 `this.steop()` , `this.setPosition(top,left)`는 왜 꼭 서야지만 될까? 
나의 추측은 이렇다 .
~~1. step 메서드 에 setTimout 이 있다 이떄 콜백 을 bind 를 하여 현재 this가 callback함수를 고정시키기 위해서 ?~~
~~2. 새롭게 생기는 각 객체마다 다 다른 값을 분간 시키기 위해서 ~~
`constructor` 에 저 메서드 들을 해놓은 이유는 저렇게 해놓지 않으면 실행을 할 수 없어서 였다. ✔️




