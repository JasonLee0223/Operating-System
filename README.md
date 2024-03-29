# Operating-System
운영체제 - 이화여대 반효경 교수님 2014년 1학기 강의 정리

|Section|Title|Subject|
|:---|:---|:---|
|01|[Introduction to Operating Systems](IntroductionToOperatingSystems.md)|운영체제란 무엇인가? <br/>운영체제의 목적, 분류, 구조|
|02|[System Structure & Program Execution 1](Section2/SystemStructure_ProgramExecution.md)|Mode bit, Timer <br/>Device Controller <br/>입출력(I/O)의 수행 <br/>`Interrupt`<br/>`System call`|
||[System Structure & Program Execution 2](Section2/SystemStructure_ProgramExecution2.md)|동기, 비동기식 입출력<br/>DMA|
|03|[🔥 Process 1](Section3/Process1.md)|프로세스의 개념, 문맥, 상태 <br/>PCB, Context Switch, Scheduler|
||[🔥 Process 2](Section3/Process2.md)|Thread에 대한 정의|
||[Process 3](Section3/Process3.md)|Thread의 장점|
|04|[Process Management 1](Section4/Process_Management.md)|프로세스의 생성 및 종료|
||[🔥Process Manangement 2](Section4/Process_Management2.md)|프로세스의 시스템콜(fork, exec, wait, exit) <br/>프로세스의 협력(message)|
|05|[CPU Scheduling 1](Section5/CPU_Scheduling.md)|FSFS, SJF, Round Robin(RR)|
||[CPU Scheduling 2](Section5/CPU_Scheduling2.md)|Multilevel Queue|
|06|[Process Synchronization 1](Section6/Process_Synchronization.md)|Race Condition <br/> lock & unlcok SW적으로 해결하는 방법|
||[Process Synchronization 2](Section6/Process_Synchronization2.md)|Semaphore <br/> Deadlock 소개|
||[Process Synchronization 3](Section6/Process_Synchronization3.md)|Bounded-Buffer Problem <br/> Readers and Writers Problem <br/> Dining-Philosophers Problem <br/> Semaphore, Monitor|
|07|[Deadlocks](Section7/Deadlock.md)|Deadlock 발생의 4가지 조건 <br/> Deadlock의 처리 방법|
|08|[Memory Management 1](Section9/Memory_Management.md)|가상 vs 물리적 주소 <br/> 주소 바인딩 <br/> MMU <br/> Dynamic Loading, Dynamic Linking <br/> Overlays, Swapping|
||[Memory Management 2](Section9/Memory_Management2.md)|Paging 기법, Page Table <br/> Two-Level Page Table|
||[Memory Management 3](Section9/Memory_Management3.md)|Multilevel-Paging 기법 <br/> Inverted page table, Shared Page<br/> Segmentation 기법|
|09|Virtual Memory 1||
||Virtual Memory 2||
|10|File Systems||
||File Systems Implementation 1||
||File Systems Implementation 2||
|11|Disk Management and Scheduling 1||
||Disk Management and Scheduling 2||
