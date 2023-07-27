# Process1

## 🔥프로세스의 개념
✅ "Process is `a program in execution`"  
즉, 실행중인 프로그램을 말한다.

✅ 프로세스의 문맥(Context)
- **CPU 수행 상태를 나타내는 하드웨어 문맥**
  - Program Counter (PC)
  - 각 종 Register
- 프로세스의 주소 공간
  - code, data, stack
- 프로세스 관련 커널 자료 구조
  - PCB (Process Control Block)
  - Kernel stack 

<img src= "https://user-images.githubusercontent.com/92699723/215304926-0f4f5a9c-6ec6-410a-81fc-63d57bcb5720.png" width=500>

---
## 🔥 프로세스의 상태
✅ 프로세스는 상태(state)가 변경되며 수행된다. 아래의 내용을 실행중인 프로그램으로 상상해보자!
- `Running`
  - CPU를 잡고 instruction(명령)을 수행중인 상태
- `Ready`
  - CPU를 기다리는 상태 (메모리 등 다른 조건을 모두 만족하고)
- `Blocked (wait, sleep)`
  - **`🎈 자신이 요청한 event가 만족되면 Ready`**
  - CPU를 주어도 당장 instruction을 수행할 수 없는 상태 (보통 오래걸리는 작업을 진행할 때)
  - Process 자신이 요청한 event (ex. I/O)가 즉시 만족되지 않아 이를 기다리는 상태
  - I/O 등의 event를 (스스로) 기다리는 상태
  - ex) 디스크에서 file을 읽어와야 하는 경우
- `Suspended(stopped)`
  - **`🎈 외부에서 resume해주어야 Active`**
  - 외부적인(Medium-term) 이유로 프로세스의 수행이 정지된 상태
  - 프로세스는 통째로 디스크에 swap out된다.
  - ex) 사용자가 프로그램을 일시 정지시킨 경우(break key)   
    시스템이 여러 이유로 프로세스를 잠시 중단시킴(메모리에 너무 많은 프로세스가 올라와 있을 때)

- New: 프로세스가 생성중인 상태
- Terminated: 수행(execution)이 끝난 상태

<img src = "https://user-images.githubusercontent.com/92699723/215305479-7a12ca90-a964-44be-b6d2-29beca277d79.png" width=500>

---
## 🔴 PCB(Process Control Block)
- 운영체제가 각 프로세스를 관리하기 위해 프로세스당 유지하는 정보
- 다음의 구성 요소를 가진다 (구조체로 유지)
  - (1) OS가 관리상 사용하는 정보
    - Process state, Process ID
    - Scheduling information, priority(우선순위)
  - (2) CPU 수행 관련 하드웨어 값
    - Program counter(PC), registers
  - (3) 메모리 관련
    - code, data, stack의 위치 정보
  - (4) 파일 관련
    - Open file descriptors...

<img src = "https://user-images.githubusercontent.com/92699723/215305762-ef01aa10-6a52-40f1-a969-591ba3c4b074.png" width=300>   

프로세스 하나당 위 같은 PCB 정보를 보유하고 있다.

---
## 🔥문맥 교환 (Context Switch)
✅ CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정   
✅ CPU가 다른 프로세스에게 넘어걸 때 운영체제는 아래의 내용을 수행한다.
  - CPU를 내어주는 프로세스의 상태를 그 프로세스의 PCB에 저장
  - CPU를 새롭게 얻는 프로세스의 상태를 PCB에서 읽어온다.

<img src = "https://user-images.githubusercontent.com/92699723/215305945-e801b934-aa96-4da0-994d-6db850e6efc9.png" width=500>

### 헷갈릴 수 있는 내용
✅ System call이나 Interrupt 발생 시 반드시 context switch가 일어나는 것은 아니다.   

<img src = "https://user-images.githubusercontent.com/92699723/215306091-8de29564-b960-47f5-8a1c-51258ea34e28.png" width=500>

💡(1)의 경우에도 CPU 수행 정보 등 context의 일부를 PCB에 save해야 하지만   
**문맥 교환을 하는** (2)의 경우 그 부담이 훨씬 크다(eg. cache memory flush)

(1) - 단순히 User mode에서 Kernel mode로 빠졌다가 돌아오는 경우  
(2) - `timer Interrupt`는 <U>다른 프로세스로 넘기기 위한 의도를 가진 interrupt</U>이므로   
`사용자 프로세스A`에 할당된 CPU시간이 끝나서 `사용자 프로세스B`로 넘어가기 때문에 Context switch가 발생하는 것이다.

---
## 프로세스를 스케쥴링하기 위한 큐
✅ <U>Job queue</U>
  - 현재 시스템 내에 있는 모든 프로세스의 집합

✅ <U>Ready queue</U>
  - 현재 메모리 내에 있으면서 CPU를 잡아서 실행되기를 기다리는 프로세스의 집합  

✅ <U>Device queues</U>
  - I/O device의 처리를 기다리는 프로세스의 집합

✅ 프로세스들은 각 큐들을 오가며 수행된다

---
## 🔥 스케쥴러(Scheduler)
✅ <U>**`Long-term scheduler`**</U>(장기 스케쥴러 or job scheduler) 
- 시작 프로세스 중 어떤 것들을 **`ready queue`** 로 보낼지 결정
- 프로세스에 `memory(및 각 종 자원)`을 주는 문제
- **`degree of Multiprogramming`** 을 제어
- **time sharing system** 에는 보통 장기 스케쥴러가 없음 (무조건 ready)

✅ <U>**`Short-term scheduler`**</U>(단기 스케쥴러 or CPU scheduler)    
- 어떤 프로세스를 다음번에 **`running`** 시킬지 결정
- 프로세스에 `CPU`를 주는 문제
- 충분히 빨라야 한다. (millisecond 단위)

✅ <U>**`Medium-term scheduler`**</U>(중기 스케쥴러 or Swapper)   
- **여유 공간 마련을 위헤 프로세스를 통째로 메모리에서 디스크로 쫒아냄**
- 프로세스에게서 memory를 뺏는 문제
- **`degree of Multiprogramming`**(메모리에 프로그램이 몇 개 올려놓을지)을 제어

<img src = "https://user-images.githubusercontent.com/92699723/215308807-a35a5fbb-ffbc-440a-b06f-e0edfcc65648.png" width=500>
