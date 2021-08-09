---
title:  "[Python] Python 함수 여러 개의 입력값 받기"
excerpt: "여러 개의 입력값을 받는 함수 만들기"

categories:
  - python
tags:
  - python
  - function

last_modified_at: 2019-04-13T08:06:00-05:00
---

# 여러 개의 입력값을 받는 함수 만들기

함수를 정의할 때 입력값이 한 개, 두 개, 이렇게 입력받을 값의 갯수가 정해지지 않는다면 어떻게 할까?

파이썬에서는 이러한 상황을 해결하기 위해 함수를 정의할 때, 파라미터 이름 앞에 **'*'**문자를 붙여 정의해준다.

```python
def add_many(*args):
    result = 0
    for i in args:
        result += i
    return result


result1 = add_many(1, 2, 3)
print(result1)			#결과는 6

result2 = add_many(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
print(result2)			#결과는 55
```

*파라미터이름 -> 이런 식으로 파라미터를 정의해주면 입력값이 몇 개이든 상관이 없어진다.

*args 처럼 정의한다면 함수 add_many에 전달되는 입력값을 전부 모아 튜플로 만들어준다. add_many(1,2,3)처럼 호출한다면 args는 (1,2,3)이 되는 것이다. (args는 임의의 이름, 어떤 이름을 정해주어도 상관없음)



### *args 외에 또 다른 파라미터 설정하기

```python
def add_and_mul(choice, *args) :
    if choice == 'add' :
        result = 0
        for i in args :
            result += i
        return result
    
    elif choice == 'mul':		#곱셈
        result = 1
        for i in args :
            result *= i
        return result
    
result1 = add_and_mul('add', 1,2,3,4,5)
print(result1)			#결과는 15

result2 = add_and_mul('mul', 1,2,3,4,5)
print(result2)			#결과는 120
```

choice 파라미터에 'add'가 전될되면 덧셈을, 'mul'이 전달되면 곱셈을 진행시키는 함수이다. *args를 사용하여 몇 개의 값을 입력받든 choice에 따라 덧셈 혹은 곱셈을 하게 된다.



### 키워드 파라미터

키워드 파라미터를 사용할 때는 파라미터 앞에 '**' 별 두개를 붙인다.

```python
def print_kwargs(**kwargs):
    print(kwargs)
    
print_kwargs(a=1)
print_kwargs(name = '태헌', age=25 )
```

print_kwargs는 파라미터 kwargs를 출력하는 함수이다.

첫번째 호출의 결과는 {a : 1}

두번째 호출의 결과는 {name : '태헌', age = 25}

모두 딕셔너리 형태로 만들어져서 출력되는 것을 알 수 있다.

즉, 함수를 정의할 때 파라미터 앞에 '**'를 붙이면 그 파라미터는 딕셔너리가 되어 key=value 형태의 결과값이 그 딕셔너리에 저장된다.(역시 kwargs라는 이름은 관례적으로 사용하는 것이고 어떤 이름이든 상관없다.)

