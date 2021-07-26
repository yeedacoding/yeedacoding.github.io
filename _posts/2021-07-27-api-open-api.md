---
title:  "[용어] API? OPEN API?"
excerpt: "api 와 open api 소개"

categories:
  - 용어
tags:
  - api
  - open api

last_modified_at: 2019-04-13T08:06:00-05:00
---

# API 란?

API란 Application Programming Interface의 줄임말로 **<span style="background-color : darkslateblue ; border-radius : 8px">어떠한 응용 프로그램에서 데이터를 주고 받기 위한 방법을 의미</span>**한다.

역시 딱딱한 정의는 이해가 잘 가지 않는다.

실생활을 예로 들면,

우리가 음식점에 갔다고 가정하자. 음식점에서 주문을 받는 직원에게 음식을 주문하고 시간이 지난 후 주방에서 나온 음식을 직원이 우리에게 가져다준다.

여기서 우리는 음식을 직접 만든 요리사에게 주문을 한 것이 아니라 우리의 주문을 받고 주방의 요리사에게 전달해준 점원에게 주문을 한 것이다.

손님 - **직원** - 요리사

API는 이러한 예시의 직원처럼 응용 프로그램(애플리케이션)에서 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다.  즉 **<span style="background-color : darkslateblue ; border-radius : 8px">데이터(주문한 음식)를 주고 받기 위한 방법(직원)</span>**인 것이다.





## 웹 API

PC를 통해 보는 웹에서는 우리가 이용할 수 있도록 서비스를 제공하기 위해서는 기본적으로 **<span style="background-color : darkslateblue ; border-radius : 8px">요청(request)와 응답(response)</span>**가 이루어져야 한다.

![웹 api](https://user-images.githubusercontent.com/83167676/126985755-8e56043d-ac1d-4346-b1fd-a2ee9a66608b.png)

클라이언트에서 서버에 필요한 서비스를 요청하면 서버에서는 응답을 하게 된다.

웹에서는 이러한 서비스의 오고감이 있기 위해서는 front-end, back-end, front-end와 back-end의 인터페이스가 필요한데 front-end는 화면 구성을 처리, back-end는 화면에 보여줄 정보를 처리해준다. 프론트엔드가 백엔드에 요청을 할 때 특정 규칙에 맞게 요청을 하게 되고 이에 대한 응답을 백엔드에서 하게 되는 데 이러한 요청과 응답을 주고 받기 위해서 정의한 API가 웹 API이다.





# Open API

Open API는 **<span style="background-color : darkslateblue ; border-radius : 8px">개발자라면 누구나 사용할 수 있도록 공개된 API를 말하며, 개발자에게 사유 응용 소프트웨어나 웹 서비스의 프로그래밍적인 권한을 제공</span>**한다. (출처 : 위키백과)

서버가 가지고 있는 데이터베이스의 정보를 누구나 열람하게 된다면 서버를 이용하는 사용자들의 개인 정보 등이 모두에게 보여질 것이다. 따라서 필요에 의해서만, API에 접근 권한이 인가된 사람에게만 서버와 데이터베이스에 접근할 수 있게 되는 데,  Open API는 누구나 쉽게 접근하여 정보를 공유하기 위해 만들어진 것이라고 할 수 있다.

각 Open API 제공처에서는 사용자가 접근하여 이용할 수 있도록 백엔드를 만들어 놓고, 그 백엔드를 이용하는 방법을 제공한다. (요청은 어디로, 어떻게 해야하는지, 응답을 받으면 어떠한 결과를 전달 받는지 등) 따라서 Open API 사용자는 제공처에서 제공하는 백엔드 주소와 사용 규칙만 알면, 백엔드의 소스들을 이용할 수 있게 된다.