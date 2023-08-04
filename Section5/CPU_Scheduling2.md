# CPU Scheduling - 2

## Multilevel Queue

✅ Ready queue에 여러 개로 분할
- <u>**`foreground`**</u> (**interactive**)
- <u>**`background`**</u> (**batch - no human interaction**)

✅ 각 큐는 독립적인 스케쥴링 알고리즘을 가짐
- <u>**`foreground`**</u> - **RR**
- <u>**`background`**</u> - **FCFS**

✅ 큐에 대한 스케쥴링 필요
- **Fixed priority scheduling**
  - serve `all from foreground` `then from background`.
  - Possibility of `starvation`.
- **Time Slice**
  - 각 큐에 CPU Time을 적절한 비율로 할당
  - Eg. `80% to foreground in RR`, `20% ti background in FCFS`

<img src = "https://user-images.githubusercontent.com/92699723/258297478-18772d12-3bea-4646-93bd-d68e5bf28ba4.jpg" width=60%>

---

## Multilevel Feedback Queue (계급적이지 않도록한다.)

✅ 프로세스가 다른 큐로 이동 가능

✅ 에이징(Aging)을 이와 같은 방식으로 구현할 수 있다.

✅ `Multilevel-feedback-queue scheduler`를 정의하는 파라미터들
- Queue의 수
- 각 큐의 scheduling algorithm
- Process를 상위 큐로 보내는 기준
- Process를 하위 큐로 내쫓는 기준
- 프로세스가 CPU 서비스를 받으려 할 때 들어갈 큐를 결정하는 기준

<img src = "https://user-images.githubusercontent.com/92699723/258303517-9302614b-3e33-4e54-920a-f69d150ee641.jpg" width=60%>

---

### Example of Multilevel Feedback Queue

✅ Three queues:
- Q0 - time quantum 8 milliseconds
- Q1 - time quantum 16 milliseconds
- Q2 - FCFS

✅ Scheduling
- new job이 queue Q0로 들어감
- CPU를 잡아서 할당 시간 8 milliseconds 동안 수행됨
- 8 milliseconds 동안 다 끝내지 못했으면 queue Q1으로 내려감
- Q1에 줄서서 기다렸다가 CPU를 잡아서 16ms 동안 수행됨
- 16ms에 끝내지 못한 경우 queue Q2로 쫓겨남

---

## Multiple - Processor Scheduling

✅ CPU가 여러 개인 경우 스케쥴링은 더욱 복잡해짐   
(화장실이 여러칸 생긴 것으로 상상해보자.)

✅ **`Homegeneous processor인 경우`**
- Queue에 한 줄로 세워서 각 프로세서가 알아서 꺼내가게 할 수 있다.
- 반드시 특정 프로세서에서 수행되어야 하는 프로세스가 있는 경우는 문제가 더 복잡해진다.

✅ **`Load sharing`**
- 일부 프로세서에 job이 몰리지 않도록 <u>부하를 적절히 공유하는 메커니즘 필요</u>
- 별개의 큐를 두는 방법 vs 공동 큐를 사용하는 방법

✅ **`Symmetric Multiprocessing (SMP)`**
- 각 프로세서가 각자 알아서 스케줄링 결정
- 모든 프로세스가 동등한 관계를 가진다.

✅ **`Asymmetric Multiprocessing`**
- 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지 프로세서는 거기에 따름

---

## Real-Time Scheduling

✅ <u>Hard real-time systems</u>
- Hard real-time task는 정해진 시간 안에 반드시 끝내도록 스케쥴링해야한다.

✅ <u>Soft real-time computing</u>
- Soft real-time task는 일반 프로세스에 비해 높은 우선순위를 갖도록 해야한다.
  
---

## Thread Scheduling

✅ <u>Local Scheduling</u>
- **User level thread**의 경우 사용자 수준의 **thread library**에 의해 어떤 thread를 스케쥴할 지 결정

✅ <u>Global Scheduling</u>
- **Kernel level thread**의 경우 일반 프로세스와 마찬 가지로 커널의 단기 스케쥴러가 어떤 **thread**를 스케쥴할지 결정

---

## Algorithm Evalution

<img src = "https://user-images.githubusercontent.com/92699723/258310575-10d9a425-de5e-4f95-9a6c-aed0eee6ce05.jpg" width=60%>

✅ <u>**`Queueing models (이론적인 방법)`**</u>
- `확률 분포`로 주어지는 `arrival rate`와 `service rate`등을 통해 각종 `performance index`값을 계산

✅ <u>**`Implementation (구현) & Measureement (성능 측정)`**</u>
- `실제 시스템`에 알고리즘을 `구현`하여 실제 작업(`workload`)에 대해서 성능을 `측정` 비교

✅ <u>**`Simulation(모의 실험)`**</u>
- 알고리즘을 `모의 프로그램`으로 작성 후 `trace`를 입력으로 하여 결과 비교

---

