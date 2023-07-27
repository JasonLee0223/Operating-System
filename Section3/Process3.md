# Process3

<img src = "https://user-images.githubusercontent.com/92699723/256531106-f02d68bc-a396-4a3a-9660-297804d2ae3a.jpg" width=70%>

> 프로세스는 하나이기 때문에 PCB는 하나만 만들어지게된다.   
> 그런데 이 프로세스안에 Thread가 여러개 있다면 CPU 수행과 관련된 정보만 각각 Thread마다 별도의 복사본을 가지게된다.   

## Thread의 장점 👍🏻 
✅ Responsiveness (응답성)
- eg) multi-threaded Web 
  - if one thread is blocked (eg network)
  - another thread continues (eg display)

> 사용자 입장에서 뻐르게 느껴진다.    
> (Thread를 여러개 가지고있다면 무거운 작업을 진행할 때 다른 일을 병렬처리할 수 있음으로)

✅ Resource Sharing (자원 공유)
- N threads can share binary code, data, resource of the process

✅ Economy (빠르다는 의미.. 비용 절감의 느낌 - overhead의 관점)
- <u>creating</u> & <u>CPU switching</u> `thread` (rather than a `process`)
- Solaris의 경우 위 두 가지 overhead가 각각 30배, 5배

> `문맥 교환`은 하나의 프로세스로부터 또 다른 프로세스로 CPU가 넘어가는 행위는 overhead가 크다.   
> 프로세스를 하나 생성 또는 CPU를 스위치하는 것보다 Thread 하나를 생성하고 스위치하는것이 훨씬 overhead가 적다.
> 가능하면 동일한 작업을 한다면 Thread를 사용하는 것이 효율적이다.

✅ Utilization of MP Architectures (Multi Processor의 경우)
- <u>each thread</u> may be running in `parallel` on a <u>different processor</u>
 
---

### Thread를 구현하는 방법
✅ Some are supported by `kernel` -> `Kernel Threads`
- Windows 95/98/NT
- Solaris
- Digital UNIX, Mach

✅ Others are supported by `library` -> `User Threads`
- POSIX P-threads
- Mach C-threads
- Solaris threads

✅ Some are real-time threads