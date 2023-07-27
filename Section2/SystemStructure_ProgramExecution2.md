# System Structure & Program Execution2

## 🔴 동기식 입출력과 비동기식 입출력
✅ 동기식 입출력 (synchronous I/O)
- I/O 요청 후 입출력 작업이 완료된 후에야 제어가 사용자 프로그램에 넘어감
- 구현 방법1
  - I/O가 끝날 때까지 CPU를 낭비시킴
  - 매시점 하나의 I/O만 일어날 수 있음
- 구현 방법2
  - I/O가 완료될 때까지 해당 프로그램에게서 CPU 자원을 빼앗음
  - I/O 처리를 기다리는 줄에 그 프로그램을 줄 세움
  - 다른 프로그램에게 CPU를 줌

✅ 비동기식 입출력(asynchronous I/O)
- I/O가 시작된 후 입출력 작업이 끝나기를 기다리지 않고 제어가 사용자 프로그램에 즉시 넘어감

🔥 두 경우 모두 I/O의 `완료는 인터럽트로 알려준다.`

---
## 🔴 DMA(Direct Memory Access)
- 빠른 입출력 장치를 메모리에 가까운 속도로 처리하기 위해 사용   
  (CPU에 너무 자주 많은 Interrupt를 부여하면 성능 측면에서 좋지 않기에 메모리에 직접 접근할 수 있도록 지원하기 위해 설계됨)
- CPU의 중재 없이 device controller가 device의 buffer storage의 내용을   
  메모리에 `block`단위로 직접 전송
- `Byte 단위`가 아니라 `block 단위`로 인터럽트를 발생시킴

---
## 🔴 프로그램의 실행 (메모리 load)
|File System|Virtual Memory|Physical Memory|
|:--:|:--:|:--:|
|실행파일|프로세스의 Address space<br/>-> Address translation ->|프로세스|

## 🤔 Kernel 주소 공간의 내용
|Code|Data|Stack|
|:---|:---|:---|
|커널 코드<br/>시스템 콜, 인터럽트 처리코드<br/>자원 관리를 위한 코드<br/>편리한 서비스 제공을 위한 코드|PCB(Process Control Block)<br/>CPU<br/>mem<br/>disk|Process의 커널 스택|

## 🟢 사용자 프로그램이 사용하는 함수
- 사용자 정의 함수
  - 자신의 프로그램에서 정의한 함수
- 라이브러리 함수
  - 자신의 프로그램에서 정의하지 않고 갖다 쓴 함수
  - 자신의 프로그램의 실행 파일에 포함되어 있다
- 커널 함수
  - 운영체제 프로그램의 함수
  - 커널 함수의 호출 = 시스템 콜

## 🟢 프로그램의 실행
|A의 주소공간|Kernel의 주소공간|A의 주소공간|Kernel의 주소공간|
|:---:|:---:|:---:|:---:|
|User Mode|Kernel Mode|User Mode|Kernel Mode|
|---->|---->|---->|---->|
|User defined <br/>function call||Library <br/>function call||
|Program Begins<br/>~ System Call|System Call<br/>~ Return from Kernel|Return from Kernel<br/>~ System call|System call<br/>~Program ends|