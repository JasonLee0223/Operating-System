# DeadLock (교착 상태)

## The Deadlock Problem

✅ **`Deadlock`**

일련의 프로세스들이 서로가 가진 자원을 기다리며 block된 상태

✅ **`Resource`** (자원)
- HW, SW 등을 포함하는 개념
- (예) I/O device, CPU cycle, memory space, semaphore 등
- 프로세스가 자원을 사용하는 절차
  - Request, Allocate, Use, Release

✅ Deadlock Example 1
- 시스템에 2개의 tape drive가 있다.
- 프로세스 $P_1$과 $P_2$ 각각이 하나의 tape drive를 보유한 채 다른 하나를 기다리고 있다.

✅ Deadlock Example2
- Binary semaphores A and B   

|$P_0$|$P_1$|
|:--|:--|
|P(A)|P(B)|
|P(B)|P(A)|

---

## Deadlock 발생의 4가지 조건

✅ <u>**`Mutual exclusion (상호 배제)`**</u>
- 매 순간 하나의 프로세스만이 자원을 사용할 수 있음

> 독점적으로 써야지만 Deadlock이 발생한다.

✅ <u>**`No preemption (비선점)`**</u>
- 프로세스는 자원을 스스로 내어놓을 뿐 강제로 빼앗기지 않음

> 가진 자원을 빼앗기지 않기 때문에 Deadlock이 발생한다.

✅ <u>**`Hold and wait (보유 대기)`**</u>
- 자원을 가진 프로세스가 다른 자원을 기다릴 때 보유 자원을 놓지 않고 계속 가지고 있음

> 내가 가진 자원은 내어놓지 않으면서 추가적으로 자원을 요청하는 경우

✅ <u>**`Circular wait (순환 대기)`**</u>
- 자원을 기다리는 프로세스간에 사이클이 형성되어야 함
- 프로세스 $P_0$, $P_1$, ..., $P_n$ 이 있을 때
  - $P_0$ 는 $P_1$이 가진 자원을 기다림
  - $P_1$ 는 $P_2$이 가진 자원을 기다림
  - $P_{n-1}$ 는 $P_n$이 가진 자원을 기다림
  - $P_n$는 $P_0$이 가진 자원을 기다림

---

<img src = "https://user-images.githubusercontent.com/92699723/258701163-cb2abfd6-cd58-4ffd-85af-6d9b6f40eebe.jpg" width=60%>

> Deadlock이 발생 했는지 알아보기 위해 자원 할당 그래프를 이용한다.   
> 동그라미 = 프로세스, 사각형 = 자원

<img src = "https://user-images.githubusercontent.com/92699723/258701473-88ce8bee-4d4f-4cf4-956d-e0e9497ebb81.jpg" width=60%>

> 화살표를 따라가서 Cycle을 확인한다.   
> 왼쪽 그림은 Cycle이 총 2개이며 Deadlock 상태이다.   
> 오른쪽 그림은 Cycle이 2개이지만 Deadlock 상태는 아니다.   
> 여분의 자원을 2, 4번 프로세스가 가지고 있지만 2개의 프로세스는 Deadlock에 연루되어있는 경우는 아니다.

---

## Deadlock의 처리 방법

> 위 2가지 방법은 미연에 방지하는 방법으로    
> 밑에 있는 2가지 방법은 Deadlock이 생기는것을 놔두는 방법이다.   
> 현대에는 4번째 방법을 채택하여 Deadlock이 생기는 것을 해결하지않고 놔두면서 사람이 프로세스가 느려지면 알아서 해결하는 방법을 채택한다.   
> (데드락은 자주 발생하는 이벤트가 아니기때문에 훨씬 더 많은 오버헤드를 두는 것이 비효율적이라서 채택하였다.)

✅ <u>**`Deadlock Prevention`**</u>
- 자원 할당 시 Deadlock의 `4가지` 필요 조건 중 어느 하나가 만족되지 않도록 하는 것

> 가장 강력하게 Deadlock을 미연에 방지하는 방법

✅ <u>**`Deadlock Avoidance`**</u>
- 자원 요청에 대한 부가적인 정보를 이용해서 `deadlock`의 가능성이 없는 경우에만 자원을 할당
- 시스템 `state`가 원래 `state`로 돌아올 수 있는 경우에만 자원 할당

✅ <u>**`Deadlock Detection and recovery`**</u>
- `Deadlock` 발생은 허용하되 그에 대한 `detection` 루틴을 두어 `deadlock` 발견 시 `recover`

✅ <u>**`Deadlock Ignorance`**</u>
- `Deadlock`을 시스템이 책임지지 않음
- `UNIX`를 포함한 대부분의 `OS`가 채택

---

## Deadlock Prevention

✅ <u>**`Mutual Exclusion`**</u>
- 공유해서는 안되는 자원의 경우 반드시 성립해야 함

> 개발자가 배제할 수 있는 조건은 아니다.

✅ <u>**`Hold and Wait`**</u>
- 프로세스가 자원을 요청할 때 다른 어떤 자원도 가지고 있지 않아야 한다.
- 방법1. 프로세스 시작 시 모든 필요한 자원을 할당받게 하는 방법
- 방법2. 자원이 필요한 경우 보유 자원을 모두 놓고 다시 요청

✅ <u>**`No Preemptive`**</u>
- `process`가 어떤 자원을 기다려야 하는 경우 이미 보유한 자원이 선점됨
- 모든 필요한 자원을 얻을 수 있을 때 그 프로세스는 다시 시작된다.
- `State`를 쉽게 `save`하고 `restore` 할 수 있는 자원에서 주로 사용 (CPU, memory)

✅ <u>**`Circular Wait`**</u>
- 모든 자원 유형에 할당 <u>순서를 정하여</u> 정해진 순서대로만 자원 할당
- 예를 들어 순서가 3인 자원 Ri를 보유 중인 프로세스가 순서가 1인 자원 Rj을 할당받기 위해서는 우선 Ri를 release 해야 한다.

=> Utilization 저하, throughput 감소, starvation 문제   

---

## Deadlock Avoidance

✅ Deadlock avoidance
- 자원 요청에 대한 **`부가 정보를 이용해서 자원 할당이 deadlock으로 부터 안전(safe)한 지`** 를 동적으로 조사해서   
안전한 경우에만 할당
- 가장 단순하고 일반적인 모델은 프로세스들이 필요로 하는 각 자원별 최대 사용량을 미리 선언하도록 하는 방법임

✅ **`safe state`**
- 시스템 내의 프로세스들에 대한 `safe sequence`가 존재하는 상태

✅ **`safe sequence`**
- 프로세스의 sequence <P1, P2,... , Pn>이 safe하려면 Pi(1 $\leq$ i $\leq$ n)의 자원 요청이   
**`"가용 자원 + 모든 $P_j(j<i)$의 보유 자원"`** 에 의해 충족되어야 한다.   

- 조건이 만족하면 다음 방법으로 모든 프로세스의 수행을 보장
  - $P_i$의 자원 요청이 즉시 충족될 수 없으면 모든 $P_j(j<i)$가 종료될 때까지 기다린다.
  - $P_{i-1}$이 종료되면 $P_i$의 자원 요청을 만족시켜 수행한다.

✅ 시스템이 safe state에 있으면   
=> no deadlock

✅ 시스템이 <u>`unsafe`</u> state에 있으면   
=> <u>`possibility of deadlock`</u>

✅ Deadlock Avoidance
- 시스템이 unsafe state에 들어가지 않는 것을 보장
- 2가지 경우의 avoidance 알고리즘
  - `Single instance` per resource types
    - <u>**`Resource Allocation Graph algorithm 사용`**</u>
  - `Multiple instances` per resource types
    - <u>**`Banker's Algorithm 사용`**</u>

<img src = "https://user-images.githubusercontent.com/92699723/258712664-9b399c6f-d037-49d4-a8cd-e10f7b91112a.jpg" width=50%>

---

## Resource Allocation Graph algorithm

✅ <u>**`Claim edge`**</u> $P_i$ -> $R_j$   
- 프로세스가 $P_i$가 자원 $R_j$를 미래에 요청할 수 있음을 뜻함 (점선으로 표시)   
- 프로세스가 해당 자원 요청 시 **request edge**로 바뀜 (실선)
- $R_j$가 **release**되면 **assignment edge**는 다시 **claim edge**로 바뀐다.

✅ **request edge**의 **assignment edge** 변경 시 (점선을 포함하여) **cycle**이 생기지 않는 경우에만   
요청 자원을 할당한다.

✅ **Cycle** 생성 여부 조사 시 프로세스의 수가 n일 때 $O(n^2)$ 시간이 걸린다.

<img src = "https://user-images.githubusercontent.com/92699723/258713428-4c7f2eb7-c078-4117-aa11-4e4e7587cdda.jpg" width=60%>

---

## Banker's Algorithm

✅ 가정
- 모든 프로세스는 자원의 최대 사용량을 미리 명시
- 프로세스가 요청 자원을 모두 할당받은 경우 유한 시간 안에 이들 자원을 다시 반납한다.

✅ 방법
- 기본 개념: 자원 요청 시 `safe` 상태를 유지할 경우에만 할당
- 총 요청 자원의 수가 가용 자원의 수보다 적은 프로세스를 선택   
(그런 프로세스가 없으면 `unsafe` 상태)
- 그런 프로세스가 있으면 그 프로세스에게 자원을 할당
- 할당받은 프로세스가 종료되면 모든 자원을 반납
- 모든 프로세스가 종료될 때까지 이러한 과정 반복

<img src = "https://user-images.githubusercontent.com/92699723/258714236-3eb2e524-4dac-4fea-ad4e-711dbe2bf7a9.jpg" width=60%>

> 프로세스들이 자원을 요청했을 때 받아들일 것이냐 아니냐를 따지는 알고리즘   
> 받아들이는 경우는 최대 조건이 만족되는 경우만 해당된다.   

<img src = "https://user-images.githubusercontent.com/92699723/258717254-b78faf48-39c2-4d37-afcb-c39f3cbcd18f.jpg" width=60%>

---

## Deadlock Detection and Recovery

✅ Deadlock Detection
- Resource type 당 single instance인 경우
  - 자원할당 그래프에서의 cycle이 곧 deadlock을 의미
- Resource type 당 multiple instance인 경우
  - Banker's algorithm과 유사한 방법 활용

✅ Wait-for graph 알고리즘
- Resource type 당 single instance인 경우
- Wait-for graph
  - 자원할당 그래프의 변형
  - 프로세스만으로 node 구성
  - $P_j$가 가지고 있는 자원을 $P_k$가 기다리는 경우 $P_k -> P_j$
- Algorithm
  - Wait-for graph에 사이클이 존재하는지를 주기적으로 조사
  - **$O_(n^2)$**

<img src = "https://user-images.githubusercontent.com/92699723/258718932-d96a638b-eea6-4596-b175-9534b7d80d26.jpg" width=60%>

<img src = "https://user-images.githubusercontent.com/92699723/258719682-fe6070bf-621f-4b66-8a52-c2ff334640d4.jpg" width=60%>

✅ Recovery
- <u>**`Process termination`**</u>
  - Abort <u>**`all`**</u> deadlocked processes
  - Abort <u>**`one process at a time`**</u> until the deadlock cycle is eliminated

- <u>**`Resource Preemption`**</u>
  - 비용을 최소화 할 **victim**의 선정
  - `safe state`로 `rollback`하여 `process`를 `restart`
  - `Starvation` 문제
    - 동일한 프로세스가 계속해서 `victim`으로 선정되는 경우
    - `cost factor`에 `rollback` 횟수도 같이 고려

---

## Deadlock Ignorance

✅ Deadlock이 일어나지 않는다고 생각하고 아무런 조치도 취하지 않음
- Deadlock이 매우 드물게 발생하므로 deadlock에 대한 조치 자체가 더 큰 **overhead**일 수 있음
- 만약, 시스템에 `deadlock`이 발생한 경우 시스템이 비정상적으로 작동하는 것을 사람이 느낀 후 직접 process를   
죽이는 등의 방법으로 대처
- UNIX, Windows 등 대부분의 범용 OS가 채택

---

