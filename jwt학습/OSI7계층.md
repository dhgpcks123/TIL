# OSI 7계층

통신 OSI 7계층
: 네트워크에서 통신이 일어나는 과정을 7단계로 나눈 것

- 흐름을 한눈에 알아보기 쉬움

ㅣ 응용 계층 응용 계층 ㅣ
ㅣ 프레젠테이션 계층 프레젠테이션 계층 ㅣ 암호화, 압축
ㅣ 세션 계층 세션 계층 ㅣ 인증 체크
ㅣ 트랜스포트 계층 트랜스포트 계층 ㅣ TCP/UDP 통신 결정
ㅣ 네트워크 계층 네트워크 계층 ㅣ IP  
ㅣ 데이터링크 계층 데이터링크 계층 ㅣ LAN ex 공유기 연결 된 기기?  
ㅣ 물리 계층 물리 계층 ㅣ 물리 선
-------------------- 전송 ------------------>

## 1게층 물리 계층

전기, 기계, 기능적인 특성을 이용해서 통신 케이블로 데이터 전송

## 2계층 데이터 링크 계층

- 물리계층을 통해 송수딘 되는 정보의 오류와 흐름 관리
- 정보의 전달 수행을 돕는 역할
- 맥 주소로 통신

## 3게층 네트워크 계층

- 데이터를 목적지까지 가장 안전하고 빠르게 전달하는 기능(라우팅)
- IP

## 4게층 전송 계층

- 통신을 활성화 하기 위한 계층 포트를 열어서 응용 프로그램 전송

### TCP :

- 데이터 보내고, 잘 받았는지 확인 -> 신뢰성 있음
- 연결 지향
- 속도 느림
- 웹은 TCP 통신 필요

### UDP :

- 비 연결성, 순서화되지 않음
- 속도 빠름
- 유실 가능성
- 동영상 스트리밍

## 5게층 세션 계층

- 세션 설정, 유지, 종료, 전송 중단시 복구 등의 기능

## 6게층 프레젠테이션 계층

- MIME 인코딩이나 암호화 등의 동작

## 7게층 응용 계층

- 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행
