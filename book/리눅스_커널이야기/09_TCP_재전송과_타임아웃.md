# 09. TCP 재전송과 타임아웃

아래 그림을 새 창에 띄워놓고 같이보자!

![TCP Flow](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile2.uf.tistory.com%2Fimage%2F9910A8345BB0B75F2A0A82)

## TCP 재전송

TCP는 "신뢰성 있는 연결"이다.

* TCP는 (UDP와 다르게) 두 종단간 데이터 주고받음이 확실해야한다.
* 보낸 쪽에서 상대방이 받았다는 응답을 보내야만 나머지를 보낼 수 있다.
* 패킷을 보낸 후 응답패킷(ACK)을 받지 못하면 패킷이 유실되었다고 판단하고 보냈던 패킷을 재전송한다.

![TCP Retransmission](https://assets.extrahop.com/images/infographics/TCP-RTO-Retransmission-Timeout-Diagram.jpg)

### RTO 

* Retransmission Timeout
* "ACK를 얼마나 기다려야하는 지"에 대한 값이다.
* RTO 안에 ACK를 받지 못하면 보내는 쪽에서 재전송을 진행한다.

RTO에는 일반적인 RTO와 InitRTO 두가지가 있다.

1. 일반적인 RTO

   * 두 종단간 패킷전송에 필요한 시간(RoundTripTime)을 기준으로 설정된다.
   * 예를들어 RTT=1초이면 최소 1초는 기다려야 내가보낸 패킷이 손실되었는지 아닌지 판단할 수 있다.  
   
2. InitRTO: TCP Handshake가 일어나는 첫번째 SYN 패킷에 대한 RTO를 의미한다. 

   * 맨 처음 연결할때는 두 종단간 RTT시간을 알 수 없기때문에 임의로 설정한 값으로 RTO를 계산한다. 이때의 RTO를 InitRTO라 한다.
   * 리눅스에서는 소스코드에 1초로 구현해놓았다. 

> ★리눅스에서는 ss 명령을 이용해 현재 설정되어있는 세션의 RTO 값을 확인할 수 있다. (p.183)
> 
> * 한번 ACK를 못받으면 재전송이 계속해서 일어난다.
> * 재전송 텀(타이머): RTO 값은 초기값을 기준으로 2배씩 증가한다. 
> * 재전송 횟수: 커널 파라미터값을 통해 결정된다.

### 재전송 관련 커널 파라미터


```
net.ipv4.tcp_syn_retries = 5
net.ipv4.tcp_synack_retries = 5
net.ipv4.tcp_retries1 = 3
net.ipv4.tcp_retries2 = 15
net.ipv4.tcp_orphan_retries = 0
```

1. `net.ipv4.tcp_syn_retries = 5`

   * SYN 재전송 횟수를 결정한다. (최초 연결을 시도하는 과정)
   * 기본값: 5

2. `net.ipv4.tcp_synack_retries = 5`

   * SYN+ACK 재전송 횟수를 결정한다. 
   * 기본값: 5 

3. `net.ipv4.tcp_orphan_retries = 0`

   * orphan socket의 재전송 횟수를 결정한다.
   * 사실 FIN을 보낸 후 아주 짧은 시간에 `FIN_WAIT1`, `FIN_WAIT2`, `TIME_WAIT` 상태가 된다.
   * orphan socket 상태에서 `net.ipv4.tcp_orphan_retries` 횟수만큼 재전송을 시도하고 그 후에도 응답을 받지 못하면 이미 죽은 소켓으로 판단하여 커널이 소켓을 강제로 회수해버린다.
      * `FIN_WAIT2`, `TIME_WAIT` 상태도 거치지 않는다.
   * 권장값: 7
      * 값이 너무 작으면 FIN 소켓이 유실된 상태에서 `FIN_WAIT1` 소켓이 너무 빨리 정리될 수 있으며, 상대편에 닫혀야 하는 소켓이 닫히지 않는 결과를 초래할 수도 있다. 
 
> orphan(고아) socket 이란?
>  
> TCP가 연결을 끊는 과정(그림)을 살펴보자. 
> 
> * FIN을 보내고 해당소켓은 `FIN_WAIT1` 상태가 된다.
> * 이때부터 소켓은 특정 프로세스에 할당되지 않고 커널에 귀속되어 정리되기를 기다린다.
> 
> 특정 프로세스에 할당되지않고 커널에 귀속되어 정리되기를 기다리는 소켓 중 `FIN_WAIT1` 상태의 소켓을 orphan socket이라 한다. 
> 
> 왜 `FIN_WAIT2`, `TIME_WAIT`는 아니고 `FIN_WAIT1` 상태만 orphan socket이라 할까?
> 
> * TCP가 연결을 끊는 과정(그림)을 살펴보면 연결을 끊을 때 자신이 보내는 마지막 패킷이 FIN이다. 그 이후로는 상대방으로부터 받는 패킷만 있다.
> * 내가 보내는 패킷에 대해 재전송하는것이기 때문에 `FIN_WAIT1` 상태의 소켓만 해당된다.

TCP 재전송 임계치 값으로 두 개의 값을 가지고있다. 

4. `net.ipv4.tcp_retries1 = 3`

   * IP 레이어에 네트워크가 잘못되었는지 확인하도록 사인을 보내는 기준.
   * soft threhold(기준점)

5. `net.ipv4.tcp_retries2 = 15`

   * 더이상 통신을 할 수 없다고 판단하는 기준.
   * hard threhold
   * 결과적으로는 이 값에 정의된 횟수만큼을 넘겨야 실제 연결이 끊긴다.

### 재전송 추적하기

TCP 재전송이 일어나는지 여부를 어떻게 추적할 수 있을까?

1. 가장 좋은 방법은 의심되는 서버에서 tcpdump를 추출하는것이지만, 너무 많은 패킷이 잡혀서 파악이 힘들 수 있다.
2. tcpretrans 스크립트를 활용하는 방법이 있다.

> tcpretrans 스크립트로 재전송이 일어나는 패킷 추적하기 (p.193)

### RTO_MIN 값 변경하기

커널 소스코드에 `TCO_RTO_MIN=200ms`로 설정되어있기 때문에 아무리 RTT 값이 작은(=빠른) 내부통신의 경우에도 RTO값은 200ms 밑으로 내려갈 수 없다.

> ★ss 명령으로 RTO, RTT 정보 확인하기 (p.194)

리눅스에 있는 ip route 명령의 `rto_min` 옵션을 통해 RTO 최솟값을 `TCO_RTO_MIN`보다 작게 바꿀 수 있다. 세션별로 바꿀 수는 없고 하나의 네트워크 디바이스를 기준으로 바꿀 수 있다.

> ip route `rto_min` 옵션으로 RTO 최솟값 바꾸기 (p.195)

`rto_min` 값이 어느정도면 적당한가에 대한 정답은 없다.

* 외부에 노출된 웹서버는 다양한 고객들이 접근하기 때문에 기본값으로 정해진 200ms를 따르는게 좋겠지만, 내부와 통신하는 서버에서는 200ms 라는 값이 길게 느껴지는것이 사실이다.
* 내부 통신 RTT는 매우 짧기 때문에 좀더 빠른 재전송이 필요한지 확인하고, `rto_min`값을 낮춰서 빨리 보내는것이 서비스 품질을 높일 수 있는 좋은 방법이다. 
* 하지만 이 값이 너무 낮다면 너무 잦은 재전송이 일어날 수도 있기 때문에 신중해야한다. 

## 애플리케이션 타임아웃

TCP 재전송이 발생하면 애플리케이션에서는 요청한 내용을 전달받지 못했기 때문에 타임아웃이 발생한다.

하지만 **타임아웃의 임계치를 몇초로 설정했느냐**에 따라 타임아웃이 발생할수도, 발생하지 않을수도 있다.

애플리케이션에서 발생할 수 있는 타임아웃은 크게 두종류가있다.

1. Connection Timeout
2. Read Timeout

### Connection Timeout

connection 과정에서 Timeout이 발생했다.

* 최초 TCP Handshake 과정에서 실패한것이다.
* SYN 패킷 혹은 SYN+ACK 패킷 중 하나가 유실되어서 재전송일 일어날 때 발생할 수 있다.

Handshake(SYN→SYN+ACK→AKC)과정의 실패라고 했는데, 왜 마지막 ACK 패킷의 유실은 포함되지 않을까?

* SYN, SYN+ACK 패킷은 종단에 대한 정보가 없기 때문에 RTO를 계산하기위한 RTT 값을 구할 수 없다. 그렇기 때문에 기본 1초로 설정되어있다.
* 하지만 SYN, SYN+ACK를 주고받은 후에는 종단에 대한 정보가 생기기 때문에 해당 패킷에 대한 RTT값을 측정할 수 있게 되고, 이때부터는 RTO가 계산된다.
* 그래서 Connection Timeout은 SYN, SYN+ACK 유실에서 발생한다.

### Read Timeout

(이미 연결되어있는 세션을 통해) 데이터를 읽으려고 하다가 타임아웃이 발생했다.

* 주로 커넥션 풀 방식을 이용해서 특정 서버와 다수의 네트워크 세션을 만들어놓은 상태에서 발생하는 타임아웃이다. 

### 각각의 값은 어느정도로 하는 것이 좋을까?

Timeout이 언제 발생하느냐를 이해해야 적절한 값을 설정할 수 있다.

#### Connection Timeout은 어느정도가 적당할까?

만약 Connection Timeout을 1초로 설정한다면?

* SYN, SYN+ACK 패킷의 재전송은 무조건 1초 정도가 필요하다. 
* 그렇다면 단 한번의 SYN, SYN+ACK 패킷 유실은 무조건 타임아웃을 일으키게 된다.
* 따라서 한번의 재전송 정도는 커버할 수 있도록 1초보다는 큰 값을 설정하는게 좋다.

Connection Timeout은 **대체로 3초로 설정하는것이 좋다.**

* 내가 보낸 SYN 재전송(1초)에 상대방의 SYN+ACK 재전송(1초) 를 더한 2초 보다 큰 값으로 설정해야 불필요한 타임아웃 에러메시지를 줄일 수 있기 때문이다. 

#### Read Timeout은 어느정도가 적당할까?

이미 맺어져있는 세션에서 패킷을 읽어오다가 타임아웃이 발생하기 때문에, Connection Timeout과 마찬가지로 한번의 재전송정도는 커버할 수 있는 값으로 설정해야한다. 

**일반적으로 300ms 정도로 설정한다.**

* `rto_min`값이 200ms이고, 맺어져있는 세션이 재전송할때 최소 200ms의 시간이 필요하기 때문이다.
* 만약 200ms보다 작은 값을 설정하면 단 한번의 재전송에도 타임아웃 메시지가 발생한다. 
* 300ms 정도로 설정하면 한번의 재전송은 커버하고 두번 이상의 연속된 재전송은 잡아서 타임아웃 메시지를 출력하게 된다.

만약 RTT가 길어서 RTO가 200ms 이상이라면 300ms보다 더 크게 설정해야한다. 하지만 대부분 내부서버끼리의 통신의 경우 RTT가 짧기 때문에 RTO가 200ms 보다 커지는 경우는 없을것이다.

## 요약

1. TCP 재전송은 RTO를 기준으로 발생하며, RTO 동안 응답을 받지 못하면 재전송이 발생한다.
2. RTO는 RTT 값을 기반으로 동적으로 생성된다. 
3. TCP Handshake 구간에서 설정되는 RTO는 InitRTO라 부르며, 리눅스에서는 1초가 기본이다.
4. RTO는 초기 설정값에서 2배수씩 증가한다.
5. `net.ipv4.tcp_syn_retries = 5`, `net.ipv4.tcp_synack_retries = 5`: SYN, SYN+ACK 재전송 횟수. TCP Handshake할때 적용되는 값이다.
6. `net.ipv4.tcp_orphan_retries = 0`: FIN 패킷 전송 후 `FIN_WAIT1`상태가 되는 소켓이 FIN 패킷을 몇번 재전송할지를 결정한다. 0으로 설정한다고 Disable 되지는 않는다.
7. `net.ipv4.tcp_retries1 = 3`, `net.ipv4.tcp_retries2 = 15`: TCP 재전송의 soft, hard 임계치다. 실제 연결은 `net.ipv4.tcp_retries2 = 15`횟수를 넘겼을 때 종료된다.
8. TCP 재전송이 발생하면 애플리케이션에서도 타임아웃이 발생할 수 있다. 
9. 애플리케이션 타임아웃은 Connection Timeout(보통 3초), Read Timeout(보통 300ms)이 있다.
10. 애플리케이션 타임아웃 값은 최소 한번의 재전송은 견딜 수 있도록 설정하는것이 좋다.