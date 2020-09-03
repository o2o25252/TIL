
# Data Structure 💻

        1.개념의 이해 뿐만 아니라 직접 자료구조를 구현해야 한다면 어떻게 코드를 작성해야 할지 생각하기
        2.자료 구조의 모양 추상적으로 그림 그리기
        3.해당 자료구조가 가지고 있는 property 와 method 찾아보기
        4.각 method는 어떻게 동작되는 것인지 알아보고 의사 코드 직접 작성해보기



<hr>

## 스택
특징!
        LIFO 
        공간이 꽉차면 더이상 푸쉬 불가
        `pop` 메서드는 스택의 맨 위에 값을 꺼내고 , 해당 값을 반환 한다.
        하노이의 탑 구조와 유사 

```
    class Stack {
        constructor() {
            this.storage = {};
            this.top = 0;
        }

        size() {
            return Object.keys(this.storage).length;
            
        }

        push(element) {
            this.element = element;

            this.storage[this.top] = this.element;
            
            this.top++;
                    }

        pop() {
            if (this.top - 1 === Number(Object.keys(this.storage)[this.top - 1])) {
            
                let save = this.storage[this.top - 1];
                delete this.storage[this.top - 1];
                this.top--;
                return save;
            }
        }
    }
```

```
        push(element) {
        this.element = element;

        this.storage[this.top] = this.element;        
```
현재 맨 마지막 위의 값 = element
`this.top` 의 값은현재 없는 값이다. 그러니 *-1* 을 해야 현재 
값을 가진 KEY [인덱스] 를 가리킨다. 

Object.keys(this.storage) 는 모든 키만 `["key1","key2"...]`이런식 으로 나온다.
Object.keys(this.storage)[0] -> `"key1"` 이렇게
그 인덱스의 키값만 나온다.
그래서 비교 할떄 `Number` 로 자료형을 맞춰 준것이다.

```
this.top++;
```
 top 의 값을 올려야 그 다음 인덱스가 들어올때 제일 위에 올라 올 수있다.

*pop( )*  부분

```
let save = this.storage[this.top - 1];
delete this.storage[this.top - 1];
this.top--;

return save;
```

맨위의 값  을 저장 을 한건 `return`을 위해 
삭제를 했기 떄문에 `top` 의 값을 내린다.

---

## 큐

특징!
        FIFO
        큐의 가장 앞에 있는 값을 꺼내서 반환
        rear는 큐의 마지막 값의 인덱스를 가리킨다.

```
class Queue {
    constructor() {
        this.storage = {};
        this.front = 0;
        this.rear = 0;
    }

    size() {
        return Object.keys(this.storage).length;
    }

    enqueue(element) {
        this.storage[this.rear] = element;
        this.rear++;
    }

    dequeue() {
        // 값 저장
        let save = this.storage[Number(Object.keys(this.storage)[0])];
        // 삭제
        delete this.storage[Number(Object.keys(this.storage)[0])];
        // front 할당
        this.front = Number(Object.keys(this.storage)[0]);
        // 리턴
        return save;
    }
}
```

```
    this.storage[this.rear] = element;
    this.rear++;
```

`FIFO`
    마지막 위치에 원소를 저장 한다.
    마지막 을 알기위해 `rear` 의 값을 올린다.

```
// 값 저장
let save = this.storage[Number(Object.keys(this.storage)[0])];
```
KEY 저장 리턴용 
```
// 삭제
delete this.storage[Number(Object.keys(this.storage)[0])];
```
```
// front 할당
this.front = Number(Object.keys(this.storage)[0]);
```
맨 앞의 키 저장 
```
// 리턴
return save;
```
    
    
