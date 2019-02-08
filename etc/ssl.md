SSL
---

네트워크 통신은 핸드쉐이크 -> 세션 -> 세션 종료의 과정을 거침

![SSL 통신 과정](https://i.imgur.com/YIfy1wK.png)

공개키 암호 방식은 계산이 복잡함. 따라서 효율을 위해 ssl은 공개키+대칭키 방식 사용.

* 실제 데이터: 대칭키
* 위의 대칭키를 서로 공유하기 위한 암호화 방식: 공개키

다시 말해 대칭키를 공개키로 암호화해서 서로 공유

## 1. client hello

* ssl 버전 정보 (사용 가능한 암호화 방식 후보들)
* 무작위 바이트 문자열 (클라이언트가 생성한 랜덤 데이터)

이미 ssl handshake를 했었다면 세션 재활용가능. 


## 2. server hello

* 서버 인증서 (클라이언트가 요구할 경우)
* 무작위 바이트 문자열 (서버가 생성한 랜덤 데이터)
* 암호와 통신에 사용할 알고리즘(서버가 선택한 클라이언트 암호화 방식, CipherSuite)


## 3. 인증서 확인
여기서의 pre master secret 키는 대칭키


## 4. 클라이언트가 서버에 pre master secret 전송

pre master secret 키를 인증서 안에 들어있는 공개키를 이용해 암호화하여 서버에 전송


## 5. 클라이언트 인증서 확인

서버는 클라이언트에서 받아온 pre master secret 키를 자신이 가진 비공캐 키를 가지고 복호화. 이후 pre master secret 키는 이후 대칭키 방식을 사용할 때(데이터를 주고 받을 때) 사용 


## 6. master secret 및 session key(대칭키) 생성


## 7. session key를 대칭키 방식으로 이용하여 통신

통신 메시지는 공유 비밀 키를 사용해서 암호화됨


## 8. 세션 종료. session key 폐기

https://opentutorials.org/course/228/4894