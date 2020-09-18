
# Data Structure 💻

        1.개념의 이해 뿐만 아니라 직접 자료구조를 구현해야 한다면 어떻게 코드를 작성해야 할지 생각하기
        2.자료 구조의 모양 추상적으로 그림 그리기
        3.해당 자료구조가 가지고 있는 property 와 method 찾아보기
        4.각 method는 어떻게 동작되는 것인지 알아보고 의사 코드 직접 작성해보기



<hr>

*자료구조* 는 데이터들의 모임 , 관계,  함수 , 명령 등의 집합을 의미한다.


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
## 해쉬 테이블 
[참고](https://evan-moon.github.io/2019/06/25/hashtable-with-js/)
 *해시 테이블*은 어떤 특정 값을 받으면 그 값을 해시 함수에 통과시켜 나온 `index`에 저장하는  자료 구조이다.

직접 주소 테이터 (Direct Address Table ) 에서 시작 되었다. 
*단점* 을 해시 함수로 보완 하면서 등장?

`해시 함수` 를 통해 변환 되어 나온 인덱스 값 을 통하면 만든다. === `해싱`
그런데 이렇게 `해싱` 되어 나온 `Index` 의 값이 두개 이상이 되었을떄 즉, 같은 값 이 나올떄
*해시의 충동 (Collision)*이 발생한다.
##### 충돌 해결

        1.선형 탐사법 (Linear Probing)
        선형으로 순차적으로 탐사하는 방법 충돌 발생시 정해진 n칸만큼의 옆 방을 주는 방법
        n = 1 이라면 2번 인덱스를,  n = 3 이라면 4번 인덱스에 저장
        2.제곱 탐사법 (Quadratic Probing)
        탐사하는 폭이 고정폭 아닌 제곱으로 늘어난다
        충돌 지점으로 부터 1(제곱)만큼 , 두번째 충돌이 발생시 2의 제곱, 세번째는 3의 제곱 이런식
        으로 커진다.
        3.이중 해싱(Double Hashing)
        해시 함수를 이중으로 사용하는 방법 
        최초 해시를 얻을 떄 사용후 , 충돌이 났을 경우 탐사 이동폭을 얻기 위해 사용한다. 
        최초 해시로 같은 값이 나오더라도 다시 다른 해시 함수를 거치면서 다른 탐사 이동폭이
        나올 확률이 높기 떄문에 매번 다른 공간에 값이골고루 저장될 확률도 높아진다.
        
        
```
            const myTableSize = 23; // 테이블 사이즈가 소수여야 효과가 좋다
            const myHashTable = [];

            const getSaveHash = value => value % myTableSize;

            // 스텝 해시에 사용되는 수는 테이블 사이즈보다 약간 작은 소수를 사용한다.
            const getStepHash = value => 17 - (value % 17);

            const setValue = value => {
              let index = getSaveHash(value);
              let targetValue = myHashTable[index];
              while (true) {
                if (!targetValue) {
                  myHashTable[index] = value;
                  console.log(`${index}번 인덱스에 ${value} 저장! `);
                  return;
                }
                else if (myHashTable.length >= myTableSize) {
                  console.log('풀방입니다');
                  return;
                }
                else {
                  console.log(`${index}번 인덱스에 ${value} 저장하려다 충돌 발생!ㅜㅜ`);
                  index += getStepHash(value);
                  index = index > myTableSize ? index - myTableSize : index;
                  targetValue = myHashTable[index];
                }
              }
            }

```
        5.분리 연결법 (Separate Chaining)
        해쉬 테이블의 버킷에 하나의 값이 아니라 `Linked List` 나 `Tree`를 사용 
        
         `Linked List` 일떄 모습
         [0][1][2][3]
         [0] 의 안의 값은 [1]{key:value,next node 2}[2]{key:value,next node 3}[3]{key:value,next node null}


#### 테이블 크기 재할당(Resizing)

정적인 공간을 할당해서 많은 데이터를 담기 위한 자료구조인 만큼 언젠가 데이터가 넘치기 마련

분리 연결법을 사용하는 경우에는 테이블에 빈 공간이 적어지면서 충돌이 발생할 수록 각각의 버킷에 저장된 리스트가 점점 더 길어져서 리스트를 탐색하는 리소스가 너무 늘어난 상황이 발생

어느 정도 비워져 있는 것이 성능 상 더 좋으며, 해시 테이블을 운용할 때는 어느 정도 데이터가 차면 테이블의 크기를 늘려줘야한다

즉, `storage` 의 크기가 8 이라고 해보자 그떄 size 가 75 %가 되거나 높아지면 `리미티드`를 2배로 늘리고

만약 두배가 되어서 16 이되었고 size는  12라고 할떄 이떄 remove되어 size가 `리미티드` 
의 25% 가 되면 `리미티드` 를 절반으로 줄인다

또한 , 새로운 `storage` 에  기존의 `storage` 에 있던 값들을 해싱 해서 넣어주어야 한다.

*해쉬 함수*

```
const hashFunction = function(str, max) {
  let hash = 0;
  for (let i = 0; i < str.length; i++) {
    hash = (hash << 5) + hash + str.charCodeAt(i);
    hash &= hash; // Convert to 32bit integer
    hash = Math.abs(hash);
  }
  return hash % max;
};
```
*정적인 공간*
```
const LimitedArray = function(limit) {
  const storage = [];

  const limitedArray = {};
  limitedArray.get = function(index) {
    checkLimit(index);
    return storage[index];
  };
  limitedArray.set = function(index, value) {
    checkLimit(index);
    storage[index] = value;
  };
  limitedArray.each = function(callback) {
    for (let i = 0; i < storage.length; i++) {
      callback(storage[i], i, storage);
    }
  };

  var checkLimit = function(index) {
    if (typeof index !== 'number') {
      throw new Error('setter requires a numeric index for its first argument');
    }
    if (limit <= index) {
      throw new Error('Error trying to access an over-the-limit index');
    }
  };

  return limitedArray;
};
```
*해쉬 테이블*

```
class HashTable{
    constructor(){
        this._size = 0;
        this._limit = 8;
        this._storage = LimitedArray(this._limit);
}
``` 
storage 는 Table 이라고 생각 해보자 해쉬 테이블은 `정적` 이다.


추가 및 리사이징
```
insert(key, value){ // key = 'cat' , value = 'fastly'
    const index = hashFunction(key, this._limit);
    //index 는 해쉬 함수로 해싱 되어온 값이다.
    //값이 1 로 해싱 되어 왔다 
    let tuple = {} // 인덱스에 들어갈 녀석
    
    // index에 값이 없는 경우
    if(!this._storage.get(index)){ // index = 1 
    tuple[key] = value;  // {cat:'fastly'}
    }
    // index에 값이 이미 있는 경우
    else {
    tuple = this._storage.get(index) //이미 있는 값을 불러와  tuple에 할당
    tuple[key] = value // ex) [1]{cat:'fastly',dog:'fastly'}
    this._storage.set(index, tuple); //갱신
     }
     this._size++;
     
     //resizing 해야 하는 경우 
     if((this._size / this._limit * 100 >= 75) { // size가 75를 넘을떄 
        this._resize(this._limit * 2) // 리미티를 두 배로
     }
}
```
삭제 및 리사이징
```
remove(key) {
  const index = hashFunction(key, this._limit);
  // index가 undefined -> return undefined
  //삭제할 값이 없을떄
  if (!this._storage.get(index)) {
    return undefined;
  }
  
  else {//삭제할 값이 있을떄 
    let tuple = this._storage.get(index); // {cat:'fastly',dog:'fastly'}
    let save = tuple[key] //key 'cat'이라면 {cat : 'fastly'} 저장 
    //왜? 리턴용..
    delete tuple[key] // 하고 남는 값은 {dog:'fastly'}
    this._storage.set(index, tuple); // 갱신 
    this._size--;
    //리사이징 
    //만약 limit  이 16 까지 늘어난 상태라고 생각 하라 그때 size 는 12
    //라 가정 이떄 remove로 인하여 값이 4 까지 내려 같다고 생각 하라 
    if (this._size / this._limit * 100 <= 25 && this._limit > 8) {
      this._resize(this._limit / 2);
    } else if (this._limit > 8 && this._size <= 4) {
      this._resize(this._limit / 2);
    }
    return save;
  }
}
```
찾기

```
retrieve(key) {
  const index = hashFunction(key, this._limit);
  // index에 값이 없는 경우 -> undefined 리턴
  if (!this._storage.get(index)) {
    return undefined;
  }
  // index에 값이 있는 경우 -> value 리턴
  else {
    return this._storage.get(index)[key]; //{cat:'fastly',dog:'fastly'} 
    // 이중 key cat 인 녀석의 값인 'fastly' 가 리턴 된다. 
  }
}
```
*리사이징*
```
 _resize(newLimit) { // this._limit * 2 
    let before = this._limit  // 16     // 8
    this._limit = newLimit;  // 8       // 16
    // 새로운 스토리지
    let newStorage = LimitedArray(this._limit);
    // 기존 스토리지 -> 새로운 스토리지에 넣기
    for (let i = 0; i < before; i++) { // 기존의 것
      // tuple의 키를 모두 추출해서
      for (let key in this._storage.get(i)) {
        // 키 -> 해시 함수에 넣어서 새로운 인덱스를 추출
        let index = hashFunction(key, this._limit);
        let value = this._storage.get(i)[key] 
        let tuple = {};
        // 새로운 인덱스를 새로운 스토리지에 넣는다
        if (!newStorage.get(index)) {
          tuple[key] = value;
          newStorage.set(index, tuple);
        } else {
          tuple = newStorage.get(index);
          tuple[key] = value;
          newStorage.set(index, tuple);
        }
      }
    }
    this._storage = newStorage;
  }
}
```

#Tree

#Graph

#binary
