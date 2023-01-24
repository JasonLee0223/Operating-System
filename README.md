# Operating-System
운영체제 - 이화여대 반효경 교수님 2014년 1학기 강의 정리

|Section|Title|Subject|
|:---|:---|:---|
|01|Introduction to Operating Systems|운영체제란 무엇인가, 운영체제의 목적, 운영체제의 분류, 운영체제의 예, <br/>운영체제의 구조|
|02|System Structure & Program Execution 1|컴퓨터 시스템 구조, Mode bit, Timer, Device Controller, <br/>입출력(I/O)의 수행, 동기식 입출력과 비동기식 입출력, <br/>시스템콜(System Call), 인터럽트(Interrupt)|
||System Structure & Program Execution 2|컴퓨터 시스템 구조, 인터럽트(Interrupt), 동기식 입출력과 비동기식 입출력 <br/>시스템콜(SystemCall), DMA(Direct Memory Access), 서로 다른 입출력 명령어 <br/>저장장치 계층 구조, 프로그램의 실행(메모리 load), 커널 주소 공간의 내용 <br/>사용자 프로그램이 사용하는 함수, 프로그램의 실행|
|03|Process 1|프로세스의 개념, 프로세스의 상태(Process State), 프로세스의 개념, 프로세스 상태도, Process Control Block(PCB), 문맥교환(Context Switch), 프로세스를 스케줄링하기 위한 큐, Ready Queue와 다양한 Device Queue, 스케줄러(Scheduler)|
||Process 2|동기식 입출력과 비동기식 입출력, 프로세스 스케줄링 큐의 모습, Thread|
||Process 3|Thread, Single and Multithreaded Processes, Benefits of Threads, Implemetation of Threads|
|04|Process Management 1|프로세스 생성(Process Creation), 프로세스 종료(Process Termination)|
||Process Manangement 2|프로세스 생성(Process Creation), 프로세스와 관련한 시스템콜, 프로세스 간 협력, Message Passing, <br/>Interprocess communication, CPU and I/O Bursts in Program Execution, <br/>CPU-burst Time의 분포, 프로세스의 특성 분류, CPU Scheduler & Dispatcher|
|05|CPU Scheduling 1|CPU and I/O Bursts in Program Execution, CPU-burst Time의 분포, CPU Scheduler & Dispatcher, <br/>Scheduling Algorithms, Scheduling Criteria, FCFS(First- Come First-Served), <br/>SJF(Shortest-Job-First), 다음 CPU Burst Time의 예측, Exponential Averaging, Priority Scheduling, <br/>Round Robin(RR)|
||CPU Scheduling 2 <br/>Process Synchronization 1|CPU-burst Time의 분포, Schedulling Algorithms, Round Robin(RR), Multilevel Queue, <br/>Multilevel Feedback Queue, Multi-Processor Scheduling, Real-time Scheduling, <br/>Thread Scheduling, Algorithm Evaluation|
|06|Process Synchronization 1|데이터의 접근, Race Condition, OS에서의 race condition(3/3), The Critical-Section Problem, <br/>OS에서 race condition(1/3), If you preempt CPU while in kernel mode…, <br/>Initial Attempts to Solve Problem, 프로그램적 해결법의 충족조건, Algorithm 1, Algorithm2, <br/>Algorithm3(Peterson's Algorithm), Synchronization Hardware, Semaphores|
||Process Synchronization 2|Semaphores, Critical Section of n Processes, Block / Wakeup Implementation, Implementation, <br/>Two Types of Semaphores, Deadlock and Starvation, Dining-Philosophers Problem|
||Process Synchronization 3|Semaphores, Implementation, Classical Problems of Syncronization, Bounded-Buffer Problem, <br/>Readers-Writers Problem, Dining-Philosophers Problem, Monitor
||Process Synchronization 4 (Concurrency Control)|Semaphores, Monitor, Bounded-Buffer Problem, Dining Philosophers Example|
|07|Deadlocks 1|교착상태(deadlock), The Deadlock Problem, Deadlock 발생의 4가지 조건, <br/>Resource-Allocation Graph(자원할당그래프), Deadlock Prevention, Deadlock의 처리 방법, <br/>Deadlock Avoidance, Resource Allocation Graph algorithm, Banker's Algorithm|
||Deadlocks 2|Deadlock의 처리 방법, Deadlock Avoidance, Deadlock Detection and Recovery, <br/>Deadlock Ignorance|
|08|Memory Management 1|Logical vs. Physical Address, 주소바인딩(Address Binding), Memory-Management Unit(MMU), <br/>Dynamic Relocation, Hadware Support for Address Translation, Some Treminologies, <br/>Dynamic Loading, Overlays, Swapping, Dynamic Linking, Allocation of Physical Memory, <br/>Contiguous Allocation, Paging|
||Memory Management 2|Paging, Dynamic Relocation, Paging Example, Address Translation Architecture, <br/>Implementation of Page Table, Paging Hardware with TLB, Associative Register, <br/>Effective Access Time, Two-Level Page Table, Address-Translation Scheme, <br/>Two-Level Paging Example|
||Memory Management 3|Multilevel Paging and Performance, Two-Level Page Table, Valid (v)/ Invalid (i) Bit in a Page Table, <br/>Memory Protection, Inverted Page Table, Inverted Page Table Architecture, Shared Page, <br/>Shared Pages Example, Segmentation, Segmentation Architecture, Segmentataion Hardware|
||Memory Management 4|Segmentation, Segmentation Hardware, Segmentation Architecture, Example of Segmetation, <br/> Segmentation Architecture(Cont.), Sharing of Segments, Segmentation with Paging, <br/>Address Translation Architecture|
|09|Virtual Memory 1|Demand Paging, Memory에 없는 Page의 Page Table, Page Fault, Steps in Handling a Page Fault, <Br/>Performance of Demand Paging, Free Frame이 없는 경우, Page Replacement, Optimal Algorithm, <br/>FIFO(First In First Out) Algorithm, LRU(Least Recently Used) Algorithm, <br/>LFU(Least Frequently Used) Algorithm, LRU와 LFU 알고리즘 예제, LRU와 LFU 알고리즘의 구현, <br/>다양한 캐슁 환경|
||Virtual Memory 2|다양한 캐슁 환경, LRU와 LFU 알고리즘의 구현, Paging System에서 LRU, LFU 가능한가?, Clock Algorithm, <br/>Page Frame의 Allocation, Global vs. Local Replacement, Thrashing, Thrashing Diagram, <br/>Working-Set Model, Working-Set Algorithm, PFF(Page-Fault Frequency) Scheme, <br/>Page Size의 결정|
|10|File Systems|File and File System, Directory and Logical Disk, open( ), File Protection, File System의 Mounting,<br/> Access Methods|
||File Systems Implementation 1|Allocation of File Data in Disk, Contiguous Allocation, Linked Allocation, Indexed Allocation, <br/>UNIX 파일시스템의 구조, FAT File System, Free-Space Management, Directory Implementation, <br/>VFS and NFS, Page Cache and Buffer Cache|
||File Systems Implementation 2|Page Cache and Buffer Cache, 프로그램의 실행|
|11|Disk Management and Scheduling 1|Disk Structure, Disk Scheduling, Disk Management, Disk Scheduling Algorithm, <br/>FCFS(First Come First Service), SSTF(Shortest Seek Time First), SCAN, C-SCAN, <br/>Other Algorithms, Disk-Scheduling Algorithm의 결정|
||Disk Management and Scheduling 2|Swap-Space Management, RAID|
