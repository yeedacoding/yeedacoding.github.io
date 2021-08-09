---
title:  "[Python] Python 예외 처리"
excerpt: "try, except 예외 처리"

categories:
  - python
tags:
  - python
  - try, except

last_modified_at: 2019-04-13T08:06:00-05:00
---

# 예외 처리

지금까지 코딩을 하면서 많은 오류들을  만나 구글링을 통해 해결해왔었다. 오류가 발생하기에  프로그램이 잘못 동작되는 것을 방지할 수 있지만 때때로 이러한 오류가 발생하는 것을 무시할 수도 있다. 파이썬에서는 **try, except를 사용하여 예외적으로 오류를 처리**할 수 있다. 



![예외처리](https://user-images.githubusercontent.com/83167676/128722826-909c9a35-125f-4553-a6d4-5ae2bfd30293.png)

try  블록 수행 중 오류가 발생하면 except 블록이 수행된다. 오류가  발생하지  않는다면 except 블록은  수행되지 않는다.



## 기본 구조

```python
try :
    "예외가 발생할 수 있는 문장"
except ("발생 오류") :
    "예외를 처리하는 문장"
```

except 뒤의 괄호("발생  오류")는 생략할 수도 있다.



1. 기본 예시 :

```python
try :
    4/0
except :
    print("0으로 나눌 수 없습니다.")
```

위의 간단한 코드를 보면, 나눗셈에서 분모에는 0이 들어갈 수 없기 때문에 ZeroDivisionError가 발생한다. 오류(예외)가 발생했기 때문에 except 블록이 수행되어 "0으로 나눌 수 없습니다."가 출력되게 된다.



2. 발생할 오류만 포함한 except문 :

```python
try :
    4/0
except ZeroDivisionError:
    print("0으로 나눌 수 없습니다.")

# 잘못된 오류 이름 지정
try :
    4/0
except IndexError :
    print("0으로 나눌 수 없습니다.")			#결과는 ZeroDivisinoError
```

4/0을 수행했을 때 발생하는 오류는 ZeroDivisionError 뿐이기 때문에 오류가 발생했을 때 except문에 미리 정해 놓은 오류 이름과 일치할 때만 except 블록이 수행할 수 있도록 했다.

그렇기 때문에 잘못된 오류 이름을 except문에 적용했을 때 error가 발생하게 된다.



3. 오류 메세지 변수에 담아 표현 :

```python
try :
    4/0
except ZeroDivisionError as e:
    print(e)
```

위 코드는 오류 메세지의 내용까지 알고 싶을 때 사용하는 방법이다.

결과는 **division by zero** 이다.



### 여러  개의 오류 처리하기

```python
try:
    a = [1, 2]
    print(a[2])
    4/0
except ZeroDivisionError:
    print("0으로 나눌 수 없습니다.")
except IndexError:
    print("인덱싱을 할 수 없습니다.")
```

맨 위의 표에서 알 수 있듯이 try블록 수행 중 여러 가지 오류가 발생하게 되면, **첫 번째로 발생한 오류에 대한 except 블록을 수행**하게 된다.

위의 코드에서는 try 블록에서 IndexError가 ZeroDivisionError보다 먼저 발생했기 때문에 "인덱싱을 할 수 없습니다."가 출력되고 "0으로 나눌 수 없습니다."는 출력되지 않는다.

코드를 간단하게 표현하기 위해

```python
try:
    a = [1, 2]
    print(a[2])
    4/0
except (ZeroDivisionError, IndexError) as e:
    print(e)
```

2개 이상의 오류를 동시에 처리하기 위해서는 위와 같이 except문에 발생할 오류를 괄호 안에 묶어서 처리하면 된다.

결과는 **index out of range**



### finally 블록

**finally 블록은 try문 수행 도중 예외 발생 여부에 상관없이 항상 수행**된다. 사용한 리소스를 close해야 할 때 많이 사용.

```python
f = open('myfile.txt', 'w')
data = "안녕하세요"
f.write(data)

try :
	f = open('myfile.txt', 'r')
	line = f.readline()
	print(line[5])
finally :
    f.close()
```

위의 예제에서 try 블록을 수행하게 되면 IndexError가 발생하지만 오류의 발생 여부와 관계없이 finally 절을 통해 f.close()로 열린 파일을 닫을 수 있다.



### 예외 발생시키기

**raise 명령어를 사용하여 오류를 강제로 발생**시킬 수 있다.

예를 들어, 어떠한 클래스를 상속받는 자식 클래스는 반드시 부모 클래스에서 정의된 함수를 구현하도록 하고 싶은 경우 raise 명령어를 사용할 수 있겠다.

```python
class Car:
    def drive(self):
        raise NotImplementedError	

#NotImplementedError : 꼭 작성해야 하는 부분이 구현되지 않았을 경우 발생되는 오류
```

Car 클래스를 상속받는 자식 클래스는 반드시 drive 함수를 구현해야한다.

만약 자식 클래스에서 drive함수를 구현하지 않는다면?

```python
class Benz(Car):
    pass

benz = Benz()
benz.drive()
```

Car 클래스를 상속받는 Benz 클래스에서 drive 함수를 구현하지 않았기 때문에 Car 클래스의 drive함수가 호출되어 raise 문에 의해 NotImplementedError가 발생하게 된다.



에러가 발생되지 않게 하기 위해서는..

```python
class Benz(Car):
    def drive(self):
        print("fast")

benz = Benz()
benz.drive()	# 결과는 fast
```

위 코드처럼 drive함수를 Benz 클래스 내에 정의하면 된다.(메서드 오버라이딩)





### 예외 만들기

**직접 예외를 만드는 경우, 파이썬 내장 클래스인 Exception 클래스를 상속**하여 만들 수 있다.

```python
class MyError(Exception):
    pass
```

```python
# pepsi는 없는 음료수 자판기
def beverage(bev) :
    if bev == "pepsi":
        raise MyError()
    print(bev)
    
beverage("coke")
beverage("sprite")
beverage("pepsi")
```

![예외1](https://user-images.githubusercontent.com/83167676/128731081-38b9ea05-d8a5-4ef6-8216-cce2381668d9.png)

coke와 sprite는 잘 출력되었지만 pepsi가 입력되면 MyError가 발생되게 코드를 짰기 때문에 정상적으로 에러가 발생한 결과이다.



```python
try :
    beverage("coke")
    beverage("pepsi")
except MyError:
    print("없는 음료입니다.")
```

이번에는 예외처리 기법으로 MyError를 처리해보았다.

![에외](https://user-images.githubusercontent.com/83167676/128731553-b0817442-74b9-4192-94aa-d5cb8246ff34.png)



전체 코드

```python
class MyError(Exception):
    pass

def beverage(bev):
    if bev == "pepsi":
        raise MyError()
    print(bev)

try:
    beverage("coke")
    beverage("pepsi")
except MyError:
    print("없는 음료입니다.")
```

try 블록에서 beverage()함수를 호출하여 각각 "coke"와 "pepsi"를 전달하였을 때 pepsi는 beverage()함수에서 MyError()를 발생시키게 된다.

발생할 에러 이름이 MyError기 때문에 except 문 뒤에 설정해준 MyError와 일치하여 "없는 음료입니다."가 출력되게 된다.