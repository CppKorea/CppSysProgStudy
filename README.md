## C++ Korea 시스템 프로그래밍 스터디

 - 진행: [박동하](https://github.com/luncliff) ( luncliff@gmail.com )

C++ Korea에서 진행하는 2018년 여름 특강입니다. S/W 관련학과에서 관련 지식을 처음 접하는 2~3학년 학생들을 대상으로 합니다. 

### Note
최종 목표는 **참여하시는 분들이 스스로 찾아서 공부할 수 있을 정도의 기반 지식을 갖추는 것**입니다. 이를 위해 스터디 시간의 60~80%는 설명, 나머지 시간은 QnA로 진행합니다.

진행하면서 참고한 자료, 내용은 간략한 형태로 정리되어 Repo에 업로드 됩니다. Contribution도 환영합니다 :D 

다소 촉박한 시간계획으로 진행하기에, 코드를 작성하기보단 공개된 예제 코드(Man page, MSDN)를 Line by Line으로 설명하는 방향으로 진행됩니다.

#### Audience?

 - 난 짱짱 게으르기 때문에 **가을학기 공부를 여름에** 하지!
 - 검색하면 나온다는데 **뭘로 검색해야 되는지 몰라요**
 - **C++** 를 배우긴 했는데... 어떻게 써야 하는건지...?

#### 교재

학교 수업에서 사용하는 교재 or 학생이 직접 도서관에서 목차 / 내용을 확인하고 대여해온 책

**다섯 수 앞을 내다보고** 공부하는 경우: [리눅스 시스템/네트워크 프로그래밍(김선영 저) 3판 - 가메 출판사](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788980782819&orderClick=LAG&Kc=)

### Coverage
 - C++ 프로그래밍 언어
 - 컴파일 과정
    - 소스 코드에서 바이너리로...
    - 라이브러리
 - OS가 하는 일
 - 윈도우/리눅스 시스템 API 예제

### Milestone

#### 7월 14일

 - 자기소개, 스터디 일정 조율
 - 시스템 + 프로그래밍?
   - 용어 정리 + 각 용어 별 설명
   - 검색해보기: Ask StackOverflow! 
 - 소스코드
   - 컴파일-빌드의 과정
   - 바이너리의 구조

#### 7월 21일

 - 운영체제
   - Process & Scheduling
   - Process 실행 전/후 발생하는 일
   - 가상 메모리 / 페이징
   - 파일 시스템
 - 파일 입출력
    - File Descriptor
    - C, C++ 표준 라이브러리들과의 비교

#### 7월 28일

 - OS가 제공하는 보안 서비스
   - Confidentiality, Integrity, Availability (CIA Triad)
   - 권한 관리
 - OS Signal
   - Signal이 왜 필요한가?
   - Manual 정독하기 (RTFM!)
   - SIGINT, SIGSEGV ...
   - Signal을 어떻게 처리하는가?

#### 8월 11일

 - Inter Process Communication
   - 한번에 여러 프로세스를 
   - Pipe, Shared Memory
 - 네트워크 기본 지식
   - OSI 7 Layer
   - 패킷 스위칭
   - ARP, IP

#### 8월 25일

 - 소켓을 만들어서 사용해보기
   - File descriptor와의 차이점?
   - 프로그래밍 이슈들
 - http 프로토콜 Top-Down 방식으로 살펴보기
   - Http
   - TCP
   - IP

#### 9월 1일
 - 정산 및 스터디 리뷰, QnA

