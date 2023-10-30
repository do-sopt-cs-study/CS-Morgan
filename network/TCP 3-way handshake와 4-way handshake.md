## TCP

TCP는 OSI 7계층 중 4계층인 전송 계층의 프로토콜로 **신뢰할 수 있는 데이터 전송 서비스**를 제공한다. TCP는 **연결 기반** 프로토콜이기 때문에 데이터를 송신하기 전, 시스템 간의 연결을 설정한다. 연결을 설정한 후, 데이터를 전송하고 연결을 해제한다. 이러한 전송 방식에 사용되는 것이 **3-way handshake**와 **4-way handshake**이다.

## TCP 연결과정

1. **클라이언트가 서버에 연결 요청 (3-way handshake)**
2. **데이터 전송**
3. **연결 종료(4-way handshake)**

## 들어가기 전

- **포트(PORT) 상태**
  - CLOSED: 포트가 닫힌 상태
  - LISTEN: 포트가 열린 상태로 연결 요청 대기 중
  - SYN_RCV: SYNC 요청을 받고 상대방의 응답을 기다리는 중
  - ESTABLISHED: 포트 연결 상태
- **플래그 정보**
  - TCP Header에는 CONTROL BIT(플래그 비트, 6bit)가 존재하며, 각각의 bit는 "URG-ACK-PSH-RST-SYN-FIN"의 의미를 가진다.
    - 즉, 해당 위치의 bit가 1이면 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타낸다.
  - SYN(Synchronize Sequence Number) / 000010
    - 특수 패킷의 일종. synchronize의 약자로
    - 연결 설정. Sequence Number를 랜덤으로 설정하여 세션을 연결하는 데 사용하며, 초기에 Sequence Number를 전송한다.
  - ACK(Acknowledgement) / 010000
    - 응답 확인. 패킷을 받았다는 것을 의미한다.
    - Acknowledgement Number 필드가 유효한지를 나타낸다.
    - 양단 프로세스가 쉬지 않고 데이터를 전송한다고 가정하면 최초 연결 설정 과정에서 전송되는 첫 번째 세그먼트를 제외한 모든 세그먼트의 ACK 비트는 1로 지정된다고 생각할 수 있다.
  - FIN(Finish) / 000001
    - 연결 해제. 세션 연결을 종료시킬 때 사용되며, 더 이상 전송할 데이터가 없음을 의미한다.

## **클라이언트가 서버에 연결 요청 (3-way handshake)**

![image](https://github.com/do-sopt-cs-study/CS-Morgan/assets/51692363/3f5ad663-dcaf-431e-9a1a-53113bb3134f)

![image](https://github.com/do-sopt-cs-study/CS-Morgan/assets/51692363/19a14dd5-52a2-42f1-b4b6-f47f4d97f678)

1. Client가 Server에게 연결 요청(SYN)을 보냄
2. Server가 요청을 수락하고 응답(SYN-ACK)을 보냄
   1. **SYN-ACK:** SYN 패킷을 받은 서버가 클라이언트에게 요청을 수락하고 포트 상태가 SYN_RCV로 변경. 요청을 받았다는 ACK를 보냄. 또한, 클라이언트의 Port를 열어달라는 SYN 패킷을 보냄. 이 때, SYN의 sequence number는 Server에서 결정
3. Client가 요청을 수락하고 응답(ACK)을 보냄

## **연결 종료(4-way handshake)**

![image](https://github.com/do-sopt-cs-study/CS-Morgan/assets/51692363/b991753b-c4f8-407e-9039-ed67773f9a14)

1. Client가 Server에게 연결 종료 요청(**FIN)**
2. Server가 요청을 받고 ACK를 전송 → Server의 통신이 종료될 때까지 Wait
3. Server가 Client에게 연결 종료 요청(**FIN**)
4. Client가 요청을 받고 Server에게 ACK 전송 → Client 연결 종료
