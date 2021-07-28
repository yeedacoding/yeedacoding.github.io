---
title:  "[Python_mini_projects_4] 나에게 카카오톡 메세지 보내기 #1. 접근권한 설정 및 카카오톡 메세지 API 사용권한 받기"
excerpt: "접근 권한 설정 및 메세지 API 사용 권한받기(인증 코드와 사용자 토큰 발급)"

categories:
  - Python mini projects
tags:
  - python
  - mini projects
  - open api
  - requests
  - json
  - redirect URI
  - token
 
last_modified_at: 2019-04-13T08:06:00-05:00  
---

# 사전 준비하기



1. 접근 권한 설정하기
2. 카카오톡 메세지 API 사용 권한받기(인증 코드와 사용자 토큰 발급)
3. token 관리하기



## 1. 접근 권한 설정하기

step 1 : 앱 선택

step 2 : 동의항목 설정

step 3 : Redrect URI 설정

step 4 : Web 사이트 도메인 설정 



### 1 - step 1 : 앱 선택

![afewf](https://user-images.githubusercontent.com/83167676/127138710-5a19ff22-10c5-4479-b204-6d9ffabcaa6d.png)

접근 권한을 설정하기 위해 사전에 만들어 놓은 애플리케이션을 선택한다.

(kakao developers -> 내 애플리케이션)



### 1 - step 2 : 동의항목 설정

접근 권한을 허용하기 위해 필요한 '동의 항목' 설정

(왼쪽 배너에서 카카오 로그인 -> 동의항목)

![image](https://user-images.githubusercontent.com/83167676/127139475-b76f73d2-0a23-4d39-806f-a8ab22c451ca.png)



### 1 - step 3 : Redirect URI 설정

Redirect URI란 인증 코드 발급 요청 시 전달될 위치를 의미한다. 

redirect를 하는 이유는 예를 들어,

네이버 카페의 글들은 네이버 회원이라고 하더라도 카페 회원이 아니라면 열람할 수 없다고 하자. 만약 카페 회원이 아닌 사람이 해당 글의 URL을 알아내어 브라우저의 주소창에 입력 후 접근을 시도했을 때 글을 읽을 권한이 없다면, URL의 접근에 대한 시도에 웹 서버는 응답하면 안될 것이다. 이러한 경우에는 접근을 시도했을 때 로그인 페이지로 리다이렉트 시킬 필요가 있는 것이다.

내 컴퓨터를 서버처럼 사용하므로, Redirect URI를 https://localhost.com으로 설정한다.

![awef](https://user-images.githubusercontent.com/83167676/127140301-a6a402b9-6f2a-4d82-bc3e-4cb58b8dbe63.png)



### step4 : Web 사이트 도메인 설정

나중에 메세지를 전송해보면 알겠지만 전송할 카카오 메세지에는 사용자가 클릭할 수 있는 버튼이 있다. 메세지를 받아 그 버튼을 누르면 네이버(www.naver.com)으로 이동시키도록 해보자.

좌측 배너에 플랫폼 -> Web에서 사이트 도메인을 네이버 주소로 등록하자.

![dasfwea](https://user-images.githubusercontent.com/83167676/127140875-e9f962bd-329e-474f-a9ec-57bb67912a22.png)





## 2. 카카오톡 메세지 API 사용 권한받기(인증 코드와 사용자 토큰 발급)

메세지 API 사용 권한을 얻기 위해 **인증 코드**와 **사용자 토큰**을 발급받아야 한다.



**step 1: 인증 코드 발급받기(인증 코드 받기)**

developers.kakao.com/docs/latest/ko/kakaologin/rest-api#request-code



**step 2: 사용자 토큰 발급받기(사용자 토큰 받기)**

developers.kakao.com/docs/latest/ko/kakaologin/rest-api#request-token



인증 코드와 사용자 토큰을 발급받는 전체적인 프로세스이다.

![ㅇㄻㄷㅈㄹ](https://user-images.githubusercontent.com/83167676/127142199-25e15a18-d80a-472e-a5f3-796af7fb6714.png)

1. 애플리케이션에서 서버 쪽으로 인증 코드를 요청한다.

   (애플리케이션이 카카오에서 만든 정상적인 애플리케이션임을 증명하기 위해 앱 키를 파라미터로 전달)

2. 정상적인 정보임이 확인 되면 **카카오 로그인 동의 화면**을 통해 사용자로부터 사용자 정보 및 기능 활용 동의를 받는다. 사용자가 필수 항목에 동의하고 로그인을 요청하면 인증 코드(Authorization Code)가 발급된다. 이 코드는 애플리케이션 정보의 Redirect URI에 전달된다.

3.  전달받은 인증 코드를 기반으로 토큰을 요청한다.

4. 사용자가 동의를 해준 항목에 접근할 수 있는 **토큰(Access Token)**을 전달한다.

   (**토큰은 일정 시간 후에 만료되도록 설정**되어있다.)

5. 전달받은 토큰으로 API를 호출한다. 이때 서버에서는 토큰이 만료되었는지 아닌지의 유효성을 확인한 후,
6. 요청에 대한 응답을 애플리케이션에 전달한다.





### 2 - step 1 : 인증 코드 발급받기(인증 코드 받기)

크롬 시크릿 창을 열고 아래와 같은 URL을 요청한다.

https://kauth.kakao.com/oauth/authorize?client_id=(REST_API_KEY)&response_type=code&redirect_uri=https://localhost.com

(REST_API_KEY)에는 발급받은 Rest API 키 넘버를 입력해준다. (괄호없이)

![dawef](https://user-images.githubusercontent.com/83167676/127143725-d9ef2519-74fb-4f4b-9879-547b66e8d8b2.png)

localhost.com/?code=~~~~ 가 나왔으면 성공이다. 인증코드는 code= 뒤에 오는 빨간 박스내의 문자들이 되겠다. (''사이트에 연결할 수 없음''은 무시하자.)



### 2 - step 2 : 사용자 토큰 발급받기(사용자 토큰 받기)

![rct](https://user-images.githubusercontent.com/83167676/127144158-c1448fa7-5d86-48d5-985e-083b17159ceb.png)

developers.kakao.com/docs/latest/ko/kakaologin/rest-api#request-token 주소로 들어가 나오는 내용들을 요약해보자면,

- 인가코드(앞서 받은 인증코드)로 엑세스 토큰(access_token), 리프레시 토큰(refresh_token)을 발급받는 api이며
- 인가코드만으로는 카카오 로그인이 완료되지 않아 토큰을 발급받아야 한다.
- 요청 시 필수 파라미터 값들은 post()형식으로 요청해야한다.
- response는 JSON 객체로 전달된다.
- 객체 내에는 토큰 값, 타입, 초 단위로 된 만료기간이 담겨있다.
- access_token의 만료기간은 refresh_token에 비해 비교적 짧지만, access_token이 만료되더라도 유효한 refresh_token이 존재한다면, 다시 로그인 했을 때 refresh_token으로 access_token을 재발급 받을 수 있다.



다음은 token 발급 시 필요한 request와 response 양식이다.

![fse](https://user-images.githubusercontent.com/83167676/127145117-4696b456-a988-4d1e-9c87-af4e9f8e66e6.png)

![asece](https://user-images.githubusercontent.com/83167676/127145160-4d6cfe7d-02bb-42f0-a5f9-f0908e1a9088.png)



그럼 token을 받아와 보자.



```python
import requests

url = "https://kauth.kakao.com/oauth/token"

data = {
    "grant_type" : "authorization_code",
    "client_id" : "(REST_API 앱 키)",
    "redirect_uri" : "https://localhost.com",
    "code" : "(발급받은 인증 코드)"
}

response = requests.post(url, data=data)

#요청에 실패했다면?
if response.status_code != 200 :
    print("error! because ", response.json())
else: #성공했다면?
    tokens = response.json()
    print(tokens)
```

clinet_id 에는 ""안에 rest_api 앱 키를 넣으면 되고, code에는 크롬 시크릿 창을 통해 받은 인증 코드를 입력하면 된다.

request 양식에 맞게 post()메서드를 통해 request 해주었고 response가 성공적으로 이루어졌다면(200) tokens라는 변수에 전달받은 JSON 객체의 token 정보를 넣어주었다.

![arwef](https://user-images.githubusercontent.com/83167676/127146561-439807d4-13f2-4fe1-a2c4-f818ee15ee07.png)

access_token, refresh_token, 각 token의 expire 시간, token_type을 json 형태로 받았다.

token을 발급받았다면 크롬 시크릿 창을 통해 받은 인증 코드는 더 이상 필요가 없어진다.

이젠 token을 관리하는 방법에 대해 알아보자.



### 2 - step 3 : token 관리하기

access_token은 유효기간이 있어 만료가 되면 사용할 수 없기 때문에 refresh_token을 통해 갱신해줄 필요가 있다.

다음은 전달받은 token을 저장하고 refresh_token을 통해 갱신하는 코드이다.

```python
import json
import requests
import datetime
import os

url = "https://kauth.kakao.com/oauth/token"

data = {
    "grant_type" : "authorization_code",
    "client_id" : "(REST_API 앱 키)",
    "redirect_uri" : "https://localhost.com",
    "code" : "(발급받은 인증 코드)"
}

response = requests.post(url, data=data)

tokens = response.json()

#카카오 토큰을 저장할 파일명
KAKAO_TOKEN_FILENAME = "res/kakao_message/kakao_token.json"

#토큰을 저장하는 함수
def save_tokens(filename, tokens) :
    with open(filename, "w") as fp:
        json.dump(tokens, fp)

#토큰을 읽어오는 함수
def load_tokens(filename) :
    with open(filename) as fp:
        tokens = json.load(fp)
        
    return tokens

#refresh_token으로 access_token을 갱신하는 함수
def update_tokens(app_key, filename) :
    tokens = load_tokens(filename)
    
    url = "https://kauth.kakao.com/oauth/token"
    data = {
        "grant_type" : "refresh_token",
		"client_id" : app_key,
        "refresh_token" : tokens['refresh_token']
    }
    response = requests.post(url, data=data)
    
    #요청에 실패했다면?
    if response.status_code != 200 :
        print("error! because ", response.json())
        tokens = None
    else : #성공했다면,
        print(response.json())
        #기존 파일 백업
        now = datetime.datetime.now().strftime("%Y%m%d_%H%M%S")
        backup_filename = filename+"."+now
        os.rename(filename, backup_filename)
        #갱신된 토큰 저장
        tokens['access_token'] = response.json()['access_token']
        save_tokens(filename, tokens)
        
    return tokens

#토큰 저장
save_tokens(KAKAO_TOKEN_FILENAME, tokens)

#access_token이 유효하지 않은 경우?  --> update_tokens함수를 호출하여 재발급
#KAKAO_APP_KEY = "(REST_API APP KEY)"
#tokens = update_tokens(KAKAO_APP_KEY, KAKAO_TOKEN_FILENAME)
#save_tokens(KAKAO_TOKEN_FILENAME, tokens)
```

![awefg](https://user-images.githubusercontent.com/83167676/127166064-aec6fe4a-bd6b-4397-be1e-5b4da61b1261.png)

kakao_token.json 파일이 지정한 폴더 내에 저장된 것을 볼 수 있다.



```python
#토큰을 저장하는 함수
def save_tokens(filename, tokens) :
    with open(filename, "w") as fp:
        json.dump(tokens, fp)
```

전달받은 토큰을 저장하는 함수이다. filename, tokens를 받고 이를 json 형태로 저장한다.

- **<span style="background-color : darkslateblue ; border-radius : 8px">json.dump()</span>**

  Python 객체를 JSON문자로 변환한 결과를 파일에 바로 쓰고(write) 싶은 경우 사용하는 함수[json handling](https://yeedacoding.github.io/python/handling-json/)

tokens = response.json() 즉 요청한 데이터의 정보가 들어있는 json 형태가 tokens 변수에 들어가 있는 것이고 json.dump(tokens, fp)를 통해 filename이란 이름의 파일명으로 tokens의 정보가 저장되는 것이다.



```python
#토큰을 읽어오는 함수
def load_tokens(filename) :
    with open(filename) as fp:
        tokens = json.load(fp)
        
    return tokens
```

저장된 파일에서 token 값을 읽어오는 함수이다. filename을 입력으로 받고 json 형태의 파일을 읽은 후 token 값을 return 해주게 된다.

- **<span style="background-color : darkslateblue ; border-radius : 8px">json.load()</span>**

  JSON 파일에 저장된 데이터를 읽어서 Python 객체로 불러오고 싶은 경우에 사용하는 함수[json handling](https://yeedacoding.github.io/python/handling-json/)

즉, tokens라는 파이썬 객체에 filename이란 이름의 파일명으로 저장된 json 파일을 불러오는 것이다.

```python
#refresh_token으로 access_token을 갱신하는 함수
def update_tokens(app_key, filename) :
    tokens = load_tokens(filename)
    
    url = "https://kauth.kakao.com/oauth/token"
    data = {
        "grant_type" : "refresh_token",
		"client_id" : app_key,
        "refresh_token" : tokens['refresh_token']
    }
    response = requests.post(url, data=data)
    
    #요청에 실패했다면?
    if response.status_code != 200 :
        print("error! because ", response.json())
        tokens = None
    else : #성공했다면,
        print(response.json())
        #기존 파일 백업
        now = datetime.datetime.now().strftime("%Y%m%d_%H%M%S")
        backup_filename = filename+"."+now
        os.rename(filename, backup_filename)
        #갱신된 토큰 저장
        tokens['access_token'] = response.json()['access_token']
        save_tokens(filename, tokens)
        
    return tokens
```

access_token을 갱신하는 함수이다. tokens = load_tokens(filename)을 통해 token값을 tokens 변수에 입력하고 카카오 토큰 갱신 양식에 맞춰 request를 해준다.[토큰 갱신](https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api#refresh-token)

요청에 실패하면 오류 메세지를 출력하고, 성공했다면 tokens['access_token'] = response.json()['access_token']를 통해 기존 access_token 값을 새로 발급받은 access_token 값으로 변경 후 save_tokens()함수를 호출해 파일에 저장한다.



여기까지 카카오 메세지 api를 사용하기 위해 준비해야하는 것들이다. 다음은 메세지 api를 통해 실제로 나에게 메세지를 보내보도록 하겠다.



### 발생했던 오류



전체적인 코드를 이해하면서 json파일이 어디서 어떻게 저장되고 읽혀지며 어떤 형태를 가지고 있는지 파악하고 싶어 중간중간에 print문을 넣어서 코드를 계속 실행해봤더니 

{'error': 'invalid_grant', 'error_description': 'authorization code not found for code=v5A...(생략)... 'error_code': 'KOE320'}

오류가 계속해서 발생했다.

error_code를 구글링해보았더니 카카오 개발자 사이트에서 친절하게 설명이 나와있었다.

![23452346](https://user-images.githubusercontent.com/83167676/127172413-ca846fbe-7b5e-44b1-8ccf-89292d05a598.png)

인가코드를 받자마자 코드를 실행시켜 보았기 때문에 만료가 되진 않았을테고, token을 받기 위해 request 했을 때 파라미터로 들어가있던 "code" = "(발급받은 인증 코드)" 이 부분이 실행이 되어 동일한 인가 코드를 두 번 이상 사용하여 오류가 발생했던 것이다.

이럴 때마다 새로운 인가 코드를 사용해야 되서 크롬 시크릿 창에도 복붙과 끄고 키는 것을 수 없이 반복했던 것 같다...

이외의 오류코드에 대한 설명이 나와있다.[인가 코드, 엑세스 토큰 관련 오류](https://developers.kakao.com/docs/latest/ko/kakaologin/trouble-shooting)

