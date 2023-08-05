# Part1. Process Synchronization

## 데이터의 접근 및 Race Condition

<img src = "https://user-images.githubusercontent.com/92699723/258345773-2e1c6d75-ae74-4508-88c8-ac91b1f9a5fb.jpg" width=60%>

<img src = "https://user-images.githubusercontent.com/92699723/258347468-8f6c9f84-0535-468b-ad5f-ad956a9338ec.jpg" width=60%>

> 주체가 하나의 데이터에 동시에 접근하려고하는 경우를 `Race Condition`이라고한다.   
> 메모리보다 커널에서 발생하는 부분이 좀 더 중요하게 생각된다.   

---

## OS에서 race condition은 언제 발생하는가?

1. Kernel 수행 중 인터럽트 발생 시

2. Process가 system call을 하여 kernel mode로 수행중인데   
context switch가 일어나는 경우

3. Multiprocessor에서 shared memory 내의 kernel data

---

### 🚫 Kernel 수행 중 인터럽트 발생 시의 예시
<img src = "https://user-images.githubusercontent.com/92699723/258354705-d0f0af46-b7dd-4463-a450-4930a8474928.jpg" width=60%>

> +1 증가한 것만 수행한다. (도중에 인터럽트를 처리하게되면)   
> 중요한 값을 건드리는 동안에는 인터럽트가 들어와도 인터럽트 처리루틴으로 들어가는 것이 아닌   
> 해당 작업이 끝나기 전까지 disable시켰다가 인터럽트 처리루틴으로 넘어가도록하여 문제를 해결한다.

---

### 🚫 Kernel Mode 수행 중에 context switch가 일어나는 경우
<img src = "https://user-images.githubusercontent.com/92699723/258357847-051932b5-d015-4c79-8a22-a57ee0771e9f.jpg" width=60%>

<img src = "https://user-images.githubusercontent.com/92699723/258358226-482bded5-404b-48f3-9ff1-7f015f62ec2e.jpg" width=60%>

---

### 🚫 CPU가 여러개인 Multiprocessor인 상황
<img src = "https://user-images.githubusercontent.com/92699723/258359077-20e80e9d-9905-420c-a6f6-9123c79cd03f.jpg" width=60%>

> 자주 등장하는 경우는 아니지만 앞에서 해결하는 방법으로는 해결이 안되는 경우이다.   

---

## Process Synchronization 문제
✅ 공유 데이터(shared data)의 동시 접근(concurrent access)은 데이터의 불일치 문제(inconsistency)를 발생시킬 수 있다.   

✅ 일관성(consistency) 유지를 위해서는 협력 프로세스(cooperating process)간의   
실행 순서(orderly execution)를 정해주는 메커니즘 필요

✅ <u>**`Race Condition`**</u>
- 여러 프로세스들이 동시에 공유 데이터를 접근하는 상황
- 데이터의 최종 연산 결과는 마지막에 그 데이터를 다룬 프로세스에 따라 달라진다.

✅ race condition을 막기 위해서는 concurrent process는 동기화(synchronize)되어야한다.

<img src = "https://user-images.githubusercontent.com/92699723/258360490-09ad3227-63d4-4810-b559-59abde4a3910.jpg" width=60%>

---

## The Critical-Section Problem

✅ n 개의 프로세스가 공유 데이터를 동시에 사용하기를 원하는 경우

✅ 각 프로세스의 code segment에는 공유 데이터를 접근하는 코드인 <u>**`critical section`**</u>이 존재

✅ 문제점
- 하나의 프로세스가 **`critical section`** 에 있을 때   
다른 모든 프로세스는 critical section에는 들어갈 수 없어야 한다.

> 공유 데이터에 접근하는 코드를 critical section이라고한다.

<img src = "https://user-images.githubusercontent.com/92699723/258361620-90f43b6f-20d9-4cbf-8aeb-ee76bcf5c0ad.jpg" width = 60%>

---

## Initial Attempts to Solve Problem

✅ 두 개의 프로세스가 있다고 가정 P0, P1
✅ 프로세스들의 일반적인 구조

```C
do {
    entry section
    critical section
    exit section
    remainder section
}while(1)
```

✅ 프로세스들은 수행의 동기화(synchronize)를 위해 몇몇 변수를 공유할 수 있다. -> synchronization variable

---

## 프로그램적 해결법의 충족 조건

✅ <u>**`Mutual Exclusion (상호 배제)`**</u>
- 프로세스 Pi가 critical section 부분을 수행 중이면 다른 모든 프로세스들은   
그들의 critical section에 들어가면 안된다.

✅ <u>**`Progress`**</u>
- 아무도 critical section에 있지 않은 상태에서 critical section에 들어가고자 하는 프로세스가 있으면   
critical section에 들어가게 해주어야 한다.

✅ <u>**`Bounded Waiting (유한 대기)`**</u>
- 프로세스가 critical section에 들어가려고 요청한 후부터 그 요청이 허용될 때까지 다른 프로세스들이   
critical section에 들어가는 횟수에 한계가 있어야 한다. (기아 현상 발생하지 않도록 한다.)

> 예시로 critical section에 들어가고자 하는 프로세스가 3개 있는데 2개만 서로 번갈아가며 수행하고    
> 1개는 critical section에 진입하지 못하는 경우에 사용된다.

❓ 가정
- 모든 프로세스의 수행 속도는 0보다 크다
- 프로세스들 간의 상대적인 수행 속도는 가정하지 않는다.

--- 

> 소프트웨어적으로 lock을 어떻게 하면 풀 수 있는지 3가지의 알고리즘에 대해 알아봅니다.

## Algorithm 1
> Progress 조건에 만족하지 못하기 때문에 critical section 문제를 잘 풀지 못한 알고리즘이다.

✅ Synchronization variable

```C
int turn;
initially turn = 0;  => Pi can enter its critical section if (turn == i)
```

✅ Process P0

```C
do {
    while(turn != 0);   /* My turn? */
    critical section
    turn = 1;           /* Now it's your turn*/
    remainder section
}while(1);
```

Satisfies mutual exclusion, but not progress   
즉, 과잉양보: 반드시 한 번씩 교대로 들어가야만 한다. (swap-turn)   
그가 turn을 내 값으로 바꿔줘야만 내가 들어갈 수 있음   
특정 프로세스가 더 빈번히 critical section을 들어가야 한다면??

--- 

## Algorithm 2

> 프로세스마다 각각의 자신의 flag를 가지고 있으며    
> 본인이 critical section에 들어가고자 한다는 의중을 표시하는 것이다.

✅ Synchronization variable

- boolean flag[2];   
initial flag[모두] = false;   /* no one is in CS */
- "Pi ready to enter its critical section" if(flag[i] = true)

✅ Process Pi

```C
do {
    flag[i] = true;     /* Pretend I am in */
    while(flag[i]);     /* Is he also in? then wait */
    critical section
    flag[i] = false;    /* I am out now */
    remainder section
}while(1);
```

✅ Satisfies mutual exclusion, but not **`progress requirement`**.   
✅ 둘 다 2행까지 수행 후 끊임 없이 양보하는 상황 발생 가능

---

## Algorithm 3 (Peterson's Algorithm)

> 위의 turn value와 flag value를 모두 사용한 기법
> 3가지 경우 모두 충족하는 알고리즘

✅ Combined synchronization variables of algorithms 1 and 2.

✅ Process Pi

```C
do {
    flag[i] = true;                 /* My intention is to enter... */
    turn = j;                       /* Set to his turn */
    while(flag[j] && turn == j);    /* wait only if...*/
    critical section
    flag[i] = false;
    remainder section
}while(1);
```

✅ **`Meets all three requirements`**   
solves the critical section problem for two processes.

✅ **`💥 Busy Waiting (=spin lock)!`** (계속 CPU와 memory를 쓰면서 wait)

---

## Synchronization Hardware

✅ 하드웨어적으로 **`Test & modify`** 를 <u>`atomic`</u>하게 수행할 수 있도록   
지원하는 경우 앞의 문제는 간단히 해결

<img src = "https://user-images.githubusercontent.com/92699723/258565386-5eca95ac-ed23-4971-80e2-fba10342d4c0.jpg" width = 30%>

✅ Mutual Exclusion with Test & Set
```C
Synchronization variable;
    boolean lock = false;

Process Pi

do {
    while(Test_and_Set(lock));
    critical section
    lock = false;
    remainder section
}
```

---

