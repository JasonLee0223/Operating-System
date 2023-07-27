# Process2

✅ "A thread (or lightweight process) is a basic unit of CPU utilization"   
(스레드(또는 경량 프로세스)는 CPU 사용률의 기본 단위입니다.)    

✅ Thread의 구성
- program counter (pc)
- register set
- stack space

✅ Thread가 동료 thread와 공유하는 부분 (=task)
- code section
- data section
- OS resources

✅ 전통적인 개념의 heavyweight process는 하나의 thread를 가지고 있는 task로 볼 수 있다.

> 프로세스 내부에 CPU 수행 단위가 여러개있는 경우를 Thread라고 부른다.

<img src = "https://user-images.githubusercontent.com/92699723/256440571-a46a8415-a3a0-4937-a7cb-0fc60a1645db.jpg" width=70%>

> '메모리의 어느 부분을 가리키고 있는가'를 나타내는 pc(program counter)   
> 프로세스가 하나 주어지면 프로세스만의 Code, Data, Stack 영역이 만들어지고 이런 PCB에서 현재 메모리의 어느 부분을 실행하고 있는지 PC가 가리카고 있다.    
> 어떤 동일한 일을 하는 프로세스가 여러개 있다고하면 또 다른 별도의 프로세스로 만들면 이러한 메모리 주소 공간이 여러개 만들어지고 프로세스마다 별도의 주소 공간이 만들어져서 메모리가 낭비된다.   
> 같은 일을 하는 프로세스를 여러개 띄워놓고싶다면 메모리 공간은 하나만 띄워놓고 각 프로세스마다 다른 코드 부분을 실행할 수 있게 해주는 것이 Thread의 개념이다.
> 위 그림처럼 PC의 주소값만 여러개를 두고 즉, 프로세스 하나에 CPU 수행 단위만 여러개 두고 있는 것을 Thread라고 합니다.
> CPU 수행을 위해서는 현재 코드의 어느 부분을 실행하고 있는지를 가리키는 PC가 있어야하며 
> 그 CPU가 실행되면서 메모리의 레지스터 값을 세팅해놓고 실행하고있을 것이며 각 Thread마다 현재 레지스터에 어떤 값을 넣고 PC가 코드 어디를 가리키고 실행가고 있었는지를 별도로 유지하고 있는다.

> `Thread`라는 것은 프로세스 하나에서 공유할 수 있는 건 최대한 공유하고   
> 즉, 메모리 주소공간을 공유하며 프로세스도 하나이기 때문에 프로세스 상태 또한 공유합니다.   
> 그래서 프로세스의 자원 또한 공유됩니다.
> 다만 별도로 가지고 있는 것은 CPU 수행과 관련된 정보입니다.   
> PC, Register, Stack 같은 것들을 말합니다.

---

✅ 다중 스레드로 구성된 Task 구조에서는 하나의 서버 Thread가 blocked (waiting) 상태인 동안에도   
동일한 Task 내의 다른 스레드가 실행(running)되어 빠른 처리를 할 수 있다.

✅ 동일한 일을 수행하는 다중 스레드가 협력하여 높은 처리율(throughput)과 성능 향상을 얻을 수 있다.

✅ Thread를 사용하면 병렬성을 높일 수 있다.