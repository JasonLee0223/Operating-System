# Process Management - 2

> CoW (Copy-on-Write) : Write가 발생했을 떄 그 때 Copy를 실행하겠다는 의미.   
> Write가 발생한다는 것은 원래 있던 내용이 바뀌는 경우를 말하며   
> Write가 발생하기 이전까지는 부모 프로세스의 내용을 공유하게된다는 것이다.   
> 통째로 Copy하는 것이 아닌 Code, Data, Stack에서 필요한 부분만 물리적인 메모리에 올라가기 때문에   
> 이렇게 부분적인 내용만 Copy되어 부모것과 공유하지않는다는 의미   
> 가능하면 공유할 수 있으면 공유해서 사용하는게 효율적이지만 원칙은 별개의 프로세스이기 때문에 독립적으로 가지고 있는것이 원칙이다.   

> 부모 프로세스가 자식 프로세스르 복제 생성하는 데 프로세스를 만드는 일은 무거운 일이기 때문에    
> 사용자 프로세스인 부모 프로세스가 직접 하는 것이 아니라 운영체제에게 자식 프로세스를 만들어달라고 시스템콜을 요청한다.   
> 새로운 프로세스를 만드는 시스템콜이 `fork()` 시스템 콜이다.
> 일단 복제 생성을 한 다음에 새로운 프로그램을 덮어 씌울 수 있는데 해당 시스템콜이 `exec()`이다.

---

## 프로세스와 관련한 시스템 콜

✅ fork() - create a child (copy)

✅ exec() - overlay new image

✅ wait() - sleep until child is done
 
✅ exit() - frees all the resources, notify parent

---

## fork() 시스템 콜

✅ A process is created by the `fork()` system call.   
- creates a new address space that is a duplicate of the caller.

프로세스는 fork() 시스템콜에 의해 생성됩니다.   
- Caller와 중복되는 새 주소 공간을 만듭니다.   

<img src = "https://user-images.githubusercontent.com/92699723/256730164-beb15a9b-0aec-4a9f-8268-47aefe787807.jpg" width = 50%>

> 복제를 해놓았는데 내가 부모(원본)이라고 하는 문제점이 발생할 수 있다.   
> **부모와 동일한 것이 생성되다보니 자식하고 구분이 되어야해서 `fork() 함수의 결과값(return value)을 다르게 가져간다.`**   
> fork를 실행하고나면 부모 프로세스는 결과값이 양수(+) 즉, 자식 프로세스의 PID가 얻어진다.   
> 자식 프로세스의 결과값으로 '0'을 받게된다.

---

## exec() 시스템 콜

✅ A process can execute a different program by the `exec()`system call.
- replace the memory image of the caller with a new program.

프로세스는 `exec()` 시스템 호출에 의해 다른 프로그램을 실행할 수 있습니다.
- 호출자의 메모리 이미지를 새 프로그램으로 교체합니다.

<img src = "https://user-images.githubusercontent.com/92699723/256731958-83f7a9a7-b5e2-4fad-8141-8f10957f06f4.jpg" width = 50%>

> 어떤 프로그램을 완전히 새로운 프로세스로 태어나게하는 그런 역할을 해준다.   
> exec() 함수는 꼭 자식을 만들어서 하는 것은 아니며 fork() 함수 호출이 없이도 구현할 수 있다.   
> 위와 같이 수행한다면 exec 이후에 나와있는 코드는 실행이 안된다.

---

## wait() 시스템 콜

✅ 프로세스 A가 wait() 시스템 콜을 호출하면
- 커널은 child가 종료될 떄까지 프로세스 A를 `sleep` 시킨다. (`block` 상태)
- `Child process`가 종료되면 커널은 프로세스 A를 깨운다. (`ready` 상태)

<img src = "https://user-images.githubusercontent.com/92699723/256733869-ee2beff6-0633-40d9-91a0-a2112a04d426.jpg" width = 60% >

> 자식 프로세스가 종료되면 부모 프로세스가 `block`되어서 `ready`상태로 변경되어 CPU의 소유권을 얻을 수 있게된다.   
> 자기 자식 프로세스가 종료될 때까지 기다리는 시스템 콜이다.
> 흔한 예로 타이핑의 입력을 기다렸다가 입력을 마치는 경우도 부모 & 자식 프로세스로도 볼 수 있다.

---

## exit() 시스템 콜

✅ 프로세스의 종료
- 자발적 종료
  - 마지막 statement 수행 후 exit() 시스템 콜을 통해
  - 프로그램에 명시적으로 적어주지 않아도 main 함수가 리턴되는 위치에 컴파일러가 넣어준다.

- 비자발적 종료
  - 부모 프로세스가 자식 프로세스를 강제 종료시킴
    - 자식 프로세스가 한계치를 넘어서는 자원 요청
    - 자식에게 할당된 Task가 더 이상 필요하지 않음
  - 키보드로 kill, break 등을 친 경우
  - 부모가 종료하는 경우
    - 부모 프로세스가 종료하기 전에 자식들이 먼저 종료됨

---

## 프로세스 간 협력

✅ 독립적 프로세스 (Independent process)
- 프로세스는 각자의 주소 공간을 가지고 수행되므로 원칙적으로 하나의 프로세스는 다른 프로세스의 수행에 영향을 미치지 못함

✅ 협력 프로세스 (Cooperating process)
- 프로세스 협력 메커니즘을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있음

✅ 프로세스 간 협력 매커니즘(`IPC`: Interprocess Communication)
- 메시지를 전달하는 방법
  - `message passing`: <u>커널</u>을 통해 메시지 전달

- 주소 공간을 공유하는 방법
  - `shared memory`: 서로 다른 프로세스 간에도 일부 주소 공간을 공유하게 하는 shared memory 메커니즘이 있음
  - `🔥 Thread`: `thread`는 사실상 하나의 프로세스이므로 프로세스 간 협력으로 보기는 어렵지만 동일한 process를 구성하는   thread들 간에는 주소 공간을 공유하므로 협력이 가능

---

### Message Passing

<img src = "https://user-images.githubusercontent.com/92699723/256751605-1c28deaf-cb09-4ff9-bc62-5d3599675aa8.jpg" width = 50%>

✅ Message system
- 프로세스 사이에 공유 변수(shared variable)를 일체 사용하지않고 통신하는 시스템
- 어떤 방식을 채택하더라도 결국 커널을 사용하고 인터페이스 측면에서 직접 또는 간접으로 전달된다.

✅ Direct Communication
- 통신하려는 프로세스의 이름을 **명시적으로 표시**

✅ Indirect Communication
- `mailbox` (또는 `port`)를 통해 메시지를 간접 전달

<img src = "https://user-images.githubusercontent.com/92699723/256752222-e6eba578-174a-4ef9-892d-b3152303c3f0.jpg" width = 50%>