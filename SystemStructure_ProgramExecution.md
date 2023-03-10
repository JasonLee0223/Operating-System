# System Structure & Program Execution 1
## ๐ข ์ปดํจํฐ ๊ตฌ์กฐ
- Computer: CPU, Memory(CPU์ ์์๊ณต๊ฐ)
- I/O Device: Disk, ํค๋ณด๋, ๋ง์ฐ์ค, ๋ชจ๋ํฐ, etc...

## ๐ข Mode bit
- ์ฌ์ฉ์ ํ๋ก๊ทธ๋จ์ ์๋ชป๋ ์ํ์ผ๋ก ๋ค๋ฅธ ํ๋ก๊ทธ๋จ ๋ฐ ์ด์์ฒด์ ์ ํผํด๊ฐ ๊ฐ์ง ์๋๋ก ํ๊ธฐ ์ํ ๋ณดํธ ์ฅ์น ํ์
- `Mode bit`์ ํตํด ํ๋์จ์ด์ ์ผ๋ก 2๊ฐ์ง ๋ชจ๋์ ์์(operation) ์ง์   

|Operation|Mode|Role|
|:--:|:--|:--|
|1|์ฌ์ฉ์ ๋ชจ๋|์ฌ์ฉ์ ํ๋ก๊ทธ๋จ ์ํ|
|0|**`๋ชจ๋ํฐ ๋ชจ๋`**|OS ์ฝ๋ ์ํ|
- ๋ณด์์ ํด์น  ์ ์๋ ์ค์ํ ๋ช๋ น์ด๋ `๋ชจ๋ํฐ ๋ชจ๋`์์๋ง ์ํ ๊ฐ๋ฅํ **`ํน๊ถ ๋ช๋ น`** ์ผ๋ก ๊ท์ 
- Interrupt ๋๋ Exception ๋ฐ์ ์ ํ๋์จ์ด๊ฐ mode bit์ '0'์ผ๋ก ๋ฐ๊ฟ
- ์ฌ์ฉ์ ํ๋ก๊ทธ๋จ์์ CPU๋ฅผ ๋๊ธฐ๊ธฐ ์ ์ mode bit์ '1'๋ก ์ํ
- **`๋ชจ๋ํฐ ๋ชจ๋`** = Kernel Mode, System Mode

---
## ๐ข Timer
- `์ ํด์ง ์๊ฐ`์ด ํ๋ฅธ ๋ค ์ด์์ฒด์ ์๊ฒ ์ ์ด๊ถ์ด ๋์ด๊ฐ๋๋ก Interrupt๋ฅผ ๋ฐ์์ํด
- ํ์ด๋จธ๋ ๋งค `Clock Tic`๋ ๋ง๋ค `1์ฉ ๊ฐ์`
- ํ์ด๋จธ ๊ฐ์ด 0์ด ๋๋ฉด Timer Interrupt ๋ฐ์
- CPU๋ฅผ ํน์  ํ๋ก๊ทธ๋จ์ด ๋์ ํ๋ ๊ฒ์ผ๋ก๋ถํฐ ๋ณดํธ   

โ ํ์ด๋จธ๋ time Sharing์ ๊ตฌํํ๊ธฐ ์ํด ๋๋ฆฌ ์ด์ฉ๋จ   
โ ํ์ด๋จธ๋ ํ์ฌ ์๊ฐ์ ๊ณ์ฐํ๊ธฐ ์ํด์๋ ์ฌ์ฉ

---
## ๐ข Device Controller
โ I/O device controller 

  - ํด๋น I/O ์ฅ์น์ ํ์ ๊ด๋ฆฌํ๋ ์ผ์ข์ ์์ CPU
  - ์ ์ด ์ ๋ณด๋ฅผ ์ํด control register, status register๋ฅผ ๊ฐ์ง
  - local buffer๋ฅผ ๊ฐ์ง (์ผ์ข์ data register)

โ I/O๋ ์ค์  device์ local buffer ์ฌ์ด์์ ์ผ์ด๋จ   
โ Device controller๋ I/O๊ฐ ๋๋ฌ์ ๊ฒฝ์ฐ Interrupt๋ก CPU์ ๊ทธ ์ฌ์ค์ ์๋ฆผ   

|Location|Name|Role|
|:---|:---|:---|
|Software|`device driver`(์ฅ์น ๊ตฌ๋๊ธฐ)|OS์ฝ๋ ์ค ๊ฐ ์ฅ์น๋ณ ์ฒ๋ฆฌ๋ฃจํด|
|Hardware|`device controller`(์ฅ์น ์ ์ด๊ธฐ)|๊ฐ ์ฅ์น๋ฅผ ํต์ ํ๋ ์ผ์ข์ ์์ CPU|

---
## ๐ข ์์ถ๋ ฅ(I/O)์ ์ํ
โ ๋ชจ๋  ์์ถ๋ ฅ ๋ช๋ น์ ํน๊ถ ๋ช๋ น   
โ ์ฌ์ฉ์ ํ๋ก๊ทธ๋จ์ ์ด๋ป๊ฒ I/O๋ฅผ ํ๋๊ฐ?

  - ์์คํ ์ฝ(System call)
    - ์ฌ์ฉ์ ํ๋ก๊ทธ๋จ์ ์ด์์ฒด์ ์๊ฒ I/O ์์ฒญ
  - trap์ ์ฌ์ฉํ์ฌ Interrupt Vector์ ํน์  ์์น๋ก ์ด๋
  - ์ ์ด๊ถ์ด Interrupt Vector๊ฐ ๊ฐ๋ฆฌํค๋ Interrupt Service Routine์ผ๋ก ์ด๋
  - ์ฌ๋ฐ๋ฅธ I/O ์์ฒญ์ธ์ง ํ์ธ ํ I/O ์ํ
  - I/O ์๋ฃ ์ ์ ์ด๊ถ์ ์์คํ์ฝ ๋ค์ ๋ช๋ น์ผ๋ก ์ฎ๊น

---
## ๐ข Interrupt
Interrupt ๋นํ ์์ ์ ๋ ์ง์คํฐ์ program counter(pc)๋ฅผ saveํ ํ CPU์ ์ ์ด๋ฅผ Interrupt Servuce Routine์ ๋๊ธด๋ค.

โ Interrupt์ ๋์ ์๋ฏธ
- Interrupt (ํ๋์จ์ด ์ธํฐ๋ฝํธ - I/O๊ฐ ๋ค ๋๋ฌ์ ๋): ํ๋์จ์ด๊ฐ ๋ฐ์์ํจ ์ธํฐ๋ฝํธ
- Trap (์ํํธ์จ์ด ์ธํฐ๋ฝํธ - I/O๋ฅผ ์์ฒญํ  ๋)
  - Exception: ํ๋ก๊ทธ๋จ์ด ์ค๋ฅ๋ฅผ ๋ฒํ ๊ฒฝ์ฐ
  - System call: ํ๋ก๊ทธ๋จ์ด Kernel ํจ์๋ฅผ ํธ์ถํ๋ ๊ฒฝ์ฐ

โ Interrupt ๊ด๋ จ ์ฉ์ด
|์ฉ์ด|์ค๋ช|
|:--|:--|
|Interrupt Vector<br/>(์ธํฐ๋ฝํธ ๋ฒกํฐ)| ํด๋น ์ธํฐ๋ฝํธ์ ์ฒ๋ฆฌ ๋ฃจํด ์ฃผ์๋ฅผ ๊ฐ์ง๊ณ  ์์
|Interrupt Service Routine<br/>(์ธํฐ๋ฝํธ ์ฒ๋ฆฌ ๋ฃจํด, ์ธํฐ๋ฝํธ ํธ๋ค๋ฌ)|ํด๋น ์ธํฐ๋ฝํธ๋ฅผ ์ฒ๋ฆฌํ๋ ์ปค๋ ํจ์

---
## ๐ข ์์คํ ์ฝ(System Call)
์ฌ์ฉ์ ํ๋ก๊ทธ๋จ์ด ์ด์์ฒด์ ์ ์๋น์ค๋ฅผ ๋ฐ๊ธฐ ์ํด Kernel ํจ์๋ฅผ ํธ์ถํ๋ ๊ฒ