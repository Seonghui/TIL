---
layout: default
title: Keep-Alive와 파이프라이닝
parent: http
nav_order: 2
---

---
description: 둘 다 범용적으로 모든 HTTP 통신을 고속화하는 기능
---

# Keep-Alive와 파이프라이닝

## 1. Keep-Alive

 

![Keep-Alive](https://growthpixel.com/wp-content/uploads/2017/12/Keep-Alive-Sessions.png)

HTTP 의 아래층인 TCP/IP 통신을 효율화하는 구조이다. Keep-Alive를 사용하지 않으면 하나의 요청마다 통신을 닫아야 하지만, Keep-Alive를 사용하면 연속된 요청에는 접속을 다시 이용한다. 이로써 TCP/IP는 접속까지의 대기 시간이 줄어들고, 통신 처리량이 많아지므로 속도가 올라간 것처럼 느껴진다.

* HTTP/1.0에서는 요청 헤더에 `Connection: Keep-Alive`를 추가하면 이용할 수 있다.
* HTTP/1.1부터는 이 동작이 기본으로 되어있다.
* Keep-Alive를 이용하면 핸드셰이크 횟수를 줄일 수 있다.
* 여러 번 반복되는 핸드셰이크를 줄임으로써 응답시간을 개선할 수 있다.
* Keep-Alive를 이용한 통신은 클라이언트나 서버 중 한 쪽이 다음 헤더를 부여해 접속을 끊거나 타임아웃될 때까지 연결이 유지된다. `Connection: Close`
* Keep-Alive 지속 시간은 클라이언트와 서버 모두 가지고 있다. 한쪽이 TCP/IP 연결을 끊는 순간에 통신은 완료되므로, 어느 쪽이든 짧은 쪽이 사용된다.

## 2. 파이프라이닝

 

![pipelining](https://t1.daumcdn.net/cfile/tistory/223C9C335479A5FE1A)

최초의 요청이 완료되기 전에 다음 요청을 보내는 기술이다. 다음 요청까,지의 대기 시간을 없앰으로써, 네트워크 가동율을 높이고 성능을 향상시킨다. Keep-Alive 이용을 전제로 하며, 서버는 요청이 들어온 순서대로 응답을 반환한다.

그대로 동작한다면 왕복 시간이 걸리는 모바일 통신에서 큰 효과가 나지만, 여러 이유로 동작이 제대로 되지 않는 경우가 많다. 그리고 실제로 써봤지만 성능이 거의 좋아지지 않는 경우도 있다. 요청받은 순서대로 응답해야만 하므로, 응답 생성에 시간이 걸리거나 크기가 큰 파일을 반환하는 처리가 있으면 다른 응답에 영향을 준다.

