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

- 1.4 에서의 스케쥴링은 **nonpreemptive (=강제로 빼앗지 않고 자진 반납)**
- All other scheduling is **preemptive (=강제로 빼앗음)**