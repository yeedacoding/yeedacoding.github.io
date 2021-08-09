---
title:  "[Python] Python 함수 내 변수의 효력 범위"
excerpt: "함수 내에서 정의된 변수의 효력 범위"

categories:
  - python
tags:
  - python
  - function

last_modified_at: 2019-04-13T08:06:00-05:00
---

# 함수 내 변수의 효력 범위

```python
a = 1
def var(a):
    a += 1
    
var(a)
print(a)
```

위의 코드를 실행시켜보면 var(a)의 결과는 2가 아닌 1이 나온다.

그 이유는 함수 안에서 새로 만든 파라미터는 함수 내에서만 사용할 수 있기 때문이다.

```python
a = 1

def var(a):
    a += 1
    print(id(a))		#함수 내에 정의된 변수 a의 id값 출력


result = var(a)
print(id(a))		#첫번 째 줄에서 정의된 변수 a의 id값 출력
```

위의 코드를 실행시켜보면 함수 내에서 정의된 a의 id값과 밖에서 정의된 a의 id값은 서로 다른 것을 알 수 있다. 즉, 두 a는 전혀 다른 변수인 것이다.

그렇다면 함수 내에서 함수 밖의 변수를 변경할 수는 없을 까?



### 함수 내에서 함수 밖의 변수를 변경하는 방법

1. return 사용하기

```python
a = 1

def var(a) :
    a += 1
    return a

a = var(a)		#var(a)의 결과값을 함수 밖의 변수 a에 대입
print(a)		#결과는 2
```

var()함수는 입력으로 전달된 값에 1을 더하여 그 결과값을 반환한다. 따라서 a = var(a)라고 대입하면 a가 var()함수의 결과값으로 바뀌게 된다. 여기서도 물론 var()함수 안의 a와 함수 밖의 변수 a는 다른 것이다.



2. global 명령어 사용하기

```python
a = 1

def var(a) :
    global a		#global 명령어
    a += 1
    
var()
print(a)
```

함수 밖에서 정의된 변수 a를 함수 내에서 사용하기 위해 global 명령어를 사용하였다.

