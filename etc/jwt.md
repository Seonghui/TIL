JSON Web Token(JWT)
---

JSON 기반의 open standard. 정보(claim)를 assert하는 access token을 만들어줌. 예를 들어서 서버는 "관리자로 로그인" 이라는 클레임을 가지고 있는 토큰을 생성, 클라이언트에게 이 토큰을 제공해줌. 클라이언트는 이 토큰을 사용해서 관리자로 로그인되었는지를 확인함. 


# 용도
**회원 인증용**으로 많이 사용. 유저가 로그인을 하면 서버는 유저의 정보에 기반한 토큰을 발급하여 유저에게 전달, 그리고 이후 유저가 서버에 유청을 할 때마다 JWT를 포함해서 전달. 서버는 클라이언트한테서 요청을 받을 때마다 해당 토큰이 유효하고 인증되었는지 검증을 하고 유저가 요청한 작업에 권한이 있는지 확인해서 작업을 처리

# 구조
```
aaaaaa.bbbbbb.cccccc
```
* a: 헤더
* b: 내용
* c: 서명

대략 저렇게 생김. JWT 토큰은 JWT를 담당하는 라이브러리가 자동으로 인코딩 및 해싱 작업을 해줌. 

## 헤더(Header)
두 가지의 정보(타입, 알고리즘)을 지니고 잇음.
토큰 타입은 바로 JWT, 알고리즘은 해싱 알고리즘을 지정하는 것임. 해싱 알고리즘은 토큰을 검증할 떄 사용되는 시그니쳐 부분에서 사용
```
{
 "alg" : "HS256",
 "typ" : "JWT"
}
```
HS256 indicates that this token is signed using HMAC-SHA256.


## 정보(Payload)
토큰에 담을 정보(set of claim)가 들어있음. 여기에 담는 정보의 한 조각을 클레임이라고 부르고, 클레임은 키/값의 한 쌍으로 이루어져 있음. 클레임은 크게 등록된 클레임, 공개 클레임, 비공개 클레임 세 가지로 나뉘어짐.
```
{
 "loggedInAs" : "admin",
 "iat" : 1422779638
}
```
This example has the standard Issued At Claim (iat) and a custom claim (loggedInAs).
iat 같은 스탠다드 필드는 위키피디아 참고


## 서명(Signature)
```
HMAC-SHA256(
 base64urlEncoding(header) + '.' +
 base64urlEncoding(payload),
 secret
)
```
Securely validates the token. 서명은 Base64url 인코딩을 사용해서 인코딩한 header랑 payload를 period separator(점인가..??)로 연결해서 계산된 것임. 이 문자열은 
 헤더의 인코딩값과 정보의 인코딩값을 합친 후 주어진 비밀키로 인코딩은 헤더에 적힌 알고리즘을 지나감(이 예제에서는 HMAC-SHA256).


## 예시
세 파트는 따로따로 Base64URL을 사용해서 인코딩됨. 그리고 periods를 사용해서 연결
```
const token = base64urlEncoding(header) + '.' + base64urlEncoding(payload) + '.' + base64urlEncoding(signature)
```

아래는 생성한 토근 예시. 이렇게 생성된 토큰은 HTML과 HTTP로 쉽게 전달
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJsb2dnZWRJbkFzIjoiYWRtaW4iLCJpYXQiOjE0MjI3Nzk2Mzh9.gzSraSYS8EXBxLN_oWnFSRgCzcmJmMjLiuyu5CSpyHI
```

# 사용
유저가 성공적으로 로그인을 했을 때 JWT은 무조건적으로 반환됨. 그리고 이 JWT은 로컬에도 저장되어야 함. 보통 로컬 스토리지나 세션 스토리지에 저장함. 유저가 protected route or resource에 접근하고 싶을 때마다 user agent는 JWT를 보내야함. 아무튼 JWT는 stateless authentication mechanism이고 유저 상태가 서버 메모리에 저장되지 않음. 서버의 protected routes는 Authorization header의 JWT가 유효한지를 체크하고 만약 유효할 경우 유저는 protected resources에 대한 접근을 할 수 있음. 

# 디버깅
https://jwt.io/ 사이트에서 디버깅 가능

# refs
* https://en.wikipedia.org/wiki/JSON_Web_Token
* https://velopert.com/2389
* https://blog.outsider.ne.kr/1160
* http://www.opennaru.com/opennaru-blog/jwt-json-web-token/