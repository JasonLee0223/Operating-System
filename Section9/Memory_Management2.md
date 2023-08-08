# Part2. Memory Management

## 🔥 Paging

✅ <u>**`Paging`**</u>

- Process의 virtual memory를 동일한 사이즈의 page 단위로 나눈다
- Virtual memory의 내용이 page 단위로 `noncontiguous(비연속적)`하게 저장된다
- 일부는 backing storage에, 일부는 physical memory에 저장된다

✅ <u>**Basic Method**</u>

- physical memory를 동일한 크기의 `frame`으로 나눈다
- logical memory를 동일 크기의 `page`로 나눈다 (frame과 같은 크기)
- 모든 가용 frame들을 관리한다
- page table을 사용하여 logical address를 physical address로 변환한다
- External fragmentation 발생 안함
- Internal fragmentation 발생 가능

<img src = "https://user-images.githubusercontent.com/92699723/258961019-785e9016-fcd1-4cc8-9ce0-15b6ad63d02a.jpg" width=60%>

> 각각의 페이지별로 물리적 메모리에 적당한 위치에 어디던지 비어있는 위치가 있으면 올라갈 수 있게해주는 기법이다.   
> Paging 기법에서 <u>**주소 변환을 위해서는 Page table이 사용**</u>된다.   

> Page table이라는 것은 각각의 논리적인 페이지(logical memory page)들이    
> 물리적인 메모리(physical memory page frame)의 어디에 올라가있는가를 찾아 낼 수 있도록 도와준다.

<img src = "https://user-images.githubusercontent.com/92699723/258961774-3590ad07-307c-40d6-abef-f0f699f627cc.jpg" width=60%>

---

> 그렇다면 Page table이 어디에 들어가야 할 것인가를 고민해보아야한다.   
> Page table은 꽤나 큰 용량을 차지하기 때문에 CPU에 존재할 수 없음으로 Register에다 넣을 수도 없고   
> 하드디스크에도 저장할 수 없으며 캐시메모리에 저장하기에도 용량이 너무 크기에   
> 메모리에 넣게된다.

> Page-table은 각각의 프로세스마다 존재한다.   
> 프로세스별로 논리적인 주소 체계가 다르기 때문에 이것을 위한 주소 변환을 위해 프로세스마다 존재해야한다.   
> 따라서 TLB도 각각 존재하게된다.

## Implementation of Page Table (페이지 테이블의 구현)

✅ **Page table**은 `main memory`에 상주

✅ **`Page-table base register (PTBR)`** 가 `page table`을 가리킨다

✅ **`Page-table length register (PTLR)`** 가 테이블 크기를 보관

✅ 모든 메모리 접근 연산에는 `2번의 memory access` 필요

✅ `page table` 접근 1번, 실제 `data/instruction` 접근 1번 (총 2번)

✅ 속도 향상을 위해 `associative register` 혹은 `translation look-aside buffer` **`(TLB)`** 라    
불리는 고속의 **lookup hardware cache** 사용

> Page-table은 별도의 하드웨어를 사용하게되는데 TLB는 일종의 Cache이며    
> main memory보다 빠르고 main memory와 CPU 사이에 존재하는 주소 변환을 해주는 계층이다.

<img src = "https://user-images.githubusercontent.com/92699723/258964917-b4a59953-eaa2-499e-bf56-68f6c1bef109.jpg" width=60%>

> 2번의 memory access가 필요함으로 속도가 느려지는 부분을 보완하기 위해 TLB라는 하드웨어를 도입하게되었다.   
> (Tip. 캐시메모리는 운영체제한테는 감추어진 계층)   
> TLB는 main memory보다 접근 속도가 빠른 하드웨어로 구성되어있으며 CPU가 논리적인 주소를 주게되면 물리적 메모리 상의 페이지를 접근하기전에 TLB를 먼저 검색해본다.   
> TLB에 저장이 되어있다고 한다면 TLB를 통해 바로 주소 변환이 이루어지기 때문에 메모리를 한 번만 접근하게된다.   
> (없는 경우 일반적으로 2번의 메모리 접근)

---

## Associative Register

✅ **`Associative registers`** (TLB): parallel search 가능
- `TLB에는 page table 중 일부만 존재`

✅ Address translation
- page table 중 일부가 associative register에 보관되어 있음
- 만약 해당 page #가 associative register에 있는 경우 곧바로 frame #를 얻는다
- 그렇지 않은 경우 main memory에 있는 page table로부터 frame #을 얻는다
- TLB는 context switch 때 flush (remove old entries, 비워줘야한다.)

---

## Effective Access Time

✅ Associative register lookup time = $\varepsilon$

✅ memory cycle time = 1

✅ <u>**`Hit ratio`**</u> = $\alpha$
- associative register에서 찾아지는 비율

✅ Effective Access Time (EAT)

> $hit = (1+\varepsilon)\alpha$   
> $miss = (2+\varepsilon)(1-\alpha)$

$EAT = (1+\varepsilon)\alpha + (2+\varepsilon)(1-\alpha)$   
= $2 + \varepsilon - \alpha$

---

## Two-Level Page Table

✅ 현대의 컴퓨터는 address space가 매우 큰 프로그램 지원
- 32(64) bit address 사용 시: 2<sup>32</sup>(2<sup>64</sup>) (32bit-4G)의 주소 공간
  - page size가 4K시 1M개의 page table entry 필요
  - 각 page entry가 4B시 프로세스당 4M의 page table 필요
  - 그러나, 대부분의 프로그램은 4G의 주소 공간 중 지극히 일부부만 사용하므로 page table 공간이 심하게 낭비됨

### 🔥 Point

💥 page table 자체를 page로 구성   
💥 사용되지 않는 주소 공간에 대한 outer page table의 entry 값은 NULL   
(대응하는 inner page table이 없음)

<img src = "https://user-images.githubusercontent.com/92699723/258969879-38b5bb07-ebbe-4955-bf3c-4c58b4d4466a.jpg" width=60%>

> 왜 2단계 Page table을 사용하는가??   
> 속도는 줄어들지 않는다.. -> 한 번 접근하던 것을 두 번 접근해야하기 때문   
> Page table을 위한 공간이 줄어드는 것이 2단계 Page table을 사용하는 목적이 된다.  

> 🔥 Point   
> 전체 주소 공간중에서 상당 부분이 실제 사용이 안되기 공간이기 때문에 안쪽 page table이 만들어지지 않음으로
> Two-Level page table을 사용하여 보다 공간활용도를 높이게 된 것이다.

---

## Two-Level Paging Example

✅ logical address(on `32-bit` machine with `4K page` size)의 구성
- 20 bit의 page number
- 12 bit의 page offset

✅ page table 자체가 page로 구성되기 때문에 page number는 다음과 같이 나뉜다   
(각 page table entry가 4Byte)
- 10 bit의 page number
- 10 bit의 page offset

✅ 따라서, logical address는 다음과 같다.

<img src = "https://user-images.githubusercontent.com/92699723/258985730-cf9343ec-687c-41a3-8ad1-e6ca092b8f86.jpg" width=40%>

✅ $P_1$은 outer page table의 index이고

✅ $P_2$는 outer page table의 page에서의 변위 (displacement)

<img src = "https://user-images.githubusercontent.com/92699723/258986137-0879c49d-42d7-490a-a91d-f10b0bacae3c.jpg" width=60%>