# header

### Header

## 클라이언트가 서버에 보내는 헤더 \(Request Headers\)

```text
:authority: www.naver.com
:method: GET
:path: /
:scheme: https
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
accept-encoding: gzip, deflate, br
accept-language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
cache-control: no-cache
cookie: MM_NEW=1; NFS=2; NNB=MUZLMKVF73SFY; ASID=01dd257c0000016b4aa378e40000004a; nsr_acl=1; nx_ssl=2; _ga=GA1.2.1427696816.1561344718; BMR=s=1562293033974&r=https%3A%2F%2Fm.blog.naver.com%2FPostView.nhn%3FblogId%3Dksp4797%26logNo%3D221412315086%26proxyReferer%3Dhttps%253A%252F%252Fwww.google.com%252F&r2=https%3A%2F%2Fwww.google.com%2F; nid_inf=1579121422; NID_JKL=2qegZ4TEJ/kNwHM5sVepEGD4WyZDbpizq6AEcthMGq8=; page_uid=UeIITdplyZVssaHRV9Cssssssto-449198; PM_CK_loc=d63496b46495e428993c865245d7bd9ece0fcd67817c8c656a43a1469c1c185f
pragma: no-cache
upgrade-insecure-requests: 1
user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36
```

### User-agent

* 클라이언트가 자신의 애플리케이션 이름을 넣는 곳

### Referer

* 서버에서 참고하는 추가 정보
* 클라이언트가 요청을 보낼 떄 보고있던 페이지의 URL을 보냄
* 페이지의 참조원을 서버가 참조하는데 이용

### Anthorization

* 특별한 클라이언트에만 통신을 허가할 때 인증 정보를 서버에게 전달

## 서버에서 클라이언트로 보낼때 부여하는 헤더

```text
cache-control: no-cache, no-store, must-revalidate
content-encoding: gzip
content-type: text/html; charset=UTF-8
date: Mon, 08 Jul 2019 01:37:29 GMT
p3p: CP="CAO DSP CURa ADMa TAIa PSAa OUR LAW STP PHY ONL UNI PUR FIN COM NAV INT DEM STA PRE"
pragma: no-cache
referrer-policy: unsafe-url
server: NWS
status: 200
strict-transport-security: max-age=63072000; includeSubdomains
x-frame-options: DENY
x-xss-protection: 1; mode=block
```

### Content-Type

* 파일 종류를 지정
* MIME 타입이라는 식별자를 기술 \(MIME 타입은 전자메일을 위해 만들어진 식별자\)

#### 폼 전송

**단순한 폼 전송**

> x-www-form-urlencoded

```text
<form action="POST">
  <input name="data1">
  <input name="data2">
</form>
```

이렇게 전송시 바디는 아래와 같은 형식이 된다. 키와 값이 '-'로 연결되고 각 항목이 &로 연결된 문자열이다. form에 enctype을 아무것도 설정하지 않으면 기본적으로 `x-www-form-urlencoded`이 된다.

```text
data1=hello&data2=world
```

이 방식은 지정된 문자열을 그대로 연결하는데, 구분 문자인 &와 =이 있어도 그대로 연결해버려서 읽는 쪽에서 원래 데이터 셋으로 복원할 수 없다. 예를 들어 `Head First PHP & MySQL`은 중간에 &가 있기 때문에 어디서 구분해야할지 어려워진다.

그래서 변환 포맷\(RFC1866에서 책정\)에 따라 변환을 실시하는데, 이 포맷에서는 알파벳, 수치, 별표, 하이픈, 마침표, 언더스코어 여섯 종류 문자 외에는 변환이 필요하다. 공백은 +로 변경된다.

```text
data1=Head+First+PHP+%26+MySQL
```

&는 %26로 변환되었다.

파일을 전송할 수는 없다. 만약 이 방식으로 파일을 전송할 경우, 파일 전송에 필요한 정보를 모두 보낼 수 없어 파일 이름만 전송해버린다.

**폼을 이용한 파일 전송**

```text
<form action="POST" enctype="multipart/form-data">
</form>
```

* HTML의 폼에서는 옵션으로 멀티파트 폼 형식이라는 인코딩 타입 선택 가능.
* 파일 전송할 수 있음.
* 한 번의 요청으로 복수의 파일을 전송할 수 있으므로 받는 쪽에서 파일을 나눠야 함
* 경계 문자열\(-----\)이 존재. 

### Content-Length

* 바디 크기
* 만약 다음 헤더에서 소개하는 압축이 이루어지는 경우 압축 후의 크기가 들어감

### Content-Encoding

* 압축이 이루어질 경우 압축 형식을 설명

### Date

문서 날짜

### etc

* X-로 시작되는 헤더는 각 애플리케이션이 자유롭게 사용해도 좋음

## http 메서드

* GET: 서버와 헤더에 콘텐츠 요청
* HEAD: 서버에 헤더만 요청
* POST: 새로운 문서 투고
* PUT: 이미 존재하는 URL의 문서를 갱신
* DELETE: 지정된 URL의 문서를 삭제. 삭제에 성공하면 삭제된 URL은 무효가 됨

## Status 코드

* 100번대: 처리가 계속됨을 나타냄
* 200번대: 성공했을때의 응답. 예를 들어 200은 정상 종료를 나타냄
* 300번대: 서버에서 클라이언트로의 명령. 오류가 아니라 정상 처리의 범주이다. 리디렉트나 캐시 이용을 지시
* 400번대: 클라이언트가 보낸 요청에 오류가 있음
* 500번대: 서버 내부에서 오류 발생

## ref

* 리얼월드 http

