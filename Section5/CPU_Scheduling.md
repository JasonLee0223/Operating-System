# CPU Scheduling - 1

<img src = "https://user-images.githubusercontent.com/92699723/256756556-2d6c9ff9-a82b-4f26-be0b-668cebe3f288.jpg" width = 70%>

<img src = "https://user-images.githubusercontent.com/92699723/256757472-3d875a4a-a278-493e-bdbb-7a46849be773.jpg" width = 70%>

> CPU를 짧게쓰고 중간에 I/O가 끼어드는 경우(빈도가 잦다)를 `I/O bound job`이라고 부르며   
> CPU를 오랫동안 사용하는 경우를 `CPU bound job`이라고 부른다.   
> 하여 job의 종류가 섞여있음으로 **CPU 스케쥴링이 필요하게된 것이다.**

> I/O bound job이 문제가 되며 사용자와 상호작용하는 부분이기 때문이다.   
> CPU를 사람하고 인터렉션하는 job에게 우선적으로 CPU의 제어권을 줘야한다는 것이 CPU 스케쥴링의 중요한 목적 즁 하나이다.   
> 공평함보다 효율성이 중요할 것이고 Interactive한 job이 너무 오래 기다리지 않도록 하는 것이 CPU 스케쥴링의 중요한 필요성이다.

---

## 프로세스의 특성 분류

✅ 프로세스는 그 특성에 따라 다음 두 가지로 나뉜다.
- **`I/O bound process`**
  - CPU를 잡고 계산하는 시간보다 I/O에 많은 시간이 필요한 job
  - (`many short CPU bursts`)

- **`CPU bound process`**
  - 계산 위주의 job
  - (`few very long CPU bursts`)

---

## CPU Scheduler & Dispatcher

> OS(운영체제) 안에서 `CPU 스케쥴링을 하는 코드`를 <u>CPU scheduler</u>라고 부르는 것이다.   
> <u>Dispatcher</u> 또한 `CPU를 넘겨주는 역할`을 하는 운영체제의 커널 코드이다.

✅ <u>`CPU Scheduler`</u>
- Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다.

✅ <u>`Dispatcher`</u>
- CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에게 넘긴다.
- 이 과정을 context switch(문맥 교환)라고 한다.

✅ CPU 스케쥴링이 필요한 경우는 프로세스에게 다음과 같은 상태 변화가 있는 경우이다.
1. `Running -> Blocked` (ex. I/O 요청하는 시스템 콜)
2. `Running -> Ready` (ex. 할당시간만료 timer interrupt)
3. `Blocked -> Ready` (ex. I/O 완료 후 인터럽트)
4. `Terminate`

- 비선점형 **nonpreemptive (=강제로 빼앗지 않고 자진 반납)**
- 선점형 **preemptive (=강제로 빼앗음)** (대부분 사용하는 방식)

---

## 🔴 Scheduling Criteria
### Performance Index (= Performance Measure, 성능 척도)

#### 🅰️ 시스템 입장

✅ `CPU utilization(이용률)`

> CPU는 최대한 바쁘게 만들어라.

- keep the **CPU as busy as possible**

✅ `Throughput(처리량)`

> 주어진 시간동안 몇 개의 작업을 완료했느냐?

- **# of processes** that **complete** their execution per time unit

#### 🅱️ 프로세스 입장 (고객 입장)

✅ `Turnaround time (소요시간, 반환시간)`

> CPU를 사용하러 들어와서 다 사용하고 나갈 때까지 걸린 시간

- amount of time of **execute a particular process**

✅ `Waiting time (대기 시간)`

> CPU를 사용하고자 하더라도 1개 밖에 없기 때문에 Ready Queue에서 기다리는 시간

- amount of time a process has been **waiting in the ready queue**

✅ `Response time (응답 시간)`

> Ready Queue에 들어와서 처음으로 CPU의 소유권을 얻기까지 걸린 시간

- amount of time it takes from **when a request was submitted <u>until the first response</u> is produced**,    
not output (for time-sharing environment)

---

## Scheduling Algorithms

✅ FCFS (First-Come First-Served)   
✅ SJF (Shortest-Job-First)   
✅ SRTF (Shortest-Remaining-Time-First)   
✅ Priority Scheduling   
✅ RR (Round Robin)   
✅ Multilevel Queue   
✅ Multilevel Feedback Queue   

---

## FCFS (First-Come First-Served)

<img src = "https://user-images.githubusercontent.com/92699723/257046959-4d664291-19e6-45cc-847d-9e9f442e43b6.jpg" width = 60%>

> 먼저 온 순서대로 처리하며, `비선점형 스케쥴링`이다.   
> 효율적이지는 않다. (CPU를 오래 사용하게 될 작업이 소유권을 가져가면 작업이 마칠 때까지 기다려야함으로)    
> 앞에 어떤 프로세스가 사용하고 있느냐에 따라 `waiting time`에 영향을 많이 끼치게된다.   
> Convoy: 전쟁 물자를 나르는데 보호되도록 호송하는 개념 / 컴퓨터쪽에서의 의미는 Queue에서 오래 기다리는 의미로 변형되어 사용

---

### ❗️ SJF (Shortest-Job-First)

✅ 각 프로세스의 다음번 **`CPU burst time`** 을 가지고 스케쥴링에 활용

✅ **`CPU burst time`** 이 가장 짧은 프로세스를 제일 먼저 스케쥴

✅ Two schemes:
- **<u>Nonpreemptive</u>**
  - 일단 CPU를 잡으면 이번 CPU burst가 완료될 때까지 CPU를 선점(preemption) 당하지 않음

- **<u>Preemptive</u>**
  - 현재 수행중인 프로세스의 남은 burst time보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗김
  - 이 방법을 Shortest-Remaining-Time-First(**`SRTF`**)이라고도 부른다.

✅ SJF is optimal
- 주어진 프로세스들에 대해 **`minimum average waiting time`** 을 보장 (Preemptive 버전에 해당)

<img src = "https://user-images.githubusercontent.com/92699723/258281223-207863a7-dea6-465d-a42f-0317e4c7a3fa.jpg" width=60%>

> 0초 시점에 도착한 프로세스가 P1만 있음으로 본인이 원하는 만큼 사용하고 반납한다.   
> P2 ~ P4까지 모두 도착해있는데 여기서 P3가 가장 소요시간(Burst Time)이 짧기 때문에 CPU 소유권을 먼저 얻게된다.

> 🔥 CPU 스케쥴링이 언제 이루어지는가? 
> CPU를 다 사용하고 나가는 시점에 결정된다.

<img src = "https://user-images.githubusercontent.com/92699723/258281795-936b37b9-43a4-4801-b85d-f78c5cc17fa0.jpg" width=60%>

> 위 비선점형 버전과 동일하게 P1이 가장 먼저 도착했음으로 P1이 우선 수행한다.   
> 그 다음 선점형이기 때문에 P1이 소유권이 있지만 더 짧은 Burst time이 도착하면 빼앗길 수 있는 방법이기에   
> P2가 사용하고자하는 시간이 4초이고 P1은 2초를 사용하고 5초가 남았기때문에 아래와 같이 보여지며   
> [P1:5초], [P2:4초] P2에게 소유권이 넘어가게된다.   
> 4초 시점에서는 P3가 도착하고 1초만 사용하기 때문에 가장 짧기 때문에 P3가 소유권을 얻게된다.   

> 🔥 CPU 스케쥴링이 언제 이루어지는가? 
> 새로운 프로세스가 도착하면은 언제든지 스케쥴링이 이루어질 수 있다.   
> 이렇게 두 가지 버전에는 차이점이 존재한다.

---

## SJF 문제점 1. Priority Scheduling

✅ A <u>**priority number**</u>(integer) is associated with each process

✅ **`highest priority`** 를 가진 프로세스에게 CPU 할당   
(smallest integer = highest priority).

- Preemptive
- Nonpreemptive

✅ SJF는 일종의 priority scheduling이다.
- **`priority = predicted next CPU burst time`**

✅ Problem
- <u>**`Starvation(기아 현상)`**</u>: low priority processes may **`never execute`**.

✅ Solution
- <u>**`Aging(노화)`**</u>: as time progresses **`increase the priority`** of the process.

---

## SJF 문제점 2. 다음 CPU Burst Time의 예측

<img src = "https://user-images.githubusercontent.com/92699723/258285981-413692e9-c63c-4086-bb27-5e14ed1ef6c9.jpg" width = 60%>

✅ 다음번 **`CPU Burst Time`** 을 어떻게 알 수 있는가?   
(input data, branch, user ...)

✅ 추정(`estimate`)만이 가능하다.

✅ 과거의 CPU Burst Time을 이용하여 추정
(exponential averaging)

<img src = "https://user-images.githubusercontent.com/92699723/258286699-fa92342a-a359-4d68-903e-fa2544f4b619.jpg" width=60%>

> 알파값이 0과 1은 극단적인 예시이므로 참고만 하면됩니다.

---

## ❗️ Round Robin (RR)

> 현대 컴퓨터 시스템에서는 대부분 해당 스케쥴링 기법을 채택하여 사용하고있다.   
> 장점: **`응답 시간(Response Time)`** 이 빨라진다.   
> 특징: CPU를 길게 쓰는 프로세스는 기다리는 시간도 길어지고 짧은 시간이 걸리면 짧게 기다린다.   
> 즉, **대기 시간이 CPU를 사용하려는 시간에 비례하게된다.**   

✅ 각 프로세스는 동일한 크기의 할당 시간(**`time quantum`**)을 가진다.   
(일반적으로 10-100 milliseconds)

✅ 할당 시간이 지나면 프로세스는 선점(preempted)당하고 ready queue의 제일 뒤에 가서 다시 줄을 선다.

✅ n 개의 프로세스가 ready queue에 있고 할당 시간이 **`q time unit`** 인 경우   
각 프로세스는 최대 q time unit 단위로 CPU 시간의 1/n을 얻는다.   
-> **`어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다.`**

✅ Performance
- q large => FCFS
- q small => context switch 오버헤드가 커진다.

<img src = "https://user-images.githubusercontent.com/92699723/258294098-c8851240-ef73-439e-9923-cf68a90010b0.jpg" width = 60%>