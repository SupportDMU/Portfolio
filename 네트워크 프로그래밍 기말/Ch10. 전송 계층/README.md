# 네트워크 프로그래밍 기말정리
## 전송 계층
## UDP 프로토콜
### > UDP 특징
- 비연결형 서비스를 제공
- 헤더와 전송 데이터에 대한 체크섬 기능을 제공
- Best Effort 전달 방식을 지원
### > UDP 헤더 구조
- TCP보다 단순해 의미와 기능을 쉽게 파악
- 프로토콜에서 수행하는 기능도 간단해 프로토콜의 오버헤드가 작은 편

![image](https://user-images.githubusercontent.com/85292541/206690840-60651efa-2ca0-40fc-878c-d44a29c45e20.png)

### > UDP의 데이터그램 전송
- **UDP는 비연결형 서비스를** 이용하여 데이터그램을 전송하며, 각 데이터그램은 전송 과정에서 독립적으로 중개됨
- 데이터그램이 목적지까지 도착할 수 있도록 최선을 다하지만, 반드시 목적지에 도착하는 것을 보장하지 않음.
- 슬라이딩 윈도우 프로토콜과 같은 흐름 제어 기능도 제공하지 않아 버퍼 오버플로에 의한 데이터 분실 오류가 발생할 수 있음
- 오류 유형
  - **데이터그램 분실**
  - **도착 순서 변경**
### > 데이터그램 분실
- [그림10-2]는 전송 과정에서 데이터그램을 분실하는 오류를 설명
![image](https://user-images.githubusercontent.com/85292541/206692416-9f5b006d-28aa-443d-9de4-49f8774a6409.png)

## RTP 프로토콜
### > RTP
- TCP와 UDP를 근간으로 인터넷 환경에서 실시간 서비스를 제공하는 가장 현실적인 방법 중 하나는 UDP에 데이터그램의 순서 번호 기능을 추가하는 것
- **이러한 프로토콜의 대표적인 예가 실시간 멀티미디어 데이터의 전송을 지원하는 RTP(Real Time Protocol)
- RTP는 유니캐스팅뿐 아니라 멀티캐스팅도 지원

### > RTP 특징
- 첫째, 불규칙하게 수신되는 데이터의 순서를 정렬하기 위해 타임스탬프 방식 사용
- 둘째, 프로토콜의 동작이 응용 프로그램의 라이브러리 형태로 구현되는 ALF 방식 사용
- RTP는 제한적인 형태로 실시간 응용 서비스를 제공
- 자원 예약이나 QoS 보장과 같은 기능은 제공하지 못하므로 실시간 동영상 서비스를 지원하기에는 부족

## 실시간 요구 사항
### > 버퍼의 역할
- [그림 10-4]는 전형적인 실시간 응용 환경에서 데이터를 전송하는 원리를 설명

![image](https://user-images.githubusercontent.com/85292541/206694891-2cf25a82-c790-40d2-88c2-8b1a9c3e8014.png)

### > 지터
- 실시간 데이터를 전송하는 환경에서 지터라는 중요한 변수를 고려해야 함
- 지터 분포는 데이터그램의 도착 시간을 측정하였을 때 각 데이터그램의 도착 시간이 일정하지 않고 불규칙적으로 도착하는 정도를 나타냄

![image](https://user-images.githubusercontent.com/85292541/206695516-901f7263-223e-4f74-8b75-c62e9f9f3e5d.png)

### > RTP의 데이터 전송
- [그림 10-6]은 RTP 동작 원리를 설명
- RTP는 하나의 완전한 프로그램 단위로 구현되지 않고, 기능별로 개별적으로 구현
- 각 응용 서비스의 종류에 따라 요구 조건이 다른 기능들이 추가되는 형식으로 완전한 RTP 모듈이 완성
- 이를 위해 RTP 헤더 부분에 첨삭이 용이하도록 설계
  - 그림은 비디오 데이터를 전송하는 데 필요한 인코딩 표준 모듈과 RTP의 관계를 설명
  
  ![image](https://user-images.githubusercontent.com/85292541/206696399-36afaaba-edc9-4001-9f9b-f8b0d284a250.png)
- **RTP는 믹서와 트랜슬레이터라는** 두 종류의 RTP 릴레이를 지원
- 릴레이 : 데이터 전송 과정에서 송수신 프로세스가 데이터를 직접 전송할 수 없는 상황이 발생했을 때, 데이터를 중개하는 기능
  - 송수신 프로세스 사이에 방화벽 설치 또는 데이터 형식의 다를 경우
- 믹서
  - 여러 송신 프로세수로부터 RTP 데이터그램 스트림을 받아 이들을 적절히 조합하여 새로운 데이터그램 스트림을 생성
  - 시간에 민감
- 트랜슬레이터
  - 입력된 각 RTP 데이터그램을 하나 이상의 출력용 RTP 데이터그램으로 만들어주는 장치로, 이 과정에서 데이터 형식이 변할 수 있음

### > RTP 헤더 구조
- 고정 헤더의 뒷부분에는 CSRC identifier 필드가 여러 개 존재할 수 있으며, 이는 믹서가 제공하는 구분자
 ![image](https://user-images.githubusercontent.com/85292541/206706007-2f88bef3-207c-4fe1-bfcf-40cbabcd1c6a.png)
  - RFC 1890에서 권고한 표준 오디오,비디오 인코딩의 일부
  
  ![image](https://user-images.githubusercontent.com/85292541/206706623-c9b19ef7-b007-4637-9979-57dc91d34dd5.png)
### > RTP 제어 프로토콜
- **RTP 제어 프로토콜을 RTP 데이터 전송 프로토콜과 구분하기 위해 RTCP**라 부름
- 데이터 전송 프로토콜은 세션 참가자 사이의 멀티캐스트 기능을 이용한 사용자 데이터 전송을 담당하지만 RTCP는 제어와 관련된 기능을 수행
- **RTCP에서 제공하는 주요 기능**
  - **QoS(Quality of Service)와 혼잡 제어** : RTCP는 세션에서의 데이터 분배 과정에서 발생하는 서비스 품질에 관한 피드백 기능을 지원
  - **identification(구분자)** : 서로 다른 세션에서 발신된 스트림 정보들을 서로 연관시키는 근거를 제공
  - **세션 크기**
    - RTCP의 기능이 올바로 동작하기 위해서는 세션 참가자들 사이에 RTCP 패킷이 주기적으로 전송되어야 함
    - 세션 참가자의 수가 증가함에 따라 RTCP 패킷 전송률은 감소할 수밖에 없음
### > RTCP 패킷의 종류와 역할
- (표) 패킷의 종류와 역할

|종류|역할|
|:--|:--|
|Sender Report(SR), Receiver Report(RR)|데이터 전송 품질을 피드백하기 위한 목적으로 사용된다.|
|Source Description(SDES)|송신 프로세스가 자신에 대한 정보를 더 많이 제공할 수 있게 한다.|
|GoodBye(BYE)|송신 프로세스가 더 이상 존재하지 않음을 의미하고, 이를 통해 수신 프로세스가 송신 프로세스를 무한정 기다리지 않도록 한다. 즉, 상대편과의 통신상 문제가 네트워크 오류가 아닌 세션에 더 이상 참가하지 않기 때문임을 알려준다.|
|Applicationdefined Packet|응용 환경에 따른 기능을 점검하기 위해 제공된다.|

## OSI TP 프로토콜
- OSI에서 정의한 Transport Protocol
- OSI TP 프로토콜이 5개의 클래스로 서비스를 분류해 지원
- (표) OSI TP 프로토콜이 제공하는 서비스

|클래스|제공하는 서비스|
|:--|:--|
|클래스 0|기본 기능|
|클래스 1|기본 오류 복구 기능|
|클래스 2|멀티플렉싱 기능|
|클래스 3|오류 복구, 멀티플렉싱 기능|
|클래스 4|오류 검출, 오류 복구, 멀티플렉싱 기능|
    - 클래스 0이 구조가 가장 단순하고, 클래스 번호가 커질수록 기능이 추가된다.

## 서비스 프리미티브
### > **TP가 상위 계층에 제공하는 전송 서비스**
  - 연결형과 비연결형
    - [표 10-4]에서 연결형 서비스를 이용하기 위한 연결 설정과 연결 해제는 T-CONNECT와 T-DSICONNECT로 정의
    - 데이터는 일반 데이터를 의미하는 T-DATA와 긴급 데이터를 의미하는 T-EXPEDITED-DATA로 정의
    - 비연결형 서비스는 연결 설정과 해제 과정이 불필요하므로 데이터 전송을 위한 T-UNITDATA 프리미티브만 존재
    
    ![image](https://user-images.githubusercontent.com/85292541/206711040-0c263da1-2a5c-4913-a19f-12cbf77852e2.png)
### > 데이터 전송
- [그림 10-8]은 서비스 프리미티브를 이용한 연결형 서비스의 동작 과정을 설명
![image](https://user-images.githubusercontent.com/85292541/206712160-e8c258bd-30a2-4f02-8821-f2946c47ea20.png)
