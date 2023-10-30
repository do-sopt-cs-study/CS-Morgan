# TLS

암호화 프로토콜.

- 전송 중인 데이터의 프라이버시 확보
- 노드 ID 검증
- 무결성 점검을 통해 데이터 분실, 변조 예방.

서버와 클라이언트 간에 TLS연결을 설정하려면 통신 노드가 핸드셰이크를 해서 공유 비밀 키를 구성해야 해야함.

TLS는 인증기관(CA, Certificate Authorities)를 활용.

CA는 디지털 인증서를 서비스 운영업체에 발급.

왜 CA가 필요하지? → 이 도메인이 진짠지 아닌지 확인해주는거다!

- 서버 ↔ 클라이언트 가운데에서 해커가 있을 때
- 클라이언트의 요청을 중간에서 가로채서 서버인척 행세할 수 있음
- 이런 경우에 CA로부터 받은 인증서가 없으면 안되도록

**[ 인증서의 내용 ]**

- 서버 이름
- 소유자 ID
- 공개키 사본
- CA 암호화 서명

**[ Handshake 과정 ]**

![image](https://github.com/do-sopt-cs-study/CS-Morgan/assets/51692363/55e9ac6b-272f-4c0e-8286-a311bb1b2828)

1. ClientHello

   클라이언트가 서버에 연결을 시도하며 전송하는 패킷. 사용가능한 Cipher Suite, Session ID, SSL 프로토콜 버전, Random Byte 전송.

2. ServerHello

   클라가 보낸 Cipher Suite 중 하나를 선택한 다음 클라이언트에게 발송. SSL 프로토콜 버전도 같이 발송

3. Certificate

   서버가 자신의 SSL인증서를 클라에게 전달. 인증서 내부에는 Server가 발행한 공개키 첨부. 서버 공개키가 인증서 내부에 없는 경우, Server Key Exchange과정을 수행하여 키 전달.

4. Client Key Exchange

   클라는 데이터 암호화에 사용할 대칭키를 생성한 후, SSL 인증서 내부에 서버의 공개 키를 이용해 암호화하여 서버에게 전달. 여기서 암호화하여 전달한 대칭키가 앞으로의 데이터를 실제로 암호화할 대칭키. 이 대칭키를 이용해 실제 교환하고자 하는 데이터 암호화.

5. Change Cipher Sepc

   클라이언트와 서버가 서로에게 통신할 준비가 되었음을 알리는 패킷. 그후 **Finished 패킷**을 보내어 SSL HandShake 종료.
