---
title:  "[Python_mini_projects_3] 카카오 Open Api 이미지 크롤링"
excerpt: "카카오 Open Api를 이용하여 이미지 크롤링 하기"

categories:
  - Python mini projects
tags:
  - python
  - mini projects
  - open Api
  - requests
  - json

last_modified_at: 2019-04-13T08:06:00-05:00
---

# 카카오 Open API로 사진 크롤링 하기



## 1. 카카오 developers 가입 후 APP KEY 받기

Open API를 이용하기 위해서는 해당 제공처에서 개인적으로 제공해주는 APP KEY를 받아야 한다.

1. 카카오 개발자 사이트(https://dvelopers.kakao.com)에 접속하여 회원가입을 해준다.

2. 로그인 후 상단[내 애플리케이션] 버튼 클릭

3. 앱 이름과 회사 이름 작성 후 [저장]

   (앱 이름과 회사 이름은 자유롭게 작성해도 된다. 무직이라해도 슬퍼하지 말자.)

4. 내 애플리케이션에 생성된 애플리케이션 목록이 보인다. 클릭

   ![앱](https://user-images.githubusercontent.com/83167676/126989185-fdc706a2-dcbf-447e-9619-3d6da7a626d7.png)

5. 발급받은 앱 키를 확인한다. 여기서 이용할 키는 'REST API'키 이다.

   ![APPKEY](https://user-images.githubusercontent.com/83167676/126989520-8a4a87b8-e36c-4cc2-b2b7-4cb56e8770e0.png)



## 2. Python - 파일 읽고 쓰기

```python
#파일 쓰기
data = "hello"
with open("test.txt", "w") as fp:
    fp.write(data)
```

data 변수에 입력하고 싶은 내용을 적고 write(쓰기모드)로 open 해주면 작업하는 디렉터리에 test.txt 파일이 생성된 것을 확인할 수 있다.

![test](https://user-images.githubusercontent.com/83167676/126990162-92a6d072-7949-42cd-8902-f2b60758fd39.png) ![hello](https://user-images.githubusercontent.com/83167676/126990235-89ccbd83-7f3f-41eb-ad33-5b72531799bb.png)

test.txt 파일에 hello가 적혀있는 것을 볼 수 있다.



```python
# 파일 읽기
with open("test.txt", "r") as fp:
    print("========= [파일 읽기 결과] =========")
    print(fp.read())
```

쓰기모드로 쓴 test.txt 파일을 read(읽기모드)로 읽어보겠다.

![읽기](https://user-images.githubusercontent.com/83167676/126990505-143a4059-63ee-4b25-bdca-078e8093650f.png)

요청한대로 print된 것을 볼 수 있다.





## 3. 웹에 있는 이미지 파일 저장하기



https://search1.kakaocdn.net/argon/600x0_65_wr/ImZk3b2X1w8 

해당 링크에 들어가보면 

![펭수](https://user-images.githubusercontent.com/83167676/126990702-cee4fd6f-8958-4060-9258-492e9ea83663.png)

귀엽게 보이는 펭수 이미지가 보인다.

브라우저는 URL을 서버에게 요청(request)하고, 전달받은 정보(html 형태)를 해석하여 사용자에게 보여준다. 이렇게 전달 받은 이미지 정보를 저장하면 된다.



```python
import requests

#이미지가 있는 url 주소
url = "https://search1.kakaocdn.net/argon/600x0_65_wr/ImZk3b2X1w8" 

#해당 url로 서버에게 요청
img_response = requests.get(url)

#요청에 성공했다면,
if img_response.status_code == 200:
    #print(img_response.content)
    
    print("========= [이미지 저장] =========")
    with open("test.jpg", "wb") as fp:
        fp.write(img_response.content)
```

requests 모듈이 설치되어 있지 않다면 import할 수 없다.

cmd에 **pip install requests** 를 통해 requests 모듈을 설치해주자.

위 코드에서 requests.get(url)을 통해 GET 요청을 보냈고 서버에서는 그 요청을 받아 나에게 응답(response)를 준다.

그 응답 상태가 **200이라면 요청이 서버에서 잘 처리되어서 정상적인 응답을 보내줬다는 것을 의미**한다.

성공했다면 test.jpg라는 파일을 'wb'모드, 바이너리 형식을 쓰는 모드로 열고 fp 변수에 담아주고

이미지에 해당하는 img_response.content를 write함수를 호출하여 저장한다.

![vpdtn](https://user-images.githubusercontent.com/83167676/126997526-4eb002d4-b636-4c52-a18a-235afa1ac421.png)

작업한 디렉터리에 test.jpg 파일이 생겼다.



```python
print(img_response.content)
```

주석처리 되어있던 이미지에 해당하는 img_response.content를 호출해보자.

![asdf](https://user-images.githubusercontent.com/83167676/126997930-6ce462ff-292e-4b99-bc50-37623aebf7dc.png)

이미지(jpeg)를 바이너리 형태로 읽어들이면 이런 식으로 정보 값이 입력되고 jpeg binary 데이터를 파일로 저장하면 이미지가 보이게 된다.



## 4. 카카오 이미지 검색 Open API 호출하기

https://developers.kakao.com/docs/latest/ko/daum-search/dev-guide

카카오 Open API에서는 이미지 뿐만 아니라 웹문서, 동영상, 블로그, 책, 카페 등 다양한 API를 제공해준다.

이미지 검색을 할 시 Request와 Response 가이드이다.

- REST API 키를 헤더에 담아 GET으로 요청한다.

- 원하는 검색어와 함께 결과 형식 파라미터를 선택적으로 추가할 수 있다.

- 응답(Response) 바디는 meta, documents로 구성된 JSON 객체이다.

  <img width="800" alt="WEFF" src="https://user-images.githubusercontent.com/83167676/126999421-01283e5a-d7fd-4a44-ba9b-26604a2d0df9.png">

![FAE](https://user-images.githubusercontent.com/83167676/126999625-b663a4c4-4d34-4868-910d-559031d42407.png)



```python
import requests
import json

# 이미지 검색 
url = "https://dapi.kakao.com/v2/search/image"
headers = {
    "Authorization" : "KakaoAK <REST_API 앱키를 입력하세요>"
}
data = {
    "query" : "펭수"
}

# 이미지 검색 요청
response = requests.post(url, headers=headers, data=data)
# 요청에 실패했다면,
if response.status_code != 200:
    print("error! because ",  response.json())
else: # 성공했다면,
    count = 0
    for image_info in response.json()['documents']:
        print(f"[{count}th] image_url =", image_info['image_url'])
        # 저장될 이미지 파일명 설정
        count = count + 1
```

- 호스트와 이미지 검색에 해당하는 url을 url변수에 담는다

- 헤더에는 Request 양식에 맞게 {"Authorization" : "KakaoAK 발급받은 REST_API 앱키"}를 작성한다.

  *KakaoAK 한칸 띄고 <>괄호 없이 rest api키를 입력하면 된다

- 원하는 검색어를 query로 요청한다.(Request 양식에 query는 Required이기 때문에 반드시 입력해야한다.)

- response = requests.post(url, headers=headers, data=data)

  get(), post() 둘 다 API 통신을 할 때 호출하는 함수로 둘의 차이는 함수 내 파라미터 명이 다르다는 것**(get 은 param / post 는 data)**이다. 자세한 내용은 [Requests 모듈]을 참고하자.

  카카오 이미지 검색 Open API의 Request 양식에 헤더에 REST API 앱 키를 담아 요청한다고 하였으니 headers 변수를 headers 파라미터로 세팅해 호출을 해주어야 한다.

- response.json()['documents']

  ```python
  print(response.json())
  ```

  위 코드를 호출하게 되면

  ```python
  {'documents': [{'collection': 'blog', 'datetime': '2020-03-09T19:26:00.000+09:00', 'display_sitename': '네이버블로그', 'doc_url': 'http://blog.naver.com/2012kiss/221845372207', 
  'height': 773, 'image_url': 'http://postfiles1.naver.net/MjAyMDAzMDlfMTM4/MDAxNTgzNzQ5NTk0Njkw.gvrYNHHvHfwrRUVu2xSCqE2kp_OLzg1Y2EFoafU3lEMg.spMelefH5nVyAsuH1SLbLarM4L1KHzR8oHHtdS0v1wUg.PNG.2012kiss/%ED%8E%AD%EC%88%98.png?type=w773', 'thumbnail_url': 'https://search4.kakaocdn.net/argon/130x130_85_c/1uTVnFKcose', 'width': 773}, {'collection': 'etc', 'datetime': '2020-04-11T10:36:40.000+09:00', 'display_sitename': '', 'doc_url': 'http://isplus.live.joins.com/news/article/article.asp?total_id=23749688',
  
  ... #너무 길어서 생략
  
  'http://postfiles15.naver.net/MjAyMDAxMThfMTIz/MDAxNTc5MzUwMzk3MTI0.9duHjG3eFwjdw0AK13yqqpyMhH18fHkzTtyNlkEiv0sg.I8DPf2Pe-Dd_k6olTYwXmgtFTe09-OCcfvsS62vmhyMg.JPEG.juhye0401/%ED%8E%AD%EC%88%981.jpg?type=w773', 'thumbnail_url': 'https://search2.kakaocdn.net/argon/130x130_85_c/FfJhkHTDp8d', 'width': 
  600}], 'meta': {'is_end': False, 'pageable_count': 3896, 'total_count': 818140}}
  ```

  응답(Response) 바디는 meta, documents로 구성된 JSON 객체이다.

Response 양식에 쓰여진 것처럼 json 파일은 key 값으로 documents와 meta를 갖고 있는 큰 하나의 딕셔너리이고, documents는 리스트를 value로 가지고 그 리스트 안에는 또 여러 개의 딕셔너리가 저장되어있다. 리스트 안의 딕셔너리에는 위의 표에 나와있는 것처럼 collection, thumbnail_url, image_url, width, height, display_sitename 등의 key값을 가지고 있다.

딕셔너리의 value 값에 접근하는 방식을 통해 response.json()['documents'], 즉 documents라는 key에 접근하여 그 value를 뽑아내어 for문을 돌게 된다.

-  image_info['image_url']

  for문을 돌면서  image_info['image_url']가 출력되는데 이 또한 딕셔너리 안의 해당하는 key값의 value값을 가져오는 것으로 우리는 documents라는 key값의 value 중 image_url만을 가져오게 된다.



## 5. 이미지 파일 저장하기

```python
# 이미지가 있는 image_url을 통해 file_name 파일로 저장하는 함수
def save_image(image_url, file_name):
    img_response = requests.get(image_url)
    # 요청에 성공했다면,
    if img_response.status_code == 200:
        # 파일 저장
        with open(file_name, "wb") as fp:
            fp.write(img_response.content)
```

save_image 함수는 image_url, file_name 두 가지 파라미터를 가지는데 

image_url은 이미지가 담긴 url 주소, file_name은 이미지를 저장할 파일명이다.

get()함수를 사용하여 image_url을 요청하고, 요청에 성공했다면(200) 바이너리 쓰기 모드로 열어 파일로서 저장하게 된다.



## 6. 전체 코드

```python
import requests
import json

# 이미지가 있는 image_url을 통해 file_name 파일로 저장하는 함수
def save_image(image_url, file_name):
    img_response = requests.get(image_url)
    # 요청에 성공했다면,
    if img_response.status_code == 200:
        # 파일 저장
        with open(file_name, "wb") as fp:
            fp.write(img_response.content)

# 이미지 검색 
url = "https://dapi.kakao.com/v2/search/image"
headers = {
    "Authorization" : "KakaoAK <REST_API 앱키를 입력하세요>"
}
data = {
    "query" : "펭수"
}

# 이미지 검색 요청
response = requests.post(url, headers=headers, data=data)
# 요청에 실패했다면,
if response.status_code != 200:
    print("error! because ",  response.json())
else: # 성공했다면,
    count = 0
    for image_info in response.json()['documents']:
        print(f"[{count}th] image_url =", image_info['image_url'])
        # 저장될 이미지 파일명 설정
        count = count + 1
        file_name = "test_%d.jpg" %(count)
        # 이미지 저장
        save_image(image_info['image_url'], file_name)
```

```python
file_name = "test_%d.jpg" %(count)
# 이미지 저장
save_image(image_info['image_url'], file_name)
```

위 코드를 통해 save_image 함수의 첫번째 파라미터인 image_url에는 image_info['image_url'] 인자가, 두 번째 파라미터인 file_name에는 "test_%d.jpg" %(count)이 들어가게 된다.



## 7. 결과

![gaerg](https://user-images.githubusercontent.com/83167676/127007571-51def4e4-438c-43d9-82c9-600b65efbff4.png)

총 80장의 펭수 사진이 저장되었다.(1번부터 11번은 캡쳐가 잘렸다ㅠㅠ)

80장인 이유는 request를 할 때 필수인 query만 요청했고 size는 요청해주지 않았기 때문이다.  size를 요청해주지 않는다면 디폴트 값인 80이 자동으로 요청되기 때문에 필요한만큼만 받아오고 싶다면 size를 지정해주도록 하자.

