# Http

## Http란?
- Hyper Text Transfer Protocol의 줄임말로, 웹서버와 클라이언트간의 하이퍼 텍스트를 주고 받기 위한 통신규약(프로토콜)이다.

### Http 더 잘 이해하기
- HTML을 웹 브라우저와 웹 서버 사이에서 주고 받기 위해 규칙을 정의하기 위해 만든 통신 방법

- 크롬에 검사 → 네트워크 탭 → 여기서 웹 브라우저와 웹 서버가 주고 받는 데이터를 보여준다. (request, response)

request Headers라고 되어있는 부분이 웹 브라우저가 웹 서버에 요청할 때 작성한 요청서이다.

그럼 웹 서버가 request 요청서에 따라 response Headers를 보낸다.

그럼 웹 브라우저가 이를 보고 행동을 함.

### **HTTP 특징**

- HTTP 메시지는 HTTP 서버와 HTTP 클라이언트에 의해 해석이 된다.
- TCP/ IP를 이용하는 응용 프로토콜이다.(컴퓨터와 컴퓨터간에 데이터를 전송 할 수 있도록 하는 장치로 인터넷이라는 거대한 통신망을 통해 원하는 정보(데이터)를 주고 받는 기능을 이용하는 응용 프로토콜)
- HTTP는 연결 상태를 유지하지 않는 비연결성 프로토콜이다.(이러한 단점을 해결하기 위해 Cookie와 Session이 등장하였다.)
- HTTP는 연결을 유지하지 않는 프로토콜이기 때문에 요청/응답 방식으로 동작한다.

## Protocol이란?
- 프로토콜(Protocol)은 컴퓨터나 네트워크 장비가 서로 통신하기 위해 미리 정해 놓은 약속, 규약


## Refer
- Http
http://wiki.gurubee.net/pages/viewpage.action?pageId=26739929
- Protocol
https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-9%ED%8E%B8-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-%EC%9D%B4%EB%9E%80-Protocol-%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80
