![모던 웹 아키텍쳐 개요](https://images.ctfassets.net/rpmifyuylbfw/6Qv56PUiQwmgYEokCWKGgW/d7d470aef20abadf6dd29c5de023e529/main.png?w=665)

# 웹 아키텍쳐
아래는 유저가 이미지를 검색했을 때의 예시

## 1. DNS(Domain Name Server)
도메인 이름과 IP주소 조회 제공 (key/value 형태로 제공함). 마치 전화번호부와 같음. (피카츄/010-1234-1234)

## 2. 로드 밸런서
> 브라우저의 요청은 로드 밸런서에 도착하고, 서버가 여러 개 있다면 여러 개의 서버 중 하나를 랜덤해서 요청

수평적 확장(더 많은 장치를 새로 추가하는 것. 반대로 수직적 확장은 이미 사용하고 있는 장치의 성능을 높이는 것음)이 가능하게 함. 들어오는 요청을 복제/미러링된 수많은 애플리케이션 서버 중 하나로 연결하고 서버의 응답을 다시 클라이언트로 보냄. 로드 밸런서는 이 서버에 과부하가 걸리지 않도록 들어오는 요청을 적절하게 분배해줌.

## 3. 웹 서버
> 웹 서버는 캐싱 서비스에서 필요한 이미지 정보를 가져옴

로직을 실행하고 그 결과를 HTML에 담아 브라우저로 보냄. 서버 구현은 특정 언어(javascript나 python, java)를 선택하고 그에 맞는 웹 MVC 프레임워크(express for node.js, django for python, spring boot for java)를 선택해야 수월하게 할 수 있음.

## 4. 데이터베이스
> 캐싱 서비스에서 필요한 이미지 정보를 가져옴. 정보는 데이터베이스에 요청

## 5. 캐싱 서비스
> 이미지 정보가 있음

단순한 키/값 데이터 저장소를 제공. 쿼리 결과, 외부 서비스 호출 결과 등등을 캐시에 저장. 예를 들면 친구 목록이나 검색 결과 등등.

## 6. 잡 큐 & 잡 서버
> 이미지의 컬러 프로필은 아직 만들어지지 않은 상태기 때문에 컬러 프로필 잡을 잡 큐에 보냄. 잡 서버는 큐에 추가된 것들을 비동기적으로 처리한 후 데이터터베이스에 결과 업데이트.

응답과는 직접적인 관련이 없는 작업을 백그라운드에서 비동기적으로 실행할 때 사용하는 아키텍처. 구글이 검색 결과를 얻기 위해 인터넷에서 데이터를 크롤링하고 인덱싱하는 것도 이 아키텍처를 사용한 것임. 잡 큐와 잡 서버 두 개의 컴포넌트로 구성이 되어있음. 잡큐는 비동기적으로 실행될 잡목록을 저장, 그리고 저장된 잡큐를 잡서버에서 처리. 잡큐는 FIFO임.

## 7. 전체 텍스트 검색 서비스
> 사진의 제목을 전달해서 비슷한 사진을 찾으려고 함.

텍스트 입력(쿼리)을 하면 가장 관련있는 결과를 보여주는 기능을 말함.

## 8. 서비스
> 사용자가 웹사이트에 로그인했는지 체크해, 만약 로그인했다면 사용자의 계정 정보를 계정 서비스에서 가져옴

앱이 특정 규모에 도달하면 별도의 애플리케이션으로 분리해서 운영하기 위한 서비스가 생김. 예를 들어 계정 HTML->PDF 변환, 결제 서비스 등등이 있음.

## 9. 데이터 (Firehose, 데이터 복사, 데이터 저장소)
> Firehose에 페이지 뷰 이벤트를 발생시킴. 이걸 클라우드 저장소에 기록하고, 이 이벤트 정보는 데이터 저장소에서 사용

데이터를 제어, 저장, 분석하기 위해 사용하는 데이터 파이프라인. 가공되지 않은 원시 데이터는 Firehose로 전달, 원시 데이터와 최종 데이터는 모두 클라우드 저장소에 **저장**됨. 변형/추가된 데이터는 **분석**을 위해 데이터 저장소에서 로드됨.

## 10. 클라우드 저장소
> 위에서 발생한 이벤트를 클라우드 저장소에 기록

## 11. CDN(Content Delivery Network)
> 서버는 HTML로 화면을 렌더링한 후 로드 밸런서를 통해 사용자의 브라우저로 보냄. CDN에 연결되어있으면 CDN 접속해서 가져오고, 사용자가 볼 수 있는 콘텐츠를 렌더링

정적인 데이터를 1개의 원본 서버에서 사용하지 않고 분산시켜서 사용. 그래서 가장 가까운 서버에서 데이터를 로드할 수 있게 됨.

# refs
* https://blog.rhostem.com/posts/2018-07-22-web-architecture-101?fbclid=IwAR2chRekA4YHcae-Ir8Ytoy1XysTlGYRze1DNUvA8UQnC7twq_i15Yvj7oM