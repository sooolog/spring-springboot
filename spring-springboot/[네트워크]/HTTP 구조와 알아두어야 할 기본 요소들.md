<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/161483356-b3db1b9f-f64f-46cb-97cb-2c0f76995989.png">
</p>

# 🎈 HTTP 구조와 알아두어야 할 기본 요소들

* HTTP에 대한 기본 내용
* HTTP Request와 HTTP Response 구조
* HTTP 메소드들과 Status Code들

> 모든 코드는 [깃헙](https://github.com/sooolog/dev-spring-springboot)에 작성되어 있습니다.

* * *

<br>



### 1.HTTP에 대한 기본 내용

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/161484168-8ce33c96-9910-4ab0-888c-59b23c4ee85a.png">
</p>

HTTP(HyperText Transfer Protocol) 란?         
* HTML(HyperText Markup Language)를 주고받기 위해 만들어진 규약(Protocol)이다.
* 80번 포트를 사용하는 통신 프로토콜이다.
* 비연결성(Connectionless)이자 무상태성(Stateless)이다.
* 비연결성과 무상태성의 단점을 극복하고자 쿠키(Cookie)와 세션(Session)을 사용한다.
* 도메인 + 자원위치(URL), 도메인 + 자원의 식별자(URI) 를 통해서 요청을 하고, 서버가 요청에 따른 HTML 문서응답을 해준다.
* HTML 문서 외에 JSON이나 XML도 주고받을 수 있다.
* HTTP 통신은 요청(Request)과 응답(Response)으로 이루어진다.

<br>

> [HTTP에 대한 기본 내용 (1)](https://velog.io/@doomchit_3/Internet-HTTP-%EA%B0%9C%EB%85%90%EC%B0%A8%EB%A0%B7-IMBETPY)        
> [HTTP에 대한 기본 내용 (2)](https://velog.io/@sehy/Http)      
> [HTTP에 대한 기본 내용 (3)](https://velog.io/@teddybearjung/HTTP-%EA%B5%AC%EC%A1%B0-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%9A%94%EC%86%8C)

<br>

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/161544914-03ff6e7d-8c0d-435d-9d03-765cd80d199b.png">
</p>

비연결성(Connectionless)은 클라이언트와 서버가 한 번 연결을 맺은 후, 클라이언트의 요청에 대해 서버가 응답을 마치면
맺었던 연결을 끊어버리는 특성을 얘기한다.(즉, 서버단이 응답을 마치면 연결이 유지되는게 아니라 바로 끊어지게 되는것을 의미)
또한, HTTP의 무상태성(Stateless)은 통신을 진행할 때 혹은 하고 나서 서버단이 클라이언트단에 대한 상태(다른말로 하면 정보)를 보존하도록 해주는
기능을 제공해주지 않는것을 의미한다. 그렇기 때문에 HTTP 요청을 진행할 때 마다 매번 새로운 요청을 진행하게 되고, 서버단은
같은 클라이언트로부터 요청이 반복적으로 들어와도 해당 클라이언트에 대한 정보를 갖고있지않기에 식별할 수 게 된다.      

실제 예를 보면 더 쉽게 이해된다.    
1.쇼핑몰에 접속    
2.로그인      
3.상품 클릭 -> 상세화면으로 이동     
4.로그인      
5.주문     
6.로그인      
7.....    

이러한 상황을 만들지 않기 위해서 우리는 쿠키와 세션을 사용하여 서버단이 HTTP 요청이 왔을때 특정 클라이언트에 대해
식별할 수 있게 해준다.

<br>

> 네트워크 파트의 쿠키와 세션 개념 글에서 자세하게 다루겠지만, 간단하게만 짚고 넘어가자면 쿠키는 클라이언트단인 브라우저에
> 사용자 정보를 저장하고 이를 HTTP 요청을 보낼 때 함께 보내게하여 서버단에서 해당 정보를 받아볼 수 있도록 한다. 이에 반해,
> 세션은 쿠키를 기반으로 사용되지만 사용자 정보를 브라우저에 저장하는 쿠키와 달리 세션은 서버 측에서 관리한다. 즉,
> 조금 더 쉽게 얘기하자면, session은 비밀번호와 같은 인증 정보를 쿠키에 저장하지 않고, 대신에 사용자 식별자인 session ID를
> 브라우저 쿠키에 저장한다. 이후 서버에 HTTP 요청을 보낼때 session ID를 서버가 받아보게 되고 해당 session ID와 부합하는 것이
> 서버단의 세션에 있으면 서버는 해당 세션의 정보들을 사용하게 된다. 만약 이해를 위해 조금 더 구체적인 예시가 필요하다면 [해당글](https://interconnection.tistory.com/74)을
> 읽어보도록 하자.      
> [쿠키와 세션 (1)](https://velog.io/@cjung5318/%EC%BF%A0%ED%82%A4-%EC%84%B8%EC%85%98-%ED%86%A0%ED%81%B0%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)          
> [쿠키와 세션 (2)](https://interconnection.tistory.com/74)

<br>

> [HTTP의 비연결성과 무상태성 (1)](https://victorydntmd.tistory.com/286)     
> [HTTP의 비연결성과 무상태성 (2)](https://minjoon950425.tistory.com/109)

<br>



### 2.HTTP Request와 HTTP Response 구조

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/161484177-a4fc147c-b430-4636-b31d-a6c98e1fbd33.png">
</p>

위에서 보았듯이, HTTP 요청은 Request(요청)와 Response(응답)로 이루어져 있다.
이제부터 이 HTTP Request(요청)와 HTTP Response(응답)의 구조와 그 안의 요소들에 대해
알아보도록 하겠다.

제일 먼저 알아볼 것은 패킷이다.
패킷은 간단하게 말해서, 데이터를 담은 블록이라고 보면 된다. 패킷 방식의 네트워크 통신에서는
데이터를 주고받을 때 이 패킷을 사용하여 주고 받는다. HTTP 통신 또한 이 패킷을 이용한다.
조금 더 쉽게 말하자면, HTTP 통신시 전송되는 데이터들이 이 패킷이라는 블록안에서 정해진 형식과 구조를 갖추어서 
전송이 되는것이다.

이 블록 형식의 패킷은 HTTP Request(요청)와 HTTP Response(응답)에 따라서
조금은 다른 구조를 갖는다. 따라서 이제부터 요청과 응답에 대한 패킷의 구조와 구조안에 있는 
요소들에 대해서 각각 알아보도록 하겠다.

<br>

> [패킷이란 무엇일까 (1)](https://ko.wikipedia.org/wiki/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC_%ED%8C%A8%ED%82%B7)       
> [패킷이란 무엇일까 (2)](https://codinggom.github.io/HTTP-%ED%8C%A8%ED%82%B7/)           
> [패킷이란 무엇일까 (3)](https://free-eunb.tistory.com/41)

<br>

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/162161460-3fd17a72-efe7-4f26-8ae5-ab7041d8b8ff.png">
</p>

**HTTP 요청(Request)**     
클라이언트(사용자)가 서버에 HTTP Request(요청)하는 경우를 의미한다.

HTTP 요청(Request)의 패킷을 보면 구조가 크게 3부분으로 나누어진걸 알 수 있다.
(공백은 말 그대로 공백이다.)  
  
* 스타트 라인(start line)
* 요청헤더(header)
* 요청바디(body)    

<br>

> 스타트 라인(start line)을 요청 라인(request-line)이라고 부르기도 하고(실제로 위의 이미지에서는 
> start line이 아닌 요청라인(request-line)이라고 나와있다.)     
> [start line을 request-line으로 (1)](https://velog.io/@doomchit_3/Internet-HTTP-%EA%B0%9C%EB%85%90%EC%B0%A8%EB%A0%B7-IMBETPY)    
> [start line을 request-line으로 (2)](https://blog.wanzargen.me/20)    

<br>

> 조금 더 이 패킷 안의 구조를 자세히 표현한다면, 라인공백(CRLF=엔터)과 더불어 
> [요청/응답 라인] [헤더] [CRLF] [바디]와 같이 표현할 수 있으나 이 부분은 나중에 우리가
> 프로젝트를 진행하면서 알아야 할 필요가 있다면 보도록 하겠다. 혹시 그래도 궁금하다면 아래 참조링크들을
> 참고하도록 하자.     
> [CRLF를 사용한 HTTP의 구체적인 형식 (1)](https://codinggom.github.io/HTTP-%ED%8C%A8%ED%82%B7/)     
> [CRLF를 사용한 HTTP의 구체적인 형식 (2)](https://blog.wanzargen.me/20)         

<br>

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/161952061-41e20e88-b8a7-4b8d-be80-e1372a440544.png">
</p>

제일 먼저 **start line**이다.    
말 그대로 HTTP Request의 첫 라인이다. start line은 3가지 요소로 구성되어져 있다.    

1. Request URL : 해당 요청(Request)의 목표 url을 의미한다. 바로 위의 이미지를 보면 
  https://www.naver.com이라고 되어 있는것이 보인다.(필자는 크롬 브라우저에서 https://naver.com를
  입력하고 개발자도구의 Network탭에 들어갔기 때문이다.)

2. Request Method :  해당 요청에 대한 종류(액션)를 정의하는 부분이다. 위 이미지 예시를 보면
  GET이라고 적혀져 있다. 주로 GET과 POST가 쓰이며 그 외에 PUT, DELETE, OPTION등이 있다. 이러한
  메서드들에 대해서는 아래에서 다시 얘기하도록 하겠다.

3. HTTP Version : 사용되는 HTTP 버전이 적힌다.(주로 1.1 버전이 널리 쓰인다.)

<br>

> request URL을 request target이라고 부르기도 한다. 
> 또한, Request Method 대신, HTTP Method라고 부르기도 한다.    
> [request target과 HTTP Method (1)](https://velog.io/@teddybearjung/HTTP-%EA%B5%AC%EC%A1%B0-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%9A%94%EC%86%8C)     
> [request target과 HTTP Method (2)](https://velog.io/@sehy/Http)

<br>

> 위의 실제 예로 보여준 이미지와 앞으로 보여줄 이미지들은 모두 크롬 브라우저의 F12(개발자 도구)를 눌러서
> Network탭에 있는 여러요소중 하나를 클릭해서 보여준것이다. 이렇게 실제 사용하는 예를 들어서 알아가면 더 많은 도움이
> 되니 참고하도록 하자.   

<br>

> [HTTP 요청 패킷의 시작라인(start line)에 대해 (1)](https://velog.io/@teddybearjung/HTTP-%EA%B5%AC%EC%A1%B0-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%9A%94%EC%86%8C)      
> [HTTP 요청 패킷의 시작라인(start line)에 대해 (2)](https://velog.io/@sehy/Http)    

<br>

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/162666108-27290a5d-2bdf-4230-9fdc-0bf0b92d314b.png">
<img src="https://user-images.githubusercontent.com/59492312/162666110-9b299eb7-8aab-4615-b749-60c20d9b6068.png">
</p>

그 다음은 **요청 헤더(header)** 이다.

이 요청(Request)의 헤더(header)부분은 다양한 정보를 갖고 있다.
예를 들어, request 패킷 body의 총 길이(Content-Length),
요청하는 클라이언트 PC, 브라우저정보, 사용자언어환경, 쿠키 등 에 대한 정보가
포함되어 있다. Request Method(HTTP Method)가 GET이냐 아니면 POST냐에 따라
전달되는 요소가 달라지는데, 그중에 알아두면 좋을 요소들을 짚고 넘어가도록 하겠다.

**GET, POST 공통**     
* user-Agent : 요청을 보내는 클라이언트의 대한 정보로 어떤 브라우저나 운영체제등을
  이용해 요청을 보냈는지 보여준다. 

* cookie : 쿠키 값으로, Key:Value로 표현된다. ex) attr1=value1; attr2=value2

* accept : 요청을 보낼 때 서버에게 어떤 터입으로 응답을 보내줬으면 좋겠다고 명시하는 것이다. 브라우저가 알아서 생성한다. 실제, 개발자도구 네트워크
  탭에서 보면 굉장히 다양한 값들이 설정되어있는걸 알 수 있다. 예를들면, */*처럼 모든 타입처리이거나 application/json 처럼 json데이터 처리가능이라고 
  명시해서 서버에 보내주는거다.

* referer : 이전에 어느 웹사이트에서 온것인지 알 수 있어서 이걸로 애널리틱스를
  하는데 많은 도움이 된다.

그 다음은, POST 메서드만이 갖고있는 요소에 대해 보도록 하겠다.

<br>

> 물론, GET 메서드만이 갖고있는 요소도 있다. GET메서드는 POST메서드와는 다르게 캐싱이 가능하기에 cache 관련 요소들이
> 있다. 하지만, POST 메서드와 다르게 이러한 요소가 있다는것만 알아두어도 된다. 조금 더 자세히 알고싶다면 아래 참조링크를
> 참고하거나 실제 스프링부트 프로젝트를 진행하면서 다시 자세히 다루도록 하겠다.     
> [GET메서드는 캐싱이 가능하고, POST메서드는 캐싱이 가능하지 않는 이유](https://github.com/sooolog/dev-spring-springboot)

<br>

> [GET, POST 헤더의 공통요소 (1)](https://bentist.tistory.com/35)    
> [GET, POST 헤더의 공통요소 (2)](https://velog.io/@sehy/Http)    
> [GET, POST 헤더의 공통요소 (3)](https://velog.io/@teddybearjung/HTTP-%EA%B5%AC%EC%A1%B0-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%9A%94%EC%86%8C)    

<br>

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/162666099-07ea2fa5-988c-4a3e-b45d-4f8e3069b6ac.png">
<img src="https://user-images.githubusercontent.com/59492312/162666105-29bbc695-d15d-4ecf-abee-af10cfa10d93.png">
</p>

**POST 요청**    
* content-length : 본문의 길이, 메세지 크기에 따라 자동으로 생성한다. 여기에서 본문이란
  Body에 보내는 메시지를 의미한다.

* content-Type : 요청할 때 보내는 BODY의 메시지 타입을 의미한다. 예를 들면,
  application/json등이 있다.

* origin : 서버로 Post 요청을 보낼 때 요청이 어느 주소에서 시작되었는지 나타내는 
  값으로 요청을 보낸 주소와 받는 주소가 다르면 CORS 에러가 난다.

<br>

> 크롬의 개발자도구 네트워크탭에서 보여지는 항목들은 실제 패킷내에 있는 요소들과는 다소 차이가 있을 수 있다. 예를들면
> HOST라는 요소도 원래는 header란에 있어야하지만, 개발자 도구의 네트워크탭에서는 보이지 않는다. 하지만, 보통 실무에서는
> 이 네트워크탭을 많이 이용하고 문제가 되지 않기에 우리는 그대로 개발자 도구의 네트워크 탭에 보여지는 내용만 다루도록 하겠다.
> 나중에 보여지지 않는 요소들에 대해서도 알아야 한다면 추가적으로 정리하도록 하겠다.     
> [개발자 도구의 네트워크탭과 실제 패킷내 요소의 차이 (1)](https://velog.io/@teddybearjung/HTTP-%EA%B5%AC%EC%A1%B0-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%9A%94%EC%86%8C)     
> [개발자 도구의 네트워크탭과 실제 패킷내 요소의 차이 (2)](https://codinggom.github.io/HTTP-%ED%8C%A8%ED%82%B7/)    

<br>

> 헤더(header) 부분도 3부분(general headers, request headers, entity headers)
> 으로 나누어져 있지만, 각각에 대해 자세하게 다루는것은 지금 당장 급하지 않으니 3부분으로 나누어져 있다는것만
> 인지하고 가자.  
> [헤어(header)의 3부분](https://velog.io/@teddybearjung/HTTP-%EA%B5%AC%EC%A1%B0-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%9A%94%EC%86%8C)

<br>

> 교차출처리소스공유(CORS)는 동일출처원칙(SOP)와는 다르다. 이는 추가 HTTP 헤더를 사용하여, 한 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 선택한 
> 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제이다. orgin이라는 요소가 CORS에서 사용된다. CORS에 대해 더 자세히 알고 싶다면 다른 글들을
> 참고하기 바란다.    
> [교차출처리소스공유(CORS)란 무엇인가](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS) 

<br>

> [HTTP 요청(Request)의 header에 대해 (1)](https://velog.io/@teddybearjung/HTTP-%EA%B5%AC%EC%A1%B0-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%9A%94%EC%86%8C)    
> [HTTP 요청(Request)의 header에 대해 (2)](https://bentist.tistory.com/35)    
> [HTTP 요청(Request)의 header에 대해 (3)](https://velog.io/@sehy/Http) 

<br>

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/162161467-f81ebee3-e8a1-4499-a891-8050d28a2936.png">
</p>

**HTTP 응답(Response)**      
서버가 사용자의 요청을 받고 클라이언트(사용자)에 HTTP Response (응답)하는 경우를 의미한다.

HTTP 응답(Response)도 구조가 크게 3부분으로 나누어진다.     
* 상태 라인(status line)
* 응답헤더(header)
* 응답바디(body)    

하나하나 봐보도록 하겠다.

<br>

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/161952061-41e20e88-b8a7-4b8d-be80-e1372a440544.png">
</p>

제일 먼저 **상태 라인(status line)** 이다.    
상태 라인(status line)은 3가지 요소로 구성되어져 있지만, 우리는 1가지 요소만 알고가도록 하겠다.    

1. Status Code : 응답메시지의 상태코드이다. 숫자로 표시된다. ex) Status Code: 200

각각의 상태코드(status code)는 100대부터 500대까지의 숫자로 표현된다. 알아두면 좋을 숫자에 대해서는
맨 아래 챕터에서 다시 설명하도록 하겠다.

<br>

> 이 외에도 HTTP Version, Status Text라는 요소가 있지만 위의 이미지에서 보이는것만 체크하고
> 넘어가도록 하겠다. 만약 나머지 두 요소에 대해서도 알고 싶다면 아래 참조링크를 보도록 하자.    
> [Status line의 나머지 두 요소 (1)](https://velog.io/@teddybearjung/HTTP-%EA%B5%AC%EC%A1%B0-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%9A%94%EC%86%8C)    
> [Status line의 나머지 두 요소 (2)](https://velog.io/@sehy/Http)    

<br>

> 위의 이미지는 아까 start line에서 본 이미지와 같은 이미지다. 크롬 개발자도구의 네트워크에서 보여주는
> 위 이미지는 HTTP 통신의 start line과 status line에 대한 요소를 General 탭에서 한번에 보여주기 때문에
> 한곳에 모아져 보이는것이다.

<br>

> [Status line과 그 요소 (1)](https://velog.io/@teddybearjung/HTTP-%EA%B5%AC%EC%A1%B0-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%9A%94%EC%86%8C)    
> [Status line과 그 요소 (2)](https://velog.io/@sehy/Http)    

<br>

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/162717961-526cadc6-5ede-4403-8f74-ba381307a7ef.png">
</p>
<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/162718018-88ae72e5-4c00-4cdf-bb6b-17b28e8bb9ed.png">
</p>

그 다음은 **응답 헤더(header)** 이다.

응답 헤더에서는 요청 헤더에서는 없는 응답 헤더에만 있는 요소들에 대해
짚고 넘어가도록 하겠다.

* server : 웹 서버의 종류를 나타낸다. ex)Nginx, Cloudflare

* location : 301, 302 상태코드일 때만 볼 수 있는 헤더로 서버의 
  응답이 다른 곳에 있다고 알려주면서 해당 위치(URI)를 지정한다. 
  예를 들면, bit.ly같은 서비스 이용시에 location요소를 볼 수 있다.

* content-Type : 요청의 content-Type과 마찬가지로 응답의 헤더에 있는 COntent-Type는
  클라이언트에게 반환하는 컨텐츠의 컨텐츠 유형이 무엇인지를 알려준다. 또한, 요청(Request)이나 응답(Response) 둘다
  패킷의 body에 데이터에 대해 사용되는것이며, 웹서버나 아니면 브라우저는 이 content-Type을 기준으로 해당 데이터를 분석한다.
  content-Type이 없다면, 특정한 형식의 데이터일지라도 데이터를 받는 입장에서는 단순히 텍스트 데이터로 받아들인다.

이 외에도 다른 요소들이 많지만, 이 두가지 요소만 보고 가도록 하겠다.

<br>

> [HTTP 응답(response)의 헤더(Header)](https://bentist.tistory.com/35)    
> [요청과 응답의 content-Type 개념 (1)](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Content-Type)    
> [요청과 응답의 content-Type 개념 (2)](https://webstone.tistory.com/66)    

<br>

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/162430759-cbb542c7-a063-4e31-82d5-209ae3b8ae54.png">
</p>
<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/162430761-08a76702-9231-4d3f-bc8d-c9b869e59db8.png">
</p>

이는 HTTP 응답(Response)의 body 부분이다.

크롬 개발자도구 네트워크 탭의 response 부분이 바로 이 응답의 body 즉, 서버에서 보내온
데이터를 의미한다. response탭의 옆에있는 preview는 브라우저에서 해당 데이터를 보여준다고 했을때
보여지는 화면을 의미한다. 

위의 이미지들은 GET 메서드에 의해 HTML을 Body에 담아서 갖고온것이다.

<br>

> 위의 POST 메서드의 HTTP 요청에 대해서 Body부분을 언급한적이 없다.
> 분명 Body에 담기는 데이터도 존재하지만, 크롬 개발자도구 네트워크 탭에서는 요청에 해당하는 Body를
> 보여주지 않기에 언급하지 않았다. 또한, 다시 한번 언급하자면 요청의 Body는 GET,DELETE는 비어있고
> POST와 PUT만 전송할 데이터가 있을시에 Body에 담아서 보낸다.

<br>

> [HTTP 응답(response)의 Body (1)](https://velog.io/@teddybearjung/HTTP-%EA%B5%AC%EC%A1%B0-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%9A%94%EC%86%8C)     
> [HTTP 응답(response)의 Body (2)](https://velog.io/@sehy/Http)     

<br>

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/162430744-00e88dba-614d-4248-80f5-d091b73537f6.png">
</p>
<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/162430753-5651db8d-0b34-4853-b3c2-10441a105878.png">
</p>

추가적으로 하나 더 보겠다. 이는 Post 메서드를 요청하고 Body에
JSON데이터를 받아온것이다. GET 메서드가 HTML, Post 메서드가 JSON 데이터를
받아오는것은 아니며, 단순 서로 다른 데이터를 받아오는것을 보여주기 위해 이미지를 추가로
첨부했다.

<br>

> 반드시 HTTP 응답(response)에 대해 Body가 채워져 있는것은 아니다. 서버에서 데이터를
> 전송할 필요가 없을경우 Body에 아무것도 담지 않는다.        
> [Body와 데이터](https://velog.io/@teddybearjung/HTTP-%EA%B5%AC%EC%A1%B0-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%9A%94%EC%86%8C)

<br>



### 3.HTTP 메소드들과 Status Code

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/162356550-56cf232e-88a8-45db-8d04-234287834285.png">
</p>

위에서, HTTP 패킷에서 HTTP Method와 Status Code가 포함되어져 있다는걸 알았다.

HTTP Method에는 GET, POST, PUT, DELETE가 있고, Status Code에는 100대부터 500대까지의
상태 코드(Status Code)가 있는데, 이러한 요소들은 잘 알고만 있다면 실제 실무에서 많은 도움이 되기때문에,
꼭 알아야 할 요소들에 대해 짚고 넘어가도록 하겠다.

<br>

> HTTP 메서드로는 PATCH와 OPTIONS 외에 여러 메서드들이 더 있으나, 우리는 제일 대중적인 GET, POST, PUT, DELETE
> 메서드들에 대해서만 알아보고 가도록 하겠다. 또한, 이 4개의 메서드들(CRUD - Create, Read, Update, Delete)만이 
> 후에 배울 REST 서비스에서 사용된다.    
> [다양한 HTTP 메서드들 (1)](https://javaplant.tistory.com/18)      
> [다양한 HTTP 메서드들 (2)](https://velog.io/@teddybearjung/HTTP-%EA%B5%AC%EC%A1%B0-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%9A%94%EC%86%8C)     
> [REST 서비스에서 사용되는 CRUD 메서드](https://velog.io/@thisisemptyyy/TIL-021-CRUD-vs-REST-API)

<br>

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/162356558-0c07e4e9-2e3f-48ba-9856-89701fef0df9.png">
</p>

제일먼저 GET 메서드이다.
* 서버로 부터 정보를 조회하기위한 메서드이다. 즉, 다른말로 하면 클라이언트가 정보를 보내는게 주 목적이 아닌
  서버로부터 정보를 갖고와서 조회하기 위한 메서드이다.
* 그렇기에, GET요청을 보낼시 패킷의 BODY와 Content-type은 비게된다.
* 물론 쿼리스트링(URL에서 ? 이후의 문자열)을 이용하여 필요한 정보(값)를 서버에 전달 할 수는 있다.
* 이 경우에는, 패킷의 BODY가 아닌 Header 안에 포함시켜서 전송한다. 

POST 메서드를 보겠다.
* POST 메서드는 주로 새로운 리소스를 생성(create)할 때 사용한다.
* 데이터를 생성하는 것이기 때문에, 요청시에 BODY를 사용하며, Content-type도
  사용하게 된다.
* BODY에 담아서 보낼경우 암호화정도는 아니여도 기본 보안은 된다.

PUT 메서드를 보겠다.
* 데이터를 수정하는것이기 때문에, POST 메서드와 마찬가지로 데이터를 서버에 전송하게 된다.
* 이 과정에서 BODY를 통해서 보내기 때문에, BODY와 Content-type을 함께 사용하게 된다. 

DELETE 메서드를 보겠다.
* DELETE 메서드는 저장된 리소스(정보)를 삭제하는 메서드이다.
* 따로 데이터를 보내지 않기에 BODY와 Content-type이 사용되지 않는다.
* 단, GET과 마찬가지로 쿼리스트링이나 URL을 통해서 어떠한 데이터를 삭제할지 보내게 된다.
* 이 또한, 패킷의 header에 담아져서 보내지게 된다.

<br>

> 그러면 사실 PUT과 POST메서드는 같은거고 GET과 DELETE가 같은건데 용도만 다르게 쓰는것 아닌가 ?
> 라고 할 수도 있다. 물론, 다른 용도에 따라 다르게 표현할 수도 있다. 또한, 실제 @PostMapping이나 @PutMapping
> 을 스프링부트(백엔드 프레임워크다.)에서 사용할 때 클라이언트가 요청한 HTTP Method에 맞게 컨트롤러를 매칭시켜줄 뿐
> 차이점은 없는것으로 봐도 무방하다. 또한, 실제 데이터베이스 쿼리에서 역활할 때도 Insert, Update, Create등과 같이   
> 작성해주어서 데이터를 생성하거나 조회하거나 수정 혹은 삭제하는 역활을 하기에 실제 데이터 작업에도 영향을 주지 않는다.
> 하지만, POST와 PUT은 멱등성이란 분명한 차이점이 존재하고, 자세하게 들어가면 다른부분들이 엄연히 존재한다. 하지만 이 부분들에 대해서는
> 나중에 필요시 다시보도록 하겠다.(GET과 DELETE도 용도에 따라 표기법만 다른것으로 봐도 무방하다. 물론 GET은 조회이고 DELETE는 삭제이니
> 사용하는 방법이 다를 뿐 메서드 자체에 큰 차이점이 있지는 않다.)
> [멱등성이란](https://wordrow.kr/%EC%9D%98%EB%AF%B8/%EB%A9%B1%EB%93%B1/)    
> [PUT 메서드와 POST의 차이점 (1)](https://velog.io/@53_eddy_jo/RESTful%ED%95%9C-%EC%84%B8%EA%B3%84%EC%97%90%EC%84%9C%EC%9D%98-POST%EC%99%80-PUT%EC%9D%98-%EC%B0%A8%EC%9D%B4-%EA%B1%B0%EA%B8%B0%EC%97%90-FETCH%EA%B9%8C%EC%A7%80)    
> [PUT 메서드와 POST의 차이점 (2)](https://2ham-s.tistory.com/265)

<br>

> 각 메서드들에 대해 더 자세하게 알고싶다면, 필자의 [깃헙 레포지토리](https://github.com/sooolog/spring-springboot)에서 GET메서드와 POST메서드에 대한 글을 읽도록 하자.
> 실제 서비스에서 어떠한 영향을 주는지 더 자세하게 알 수 있다.

<br>

> [HTTP GET, POST, PUT, DELETE 메서드에 대해 (1)](https://free-eunb.tistory.com/41)         
> [HTTP GET, POST, PUT, DELETE 메서드에 대해 (2)](https://velog.io/@yh20studio/CS-Http-Method-%EB%9E%80-GET-POST-PUT-DELETE)     
> [HTTP GET, POST, PUT, DELETE 메서드에 대해 (3)](https://javaplant.tistory.com/18)

<br>

<p align="center">
<img src="https://user-images.githubusercontent.com/59492312/162356561-8cfe717d-1352-4edd-9fc2-d40df8fe45c2.png">
</p>

마지막으로, 상태 코드(Status Code)에 대해 보도록 하겠다.
상태 코드(Status Code)는 서버가 클라이언트에게 응답의 상태를 알리는 수단이며
1xx부터 ~ 5xx까지 다섯가지로 분류된다.

* 1xx : 서버가 요청을 클라이언트에서 성공적으로 수신했으며 서버 끝에서 처리 중이라는 정보를 나타낸다.
        서버의 임시 응답이며 일반적으로 상태 줄과 선택적 헤더 만 포함하며 빈 줄로 끝난다.
        현재는 거의 사용하지 않는다.

* 2xx : 서버가 요청을 받고 성공적으로 처리되었음을 나타낸다.

* 3xx : 브라우저는 자동으로 다른 URL로 리디렉션되므로 브라우저 창에는이 코드가 표시되지 않지만,
        이미지 파일처럼 캐싱된 파일을 새로고침 후 확인하면 3xx 코드를 확인할 수 있다.

* 4xx : 서버가 해결할 수 없는 클라이언트 측 에러 코드다.
        주로 클라이언트(사용자)가 서버에 잘못된 요청을 했을 경우 발생한다.

* 5xx : 서버가 클라이언트의 요청을 처리하지 못했을 때 발생한다.
        서버는 보안 상 통신하지 않는 것이 가장 좋으므로 대부분의 에러 코드를 500 Error로 처리한다.

가장 많이 사용되는 상태코드들에 대해서도 보겠다.

* 200 : 가장 자주 보게 되는 status code이다. 문제없이 다 잘 실행 되었다는 의미

* 301 : 해당 URI가 다른 주소로 바뀌었을때 보내는 코드이다. bit.ly같은 서비스를 쓰면 볼 수 있다.

* 400 : 해당 요청이 잘못된 요청일 때 보내는 코드로, 주로 요청에 포함된 input 값들이 잘못된
        값들로 보내졌을 때 발생한다. 예를 들면, 전화번호를 보내야하는데 text를 보낸경우이다.

* 401 : 유저가 해당 요청을 진행 할려면 먼저 로그인을 하거나 회원 가입을 하거나 등등이 
        필요하다는것을 나타내려 할때 쓰이는 코드이다.

* 403 : 유저가 해당 요청에 대한 권한이 없다는 뜻이다. 과금을 한 유저만 볼 수 있는
        데이터를 요청했을 때 등과 같은 상황에서 볼 수 있다.

* 404 : 요청된 uri가 서버에 맵핑되는게 존재 하지 않는다는 뜻이다. 

* 500 : 서버에서 에러가 났을때 사용되는 코드이다. 서버 내부에 문제일 경우 주로 발생한다.

<br>

> 이 외에 상태코드들에 대해서는 그때 그때 필요할 때 보기로 하겠다.

<br>

> [HTTP 통신과 상태 코드 (1)](https://velog.io/@doomchit_3/Internet-HTTP-%EA%B0%9C%EB%85%90%EC%B0%A8%EB%A0%B7-IMBETPY)        
> [HTTP 통신과 상태 코드 (2)](https://velog.io/@teddybearjung/HTTP-%EA%B5%AC%EC%A1%B0-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%9A%94%EC%86%8C)   
> [HTTP 통신과 상태 코드 (3)](https://codinggom.github.io/HTTP-%ED%8C%A8%ED%82%B7/)   

<br>



#### 🚀 이론적인 내용들이 많아서 실제 프로젝트를 진행하면서 알아가는편이 더 쉽고 빠르게 이해된다. 언급이 되지않은 부분이나 부족한 부분에 대해서는 프로젝트를 함께 진행하면서 보도록 하겠다.

<br>



태그 : #비연결성(Connectionless), #무상태성(Stateless), #쿠키(Cookie), #세션(Session), #패킷, #스타트라인, #헤더, #바디, #상태라인, #CRLF, #CORS, #GET, #POST, #PUT, #DELETE, #상태코드
