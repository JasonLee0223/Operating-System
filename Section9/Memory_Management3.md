# Part3. Memory Management

## Multilevel Paging and Performance

✅ Address space가 더 커지면 다단계 page table 필요

✅ 각 단계의 page table이 메모리에 존재하므로 logical address의 physical address 변환에   
더 많은 메모리 접근 필요하다

✅ TLB를 통해 메모리 접근 시간을 줄일 수 있다

✅ 4단계 page table을 사용하는 경우
- 메모리 접근 시간이 100ns, TLB 접근 시간이 20ns이고
- TLB hit ratio가 98%인 경우   
  effective memory access time = 0.98 x 120 + 0.02 x 520 = 128 nanoseconds.   
  결과적으로 주소변환을 위해 28ns만 소요

> Multilevel Paging을 사용하더라도 시간이 지나치게 오래걸리지 않는다.

<img src = "https://user-images.githubusercontent.com/92699723/258993859-1dae9dd6-a193-427a-9dd7-d6ed899d9040.jpg" width=60%>

---

## Memory Protection

✅ Page table의 각 entry 마다 아래의 bit를 둔다

- **`Protection bit`**
  - page에 대한 접근 권한 (read/write/read-only)

- **`Valid-invalid bit`**
  - `"valid"`는 해당 주소의 **frame**에 그 프로세스를 구성하는 유효한 내용이 있음을 뜻함 (접근 허용)
  - `"invalid"`는 해당 주소의 **frame**에 유효한 내용이 없음을 뜻함 (접근 불허)

1) 프로세스가 그 주소 부분을 사용하지 않는 경우
2) 해당 페이지가 메모리에 올라와 있지 않고 swap area에 있는 경우 

---

## Inverted Page Table

✅ page table이 매우 큰 이유
- 모든 process 별로 그 logical address에 대응하는 모든 page에 대해 page table entry가 존재
- 대응하는 page가 메모리에 있든 아니든 간에 page table에는 entry로 존재

✅ Inverted page table
- Page frame 하나당 page table에 하나의 entry를 둔 것 (system-wide)
- 각 page table entry는 각각의 물리적 메모리의 page frame이 담고 있는 내용 표시   
(process-id, process의 logical address)
- 단점
  - 테이블 전체를 탐색해야 함
- 조치
  - associative register 사용 (expensive)

<img src = "https://user-images.githubusercontent.com/92699723/258998061-2984272b-9c80-4d08-b65c-32eb5d7ad6a4.jpg" width=60%>

---

## Shared Page

✅ Shared code
- **`Re-entrant Code (=Pure code)`**
- read-only로 하여 프로세스 간에 하나의 code만 메모리에 올림   
(e.g. text editors, compilers, window systems)
- Shared code는 모든 프로세스의 `logical address space`에서 동일한 위치에 있어야 한다.

✅ Private code and data
- 각 프로세스들은 독자적으로 메모리에 올린다
- Private data는 logical address space의 아무 곳에 와도 무방하다

<img src = "https://user-images.githubusercontent.com/92699723/258999787-2488fcba-3182-48a5-abbf-d7f105986e25.jpg" width=60%>

---

> 여기까지 Paging 기법에 대한 설명은 모두 마쳤습니다.   
> 아래부터는 Segmentation 기법을 설명합니다.

## Segmentation

✅ 프로그램은 의미 단위인 여러 개의 segment로 구성
- 작게는 프로그램을 구성하는 함수 하나 하나를 세그먼트로 정의한다
- 크게는 프로그램 전체를 하나의 세그먼트로 정의도 가능하다
- 일반적으로는 code, data, stack 부분이 하나씩의 세그먼트로 정의된다.

✅ **Segment**는 다음과 같은 logical unit들이다.
- main()
- function
- global variables
- stack
- symbol table, arrays

---

## Segmentation Architecture
✅ **Logical address**는 다음의 두 가지로 구성   
<`segment-number`, `offset`>

✅ <u>**`Segment table`**</u>
- each table entry has: 
  - `base` - **starting physical address** of the segment
  - `limit` - **length** of the segment (세그먼트의 길이)

✅ **`Segment - table base register (STBR)`**
- 물리적 메모리에서의 **`segment table의 위치`**

✅ **`Segment - table length register (STLR)`**
- 프로그램이 사용하는 **`segment의 수`**   
segment number $s$ is legal if $s$ < STLR

<img src = "https://user-images.githubusercontent.com/92699723/259002190-bb92a855-eff4-40f6-ba75-93928c38e194.jpg" width=60%>

---

## Segmentation Architecture (Cont.)

✅ <u>**`Protection`**</u>
- 각 세그먼트 별로 protection bit가 있음
- Each entry:
  - `Valid` bit = 0 => illegal segment
  - `Read/Write/Execution` 권한 bit

✅ <u>**`Sharing`**</u>
- **shared segment**
- **same segment number**

🔥 `segment`는 의미 단위이기 때문에 공유(`sharing`)와 보안(`protection`)에 있어 paging보다   
훨씬 효과적이다.

✅ <u>**`Allocation`**</u>
- **first fit / best fit**
- **external fragmentation 발생**

🔥 `segment`의 길이가 동일하지 않으므로 가변 분할 방식에서와 동일한 문제점들이 발생

<img src = "https://user-images.githubusercontent.com/92699723/259010184-89aac3fe-33d5-42b0-8df2-75742d49f4c2.jpg" width=60%>

<img src = "https://user-images.githubusercontent.com/92699723/259011634-d76947de-b5e6-43a3-a670-2f9d63d0752a.jpg" width=60%>