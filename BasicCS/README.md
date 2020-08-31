# Basic CS
========================
>🔥  :  Today I learned 

<hr/>

### Pair Programming

A,B 페어가 각각 마스터브레치 에서 포크를 각자의 repo에  복사를 해온다.

<pre><code>{git clone URL}</code></pre>

페어의 repo와 내 것을 연결하는 작업이 필요

```
git remote add pair URL
```
     연결되었는지 확인
```
git remote -v
```
A 페어가 코드 작성 후 커밋을 한다. 이후 자신의 repo 에 push 한다. 

B 페어가 깃 pull 을 하고 코드를 수정하고 자신의 repo 에 push 한다.

상대의 깃을 풀 하는법
```
git pull pair master
```

### Git branch
>분리된 작업 영역

새로운 기능을 개발할 떄 ,  원본에 영향을 주지 않고 다양한 시도를 하고 싶을 때
1. 브랜치 만들기
        브랜치는** 현재 작업 공간**을 베이스로 만들어진다.
        즉,내가 현재 작업하고 있는 곳을 항상 확인하자!
2.작업 공간을 옮기자 

        ```
        git checkour 브랜치 이름
        ```
        
        현재 작업공간이 옮기고자 하는 브랜치로 옮겨 진다.
        
3. 브랜치 생성

        ```
            git checkout -b 기능
        ```
        
        -b 는 생성과 동시에 작업공간이 옮겨진다.
        
        
<hr>

