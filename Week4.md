### 추천 자료
 - [언제나 휴일](http://ehpub.co.kr/)

## Inter Process Communication
[프로세스간 통신](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4_%EA%B0%84_%ED%86%B5%EC%8B%A0)

 - Signal 되짚어보기
 - Signal의 한계

복잡한, 혹은 변화하는 통신 방법으로는 부적합하다. 중요한 건 데이터.
그냥 메모리를 공유하면 좋겠지만 그건 불가능...

프로그램에게 가장 기본적인 입출력?
 - Standard I/O: stdin, stdout, stderr
 - File I/O

### Pipe
입력을 꼭 키보드로만 하라는 법은 없다....  표준 입출력이란 것도 사실 미리 준비된 [File Descriptor](https://en.wikipedia.org/wiki/File_descriptor)

그러면 입력/출력을 위한 file descriptor를 2개 만들면 되지 않을까? 
 - [Pipe Overview](http://man7.org/linux/man-pages/man7/pipe.7.html)
 - [`pipe` Manual](http://man7.org/linux/man-pages/man2/pipe.2.html)


#### Shell Pipeline
 - https://en.wikipedia.org/wiki/Pipeline_(Unix)
 - 프로그램에서 이런 표현식을 쓰기도... [C++ Ranges](https://github.com/CppKorea/CppKoreaSeminar2nd/tree/master/03%20-%20Ranges%20for%20The%20C%2B%2B%20Standard%20Library )


### [Shared Memory](https://en.wikipedia.org/wiki/Shared_memory)
앞서서 본 Pipe는 1:1 통신. 다수의 프로세스가 통신하려면 좀 다른 방법이 필요하지 않을까? 
Pipe와 같이 파일을 만들어서 통신하는 방식. 하지만 프로세스들이 **이름**을 통해서 접근한다는 점이 다르다.

 - [Shared Memory Overview](http://man7.org/linux/man-pages/man7/shm_overview.7.html)
 - [`shm_open`](http://man7.org/linux/man-pages/man3/shm_open.3.html)
 - [`memfd_create`](http://man7.org/linux/man-pages/man2/memfd_create.2.html)


## Network. Park 1
Socket API를 이해하기 위해 앞서서 알아볼 것들...

 - [OSI 7 Layer](https://www.joinc.co.kr/w/Site/TCP_IP/OSI7Layer):  **A**ll **P**eople **S**eems **T**o **N**eed **D**ata **P**rocessing https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%EB%84%B7_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C_%EC%8A%A4%EC%9C%84%ED%8A%B8
 - 패킷 스위칭 Packet Switching
 - 흐름제어 Flow Control

Q. 컴퓨터들의 통신과 사람의 전화통화는 어떤 점이 다를까?

#### 참고자료
 - [New 알기쉬운 TCP/IP](http://www.infopub.co.kr/bookinfo/bookinfo.asp?sku=08000022)
 - https://cohesive.net/2017/09/4-things-everyone-should-know-about-network-layers.html
 - [RFC 1180: TCP/IP Tutorial](https://tools.ietf.org/html/rfc1180)
 
#### 친숙한 용어들
 - Domain Name System (DNS)
 - [Protocol Stack](https://tools.ietf.org/html/rfc793)
 - Network Topology
 - Internet Protocol (IP)
 - Router
 - MAC address
 - Dynamic Host Configuration Protocol (DHCP)

### Data Link Layer
모든 네트워크 인터페이스들은 고유 주소, [MAC Address](https://ko.wikipedia.org/wiki/MAC_%EC%A3%BC%EC%86%8C)를 가지고 있다. (ROM에 쓰여있음)

#### ARP & RARP
 - Address Resolution Protocol 
 - `arp` 명령: `-a`

Topology 내에서 각 Machine의 식별. MAC Address를 어떻게 하면 쉽게 알아낼 수 있을까?, ARP Broadcast와 ARP Reply 

### Network Layer
[Internet Protocol](https://tools.ietf.org/html/rfc791)을 통해서 하려는 것은 Packet을 상대 Host에게 전달하는 것. 

 - 상대 Host Machine을 어떻게 구분할 수 있을까? 주소체계의 필요성
 - 데이터를 어떻게 표현할까? 패킷 구조의 정의

#### IP 주소
 - IPv4: [4 byte 주소](https://en.wikipedia.org/wiki/IPv4) + Subnet mask
 - IPv6: [16 byte 주소](https://en.wikipedia.org/wiki/IPv6)
 - `ipconfig`/`ifconfig`

##### Routing Table
 - `route` 명령
 - `tracert` 명령

> 라우팅 테이블의 구조

#### Packet

패킷 구조를 보기 전에 역시 용어를 살펴봐야 하는데...
 - [RFC 1122](https://tools.ietf.org/html/rfc1122) Terminology Section
 - [최대 전송 단위 (Maximum Transmission Unit)](https://ko.wikipedia.org/wiki/%EC%B5%9C%EB%8C%80_%EC%A0%84%EC%86%A1_%EB%8B%A8%EC%9C%84)
 - Maximum Segment Size of TCP


### [Transport Layer](https://en.wikipedia.org/wiki/Transport_layer)
IP를 사용해서 Host Machine까지 패킷을 전달한 이후, 그 패킷을 **프로세스**에게 어떻게 전달할 것인가? 재전송이 필요한가? 부분적 유실은 어떻게 처리하는가? 이 파트를 시작할때 Flow control에 대해 설명했었는데, 그 처리를 여기서... 

 - Port와 Socket의 관계
 - `netstat` 명령

#### TCP

 - [혼잡 방지](https://ko.wikipedia.org/wiki/TCP_%ED%98%BC%EC%9E%A1_%EB%B0%A9%EC%A7%80_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98) [Congestion Control of TCP](https://en.wikipedia.org/wiki/TCP_congestion_control)
 - 3-Way Handshake 
 - ACK, S-ACK

#### UDP
모든 통신이 연결을 필요로 하는 것은 아니다... Multicast를 지원할 필요성도 존재. 

 - Unicast, Multicast, Broadcast의 차이점?
 - [위키피디아: 사용자 데이터그램 프로토콜](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9A%A9%EC%9E%90_%EB%8D%B0%EC%9D%B4%ED%84%B0%EA%B7%B8%EB%9E%A8_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)
 - [RFC 768: User Datagram Protocol](https://tools.ietf.org/html/rfc768)
 - [RFC 8085: UDP 사용 가이드](https://tools.ietf.org/html/rfc8085)

