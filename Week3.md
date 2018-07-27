
> 2 주차: 7월 28일

### 참고자료 
 - Computer Security - Principles and Practice. 3rd E 
 - Advanced 리눅스 시스템/네트워크 프로그래밍
 - ...

## 보안?
> 이 부분은 특강을 진행하는 사람도 깊이 알지 못해 피상적인 이야기로만 진행합니다! 

Security의 분야들...
 - Network
 - Management
 - Cryptography
 - Software
   - [Secure Coding practices](https://wiki.sei.cmu.edu/confluence/display/seccode/Top+10+Secure+Coding+Practices)
 - Hardware

당연한 얘기지만, 보안과 관련해서도 **표준**들이 있습니다!

거대한 의문
 1. 어떤 자산(Asset)을 지켜야 하는가?
 2. 자산이 어떻게 위협(Threat)받고 있는가?
 3. 위협에 어떻게 대응(Countermeasure)할 것인가?

Q. 3가지 질문에 대해서 각 분야에 대해서 생각해보기


### 용어들

 - Computer Security : 정보 시스템이 아래의 3가지를 유지(preserve)해내는 것
   - Confidentiality(기밀성)
   - Integrity(무결성)
   - Availability(가용성)  

##### 기밀성?
 - Data Confidentiality
 - Privacy

권한이 없는 사용자는 접근할 수 없어야 한다. 권한이 있는 사용자 이외에는 접근할 수 없어야 한다.

Q. 사용할 수 없는 라이브러리를 만들 수 있을까?

##### 무결성? 
 - Data Integrity
 - System Integrity 

정보는 정해진 방법으로, 적합한 권한하에 변경되어야 한다.
시스템은 손상되지 않으면서 정해진 기능을 수행하여야 한다. 

Q. 데이터가 무결한지 검사할 방법에는 어떤 것이 있는가?

##### 가용성? 
시스템이 적절하게 동작하고 사용자에게 서비스 하는 것

Q. DoS 공격에 대해 검색해보기

##### 취약점(Vulunerability)?
 - Leaky ( Confidentiality )
 - Corrupted ( Integrity )
 - Unavailable ( Availability )

##### 공격(Attack)? 위험(Risk)? 위협(Threat)?
 - 공격: 보안 서비스를 우회하거나 정책을 위반하기 위한 지능적인 행동
    - 내부(Inside)/외부(Outside)
    - 능동(Active)/수동(Passive)
 - 위험: 취약점에 의해 해로운 결과가 발생할 가능성
 - 위협: 잠재적인 보안의 침해


### Authentication
인증: 식별 + 입증

 - Identification
 - Verification

나는 Facebook 사용자 OOO다! 그걸 어떻게 믿어?

방법론
 - Password
 - Token
 - Biometric

#### Password
 - Hashed Password
 - [Bloom Filter](https://fylux.github.io/2017/03/19/Bloom-Filter/)

#### Token
 - ID/Magnetic Card

#### Biometric
 - Fingerprint(지문)
 - Signature(필체)
 - Face(얼굴 특징)
 - Voice(성문)
 - Iris(홍채)

### Access Control
접근 관리: 인증된 사용자(Authorized User)가 자원(Resource)에 접근해도 되는가?

사용자 단위, 직책(역할) 단위로 관리하기도.

#### File Access Control
 - 다시 살펴보는 [inode](https://en.wikipedia.org/wiki/Inode)
 - [`chmod` 명령](https://ko.wikipedia.org/wiki/Chmod)

일반적인 권한들
 - Read/Write
 - Execute
 - Create/Delete
 - Search

Q. Github SSH Key setup 해보기?

## OS Signal

기본적으로, 프로그램은 프로세스 바깥을 볼 수 없다는 한계가 존재. 프로세스간 **미리 정의된 이벤트** 통지하기 위한 방법으로 시그널 기능이 추가. 프로세스간 통신 수단. Trace, 예외처리에도 사용...

Q. 프로그램? 프로세스? 둘의 차이?

예외 처리에는 **강제 종료**를 포함. Ctrl + Alt + Delete 다들 아시죠? 프로세스 내에서 발생할 수 있는 최고 우선순위 이벤트.

기본적인 메커니즘? 
 - 커널에서 시그널 수신을 확인
    - 주기적(초 단위)으로 검사
    - 트랩처리
 - 현재 실행중인 프로세스 이외에는 시그널 수신을 확인하지 않는다

시그널 보내더라도 즉시 처리되지는 않는다. **모두 소프트웨어적인 처리** 

Q. 시스템 함수를 수행하는 도중에 시그널을 수신했다면?  
A. [`man errno`](http://man7.org/linux/man-pages/man3/errno.3.html)

앞서 OS 프로세스에서 Process Control Block의 존재에 대해서 배웠는데...

### RTFM
 - [UNIX Signal Table](http://people.cs.pitt.edu/~alanjawi/cs449/code/shell/UnixSignals.htm)
 - Linux Manual 
   - [Signal](http://man7.org/linux/man-pages/man7/signal.7.html)
   - [`kill`](http://man7.org/linux/man-pages/man2/kill.2.html) 
   - [`sigaction`](http://man7.org/linux/man-pages/man2/sigaction.2.html)
 - ISO Standard
   - [`#include <csignal>`](https://en.cppreference.com/w/cpp/header/csignal)
   - https://en.cppreference.com/w/cpp/utility/program


시그널 기본 행동 및 속성

| Action    | Desc | 
|-----------|------|
| Term      | 프로세스 종료([`_Exit`](https://en.cppreference.com/w/cpp/utility/program/_Exit))
| Ign       | 무시
| Core      | 코어 메모리 덤프
| Stop      | 프로세스 정지
| NoCatch   | 핸들러 변경 불가
| NoIgn     | 무시할 수 없음


### [Signal Handling](https://en.cppreference.com/w/cpp/utility/program/signal)

Q. 왜 이런 인터페이스(hooking)가 되었을까?


코드 출처: https://www.thegeekstuff.com/2012/03/catch-signals-sample-c-code/
```c++
#include <cstdio>
#include <csignal>

#include <unistd.h>

void sig_handler(int signo)
{
    if (signo == SIGUSR1)
        printf("received SIGUSR1\n");
    else if (signo == SIGKILL)
        printf("received SIGKILL\n");
    else if (signo == SIGSTOP)
        printf("received SIGSTOP\n");
}

int main(void)
{
    if (signal(SIGUSR1, sig_handler) == SIG_ERR)
        printf("\ncan't catch SIGUSR1\n");
    if (signal(SIGKILL, sig_handler) == SIG_ERR)
        printf("\ncan't catch SIGKILL\n");
    if (signal(SIGSTOP, sig_handler) == SIG_ERR)
        printf("\ncan't catch SIGSTOP\n");

    // A long long wait so that we can easily issue a signal to this process
    while(1) {
        sleep(1);
    }

    return 0;
}
```

C++ 을 조금 더 섞은 예시. 특정 scope 내에서만 핸들러를 사용하고 싶은 경우
```c++
#include <cassert>
#include <cstdio>
#include <csignal>

// - Note
//      Hijack signal handler in existing scope
template <int sig>
struct hook
{
    using handler = void (*)(int);
    handler task = nullptr;

    hook(handler proc) noexcept
    {
        task = std::signal(sig, proc);
        assert(task != SIG_ERR);
    }

    ~hook() noexcept
    {
        std::signal(sig, task);
    }
};

void my_handler(int);

void foo(void)
{
    auto hk = hook<SIGXXX>(my_handler);

    // ...
    // do something 
    // ...
}
```

Handler는 왜 이렇게 생겼을까? 

#### Advanced Handling

경우에 따라선 좀 더 세밀하게 제어해야 할수도 있는데... [`man sigaction`](http://man7.org/linux/man-pages/man2/sigaction.2.html)

```c++
#include <unistd.h>
#include <errno.h>
#include <signal.h>

void sa_handler_usr(int signum);

int main(void)
{
    struct sigaction sa_usr1{};
    struct sigaction sa_usr2{};

    sa_usr1.sa_handler = sa_handler_usr;
    sigfillset(&sa_usr1.sa_mask); 
    sigaction(SIGUSR1, &sa_usr1, nullptr);

    sa_usr2.sa_handler = sa_handler_usr;
    sigemptyset(&sa_usr2.sa_mask); 

    sigaction(SIGUSR2, &sa_usr2, nullptr);

    while(true)
    {
        // 시그널을 받을 때까지 멈춘다
        pause();
        printf("Received siganl...\n");
    }

    return EXIT_SUCCESS;
}

void sa_handler_usr(int signum)
{
    for(int i = 0; i< 10; i++)
    {
        printf("\t Signal : %d received \n", signum);
        sleep(1);
    }
}

```