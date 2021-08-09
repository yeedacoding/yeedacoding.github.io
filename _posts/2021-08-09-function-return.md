---
title:  "[Python] Python 함수 return"
excerpt: "함수의 결과를 반환하기"

categories:
  - python
tags:
  - python
  - function
  - return

last_modified_at: 2019-04-13T08:06:00-05:00
---

# 함수의 결과를 반환(return)하기

![function](https://user-images.githubusercontent.com/83167676/128702220-a7e3aecf-a398-459c-811d-3a26b45c3b94.png)

기본적인 함수의 구조이다. x에 값을 넣으면 정의된 함수에서 계산이 진행되어 y라는 결과값을 돌려준다.



함수를 정의할 때 위의 예시에서 y값을 돌려주는 것과 같은 명령이 없다면 함수는 아무 결과값을 돌려주지 않는다. 함수의 결과값을 반환받기 위해서는 return을 사용한다.

```python
def func1(x):
    a = 3
    b = 5
    y = a * x + b
    return y

c = func1(5)
print(c)
```

위의 코드를 실행시키면 20이라는 결과를 얻을 수 있다.

함수를 호출할 때 5라는 인수가 파라미터 x에 전달되어 y = 3 * 5 + 5가 되어 결과적으로 y에는 20이라는 값이 들어갔다.

값을 반환하기 위해서 return 명령어가 사용되었다. 결과 y를 변수 c에 대입하고 print()함수를 사용하여 변수 c를 출력하였다. (굳이 반환값을 변수에 저장하지 않고 함수를 호출해도 결과는 동일)



만약, 함수를 정의할 때 return이 아닌 print(y)를 사용했다면?

```python
def func2(x) :
    a = 3
    b = 5
    y = a * x + b
    print(y)
    
d = func2(5)			# 결과는 20
print(d)				# 결과는 None
```

d=func2(5)라고 하면 파라미터 x에 인수 5가 전달되어 20이 호출되지만 return, 즉 반환되는 값은 아무것도 없기 때문에 변수 d에 전달되는 값 또한 아무것도 없게 된다. 따라서 print(d)를 하게 되면 결과값은 None이 출력된다.



위의 두 예제에서 알 수 있듯이 함수의 결과값은 오직 return 명령어로만 돌려받을 수 있다.





### return을 사용하여 함수 중간에서 빠져나오기

```python
def not_ten(x):
	if x == 10 :
        return
    print(x, '입니다.')

not_ten(5)			#결과는 5입니다.
not_ten(10)			#결과는 아무것도 출력되지 않는다.(None도 출력 안됨)
```

not_ten()함수에 5를 전달하면 print()를 통해 '5입니다.'가 출력되지만 10을 전달하면 return으로 함수 중간에서 바로 빠져나오므로 return밑의 print()함수는 실행되지 않는다.
