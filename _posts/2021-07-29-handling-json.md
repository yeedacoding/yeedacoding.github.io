---
title:  "[Python] Python 내장 모듈 json으로 JSON데이터 핸들링"
excerpt: "Python 내장 모듈 json으로 JSON 데이터 다루기"

categories:
  - python 
tags:
  - python
  - json

last_modified_at: 2019-04-13T08:06:00-05:00
---

# Python 내장 모듈 json으로 JSON데이터 핸들링

python에서 JSON 데이터를 처리하기 위해서 사용되는 python 내장 모듈인 json의 함수에 대해 알아보자.

[json 내장모듈 공식 레퍼런스]([json — JSON encoder and decoder — Python 3.9.6 documentation](https://docs.python.org/3/library/json.html))

## <span style="color : red">loads()</span> : JSON 문자열을 Python 객체로 변환

JSON 문자열을 Python의 객체로 변환하기 위해서 loads()함수를 사용한다.

**JSON Decoding **: JSON 문자열을 Python 객체로 변환하는 것

```python
import json

#문자열 형태의 JSON 데이터
json_string = '''{
  "squadName": "Super hero squad",
  "homeTown": "Metro City",
  "formed": 2016,
  "secretBase": "Super tower",
  "members": [
    {
      "name": "Molecule Man",
      "age": 29,
      "secretIdentity": "Dan Jukes",
      "powers": [
        "Radiation resistance",
        "Turning tiny",
        "Radiation blast"
      ]
    },
    {
      "name": "Madame Uppercut",
      "age": 39,
      "secretIdentity": "Jane Wilson",
      "powers": [
        "Million tonne punch",
        "Damage resistance",
        "Superhuman reflexes"
      ]
    }
  ]
}'''

#Python 객체 생성
json_object = json.loads(json_string)
print(json_object)
```

```json
# print 결과

{'squadName': 'Super hero squad', 'homeTown': 'Metro City', 'formed': 2016, 'secretBase': 'Super tower', 'members': [{'name': 'Molecule Man', 'age': 29, 'secretIdentity': 'Dan Jukes', 'powers': ['Radiation resistance', 'Turning tiny', 'Radiation blast']}, {'name': 'Madame Uppercut', 'age': 39, 'secretIdentity': 'Jane Wilson', 'powers': ['Million tonne punch', 'Damage resistance', 'Superhuman reflexes']}]}
```

json_object(python 객체)에 JSON 문자열 데이터가 잘 변환되어 저장되었다.

Python 객체의 각 요소들을 호출해보겠다.

```python
print(json_object['squadName'])
print(json_object['homeTown'])
print(json_object['formed'])
print(json_object['members'])
```

```python
# 결과
Super hero squad
Metro City
2016
[{'name': 'Molecule Man', 'age': 29, 'secretIdentity': 'Dan Jukes', 'powers': ['Radiation resistance', 'Turning tiny', 'Radiation blast']}, {'name': 'Madame Uppercut', 'age': 39, 'secretIdentity': 'Jane Wilson', 'powers': ['Million tonne punch', 'Damage resistance', 'Superhuman reflexes']}]
```

호출한 key값의 value들이 잘 출력되었다.



## <span style="color : red">dumps()</span> : Python 객체를 JSON 문자열로 변환

Python 객체를 JSON 문자열로 변환하기 위해서 dumps()함수를 사용한다.

**JSON Encoding** : Python 객체를 JSON 문자열로 변경하는 것

```python
import json

#python 객체
json_object = {
  "squadName": "Super hero squad",
  "homeTown": "Metro City",
  "formed": 2016,
  "secretBase": "Super tower",
  "members": [
    {
      "name": "Molecule Man",
      "age": 29,
      "secretIdentity": "Dan Jukes",
      "powers": [
        "Radiation resistance",
        "Turning tiny",
        "Radiation blast"
      ]
    },
    {
      "name": "Madame Uppercut",
      "age": 39,
      "secretIdentity": "Jane Wilson",
      "powers": [
        "Million tonne punch",
        "Damage resistance",
        "Superhuman reflexes"
      ]
    }
  ]
}

#JSON 문자열로 변환하기
json_string = json.dumps(json_object)
print(json_string)
```

위 코드의 결과를 보면 JSON 문자열이 한 줄로 길게 표현된다. 만약 이러한 형태의 JSON 문자열을 화면에 표시할 필요가 있는 경우 한 줄로 표현 되어 있는 문자열이라 읽기가 불편할 것이다. 따라서 json.dumps()로 함수를 호출할 때 **"indent" **파라미터에 숫자를 넘기면 그 숫자만큼 들여쓰기가 되어 JSON 문자열을 변환하게 된다.

```python
import json

json_object = #위 코드와 동일

#JSON 문자열로 변환하기, indent 파라미터 부여
json_string = json.dumps(json_object, indent=2)
print(json_string)
```

```python
# 결과

{
  "squadName": "Super hero squad",
  "homeTown": "Metro City",
  "formed": 2016,
  "secretBase": "Super tower",
  "members": [
    {
      "name": "Molecule Man",
      "age": 29,
      "secretIdentity": "Dan Jukes",
      "powers": [
        "Radiation resistance",
        "Turning tiny",
        "Radiation blast"
      ]
    },
    {
      "name": "Madame Uppercut",
      "age": 39,
      "secretIdentity": "Jane Wilson",
      "powers": [
        "Million tonne punch",
        "Damage resistance",
        "Superhuman reflexes"
      ]
    }
  ]
```



## <span style="color : red">load()</span> : JSON 파일을 Python 객체로 불러오기

JSON 파일에 저장된 데이터를 읽어서 Python 객체로 불러오고 싶은 경우 load() 함수를 사용한다.

```python
#data.json 파일

{
  "squadName": "Super hero squad",
  "homeTown": "Metro City",
  "formed": 2016,
  "secretBase": "Super tower",
  "members": [
    {
      "name": "Molecule Man",
      "age": 29,
      "secretIdentity": "Dan Jukes",
      "powers": [
        "Radiation resistance",
        "Turning tiny",
        "Radiation blast"
      ]
    },
    {
      "name": "Madame Uppercut",
      "age": 39,
      "secretIdentity": "Jane Wilson",
      "powers": [
        "Million tonne punch",
        "Damage resistance",
        "Superhuman reflexes"
      ]
    }
  ]
}
```

```python
import json

with open('data.json') as f :
    json_object = json.load(f)

print(json_object)
print(json_object['squadName'])
print(json_object['homeTown'])
print(json_object['formed'])
print(json_object['members'])
```

```json
# 결과

{'squadName': 'Super hero squad', 'homeTown': 'Metro City', 'formed': 2016, 'secretBase': 'Super tower', 'members': [{'name': 'Molecule Man', 'age': 29, 'secretIdentity': 'Dan Jukes', 'powers': ['Radiation resistance', 'Turning tiny', 'Radiation blast']}, {'name': 'Madame Uppercut', 'age': 39, 'secretIdentity': 'Jane Wilson', 'powers': ['Million tonne 
punch', 'Damage resistance', 'Superhuman reflexes']}]}
Super hero squad
Metro City
2016
[{'name': 'Molecule Man', 'age': 29, 'secretIdentity': 'Dan Jukes', 'powers': ['Radiation resistance', 'Turning tiny', 'Radiation blast']}, {'name': 'Madame Uppercut', 'age': 39, 'secretIdentity': 'Jane Wilson', 'powers': ['Million tonne punch', 'Damage resistance', 'Superhuman reflexes']}]
```



## <span style="color : red">dump()</span> : Python 객체를 JSON 파일에 저장하기

Python 객체를 JSON 문자로 변환한 결과를 파일에 바로 쓰고(write) 싶은 경우 dump() 함수를 사용한다.

```python
import json

json_object = {
  "squadName": "Super hero squad",
  "homeTown": "Metro City",
  "formed": 2016,
  "secretBase": "Super tower",
  "members": [
    {
      "name": "Molecule Man",
      "age": 29,
      "secretIdentity": "Dan Jukes",
      "powers": [
        "Radiation resistance",
        "Turning tiny",
        "Radiation blast"
      ]
    },
    {
      "name": "Madame Uppercut",
      "age": 39,
      "secretIdentity": "Jane Wilson",
      "powers": [
        "Million tonne punch",
        "Damage resistance",
        "Superhuman reflexes"
      ]
    }
  ]
}

with open('data.json', 'w') as f:
    json_string = json.dump(json_object, f, indent=2)
```

```json
# 생성된 data.json 파일

{
  "squadName": "Super hero squad",
  "homeTown": "Metro City",
  "formed": 2016,
  "secretBase": "Super tower",
  "members": [
    {
      "name": "Molecule Man",
      "age": 29,
      "secretIdentity": "Dan Jukes",
      "powers": [
        "Radiation resistance",
        "Turning tiny",
        "Radiation blast"
      ]
    },
    {
      "name": "Madame Uppercut",
      "age": 39,
      "secretIdentity": "Jane Wilson",
      "powers": [
        "Million tonne punch",
        "Damage resistance",
        "Superhuman reflexes"
      ]
    }
  ]
}
```

