
## HTTP
: 애플리케이션 계층으로서 웹서비스 통신에 사용

### HTTP/1.0
: 한 연결당 하나의 요청을 처리

RTT 증가를 불러일으킨다. (TCP의 3-way handshake 반복 떄문)

> **RTT**  
> : 패킷이 목적지에 도달하고 출발지로 다시 돌아오기까지 걸리는 **패킷 왕복 시간**

### HTTP/1.1
: 매번 TCP 연결을 발생시키지 않고 한 번 TCP 초기화 후 keep-alive라는 옵션으로 여러 개의 파일을 송수신 가능하도록 함

(HTTP/1.0에도 keep-alive가 존재는 했으나 표준화는 X. HTTP/1.1부터 표준화되어 기본 옵션이 됨)

> #### HOL Blocking
> : Head Of Line Blocking, 네트워크에서 같은 큐에 있는 패킷이 그 첫번째 패킷에 의해 지연될 때 발생하는 성능 저하 현상

### HTTP/2
: HTTP/1.x보다 지연 시간을 줄이고 응답 시간을 단축

멀티플렉싱, 헤더 압축, 서버 푸시, 요청의 우선순위 처리를 지원한다.

> #### 멀티플렉싱
> : 여러 개의 스트림을 사용하여 송수신함  
> 즉, 특정 스트림 패킷이 손실되어도 해당 스트림을 제외한 나머지 스트림에는 영향 X   
> 이를 통해 HTTP/1.x의 HOL Blocking 문제를 해결할 수 있다.  
> (한 가지 비유한 말을 보았는데 마치 한 명의 교사가 손을 든 여러 아이들의 질문을 받아준다는 비유가 있었다.)

> #### 허프만 코딩 압축 알고리즘
> : huffman coding, 헤더를 압축하는 방법 중 하나  
> 문자열을 문자 단위로 쪼개어 빈도수가 높은 정보는 적은 비트 수, 빈도수가 낮은 정보는 비트 수를 많이 사용하여 표현하는 방법이다.

### HTTPS
: 애플리케이션 계층과 전송 계층 사이 신뢰 계층인 SSL/TLS 계층을 넣은 **신뢰할 수 있는 HTTP 요청**

#### SSL/TLS
: SSL(Secure Socket Layer), TLS(Transport Layer Security Protocol)

이는 SSL에서 버전이 점점 올라가며 TLS 1.3까지 버전이 오르며 명칭이 TLS로 변경된 것이다.  
보통은 이를 합쳐서 SSL/TLS로 많이들 부른다.

SSL/TLS는 전송 계층에서 보안을 제공하는 프로토콜이다.  
(제 3자가 메시지를 도청하거나 변조하지 못하도록 함 - 인터셉트 방지)

> - **SEO**
> : Search Engine Optimization, 검색엔진 최적화  
> 예를 들어 구글에서 어떤 웹 사이트를 검색했을 때 그 결과를 페이지 상단엔 노출시켜 많은 사람들이 볼 수 있도록 최적화하는 방법을 뜻한다.

### HTTPS 구축 방법
HTTPS 구축 방법에는 세 가지가 있다.

1. CA에서 직접 구매한 인증키를 기반으로 구축
2. 서버 앞단의 HTTPS를 제공하는 로드밸런서를 두어 구축
3. 서버 앞단에 HTTPS를 제공하는 CDN을 두어 구축

### HTTP/3
: www에서 정보를 교환하는데 사용되는 HTTP 세번째 버전

#### HTTP/2 vs HTTP/3
HTTP/2는 TCP위에서 돌아가지만 HTTP/3는 QUIC라는 계층 위애서 돌아가며 TCP가 아닌 UDP 기반이다.  
(3-way handshake 과정 생략 가능)
