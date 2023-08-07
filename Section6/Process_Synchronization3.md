# Part3. Process Synchronization

## Classical Problems of Synchronization

✅ Bounded-Buffer Problem (Producer-Consumer Problem)

✅ Readers and Writers Problem

✅ Dining-Philosophers Problem

---

## Bounded-Buffer Problem (Producer-Consumer Problem)

> 임시로 데이터를 저장하는 공간인 버퍼가 유한한(bounded) 환경에서 생산자-소비자 문제가 발생한다.   
> 여기서 말하는 생산자, 소비자 모두 프로세서로 하나씩만 있는 것이 아닌 여러개가 있는 상황이다.   
> 생산자는 공유 버퍼에다가 데이터를 만들어서 집어넣는 역할을 한다.(주황색)   

<img src = "https://user-images.githubusercontent.com/92699723/258570719-718dfaa0-0863-4c8f-94ae-95040be2b974.jpg" width = 60%>

✅ Producer

1. Empty 버퍼가 있나요?? (없으면 기다림)
2. 공유데이터에 lock을 건다.
3. Empty buffer에 데이터 입력 및 buffer 조작
4. Lock을 푼다.
5. Full buffer 하나 증가

✅ Consumer

1. full 버퍼가 있나요? (없으면 기다림)
2. 공유데이터에 lock을 건다
3. Full buffer에서 데이터 꺼내고 buffer 조작
4. empty buffer 하나 증가

✅ <u>**`Shared data`**</u>   
💥 buffer 자체 및 buffer 조작 변수 (empty / full buffer의 시작 위치)   

✅ <u>**`Synchronization variables`**</u>   
💥 mutual exclusion -> Need binary semaphore   
(shared data의 mutual exclusion을 위해)   
💥 resource count -> Need integer semaphore   
(남은 full / empty buffer의 수 표시)

<img src = "https://user-images.githubusercontent.com/92699723/258571139-b2d6db19-16f8-4a86-a633-ceab28b97444.jpg" width = 60%>

---

## Readers - Writers Problem

✅ 한 process가 DB에 write 중 일 때 다른 process가 접근하면 안됨

✅ read는 동시에 여럿이 해도 된다.

✅ solution
- Writer가 DB에 접근 허가를 아직 얻지 못한 상태에서는 모든 대기중인 Reader들을 다 DB에 접근하게 해준다.
- Writer는 대기 중인 Reader가 하나도 없을 때 DB 접근이 허용된다.
- 일단 Writer가 DB에 접근 중이면 Reader들은 접근이 금지된다.
- Writer가 DB에서 빠져나가야만 Reader의 접근이 허용된다.

✅ <u>**`Shared Data`**</u>
💥 **`DB 자체`**   
💥 **`readCount`** : 현재 DB에 접근 중인 Reader의 수

✅ <u>**`Synchronization variables`**</u>
💥 **`mutex`** : 공유 변수 readCount를 접근하는 코드 (critical section)의 mutual exclusion 보장을 위해 사용   
💥 **`db`** : Reader와 Writer가 공유 DB 자체를 올바르게 접근하게 하는 역할

<img src = "https://user-images.githubusercontent.com/92699723/258573578-7b1599fb-9e25-4d5f-866f-6fbad66f7ac9.jpg" width=60%>

---

## Dining - Philosophers Problem

> 철학자는 2가지의 일을 수행하게되는데   
> 첫 번째로는 `생각을 하는 일`이며 두 번째로 `배가 고파지면 왼쪽과 오른쪽에있는 젓가락을 사용해 식사를 한다.`   
> 배가 불러지면 다시 생각을 하는 일을 한다.
> 젓가락을 잡을 수 없는 상황이게되면 기다려야한다.   
> 아래는 Semaphore를 사용하여 작성된 코드이지만 Deadlock에 빠질 위험이 있다.

<img src = "https://user-images.githubusercontent.com/92699723/258681795-23cca9c6-5546-44fe-8e00-f9ac1ef2212d.jpg" width=60%>

✅ 앞의 Solution의 문제점
- Deadlock 가능성이 있다.
- 모든 철학자가 동시에 배가 고파져 왼쪽 젓가락을 집어버린 경우

✅ 해결방안
- 4명의 철학자만이 테이블에 동시에 앉을 수 있도록 한다.
- 젓가락을 두 개 모두 집을 수 있을 때에만 젓가락을 집을 수 있게 한다.
- 비대칭
  - 짝수(홀수) 철학자는 왼쪽(오른쪽) 젓가락부터 집도록 한다.

### [참고] 2번 해결방안에 대한 솔루션 코드
<img src = "https://user-images.githubusercontent.com/92699723/258682934-db4e3837-0d55-46af-936d-65ddf0863edd.jpg" width=60%>

---

## Monitor

✅ Semaphore의 문제점
- 코딩하기 힘들다
- 정확성(correctness)의 입증이 어렵다
- 자발적 협력(voluntary cooperation)이 필요하다
- 한 번의 실수가 모든 시스템에 치명적 영향

✅ 예시   
<img src = "https://user-images.githubusercontent.com/92699723/258684011-5f4e8e37-fef6-4877-b74d-532eae33e425.jpg" width=40%>

---

<img src = "https://user-images.githubusercontent.com/92699723/258684292-7832d88b-64ae-433b-88dc-e9168d3d54ba.jpg" width=60%>

<img src = "https://user-images.githubusercontent.com/92699723/258684431-80008de7-1f22-467a-9221-3f80cfa946e3.jpg" width=60%>

> Semaphore처럼 lock을 걸지 않아도 된다는 점이 Monitor의 장점이다.

✅ 모니터 내에서는 한 번에 하나의 프로세스만이 활동 가능하다

✅ 프로그래머가 동기화 제약 조건을 명시적으로 코딩할 필요가 없다

✅ 프로세스가 모니터 안에서 기다릴 수 있도록 하기 위해 <u>**`condition variable`**</u> 사용
condition **`x, y`**;

✅ Condition variable은 **`wait`** 와 **`signal`** 연산에 의해서만 접근 가능   
**`x.wait();`**   
x.wait()을 invoke한 프로세스는 다른 프로세스가 x.signal()을 invoke하기 전까지 suspend된다.   

**`x.signal();`**   
x.signal()은 정확하게 하나의 <u>suspend</u>된 프로세스를 resume한다.   
Suspend된 프로세스가 없으면 아무 일도 일어나지 않는다.

<img src = "https://user-images.githubusercontent.com/92699723/258685382-0af1b0ba-2971-4be1-9a89-e83f64ca6505.jpg" width=60%>