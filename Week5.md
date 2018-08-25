

### 참고자료 
 - https://docs.microsoft.com/en-us/windows/desktop/winsock/using-winsock
 - TCP/IP 윈도우 소켓 프로그래밍
 - TCP/IP 완벽 가이드
 - https://beej.us/guide/bgnet/
 - Linux Socket Programming by Example

## 주소 변환
> Address관련 개념과 용어 설명

 - IP가 하는 일: 패킷을 Host(Destination Machine)에게 전달
 - 

### IP 주소

#### IPv4 with CIDR
 - IP 주소와 서브넷에 대해 다시 보기
 - 서브넷 마스크
 - [Classless Inter Domain Routing](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%EB%8D%94_(%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%82%B9))

문자열 표기법
```
// ...
```


프로그램 상에서의 형태
```c++
// ...
```

#### IPv6

문자열 표기법
```
// ...
```

프로그램 상에서의 형태
```c++
// ...
```

### Name-To-Address Translation
 - name: 문자열 형태 표현 (for Human)
 - endpoint(address): 이진 형태의 표현 (for Machine)

#### 주소 목록 획득
 - https://tools.ietf.org/html/rfc2553
 - [`getaddrinfo`](https://linux.die.net/man/3/getaddrinfo)
 - `freeaddrinfo`
 - `gai_strerror`

예제 코드 실행 후 / 메뉴얼 설명

```c++
```

Q. 왜 이런 인터페이스를 가지게 되었을까?  
Q. 기존 인터페이스와의 비교?  

#### Port & Service
 - Address: Machine을 구분하기 위한 값
 - Port: Service를 구분하기 위한 값

Port는 결국 Service ID라고 생각할 수 있다. 주소를 문자열로 사용하지 않는 것처럼, Service를 문자열이 아닌 숫자로 지칭하기 위한 방법. **Port라고 쓰고 Service라고 읽는다**.

 - Well-Known Port: 범용적으로 많이 사용되는 Service는 이미 Port 번호가 정해져있다.

Q. Port는 공유가 가능할까?  
Q. 다시 생각해보는 멀티 프로세스(Pre-fork) 구조의 특징  


## Core Socket Functions
### Socket?
Socket과 File의 다른 점?
 - 원격에 존재하는 Host와 통신하기 위한 목적
 - 패킷 유실의 가능성
    - 혼잡 / 지연 / 오류

반면 네트워크 상에서 이동하는 Packet들은 File처럼 **안정적**이지 않다. 심한 경우는 변조되었을 수도! 통신 프로그램을 작성할 때 고려할 것들
 - ...
 - ...
 - ...

특히 오류처리는 분석을 치밀하게 해놓는 것이 좋음.

### Setup/Teardown

#### Windows Socket API
WSAData의 구조와 획득 가능한 값들

Q. 왜 이런 절차를 거치도록 했을까?  
A. Virtual Memory / DLL에 대해 되짚어보기

#### Startup
```c++
// ...
```

#### TearDown
```c++
// ...
```

#### Error
```c++
// ...
```

### Socket 생성과 Bind

#### Socket 생성
```c++
// ...
```

Q. 생성자와 소멸자가 있으면 init/release 함수가 없어도 될까?

#### Port Binding
Socket은 결국 라이브러리 내부 자료구조. 단순히 Socket을 생성했다고 하더라도 아직 프로세스가 외부와 통신이 가능한 상태는 아니다. 외부와 통신하기 위해선 결국 Service를 해야한다. 달리말해, Port를 가져야 한다.

 - Bind가 하는 일: Socket 자료구조에 Port를 지정

```c++
// ...
```

### Shutdown & Close
부분적 통신종료와 Socket 해제 처리

```c++
// ...
```

> 
> - shutdown 하고 나서 읽으려는 시도(error 코드 같이)
> - shutdown 하고 나서 보내려는 시도
> 

### SendTo/RecvFrom

IP 통신은 연결 절차 없이 수행된다. (Connection-less) 연결에 대해서는 후술. Packet을 기계단위로 전달하기 위한 것이 목적. 당연히 어떤 기기에게 보낼지 사용자가 알아야 한다. pid를 사용한 IPC, 이름을 사용한 File 접근과 유사

```c++
// ...
```

### Connect

매번 remote를 명시하지 말고 아예 지정해버리면 어떨까? 수신자가 정해져있는 경우 Connect를 사용할 수 있다. Remote 주소를 새겨넣는 방법. 프로토콜의 연결 유무에 따라서 Operation의 내용이 달라진다.

### Listen/Accept

Q. 연결 지향 프로토콜을 지원하려면 어떤 처리가 필요할까?

 - listen: 연결 요청을 받기 위한 작업
 - accept: 연결에 따라서 추가적인 Socket 생성

> Connect / Listen / Accept를 함께
```c++
// ...
```

연결을 생성하는 작업을 Hand-shake라고 부른다.
 - [TCP 3-Way Handshake](http://asfirstalways.tistory.com/356)
 - [SCTP 4-Way Handshake](http://www.masterraghu.com/subjects/np/introduction/unix_network_programming_v1.3/ch02lev1sec8.html)

Q. 왜 추가적인 Socket을 생성해야만 하는 것일까?   
A. 하나의 Socket은 하나의 연결만을 관리할 수 있다는 한계. 따라서 Hand-shake 목적의 Listen Socket을 제외하고는 매 연결마다 새로운 Socket을 생성해서 connection을 관리하도록 한다

연결 지향 프로토콜이 가지는 특징
 - 패킷 순서 제어
 - 중복 데이터 해결
 - ...

연결 없이 동작하는 프로토콜이 가지는 특징
 - 단순함
 - 유연함: 다수로부터 수신/다수에게 송신이 가능하다

## Async Operation

보통 Core function에 대한 설명 이후에는 Thread를 사용한 Concurrent I/O Handling이 주제로 나온다. 하지만, Thread는 [매우 비싸다..](https://mva.microsoft.com/ko/training-courses/techdaysmini-3-c--11744?l=orSlWokEB_9604984382).

> C#을 이용한 Task 병렬화와 비동기 패턴: 초반 15분

### I/O Multiplexing
멈추지 않고 I/O를 계속하기 위한 방법. 

혹시 디자인 패턴을 공부 중이라면...
 - [Reactor/Proactor 패턴](http://ozt88.tistory.com/25)

### Callback Style
 - 시스템에 의한 자동 호출 사용하기

```c++
// ...
```

### Manual Control
 - I/O Completion Port
 - `epoll` / `aio`

스터디에서는 IOCP 예제 코드를 사용하지만, `aio` 예제 코드는 시간을 투자해서 읽어볼 가치가 있는 코드.

IOCP 예제 코드 실행해보기
```c++
// ...
```

