# Part1. Memory Management

## Logical vs Physical Address
✅ <u>**`Logical address`**</u> (=virtual address)
- 프로세스마다 독립적으로 가지는 주소 공간
- 각 프로세스마다 0번지부터 시작
- <u>**`CPU가 보는 주소는 logical address이다 ❗️`**</u>

✅ <u>**`Physical address`**</u>
- 메모리에 실제 올라가는 위치

💥 주소 바인딩: 주소를 결정하는 것   
Symbolic Address -> Logical Address ->(이 시점이 언제인가?) -> Physical address

> 프로그램은 메모리 주소를 사용하지만 프로그래머의 입장에서는 숫자가 아닌 Symbol로된 address를 사용하고   
> 이것을 컴파일하게되면 숫자로 된 주소로 변경되겠다.

---

## 주소 바인딩 (Address Binding)

✅ <u>**`Compile time binding`**</u>
- 물리적 메모리 주소(physical address)가 컴파일 시 알려진다.
- 시작 위치 변경 시 재컴파일
- 컴파일러는 절대 코드(**`absolute code`**)생성

✅ <u>**`Load time binding`**</u>
> 실행이 시작될 때 주소 변환이 이루어짐
- Loader의 책임하에 물리적 메모리 주소 부여
- 컴파일러가 재배치 가능 코드 (relocatable code)를 생성한 경우 가능

✅ <u>**`Execution time binding (=Run time binding)`**</u>
- 수행이 시작된 이후에도 프로세스의 메모리 상 위치를 옮길 수 있음
- CPU가 주소를 참조할 때마다 binding을 점검 (address mapping table)
- <u>**`하드웨어적인 지원이 필요`**</u>
  - e.g. base and limit registers, MMU

<img src = "https://user-images.githubusercontent.com/92699723/258737480-46051abe-90a3-430c-9f24-5cddaf9673cb.jpg" width=60%>

> 현재는 Compile time binding은 잘 사용하지 않는다.   
> Load Time binding은 500번지부터 물리적 메모리 주소가 비어있으니 Load 되었을 때 올린다.
> 실행 도중에 바뀌는 Run time binding은 물리적 메모리 주소를 변동이 가능하다는 점이다.   

---

## 🔥 Memory - Management Unit (MMU)
✅ <u>**`MMU (Memory Management Unit)`**</u>
- logical address를 physical address로 매핑해주는 Hardware device
  
✅ MMU Scheme
- 사용자 프로세스가 CPU에서 수행되며 생성해내는 모든 주소값에 대해   
`base register(=relocation register)`의 값을 더한다.

✅ user program
- `logical address`만을 다룬다
- 실제 `physical address`를 볼 수 없으며 알 필요가 없다

---

<img src = "https://user-images.githubusercontent.com/92699723/258740601-50030dc8-f76d-4d85-be8e-fb367bbae33b.jpg" width=60%>

## Hardware Support for Address Translation

<img src = "https://user-images.githubusercontent.com/92699723/258741923-0b787cc3-3e6e-43da-ac65-cdd848caba2a.jpg" width=60%>

운영체제 및 사용자 프로세스 간의 메모리 보호를 위해 사용하는 레지스터
- **`Relocation register (=base register)`**   
접근할 수 있는 물리적 메모리 주소의 최소값
- **`Limit register`**   
논리적 주소의 범위

---

## Some Terminologies

✅ <u>**`Dynamic Loading`**</u>

✅ <u>**`Dynamic Linking`**</u>

✅ <u>**`Overlays`**</u>

✅ <u>**`Swapping`**</u>

---

## Dynamic Loading

> 운영체제가 지원해주는 것이 아닌 프로그래머가 직접 하도록 만드는 개념이다.

✅ 프로세스 전체를 메모리에 미리 다 올리는 것이 아니라 해당 루틴이 불려질 때 메모리에 `load`하는 것

✅ `memory utilization`의 향상

✅ 가끔씩 사용되는 많은 양의 코드의 경우 유용
- 예: 오류 처리 루틴

✅ 운영체제의 특별한 지원 없이 프로그램 자체에서 구현 가능 (OS는 라이브러리를 통해 지원 가능)

💥 `Loading`: 메모리에 올리는 것

---

## Overlays

> 초창기 컴퓨터 환경에서 프로그래머가 메모리에 올려서 실행시킬 때는 쪼개서 수작업으로 진행했었다.   
> 하여 OS가 지원되지 않는다.

✅ 메모리에 프로세스의 부분 중 실제 필요한 정보만을 올린다.

✅ 프로세스의 크기가 메모리보다 클 때 유용

✅ 운영체제의 지원없이 사용자에 의해 구현

✅ 작은 공간의 메모리를 사용하던 초창기 시스템에서 수작업으로 프로그래머가 구현
- `"Manual Overlay"`
- 프로그래밍이 매우 복잡

---

## Swapping

> 하드디스크로 쫓아낸다.   
> 

✅ <u>**`Swapping`**</u>
- 프로세스를 일시적으로 메모리에서 `backing store`로 쫓아내는 것

✅ <u>**`Backing store(=swap area)`**</u>
- 디스크
  - 많은 사용자의 프로세스 이미지를 담을 만큼 충분히 빠르고 큰 저장 공간

✅ <u>**`Swap in / Swap out`**</u>
- 일반적으로 중기 스케쥴러(swapper)에 의해 swap out시킬 프로세스 선정
- priority-based CPU scheduling algorithm
  - priority가 낮은 프로세스를 swapped out 시킴
  - priority가 높은 프로세스를 메모리에 올려 놓음
- Compile time 혹은 load time binding에서는 원래 메모리 위치로 swap in 해야한다.
- Execution time binding에서는 추후 빈 메모리 영역 아무 곳에나 올릴 수 있음
- swap time은 대부분 transfer time (swap되는 양에 비례하는 시간)임

<img src = "https://user-images.githubusercontent.com/92699723/258777333-c634bdbe-58a8-4b7c-8d84-ba077b4954fa.jpg" width=60%>

---

## Dynamic Linking

✅ Linking을 실행 시간(execution time)까지 미루는 기법

✅ Static linking
- 라이브러리가 프로그램의 실행 파일 코드에 포함됨
- 실행 파일의 크기가 커짐
- 동일한 라이브러리를 각각의 프로세스가 메모리에 올리므로 메모리 낭비   
(eg. printf 함수의 라이브러리 코드)

✅ Dynamic linking
- 라이브러리가 실행 시 연결(link)됨
- 라이브러리 호출 부분에 라이브러리 루틴의 위치를 찾기 위한 `stub`이라는 작은 코드를 둠
- 라이브러리가 이미 메모리에 있으면 그 루틴의 주소로 가고 없으면 디스크에서 읽어옴
- 운영체제의 도움이 필요

---

> 아래 항목부터는 물리적인 메모리를 어떻게 관리할 것인가에 대한 내용

## Allocation of Physical Memory

✅ 메모리는 일반적으로 두 영역으로 나뉘어 사용된다

⎯⎯⎯⎯⎯⎯   
| OS 상주 영역 |   
⎯⎯⎯⎯⎯⎯⎯⎯⎯    
| 사용자 프로세스 영역 |   
⎯⎯⎯⎯⎯⎯⎯⎯⎯

- <u>**OS 상주 영역**</u>
  - interrupt vector와 함게 낮은 주소 영역 사용
- <u>**사용자 프로세스 영역**</u>
  - 높은 주소 영역 사용 

✅ 사용자 프로세스 영역의 할당 방법
- <u>**`Contiguous allocation (연속 할당)`**</u>
  - 각각의 프로세스가 메모리의 연속적인 공간에 적재되도록 하는 것
  - `Fixed partition allocation`
  - `Variable partition allocation`

- <u>**`Noncontiguous allocation (비연속 할당)`**</u> - 현대 컴퓨터 환경에서 사용
  - 하나의 프로세스가 메모리의 여러 영역에 분산되어 올라갈 수 있음
  - `Paging`
  - `Segmentation`
  - `Paged Segmentation`

---

## Contiguous Allocation

✅ <u>**`Contiguous allocation`**</u>
- **`고정 분할(Fixed partition) 방식`**
  - 물리적 메모리를 몇 개의 영구적 분할(partition)로 나눔
  - 분할의 크기가 모두 동일한 방식과 서로 다른 방식이 존재
  - 분할당 하나의 프로그램 적재
  - 융통성이 없음
    - 동시에 메모리에 load되는 프로그램의 수가 고정됨
    - 최대 수행 가능 프로그램 크기 제한
  - Internal fragmentation 발생 (external fragmentation도 발생)

💥 Internal fragmentation: 프로그램의 크기가 분할 3번의 크기보다 작지만 프로그램을 넣었지만    
안에서 남는 메모리 조각, 프로그램B한테 할당이 되었지만 사용은 안되는 공간

- **`가변 분할(Variable partition) 방식`**
  - 프로그램의 크기를 고려해서 할당
  - 분할의 크기, 개수가 동적으로 변함
  - 기술적 관리 기법 필요
  - External fragmentation 발생

💥 External fragmentation: 프로그램이 들어갈 수 있는 공간임에도 불구하고 조각이 너무 작아서 사용이 안되는 경우

<img src = "https://user-images.githubusercontent.com/92699723/258783947-5b766ace-f181-45e1-a483-36bdf86a0e2c.jpg" width=60%>

✅ <u>**`Hole`**</u>

- 가용 메모리 공간
- 다양한 크기의 hole들이 메모리 여러 곳에 흩어져 있음
- 프로세스가 도착하면 수용 가능한 hole을 할당
- 운영체제는 다음의 정보를 유지   
    a) 할당 공간 b) 가용 공간 (hole)

<img src = "https://user-images.githubusercontent.com/92699723/258785613-5958d51b-fa43-4b35-b9c7-71ac957cee79.jpg" width=60%>

✅ <u>**`Dynamic Storage-Allocation Problem`**</u>   
: **가변 분할 방식에서 size n인 요청을 만족하는 가장 적절한 hole을 찾는 문제**

✅ **`First - fit`**
- Size가 n 이상인 것 중 최초로 찾아지는 hole에 할당

✅ **`Best - fit`**
- Size가 n 이상인 가장 작은 hole을 찾아서 할당
- Hole들의 리스트가 크기순으로 정렬되지 않은 경우 모든 hole의 리스트를 탐색해야함
- 많은 수의 아주 작은 hole들이 생성됨

✅ **`Worst - fit`**
- 가장 큰 hole에 할당
- 역시 모든 리스트를 탐색해야 함
- 상대적으로 아주 큰 hole들이 생성됨

💥 First-fit과 Best-fit이 Worst-fit보다 속도와 공간 이용률 측면에서 보다 효과적인 것으로 알려짐(실험적인 결과)

✅ Compaction
- external fragmentation 문제를 해결하는 한 가지 방법
- 사용 중인 메모리 영역을 한 군데로 몰고 hole들을 다른 한 곳으로 몰아 큰 block을 만드는 것
- <u>매우 비용이 많이 드는 방법</u>
- 최소환의 메모리 이동으로 compaction하는 방법 (**매우 복잡한 문제**)
- Compaction은 프로세스의 주소가 실행 시간에 동적으로 재배치 가능한 경우에만 수행될 수 있다

---