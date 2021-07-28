---
title:  "[Python_mini_projects_4] 나에게 카카오톡 메세지 보내기 #2. 카카오 메세지 API를 활용하여 나에게 카카오톡 보내기"
excerpt: "카카오 메세지 Open API를 활용하여 나에게 카카오톡 보내기"

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

# 카카오톡 메세지 API

카카오 개발자 사이트에서 카카오톡 서비스의 기능에 대해 알아볼 수 있다.

![awef3](https://user-images.githubusercontent.com/83167676/127173858-5ca8f95c-7007-4c09-883e-da7e3c30ee61.png)

나는 REST API로 카카오톡 서비스를 이용할 것이기 때문에 카카오링크 기능은 활용할 수가 없을 듯하다. 또한 친구에게 메세지를 보내기 위해서는 친구가 '사업자 정보 등록'이 되어 있고, [내 애플리케이션]에서 팀 관리에 추가된 카카오 계정의 친구만 가능하기 때문에 친구에게 메세지를 보내는 것도 힘들 것 같다. (그렇다고 친구가 없는 건 아니다..)



- 메세지 종류

카카오 개발자 사이트 -> 문서 -> 메시지 [REST API] -> 좌측 배너 [메시지 템플릿] -> [종류]

![AW3FR](https://user-images.githubusercontent.com/83167676/127175786-743a84d9-d0ff-40f9-9845-c3c3c47ccc22.png)



## 템플릿(template)

템플릿이란 일정한 틀이나 형식을 의미하는데 텍스트 메세지는 텍스트 템플릿에, 리스트 메세지는 리스트 템플릿에 맞춰서 내용을 구성해주게 된다.

![asdfewqtr4](https://user-images.githubusercontent.com/83167676/127176733-2c94a5a4-1fd2-4081-ae5e-525113d08a4e.png)



## 구현하기

텍스트 메세지 템플릿, 리스트 메세지 템플릿을 이용하게 나에게 카카오톡 메세지를 전송해보겠다.

requests 할 때 필수로 넣어야할 파라미터들이다. [메세지 템플릿 필수 파라미터]([메시지 템플릿 | Kakao Developers 문서](https://developers.kakao.com/docs/latest/ko/message/message-template#list))

```python
# 앞 글의 2 - step 3 : token 관리하기 부분의 코드 밑에 이어서

#저장된 토큰 정보를 읽어 옴
tokens = load_tokens(KAKAO_TOKEN_FILENAME)

#텍스트 메세지 url
url = "https://kapi.kakao.com/v2/api/talk/memo/defalut/send"

#request parameter 설정
headers = {
    "Authorization": "Bearer " + tokens['access_token']
}

data = {
    "template_object": json.dumps({"object_type": "text", 
                                   "text": "Hello, world!", 
                                   "link": {"web_url": "www.naver.com"}
                                  })
}

# 나에게 카카오톡 메시지 보내기 요청(text)
response = requests.post(url, headers=headers, data=data)
print(response.status_code)

# 요청에 실패했다면,
if response.status_code != 200:
    print("error! because ", response.json())
else:  # 성공했다면
    print('메시지를 성공적으로 보냈습니다.')
```

카카오톡의 알림음이 들리지 않아 직접 메세지가 전송된 것을 확인해야 한다.

![ㅈㄷ1234ㄱㄹ](https://user-images.githubusercontent.com/83167676/127180370-83481d41-b0c1-4c16-87d0-2e621052cb1f.jpg)

오오..



다음은 리스트 메세지 템플릿이다.

```python
#저장된 토큰 정보를 읽어 옴
tokens = load_tokens(KAKAO_TOKEN_FILENAME)

#텍스트 메시지 url
url = "https://kapi.kakao.com/v2/api/talk/memo/default/send"

#request parameter 설정
headers = {
    "Authorization":"Bearer " + tokens['access_token']
}

template = {
    "object_type" : "list",
    "header_title" : "초밥 사진",
    "header_link" : {
        "web_url" : "www.naver.com",
        "mobile_web_url" : "www.naver.com"
    },
    "contents" : [
        {
            "title" : "1. 광어초밥",
            "description" : "광어는 맛있다",
            "image_url" : "https://search1.kakaocdn.net/argon/0x200_85_hr/8x5qcdbcQwi",
            "image_width" : 50, "image_height" : 50,
            "link" : {
                "web_url" : "www.naver.com",
                "mobile_web_url" : "www.naver.com"
            }
        },
        {
            "title" : "2. 참치초밥",
            "description" : "참치는 맛있다",
            "image_url" : "https://search2.kakaocdn.net/argon/0x200_85_hr/IjIToH1S7J1",
            "image_width" : 50, "image_height" : 50,
            "link" : {
                "web_url" : "www.naver.com",
                "mobile_web_url" : "www.naver.com"
            }
        }
    ],
    "buttons" : [
        {
            "title" : "웹으로 이동",
            "link" : {
                "web_url" : "www.naver.com",
                "mobile_web_url" : "www.naver.com"
            }
        }
    ]
}

data = {
    "template_object" : json.dumps(template) 
}
#나에게 카카오톡 메시지 보내기 요청(text)
response = requests.post(url, data=data, headers=headers)
print(response.status_code)


#요청에 실패했다면,
if response.status_code != 200 :
    print("error! because ",response.json())
else: #성공했다면
    print('메시지를 성공적으로 보냈습니다.')
```

마찬가지로 알림음이 울리지 않기 때문에 직접 카카오톡 앱을 열어 확인해보자.

![ㄹ4ㅂ23](https://user-images.githubusercontent.com/83167676/127181079-0ad4733a-4de4-4a64-939f-1baa7920c3a6.jpg)



전송된 텍스트나 리스트 템플릿 메세지를 클릭하게 되면 네이버 홈페이지로 이동하게 된다.

그 이유는 앞 글에서 web 사이트 도메인을 www.naver.com으로 지정해주었기 때문이다.
