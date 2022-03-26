# 네트워크(Network)_02

## Client-server architecture

### Server

-   Always-on host
-   **Permanent IP address**
-   Data centers for scaling
-   Server process: process that waits to be contacted

### Clients

-   Communicate with server
-   May be intermittently(간헐적으로) connected
-   May have dynamic IP addresses
-   Do not communicate directly with each other
-   Client process: process that initiates communication



## Processes communicating

### process

-   Program running within a host
    -   within same host, two processes communicate using **inter-process communication** (defined by OS)
    -   processes in different hosts communicate by exchanging **messages**

### Addressing processes

-   To receive messages, process must have **identifier**
-   Host device has unique 32-bit IP address
-   **Identifier** includes both **IP address** and **port numbers** associdated with process on host.
-   Example:
    -   HTTP server: 80
    -   mail server: 25



## Internet Transport Protocols Services

### TCP service:

-   **reliable transport** (말한 그대로 전달) between sending and receiving process
-   **flow control**: sender won't overwhelm receiver
-   **congestion control**: throttle sender when network overloaded
-   **does not provide**: timing, minimum throughput guarantee, security
-   **connection-oriented**: setup required between client and server processes



### UDP service:

-   **unreliable data transfer** between sending and receiving process
-   **does not provide**: reliability, flow control, congestion control, timing, throughput guarantee, security, or connection setup

|      Application       |     Application layer protocol      | Underlying transport protocol |
| :--------------------: | :---------------------------------: | :---------------------------: |
|         E-mail         |           SMTP [RFC 2821]           |              TCP              |
| Remote terminal access |          Telnet [RFC 854]           |              TCP              |
|          Web           |           HTTP [RFC 2616]           |              TCP              |
|     File transfer      |            FTP [RFC 959]            |              TCP              |
|  Streaming multimedia  | HTTP (e.g. YouTube), RTP [RFC 1889] |          TCP or UDP           |
|   Internet telephony   | SIP, RTP, proprietary (e.g. Skype)  |          TCP or UDP           |



## Chapter 2. Application Layer

-   네트워크란 컴퓨터에서 실행되는 프로세스가 다른 컴퓨터의 프로세스와 메시지를 주고받는 것. (interprocess간 통신)
-   이 chapter에서는 응용 계층만 볼 것이므로 패킷 유실을 극복하는 방법 등에 관해서는 생각하지 않는다. (계층화)
-   Socket이라는 OS의 system call을 이용한다.
-   IP address를 통해 machine을 지정하고, 이 안의 수많은 process의 socket을 port 번호를 이용해 특정한다.
-   Web browser가 DNS를 통해 `www.naver.com`의 IP 주소를 얻게되고, **80번 port에** 접근한다.



### Web and HTTP

-   **Web page** consists of **objects**
-   Object can be HTML file, JPEG file, Java applet, audio file...
-   Web page consists of **base HTML-file** which includes **several referenced objects**
-   Each object is addressable by a **URL**, e.g., `www.someschool.edu[host name]/someDept/pic.gif[path name]`

#### HTTP: hypertext transfer protocol

-   Web's application layer protocol
-   Client/server model

![image-20220326201706803](network_02.assets/image-20220326201706803.png)

-   HTTP는 TCP를 기반으로 동작 (유실되지 않는다.)
-   **uses TCP**:
    -   client initiates TCP connection (creates socket) to server, port 80
    -   server accepts TCP connection from client
    -   HTTP massages exchanged between browser (HTTP client) and Web server (HTTP server)
    -   TCP connection closed
-   **HTTP is "stateless"**
    -   server maintains no information about past client requests
    -   단순하기에 서버가 많은 사용자들을 처리할 수 있음



### HTTP connections

#### Non-persistent HTTP

-   매번 새로운 TCP 연결

-   **RTT (definition)**: time for a small packet to travel from client to server and back

-   **HTTP response time**:

    -   one RTT to tinitiate TCP connection
    -   one RTT for HTTP request and first few bytes of HTTP response to return
    -   file transmission time
    -   **non-persistent HTTP response time = 2RTT + file transmission time**

    ![image-20220326203312962](network_02.assets/image-20220326203312962.png)

#### Persistent HTTP

-   TCP 연결 재사용



### HTTP request message

-   ASCII (human-readable format)

```
GET /index.html HTTP/1.1\r\n
Host: www-net.cs.umass.edu\r\n
User-Agent: Firefox/3.6.10\r\n
...
```

### HTTP response message

```
HTTP/1.1 200 OK\r\n
...
```



### User-server state: cookies

![image-20220326205158371](network_02.assets/image-20220326205158371.png)



### Web caches (proxy server)

-   **Goal**: satisfy client request without involving origin server
-   Client는 응답을 빠르게 받는다.
-   Server는 부하가 적어진다.
-   Proxy server의 운영 주체는 나가는 트래픽이 적어진다. (사용료를 아낀다.)
-   Cach가 등장하면 항상 발생하는 문제: 일관성 (Origin에서의 업데이트가 즉각적으로 반영되지 않음)

![image-20220326205859745](network_02.assets/image-20220326205859745.png)



