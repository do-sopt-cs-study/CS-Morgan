## TCP 흐름 제어

TCP로 연결된 두 시스템 사이의 데이터 **처리 속도**가 다를 수 있다. 수신 측의 버퍼 사이즈보다 더 많은 양을 송신 측에서 보내올 때 **오버플로우** 문제가 발생한다.

송신 측의 Send Buffer와 수신 측의 Recieve Buffer에는 TCP로 통신하는 데이터를 각각 저장한다.

수신 측의 **Receive Buffer 가 넘쳐나지 않도록 하는 것**이 **흐름 제어(Flow Control)**의 \*\*\*\*핵심이다.

### Stop and Wait

![image](https://github.com/do-sopt-cs-study/CS-Morgan/assets/51692363/51c11046-31e3-408f-b3e5-5cde8268f3c4)

송신 측에서 데이터를 보내고 수신 측에서 보낸 ACK를 받을 때까지 대기한다.

### Sliding window

![image](https://github.com/do-sopt-cs-study/CS-Morgan/assets/51692363/de6403d7-76d5-4e39-9aa7-3211c3fda9be)

**수신 측**에서 처리할 수 있는 **데이터의 양(윈도우 크기)**을 **ACK 패킷**에 실어 **송신 측**에 전달한다. 수신 측은 네트워크 상태 및 처리 능력에 따라 윈도우 크기를 증가시키거나 감소시킨다.

- **Outstanding Segment**
  **: 전송 중인 데이터 세그먼트, 아직 수신측으로부터 확인 응답을 받지 못한 세그먼트.**
  - Outstanding Segment는 모두 오류 제어 대상이다
  - **만약 ACK(응답)를 받는다면 더 이상 Outstanding Segment가 아니다!!!**
- **Window Size**
  : Outstanding Segment의 최대 개수
  - Stop & Wait는 슬라이딩 윈도우의 특수 케이스 (Window Size가 1인 경우)
- 각각의 세그먼트에 대해서 `ARQ`(Ack, Timeout, 재전송)를 진행
- TCP/IP를 사용하는 모든 호스트들은 송신하기 위한 것과 수신하기 위한 2개의 Window를 가지고 있음.
- 호스트들은 실제로 데이터를 보내기 전에 수신 호스트의 Receive Window Size(수신측이 정해놓은 세그먼트의 최대 개수)에 자신의 Send Window Size를 맞추게 된다. (By 3 way Handshaking)

## TCP 혼잡 제어

네트워크 내에 패킷의 수가 과도하게 증가하는 현상을 혼잡이라고 한다.

데이터의 양이 수신 측에서 처리할 수 있는 양을 초과하게 되면 송신 측에서는 수신 측에서 처리하지 못한 데이터를 손실 데이터로 간주하고 계속 재전송하게 되므로 네트워크가 더욱 혼잡하게 되기에 이런 혼잡 상태를 제어하기 위해 송신측에서 보내는 데이터의 전송 속도를 강제로 줄이는 작업을 **혼잡제어**라고 한다.

### 혼잡 제어 방법

1.  **AIMD** (Additive Increase / Multiplicative Decrease)

    ![image](https://github.com/do-sopt-cs-study/CS-Morgan/assets/51692363/f2e099c7-a01d-4916-84c9-79af58652151)

    - **Additive Increase :** 처음에 패킷을 하나씩 보내고 이것이 문제 없이 도착하면 **Window Size를 1씩 증가** 시켜가며 전송하는 방법
    - **Multiplicative Decrease :** 패킷 전송에 실패하거나 일정 시간을 넘으면 패킷을 보내는 속도를 **절반으로 줄인다.**

    - AIMD의 문제점
      - Window Size를 1씩밖에 증가시키지 않아서 빠른 속도로 통신하기까지 시간이 오래 걸림.
      - 네트워크가 혼잡해지는 상황을 미리 감지하지 못함.
    - 즉, **네트워크가 혼잡해지고 나서야 대역폭을 줄이는 방식**이다.

1.  **TCP Slow Start** (느린 시작)

    ![image](https://github.com/do-sopt-cs-study/CS-Morgan/assets/51692363/1fd8f4d3-bcea-4aa8-8a8e-d82e0f001fc0)

    - AIMD 방식이 처음에 전송 속도를 올리는데 시간이 오래 걸리는 단점이 존재
    - Slow Start 방식은 AIMD와 마찬가지로 Window Size가 1인 상태에서 시작하고, 패킷이 문제 없이 도착하면 각각의 **ACK 패킷마다** Window Size를 1씩 늘려준다. 즉, 한 주기가 지나면 Window Size가 2배가 된다.
    - 전송속도는 AIMD에 반해 **지수 함수 꼴로 증가**한다.
      - 대신에 혼잡 현상이 발생(`3 duplicate ACKS` or timeout)하면 Window Size를 1로 떨어뜨린다.
    - Slow Start에는 Threshold(임계점)이 있는데, Threshold까지만 2배로 증가시키다가 Threshold를 넘으면 1씩 증가시키는 AIMD 방식을 사용한다.

1.  **Fast Retransmit** (빠른 재전송)

    - 빠른 재전송은 TCP의 혼잡 제어에 추가된 정책.
    - 수신 측에서는 먼저 도착해야 할 패킷이 도착하지 않고 다음 패킷이 도착한 경우에도 ACK를 보냄.
    - 단, ACK를 보낼 때는 순서대로 잘 도착한 마지막 패킷의 다음 패킷의 순번을 ACK 패킷에 실어서 보내게 되므로, 중간에 하나가 손실 되면 송신 측에서는 순번이 중복된 ACK 패킷을 받게 된다. 이것을 감지하는 순간 문제가 되는 순번의 패킷을 재전송 해줄 수 있다.
    - 중복된 순번의 패킷을 3개 받으면 재전송을 하게 된다. **약간 혼잡한 상황**이 일어난 것이므로 혼잡을 감지하고 Window Size를 줄이게 된다.

          이렇게 중복된 순번의 패킷을 3개받는 상황을 `3 Duplicative ACKs`라고 한다.

1.  **Fast Recovery** (빠른 회복)
    - 혼잡한 상태가 되면 Window Size를 1로 줄이지 않고 반으로 줄이고 선형 증가시키는 방법. 이 정책까지 적용하면 혼잡 상황을 한번 겪은 후 AIMD 방식으로 동작하게 된다.

### 혼잡 제어 정책

1. **TCP Tahoe**

   ![image](https://github.com/do-sopt-cs-study/CS-Morgan/assets/51692363/c54d9bac-f73b-40d8-a839-4acb0d7de00e)

   - 처음에는 Slow Start 방식을 사용하다가 Threshold에 도달하면 **AIMD**를 사용하는 방식.
   - 3 duplicate ACKs를 만나거나 Timeout을 만나면 Threshold를 절반으로 줄이고 Window Size를 1로 줄이는 방식.
   - 단점 : 혼잡한 상황을 만나면 Window Size를 1로 떨어뜨려버려서 속도가 느리다.

1. **TCP Reno**

   ![image](https://github.com/do-sopt-cs-study/CS-Morgan/assets/51692363/826f07ee-2580-4a37-bcce-3dae9bdc64e5)

- TCP Tahoe와 비슷하지만 3 duplicate ACKs와 Timeout을 구별해서 처리한다는 점이 차이점
- 나머지는 다 TCP Tahoe와 똑같이 동작.
- 3 duplicate ACKs를 만났을 때 : **Window Size를 절반**으로 줄이고 **Threshold를 Window Size값**으로 설정한다.
  - Window Size를 줄이는 것 : 혼잡 상황에서 네트워크에 보낼 수 있는 데이터의 양을 줄이는 것
  - Threshold를 Window Size 값으로 설정하는 것 : 혼잡 상황에서 보내는 데이터 양이 절반으로 줄어들었을 때, 다시 증가시키기 위한 기준값을 설정하는 것
- Timeout을 만났을 때 : Window Size를 1로 줄이지만 Threshold값은 변하지 않는다.
  - Window Size를 1로 줄이는 것 : 혼잡 상황에서 네트워크에 보내는 데이터의 양을 매우 작게 제한하는 것
  - 변하지 않는 이유 : Timeout이 패킷 손실을 나타내는 매우 강력한 신호이기 때문에 Window Size를 최소화하는 것으로 충분하고
    - Threshold 값은 이 값 이상의 전송량을 가질 때 혼잡 상태로 판단하기에 Timeout이 발생하면 이미 혼잡한 상태라고 간주되므로 Threshold 값을 변경 시킬 필요가 없는 것.
