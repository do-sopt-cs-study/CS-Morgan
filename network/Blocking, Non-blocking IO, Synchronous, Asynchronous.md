![image](https://github.com/do-sopt-cs-study/CS-Morgan/assets/51692363/4bfef57e-d89c-49d5-8c19-4dab94d9dd7a)

## I/O

컴퓨터 시스템에서 데이터를 입력 받거나 출력하는 프로세스. I/O 작업이 실제로 수행되는 level은 Kernel level(OS 단)에서 수행된다. 따라서 유저는 커널에 I/O 작업을 요청하고, 작업 완료 후 커널이 반환하는 결과를 기다린다.

## Blocking I/O

![image](https://github.com/do-sopt-cs-study/CS-Morgan/assets/51692363/b8a712ea-ea20-4b9f-906b-855bc6f5d499)

가장 기본적인 I/O 모델. I/O 작업이 진행되는 동안 유저는 **제어권을 kernel에 넘겨준** 채 대기한다. 작업이 종료되면 제어권을 **유저에게 반환**한다.

Blocking 방식은 I/O 응답을 기다리는 동안 다른 작업을 수행할 수 없어서 자원이 낭비된다.

## Non-Blocking I/O

![image](https://github.com/do-sopt-cs-study/CS-Morgan/assets/51692363/54ea701b-a2f1-4482-bd1e-703bcabd73bd)

I/O 작업이 진행되면 바로 **결과가 반환**된다. 반환된 데이터가 없으면 데이터가 없다는 결과 메세지를 반환한다. 입력 데이터가 있을 때까지 주기적으로 작업이 종료되었는지 확인한다. 제어권을 유저가 계속 소유하기 때문에 다른 작업을 이어나갈 수 있다.

그러나 반복적으로 시스템 호출이 발생하기 때문에 자원이 낭비된다.

## Blocking/NonBlocking 차이

**Blocking/NonBlocking**은 호출되는 함수가 **바로 제어권을 바로 주는가**(바로 리턴을 하는가)가 관심사다.

## Synchronous/Asynchronous

![image](https://github.com/do-sopt-cs-study/CS-Morgan/assets/51692363/5dbd74ac-370f-4503-9fda-4a6c09e0ff17)

### Synchronous

**직렬적**으로 테스크 수행. 어떤 작업이 수행 중이면 다음 작업은 대기한다.

호출하는 함수가 호출되는 함수의 작업 완료 후 리턴을 기다리거나, 또는 호출되는 함수로부터 바로 리턴 받더라도 **작업 완료 여부**를 호출하는 함수 스스로 계속 확인하며 **신경쓰면 Synchronous**다.

### Asynchronous

병렬적으로 테스크 수행. 어떤 작업이 수행 중이더라도 다른 작업을 바로 진행한다.

호출되는 함수에게 callback을 전달해서, 호출되는 함수의 작업이 완료되면 호출되는 함수가 전달받은 callback을 실행하고, 호출하는 함수는 **작업 완료 여부**를 **신경쓰지 않으면 Asynchronous**다.

## NonBlocking I/O + Async

![image](https://github.com/do-sopt-cs-study/CS-Morgan/assets/51692363/cd40bbd9-b418-43e6-b0fd-892883dd301c)

I/O 처리가 완료된 타이밍으로 결과를 회신하는 I/O모델이다. I/O의 회신은 시그널 또는 콜백의 형태로 이뤄진다.

제어권을 넘기지 않는 점에서 **NonBlocking I/O**와 같지만, **I/O처리를 완료 했을 때** 통지를 함으로 시스템 호출이 더욱 적어 낭비가 더 적다.
