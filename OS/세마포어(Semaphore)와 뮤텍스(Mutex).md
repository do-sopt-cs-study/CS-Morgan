# Critical Section

병렬프로그래밍에서 둘 이상의 스레드 (멀티스레드)가 동시에 접근해서는 안되는 공유 자원(파일, 입출력, 공유 데이터 등)을 접근하는 명령문 또는 코드의 일부 영역

## 해결책

### **Lock**

동시에 공유 자원에 접근하는 것을 막기 위해 임계 구역에 진입하는 프로세스는 Lock 을 획득하고 임계 구역을 빠져나올 때, Lock 을 방출함으로써 동시에 접근이 되지 않도록 하는 **하드웨어 기반 해결책.**

### **Semaphore**

Critical Section 문제를 해결하기 위한 동기화 도구로 **소프트웨어 기반 해결책**. 세마포어는 카운팅 세마포어, 이진 세마포어로 구분.

- Counting Semaphore
  현재 공유 자원에 접근할 수 있는 스레드 또는 프로세스의 수를 나타내는 값을 두어 접근 제어용으로 사용.
  Semaphore의 변수 값만큼 프로세스 또는 스레드가 접근 가능. 자원을 사용하면 Semaphore가 감소, 해제하면 Semaphore가 증가 한다.
- Binary Semaphore
  상태가 0, 1 이렇게 2개뿐인 semaphore.
  - lock을 건 Process와 Unlock을 하는 Process가 달라도 된다.

### **Mutual Exclusion Semaphore(뮤텍스)**

이진 세마포어와 비슷하지만 shared resource를 보호하기 위해 존재. 세마포어의 일부분이다.

- lock을 건 Process와 Unlock을 하는 Process가 같아야 함.

### 뮤텍스와 세마포어의 차이

뮤텍스 목적

- 공유자원에 대한 상호 배제

세마포어 목적

- 공유자원을 사용하는 프로세스 또는 스레드의 동기화
