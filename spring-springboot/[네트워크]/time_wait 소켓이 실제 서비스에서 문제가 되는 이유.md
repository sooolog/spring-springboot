<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/161195102-0dd47b15-bbf4-49bd-85ac-d73c793fb9f3.png">
</p>

# 🗝 time_wait 소켓이 실제 서비스에서 문제가 되는 이유

* 실제 서비스에서의 time-wait 발생과 적용원리
* 실제 서비스에서 time-wait 소켓이 문제를 일으키는 이유
* [부록] Nginx, WAS(톰캣), 외부AI, 데이터베이스간 통신은 HTTP 통신일까 ?

> 모든 코드는 [깃헙](https://github.com/sooolog/dev-spring-springboot)에 작성되어 있습니다.

* * *

<br>



### 1.실제 서비스에서의 time-wait 발생과 작용원리

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/154788409-5bb4fd57-b393-4b59-b8d0-37ad4783ab36.png">
</p>

사실 TCP 통신에서 TIME_WAIT 상태의 소켓이 발생하는것은 자연스러운 현상이다.
하지만, 이게 문제가 되는것은 너무많은 time_wait 상태의 소켓이 생성되어있을 때이다.

어느쪽이든 Client 입장(요청하는 쪽)에선 요청을 보내기 위해서는 커널에서 할당 받은 
로컬 포트로 소켓 생성을 요청한다.

<br>

> 3-way handshake나 4-way handshake에서는 클라이언트와 서버단에 대한 설명을 마치 유저와 서비스 서버로 나누어 놓았지만, 이해상
> 이렇게 구분되어지게 설명된것이다. 실제로는 EC2인스턴스가 클라이언트단이 되고 유저들이 서버단이 될 수도 있다. 어떠한 단이 되는지에 대한
> 기준은 요청을 먼저 하는쪽이 클라이언트단, 받는쪽이 서버단이 되는것이다. 즉, EC2서버에서 먼저 요청을 보내겠다고 하면 EC2서버가 클라이언트단이 되는것이다. 
> 그 예로,      
> (1).톰캣서버(WAS)와 DB 연동     
> (2).톰캣서버(WAS)와 외부 API 연동        
> (3).Nginx와 톰캣서버(WAS) 연동    
> 의 경우 (1),(2)는 톰캣서버가 클라이언트단이 되고, (3)에서는 Nginx가 클라이언트단이 되는것이다.   
> [클라이언트단과 서버단의 개념](https://jojoldu.tistory.com/319)

<br>

> 혹시, TCP통신의 3-way handshake와 4-way handshake에 대해 모르고 있다면 [이 글](https://sooolog.dev/TCP-%ED%86%B5%EC%8B%A0%EA%B3%BC-3-way,4-way-handshake-%EA%B7%B8%EB%A6%AC%EA%B3%A0-time_wait-%EC%86%8C%EC%BC%93%EC%9D%98-%EA%B0%9C%EB%85%90/)을 읽고 오도록 하자.     

<br>

위에서 말한데로, client 입장(요청하는 쪽)에서 요청을 보낼때 소켓을 이용하는데, 
이 때 가용한 로컬포트 즉, 사용가능한 로컬포트를 커널에서 임의로 배정받아서 사용하게 된다.
(이때 배정되는 포트들은 서로 고유하다.)

문제는 여기서부터인데, 통신이 완전히 종료되고(여기서는 time_wait까지 완전히 종료되는것을 이야기한다.) 소켓이
커널로 돌아가기전까지는 해당 소켓에 할당된 고유 포트번호를 사용할 수가 없다는 것이다. 만약 time_wait 상태의 소켓이 많아지게 되면
사용가능한 로컬포트 또한 줄어들게 되고, 끝으로는 모든 로컬포트가 time_wait소켓에 묶여있어 더이상 할당할 수 있는
로컬포트가 없게되는경우에 새로운 통신을 위한 소켓을 생성할 수 없게된다. 그러면, 더 이상 클라이언트단과 서버단은 서로 통신을
할 수 없게되는 것이다.

<br>

> 또 한가지 알아야 할것은 이 time_wait 소켓은 클라이언트단이나 서버단이나 연결을 먼저 끊으려고 하는쪽에서
> 발생한다. 즉, 꼭 클라이언트단에서 생기는 소켓이 아니라는것이다. 하지만, 통상 클라이언트단에서 먼저 연결을 끊으려고 
> FIN을 서버단에 보내기에 대부분 클라이언트단에서 time_wait 소켓이 발생한다고 볼 수 있다.      
> [time_wait 소켓의 발생 (1)](https://puzzle-puzzle.tistory.com/entry/TIMEWAIT-%EC%86%8C%EC%BC%93%EC%9D%B4-%EC%84%9C%EB%B9%84%EC%8A%A4%EC%97%90-%EB%AF%B8%EC%B9%98%EB%8A%94-%EC%98%81%ED%96%A5)     
> [time_wait 소켓의 발생 (2)](https://jojoldu.tistory.com/319)

<br>

> 예외적으로, HTTP 기반의 통신의 경우는 대부분 서버가 먼저 연결을 끊는 경우가 많기 때문에 서버에서 TIME_WAIT가 생긴다.
> 하지만, 주로 time_wait로 인해서 서비스에 문제가 생기는 경우는 로컬포트를 할당받은 클라이언트단에서 time_wait 발생으로
> 인해 로컬포트 할당 부족현상이니 클라이언트에서 발생하는 time_wait 소켓외에는 논외로 해두겠다.     
> [HTTP 통신과 서버단에서의 time_wait 발생](https://sunyzero.tistory.com/198)     
> [time_wait 소켓과 서버단, 그리고 HTTP 기반 통신](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=hanajava&logNo=221937962269)     

<br>

> [time_wait 소켓이 문제가 되는 이유](https://jojoldu.tistory.com/319)     
> [time_wait과 로컬포트에 관해](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=hanajava&logNo=221937962269)

<br>

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/156504161-6c477067-119e-43c2-9127-764c9fb963da.png">
</p>

이해를 돕기위한 실제 예를 보여주도록 하겠다. 필자는 빈스톡과 Nginx를 사용하여 스프링부트 톰캣(내장)과
연동하여 사용했다.

위의 자료는 실제 Nginx가 클라이언트단이 되고, 서버단은 톰캣(WAS)이 되어서 통신하는 과정을 나타낸다.
time_wait 소켓이 발생한 목록을 보면, 왼쪽은 127.0.0.1:고유포트번호이고 오른쪽은 127.0.0.1:8080이다. 
왼쪽이 클라이언트단인 Nginx의 소켓 고유 포트번호와 ip주소이고 오른쪽 127.0.0.1:8080이 스프링부트 어플리케이션이 
실행중인 포트번호이다.

해당 time_wait 소켓(클라이언트단의 소켓)이 사용하는 포트번호가 모두 다른것을 볼 수 있다.

<br>

> 위의 연동과정은 하나의 EC2내에 리버스프록시로써 Nginx을 사용하고 외부에서 받은 80포트의 통신을
> Nginx가 앞단에서 받아 이를 WAS(톰캣)으로 연결시켜주는 과정을 의미한다.

<br>

> 양쪽 다 ip주소가 127.0.0.1인 이유는 같은 EC2 인스턴스 내에서 이루어지는 통신이기 때문이다.

<br>

> [Nginx와 내장톰캣의 통신으로 본 ip주소와 포트번호 예시들](https://jojoldu.tistory.com/319)

<br>




### 2.실제 서비스에서 time-wait 소켓이 문제를 일으키는 이유

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/156504161-6c477067-119e-43c2-9127-764c9fb963da.png">
</p>

위의 그림을 다시보면, 클라이언트(Nginx)단에서 로컬포트가 계속해서 사용되고 있는것이 보인다.
통신을 할 때, 로컬포트는 한 연결당 한 번 밖에 쓰이지 못한다.(그래서, 클라이언트와 서버단이 연결될 때
소켓의 출발지 IP,PORT - 목적지 IP,PORT가 연결당 유일하게 존재하게 되는거다.)

계속해서 소켓끼리 연결이 되었다가 끊어지고 다시 time_wait 상태에 빠지게된다. 하지만, time_wait상태에
빠진경우는 해당 소켓에 있는 로컬포트는 커널로 되돌아간것이 아니기 때문에 재사용이 불가한 상태다. 즉, 새로운
연결을 할 경우 다른 남아있는 로컬포트중에서 끌어다가 사용해야 한다. 이때 문제가 발생한다.

로컬포트는 무한정 있는게 아니다. 임의로 사용가능한 로컬 포트 범위를 확장시켜주어도
10240 ~ 65535사이에 있는 포트를 사용하기에 이 포트마저도 고갈이 되버리면 더이상
연결을 진행할 수 없게 되는것이다.

<br>

> [timew_wait 소켓이 문제를 일으키는 이유](https://jojoldu.tistory.com/319?category=777282)

<br>

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/160988751-4735b191-b6c4-40be-8d60-73adae1b15a7.png">
</p>

필자가 EC2 내부에 있는 Nginx와 WAS(톰캣)을 사용을 예로 들었었는데,
실제로 이러한 연결말고도 흔히 time_wait가 발생할 수 있는경우가 두가지
경우가 더 있다.

위에서 말했다시피,    
* Tomcat 서버와 DB의 연동
* Tomcat 서버와 외부 API의 연동
* Nginx와 Tomcat의 연동    
가 있다.

이 중에서 세번째는 우리가 방금 본 경우이고, Tomcat 즉, WAS에서 데이터베이스에 연결한다던지,
아니면 WAS에서 외부 API에 요청을 보내는 경우 모두 time_wait 소켓 발생으로 문제가 발생할 수 있는
경우들이다.(왜냐면 위 셋 모두 클라이언트단이 EC2 인스턴스 내부에 있는 Nginx 혹은 WAS이며 이로인해
로컬포트 부족발생 현상이 나타날 수 있기 때문이다.)

내장톰캣 기반인 스프링부트 프로젝트를 예로 들어서 설명을 했으나 기존의 노드를 사용하더라도 위의
개념은 동일하게 적용이 되니 꼭 알아두도록 하자.

<br>

> 추가로, 우리가 브라우저에 특정 URL을 입력하여 서버에 접근할때는 (위와같이 EC2에 배포된 스프링부트 프로젝트(WAS)와
> 리버스 프록시인 Nginx를 사용한다고 보겠다.) Nginx에서 WAS로 연결할때의 time_wait 소켓을 고려해야하는것을 물론이고
> 동시에 WAS에서 API서버, 아니면 WAS에서 데이터베이스에 연결할 때 생성되는 time_wait도 신경써야 한다. 즉, 더 풀어말하자면 우리가
> 웹서비스에서 정적파일을 요구하는 단순 요청을 보낼수도 있으나 실제로는 정적파일 외에 추가로 동적자원에 대한 요청을 보낼 수 있다. 이 과정에서
> 외부 API를 WAS에서 요청을 보내야 하는 경우도 있고 동시에 WAS에서 데이터베이스로 특정 자료를 요청할 수도 있다. 그렇기 때문에 브라우저에서 단 한번의
> 요청을 하더라도 실제로는 여러 연결을 동시에 고려해서 time_Wait 소켓을 관리해야 한다는 것이다.(결국은 이 모든게 EC2 인스턴스가 감수해야하는 부분인것이다.)

<br>

> [timew_wait 소켓이 문제를 일으키는 이유](https://jojoldu.tistory.com/319?category=777282)

<br>



### 3.[부록] time_wait 소켓이 발생하는 Nginx, WAS, 데이터베이스, 외부API간의 통신, HTTP통신일까?

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/161027181-b39759d8-4c75-4f30-959c-dd3938ccac3f.png">
</p>

바로 위에서 우리는 Nginx, WAS, 데이터베이스, 외부API간의 통신에서 time_wait 소켓이 발생하고
이를 통해서 문제가 된다는것을 알았다. 그렇다면 과연 이들간의 통신은 HTTP 아래에서 이루어지는 통신일까?

답은 아니다 이다.     
우선, time_wait 소켓은 TCP 통신기반(3-way handshake, 4-way handshake)에서 발생하는 소켓 상태이다.
그러나, 기본적으로 HTTP 통신은 TCP 기반으로 이루어져 있기에 time_wait 소켓이 발생하는 것이고 그 외에도 다양한
통신들이 TCP 통신기반으로 이루어지기 때문에 HTTP통신이 아니여도 time_wait 소켓이 발생하게 되는것이다. 

<br>

> HTTP 통신은 포트80, HTTPS 통신은 포트 443에서 이루어진다. Nginx가 WAS에 접근할때는 8080포트(물론 이것이 고정값은 아니다.)
> WAS에서 데이터베이스에 접근할때는 3306포트를 이용하기 때문에 HTTP 규약 아래에서 이루어지는 통신이라고는 볼 수 없다. 또한, HTTP 통신이라면
> 요청과 응답에 기본적으로 정해진 구조를 갖추고 있어야 한다.     
> [각 프로토콜 별 기본 포트](https://iamfreeman.tistory.com/entry/%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-%EA%B8%B0%EB%B3%B8-%ED%8F%AC%ED%8A%B8-%EB%AA%A9%EB%A1%9D-Common-TCPIP-Protocols-and-Ports)     
> [HTTP 통신이라면 요청과 응답에 갖추어야 할 기본 구조 (1)](https://leemyungjic.tistory.com/13)     
> [HTTP 통신이라면 요청과 응답에 갖추어야 할 기본 구조 (2)](https://velog.io/@doomchit_3/Internet-HTTP-%EA%B0%9C%EB%85%90%EC%B0%A8%EB%A0%B7-IMBETPY)

<br>

> HTTP통신과 TCP통신에 관해 자세히 알고싶다면 [해당 글](https://sooolog.dev/HTTP-%ED%86%B5%EC%8B%A0%EA%B3%BC-TCP-%ED%86%B5%EC%8B%A0-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%9B%B9-%EC%86%8C%EC%BC%93%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC/)을 참조하도록 하며, 3-way handshake와 4-way handshake에 관해서 알고싶다면
> [해당 글](https://sooolog.dev/TCP-%ED%86%B5%EC%8B%A0%EA%B3%BC-3-way,4-way-handshake-%EA%B7%B8%EB%A6%AC%EA%B3%A0-time_wait-%EC%86%8C%EC%BC%93%EC%9D%98-%EA%B0%9C%EB%85%90/)을 참조하도록 하자.

<br>



### 🚀 추가로, time_wait 소켓이 발생하여 문제가 생길경우 어떻게 튜닝작업을 할것인지 방안은 무엇인지에 대해서는 실제 스프링부트 프로젝트를 다루면서 [성능튜닝] 카테고리에서 다루도록 하겠다.

<br>



태그 : #time_wait 소켓, #커널, #로컬포트, #HTTP, #TCP