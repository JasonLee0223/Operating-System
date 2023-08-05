# Part2. Process Synchronization

## 🔥 Semaphores

✅ 앞의 방식들을 추상화시킴

✅ **Semaphore** **`S`**

- integer variable
- 아래의 두 가지 atomic 연산에 의해서만 접근 가능

<img src = "https://user-images.githubusercontent.com/92699723/258566001-41d7eb09-6b30-4b2b-ae4e-e6db8563d51a.jpg" width=40%>

> 세마포어는 연산식을 추상화해놓은 것으로 보면 된다.   
> 여기서도 `busy wait` 상태가 발생한다.   

---

## Critical Section of n Processes

✅ <u>Synchronization variable</u>
```C
semaphore mutex;    /* initially 1: 1개가 CS에 들어갈 수 있다. */
```

✅ <u>Process Pi</u>
```C
do {
    P(mutex);       /* If positive, dec-&-enter, Otherwise, wait. */
    critical section
    V(mutex);       /* Increment semaphore */
    remainder section
}while(1);
```

busy-wait은 효율적이지 못하다. (=spin lock)   
Block & Wakeup 방식의 구현 (= sleep lock) (다음 장)

> 프로그래머가 일일이 앞서 보았던 알고리즘을 그때마다 코딩하는 것이 아니라   
> `Semaphore`라는 추상적 자료형을 사용하여 프로그래머는 semaphore를 사용하여 간단하게 구현할 수 있다는 장점.   

---

## Block / Wakeup Implementation

✅ Semaphore를 다음과 같이 정의

```C
typedef struct {
    int value;              /* Semaphore */
    struct process *L;      /* Process wait queue */
}semaphore;
```

✅ block과 wakeup을 다음과 같이 가정
- **`block`** - 커널은 block을 호출한 프로세스를 suspend시킨다.   
이 프로세스의 PCB를 semaphore에 대한 wait queue에 넣는다.

- **`wakeup(P)`** - block된 프로세스 P를 wakeup 시킨다.   
이 프로세스의 PCB를 ready queue로 옮긴다.

<img src = "https://user-images.githubusercontent.com/92699723/258566516-cad9f524-9a0f-4ffb-9d9c-c25abee7f1d8.jpg" width = 50%>

<img src = "https://user-images.githubusercontent.com/92699723/258566591-cd13a6ec-79a8-49ba-b117-c8aede4eca0e.jpg" width=60%>

---

## Which is better?

✅ Busy-wait vs Block/wakeup

✅ Block/wakeup overhead vs Critical section 길이
- Critical section의 길이가 긴 경우 Block/Wakeup이 적당
- Critical section의 길이가 매우 짧은 경우   
Block/Wakeup 오버헤드가 busy-wait 오버헤드보다 더 커질 수 있다.
- 일반적으로는 Block/wakeup 방식이 더 좋다.

---

## Two Types of Semaphores

✅ <u>**Counting semaphore**</u>
- 도메인이 0 이상인 임의의 정수값
- 주로 resource counting에 사용

✅ <u>**Binary semaphore**</u>(=mutex)
- 0 또는 1 값만 가질 수 있는 semaphore
- 주로 mutual exclusion (lock/unlock)에 사용

---

> Semaphore를 사용하게 되면 예기치 않은 상황이 발생할 수 있다.

## Deadlock and Starvation

✅ <u>**`Deadlock`**</u>
- 둘 이상의 프로세스가 서로 상대방에 의해 충족될 수 있는 event를 무한히 기다리는 현상

✅ S와 Q가 1로 초기화된 semaphore라 하자.

<img src = "https://user-images.githubusercontent.com/92699723/258567085-61ca5307-9342-42bd-95da-0f3d77522459.jpg" width=40%>

✅ <u>**`Starvation`**</u>
- **`indefinite blocking`**   
프로세스가 **suspend**된 이유에 해당하는 Semaphore Queue에서 빠져나갈 수 없는 현상

---