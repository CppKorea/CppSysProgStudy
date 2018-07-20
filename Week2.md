
> 2 주차: 7월 21일

## 참고자료
 - The Art of Computer Programming - 1 / 한빛미디어
 - 리눅스 커널 내부구조 / 아티오
 - 프로그래밍 언어론 / 생능출판사
 - Windows via C/C++ 5th Edition
 
## 프로그램 보충 설명

### 실행기의 구조와 어셈블리 
예시: MIX 머신과 어셈블리들

#### MIX 머신 구조
 - Register: A, X, 1, 2, 3, 4, 5, 6, J
 - Toggle: Overflow
 - Comparision Indicator: Equal / Less / Greater
 - Memory 4000 Word (1 Word == 5 byte + [MSB](https://ko.wikipedia.org/wiki/%EC%B5%9C%EC%83%81%EC%9C%84_%EB%B9%84%ED%8A%B8))
 - 자기 테이프
 - 디스크
 - 카드 판독기
 - ...

#### MIX 명령어

설명에 필요한 부분만 일부 발췌함

 - T: 대기 시간
 - CI: Comparison Indicator

| Operation | Code  | Time    | Description  |
|----------:|-------|---------|--------------|
| NOP       | 00    | 1       | No operation
| ADD       | 01    | 2       | rA := rA + value
| SUB       | 02    | 2       | rA := rA - value
| MUL       | 03    | 10      | rA := rA * value
| DIV       | 04    | 12      | rA := rA / value
| MOVE      | 07    | 1 + 2N  | r1[0...N] = M[0...N]
| LDA       | 08    | 2       | rA := value
| LD1       | 09    | 2       | r1 := value
| LD2       | 10    | 2       | r2 := value
| STA       | 24    | 2       | M(F) := rA
| ST1       | 25    | 2       | M(F) := r1
| ST2       | 26    | 2       | M(F) := r2
| IN        | 36    | 1 + T   | input
| OUT       | 37    | 1 + T   | output
| JA        | 40    | 1       | jump to rA
| J1        | 41    | 1       | jump to r1
| J2        | 42    | 1       | jump to r2
| INC/DECA  | 48    | 1       | rA := rA +- value
| INC/DEC1  | 49    | 1       | r1 := r1 +- value
| CMP1      | 56    | 2       | CI := r1 <=> value
| CMP2      | 57    | 2       | C2 := r2 <=> value 
| ...       | ...   | ...     | ... |

### Linking
프로그램을 링킹하는 과정? Symbol들의 주소를 계산하는 것. 프로그램이 완성되기 위해선 모든 Symbol들이 주소를 가지고 있어야 한다.

> 추가 필요함

### Loading
프로그램이 실행되기 위해선 메모리에 적재(Load)되어야 한다. 이 과정을 수행하는 것이 Loader. 보통 Linking까지 같이 수행하기도...

#### Absolute Loader
초창기에는 시작주소가 고정되어있기도(고정된 계산기와 계산기를 위한 프로그램). 지정한 주소부터 프로그램을 적재한다.

#### Relocating(Relative) Loader
프로그램의 시작주소는 그때그때 달라질 수 있는 형태

#### Bootstrap
그런데 Loader는 보통 OS에서 모듈로써 동작. 그러면 OS는 어떻게 Load하는것인가?

간단히 살펴보는 Linux의 부팅 과정
 1. Power-On + POST(Power-On Self Test)
 1. BIOS (Basic Input-Output System. Firmware)
 1. Master Boot Record >> Memory
 1. Boot Loader (GRUB)
 1. OS Kernel
 1. Swap Process (0번 Process)
 1. Init drivers
 1. /sbin/init (1번 Process)
 1. Command shells
 1. GUI

찾아보기: OS의 shutdown 절차

## Operating System & OS Process

Q. 개발자 입장에서 좋은 OS라고 한다면..?

 - 환경설정은 쉽나? (+ 개발환경 변경이 용이?)
 - 안정성/업그레이드 지원
 - 풍부한 기능(+ 쓸 수 있는 소프트웨어) : 게임이 안되는 OS는 OS가 아니다...!
 - 메뉴얼/문서     

### 자원 관리자

운영체제가 하는 일을 요약하자면 **자원 관리자**(Resource Manager)

 - Command Interpreter
 - Information Service
 - Accounting
 - Error Handling
 - File System
 - Storage Management
 - Protection
 - Process Management
 - Memory Management
 - I/O System

하드웨어 제어, 파일/메모리 등의 공유와 보호, 프로그램들의 스케줄링.
이 모든 것들을 동시적으로 처리할 수 있어야 한다. (프로그램 간 동기화 지원) 가령, 하나의 프로그램이 하나의 파일을 생성하는 동안, 다른 프로그램들은 **또다른 파일을** 읽거나 변경할 수 있어야 한다.


#### Accounting
CPU 사용, File 크기 등... 이런 것에 비용을 할당
여기서 프로그램은 사용자의 대리일 수 있다. 즉, 다수의 사용자에 의한 접근을 관리할 수 있어야 한다는 의미와 동일하다.

#### Error Handling
중요: S/W에서 에러가 발생하면 그 처리는 OS가 수행한다.

#### Protection
Security와 비슷한 느낌이지만 야간 다른 의미.
여기서 Protection은 접근 권한에 따른 **OS의 자원**보호를 의미함.  Security는 좀 더 넓은 범위의 이야기. DDoS나 MITM 공격 등

#### Process Management
CPU를 최대한 활용 수 있는 방법(CPU Time Sharing)에 대한 고려. I/O가 진행중인 프로세스는 대기시키고 CPU가 필요한 다른 프로세스를 실행시킨다

각 프로그램마다 공평하게 진행될 수 있도록 해야함. 적절하게 반응성을 유지(일정한 Response time)


### OS 프로세스

 - Job/Task: 하고자 하는 일들
 - Process: **실행 중**인 프로그램

프로그램 그 자체는 어떠한 일을 한다~ 라는 명령 목록에 불과하다. **하나의 프로그램이 여러개의 프로세스에서 사용될 수도 있다.**

#### 구성요소
 - Program + Process Heap + Resources(File, Lock)
 - Thread + Thread Local Storage + Run-time Stack

#### PCB (Process Control Block)
OS에서 프로세스를 관리하기 위한 모든 정보가 담긴 struct. 간단히 생각하면 프로세스에 대한 Snapshot.  
Context Switching: 프로세스가 대기열에 들어갈 때마다 PCB가 저장(Store)되고, 프로세스가 활성화 될때마다 적재(Load)된다. 

 - Process state,
 - Process id(number): Parent + Self + Children
 - Registers
 - Address space: Memory usage + limits
 - List of open files(resources)
 - Sharing options
 - ...


프로세스들은 Tree 형태의 관계를 가진다. 이 관계를 프로그램으로 표현할 수 있을까? `fork`, `exec`, `wait` 함수 메뉴얼
서로 자원(특히 보안 옵션)을 공유하는가? 서로 동시에 실행될 수 있는가?

#### Life-Cycle

대부분의 OS에서 기본적으로 5가지 상태. Interrupt에 의해서 상태 이동이 발생: Created, Ready, Running, Waiting, Terminated  

Q. 모든 프로세스는 한번쯤 Zombie가 된다?

관련 리눅스 함수들
 - [`fork`](http://man7.org/linux/man-pages/man2/fork.2.html)
 - [`exit`](http://man7.org/linux/man-pages/man2/exit.2.html)
 - [`wait`](http://man7.org/linux/man-pages/man2/wait.2.html) / [`waitpid`](http://man7.org/linux/man-pages/man2/waitpid.2.html)

#### Scheduling

Scheduling과 관련된 요인들?
 - I/O Request
 - Time Expired
 - Interrupt (Timer, System Call, Error ...)

Scheduling의 어려운 점?
 - Priority   
   각각의 중요도가 다르다
 - Affinity   
   프로세스가 실행되었던 환경(Core)에서 다시 실행되기를 원한다
 - I/O Bound & CPU Bound  
   빈번하게 I/O가 발생하면 다시 Wait Queue로 진입해야 한다.
 - Response Timeout   
   - Soft real-time : Timeout에 대해 보장하지 않는다 (검색해보기: ANR)
   - Hard real-time : 정해진 timeout안에 동작함을 보장한다

고려해야 하는 것들?
 - 하드웨어 장치들을 충분히 사용하고 있는가? (Device Queue)
 - Starvation 방지(Ready 상태가 너무 오래 유지되지 않을 것)

### 가상 메모리 & 페이징

물리 메모리(Physical Storage)는 한정되어 있다. 그렇다면 메모리보다 큰 크기의 프로그램을 실행하기 위해선 어떤 메커니즘이 필요할까?

굳이 모든 프로그램이 메모리에 존재하지 않아도 실행하는 것이 가능하다! 필요에 따라서 파일-물리 메모리 Mapping을 수행

 - 동적 링킹 / 동적 로딩: 실행 시간에 링킹(Symbol Lookup)과 로딩을 수행

실제 메모리 주소를 사용하지 않고, 가상 메모리 주소체계를 도입. 가상 주소공간의 크기는 포인터가 저장할 수 있는 주소의 범위와 같다. 동시에, 모든 프로세스는 고유한 공간을 가진다. 즉, `Process A's 0x12345678 != Process B's 0x12345678`.

#### 가상 메모리의 구성

| Partition         | Range in x86 32bit Windows
|-------------------|-------
| NULL Assignment   | 0x0000'0000 ~ 0x0000'FFFF
| User-Mode         | 0x0001'0000 ~ 0x7FF**E**'FFFF
| 64-KB Off-Limits  | 0x7FFF'0000 ~ 0x7FFF'FFFF
| Kernel-Mode       | 0x8000'0000 ~ 0xFFFF'FFFF

User Mode 영역은 대략 2GB 정도. 나머지는 Kernel Mode에서 사용한다. Kernel Mode에 위치한 코드/데이터는 반드시 보호되어야 하는 부분.(Completely protected)

하지만 처음부터 위의 모든 영역이 사용 가능(usable)한 것은 아니고, 그에 앞서서 할당(reserving)을 받아야 한다.

#### Swap In/Out
당장 필요하지 않은 내용(머신 코드 혹은 데이터)이 물리 메모리를 차지하고 있을 필요는 없다. 메모리가 충분하지 않은 경우, 혹은 정해진 조건에 따라서 보조 저장장치로 메모리의 내용을 옮김. (Block 단위 교환)

#### 페이징
디스크를 메모리처럼 사용하는 방법.

메모리에 있는 데이터는 어떤 속성을 가지는가?
 - No Access: 접근 불가
 - Read Only
 - Read + Write
 - Execute : 실행 가능한 코드
 - Execute + Read
 - Execute + Read + Write

주소 공간이 메모리에 없을 수도 있다 == 메모리에 접근할 때마다 그 주소가 RAM에 위치하는지 CPU가 검사를 해야 한다.  

##### Page Table
가상 주소에서 물리 주소를 계산하기 위한 방법. 

##### Page Fault
특정 페이지에 접근하려고 했으나 RAM에 존재하지 않음. 이 경우 디스크로부터 Swap In이 수행되어야 하고, 물리 메모리에는 Free Page가 있어야 한다. 따라서 Swap In은 또다른 Swap Out을 발생시킬 수 있다.

##### Memory Mapped File

 - Data: Read/Write 가능한 데이터 공간처럼 사용. IPC에 활용 가능
 - Image: 디스크에 존재하는 프로그램(exe & dll)을 주소 공간에 연결. 

#### Example
> Windows via C/C++ 에서 일부 발췌함

Type
 - Free: Not a storage 
 - Private
 - Image: Memory-mapped Image file
 - Mapped: Memory-mapped Data file

| Base Address  | Type    | Blocks | Attribute | Description |
|--------------:|--------:|--------|-----------|-------------|
| 0x 0000'0000  | Free    |       | 
| 0x 0001'0000  | Mapped  | 1     | RW
| 0x 0002'0000  | Private | 1     | RW
| 0x 0002'1000  | Free    |       | 
| 0x 0003'0000  | Private | 3     | RW          | Thread Stack
| 0x 0013'0000  | Mapped  | 1     | R
| 0x 0013'4000  | Free    |       | 
| 0x 0014'0000  | Mapped  | 1     | RW 
| 0x 0014'3000  | Free    |       | 
| ...           |
| 0x 0042'3000  | Image   | 7     | ERWC    | C:\some\path\program.exe
| 0x 0041'F000  | Free    |       |         |
| 0x 0042'0000  | Mapped  | 1     | R 
| ...           |
| 0x 005C'0000  | Private | 2     | RW
| ...           |
| 0x 7544'0000  | Image   | 5     | ERWC       | C:\Windows\System32\uxtheme.dll
| 0x 7547'F000  | Free    |
| 0x 7630'0000  | Image   | 4     | ERWC       | C:\Windows\System32\PSAPI.dll
| ...           |


## 파일 입출력
### 프로그래밍 대상으로서의 파일
It's just a chunk(pile) of byte(data). 따라서 많은 것들이 파일로 포현될 수 있다. 

굉장히 추상적인데... OS에선 어떤 Operation을 지원해줘야 할까?

 - Create, Write, Read, Seek
 - Delete, Truncate
 - Open, Close

파일 입출력이 발생하면?

 1. I/O controller에 작업을 요청
 2. CPU는 다른 Task를 수행
 3. I/O controller에 의한 I/O interrupt 발생
 4. CPU에 의한 Interrupt 확인
 5. OS의 Interrupt Service Routine에 의한 처리


### 파일 시스템

파일은 Storage에 저장되어 있을수도, Main Memory에 위치할 수도 있다. 실제로 파일을 읽고 쓰는 동작은 OS의 모듈들이 수행.

PCB처럼 파일을 제어하기 위한 자료구조가 존재(File Control Block). 주로 디스크에 저장되어 있지만, 필요에 따라 메모리에 올려놓는 정보들이 포함.

**파일 저장방식(NTFS, ext4), 저장 형태(확장자)에 대한 정의**: 여기서 저장 형태란 것은 결국 어떤 프로그램에 의해서 사용되는가를 의미하기도 함. 단순한 텍스트 파일이 있을 수 있고, 음악 재생을 위한 파일도 있을 수 있다.

여러 OS에서의 동작을 생각해야 할텐데? 표준화!

#### Partition
저장 공간(Storage Volume)을 분할(Partition)해서 사용. Partition 안에는 File System이 있을수도, 없을 수도 있음(Raw 상태)

#### Boot Control Block
부팅되지 않은 OS도 디스크에 저장되어 있어야 한다. 보통 첫번째 영역을 사용(Master Boot Record)

#### Volume Control Block
사용 가능한 블록의 수, 위치, 크기등을 표현

 - Sequential Allocation
   Fragmentation의 발생
 - Chain(Extent)   
   다음 블록에 대한 포인터를 사용. Seek 하기 어려움
 - Indexed Allocation   
   인덱스 블록을 만들고 데이터 블럭들의 주소를 거기에 집어넣자!
   인덱스를 저장할 별도의 공간 관리의 문제

>
> - 공간적 연속성 : Contiguous
> - 시간적 연속성 : Continuous
>

#### File Control Block
파일마다 존재. 이름(경로), 필요한 권한, 갱신된 날짜... 
폴더라면 안에 들어있는 파일들에 대한 목록도 필요

검색: INode, [참고영상](https://www.youtube.com/watch?v=_6VJ8WfWI4k)


### File API

#### [ISO C Standard File I/O](https://en.cppreference.com/w/c/io)
각각의 Function들을 살펴봅시다
 - [fopen](https://en.cppreference.com/w/c/io/fopen)
 - [freopen](https://en.cppreference.com/w/c/io/freopen)
 - [fclose](https://en.cppreference.com/w/c/io/fclose)
 - [fflush](https://en.cppreference.com/w/c/io/fflush)
 - [fread](https://en.cppreference.com/w/c/io/fread)
 - [fwrite](https://en.cppreference.com/w/c/io/fwrite)
 - [ftell](https://en.cppreference.com/w/c/io/ftell)
 - [fseek](https://en.cppreference.com/w/c/io/fseek)
 - [ferror](https://en.cppreference.com/w/c/io/ferror)
 - [remove](https://en.cppreference.com/w/c/io/remove)
 - [rename](https://en.cppreference.com/w/c/io/rename)

#### [Linux System File I/O]()
또 한번 살펴봅시다

 - [`open`](http://man7.org/linux/man-pages/man3/open.3p.html)
 - [`openat`](http://man7.org/linux/man-pages/man3/openat.3p.html)
 - [`creat`](http://man7.org/linux/man-pages/man3/creat.3p.html)
 - [`close`](http://man7.org/linux/man-pages/man3/close.3p.html)
 - [`read`](http://man7.org/linux/man-pages/man3/read.3p.html)
 - [`write`](http://man7.org/linux/man-pages/man3/write.3p.html)
 - [`lseek`](http://man7.org/linux/man-pages/man3/lseek.3p.html)
 - [`mmap`/`unmap`](http://man7.org/linux/man-pages/man2/mmap.2.html)
 