# HTTP

# HTTP의 특성

### 요청- 응답 기반 프로토콜

HTTP는 클라이언트와 서버가 서로 HTTP 요청 메시지와 응답 메시지를 주고받는 구조로 동작

같은 HTTP여도 요청과 응답은 메시지 형태가 다르다.

### 미디어 독립적 프로토콜

클라이언트는 HTTP 요청 메시지를 통해 서버의 자원을 요청할 수 있고, 서버는 응답 메시지로 요청받은 자원에 응답 가능

HTTP는 자원의 특성을 제한하지 않고 자원을 주고받을 수단(인터페이스)의 역할만을 수행 한다. 

HTTP에서 메시지로 주고받는 자원의 종류를 미디어타입 또는 MME(MultiPurpose Internet Mail Extensions Type)이라고 한다.

HTTP  = 미디어 독립적인 프로토콜

<img width="956" height="372" alt="image" src="https://github.com/user-attachments/assets/4bc2d5dc-ddaa-4aef-9c7f-7c3279477692" />

미디어 타입은 타입/서브타입 형식으로 구성된다.

타입은 데이터 유형을 서브타입은 주어진 타입에 대한 세부 유형을 나타냄

<img width="1024" height="1224" alt="image" src="https://github.com/user-attachments/assets/20583458-6cbb-4bd4-81cd-4d7d85311853" />

와일드 카드문법도 쓰인다.

`image/*`, `**/**` 

부가적인 설명을 위해 매개변수가 포함될 수도 있다.

`type/subtype;**parameter=value**`

ex) `type/html;caharset=UTF-8`

### 스테이트리스 프로토콜

HTTP는 상태를 유지하지 않는 스테이트리스 프로토콜로 서버가 HTTP요청을 보낸 클라이언트와 관련된 상태를 기억하지 않는다. 그렇기 때문에 클라이언트의 모든 요청은 독립적인 요청으로 간주된다.

<img width="886" height="320" alt="image" src="https://github.com/user-attachments/assets/1d62f832-31e2-4169-9150-1956e7c1131c" />

비효율적으로 보일수 있지만..

보통 HTTP서버는 많은 클라이언트와 동시에 상호작용하기 때문에 모든 클라이언트의 상태 정보를 유지하는것은 서버에 큰 부담

또한 서버가 여러개로 구성된다면?? 

모든 서버가 모든 클라이언트의 상태정보를 공유하는 것이 매우 어렵다. → 상태를 기억하는 특정서버 하고만 상호작용 → 특정 클라이언트가 특정 서버에 종속될 수 있다. → 그 서버에 문제가 생긴다면…?

<img width="852" height="508" alt="image" src="https://github.com/user-attachments/assets/e3743de2-592f-46ea-8a9e-09585b380973" />

HTTP의 중요한 설계 목표는 **`확장성`** 과 **`견고성`** 

상태를 유지하지 않는다 → 언제든 쉽게 서버 추가 가능 → 높은 확장성

서버중 하나에 문제가 생겨도 쉽게 대체 가능 → 높은 견고성

물론 이런 HTTP의 무상태성을 보완하기 위한 방법은 여러가지가 있음 → 쿠키, 세션, 토큰, 로컬 스토리지 등 

### 지속 연결 프로토콜

기본적으로 HTTP는 TCP상에서 동작하는데, HTTP는 비연결형 프로토콜 이지만, TCP는 연결형 프로토콜임

초기 HTTP버전( 1.0 이하 )은  쓰리 웨이 핸드쎄이크를 통해 TCP연결을 수립하고, 요청에 대한 응답을 받으면 연결을 종료하는 방식으로 동작했음. 추가적인 요청-응답을 위해선 다시 TCP연결을 수립해야했다 → 비지속 연결

하지만 최근 사용되는 HTTP버전(1.1 이상)은  **`지속 연결`** 기술을 제공 (= `keep-live` )

하나의 TCP연결 상에서 여러개의 요청-응답을 주고 받을 수 있다.

<img width="1024" height="539" alt="image" src="https://github.com/user-attachments/assets/d4c8ac08-7419-4aa0-80a4-ddc74ead101d" />

# HTTP 메시지 구조

<img width="834" height="298" alt="image" src="https://github.com/user-attachments/assets/cf3bac1d-7502-4b73-9372-b2c67f9f1df0" />

HTTP 메시지는 **`시작 라인`**, **`필드 라인`**, **`메시지 본문`** 으로 구성됨

필드라인은 없거나, 여러개 있을수 있고(0개이상)

메시지 본문은 없을 수도 있다.(0 또는 1개)

필드라인과 메시지 본문 사이에는 빈 줄바꿈이 있다.

- **`시작 라인`**
    
    HTTP메시지가 
    
    요청 메시지라면 → 요청 라인이 되고
    
    응답 메시지라면 → 상태 라인이 된다.
    
    - **요청 라인**의 형식은 다음과 같다.
        
        **<img width="834" height="298" alt="image" src="https://github.com/user-attachments/assets/fc737e69-e434-458b-9a34-10ce7aada0a3" />**

        
        **`메서드`** 
        
        클라이언트가 서버의 자원에 대해 수행할 작업의 종류
        
        **`요청 대상`** 
        
        HTTP요청을 보낼 서버의 자원, 보통 URI경로가 명시됨
        
        ex) `http://www.example.com/helllo?q=world`로 요청 보내면
        
        → 요청 대상은 “/hello?q=world”
        
        `http://www.example.com`와 같이 하위 경로가 없더라도 요청대상은 “/”로 표기해야함
        
        **`HTTP 버전`** 
        
        말그대로 사용된 HTTP버전
        
        ‘HTTP/<버전>’으로 표기 → ex) **`HTTP/1.1`**
        
    - 상태 라인의 형식은 다음과 같다.
        
        <img width="640" height="140" alt="image" src="https://github.com/user-attachments/assets/6ec80bb3-e212-4574-af3b-366130c0cd20" />



        
        **`상태 코드`** 
        
        요청에 대한 결과를 나타내는 세자리 정수
        
        **`이유 구문`** 
        
        상태 코드에 대한 문자열 형태의 설명
        
        ex) **`HTTP/1.1 200 OK` , `HTTP/1.1 404 Not Found`**
        

- **`필드 라인`**
    
    0개 이상의 `HTTP 헤더` 가 명시된다. 그래서 `헤더 라인` 이라고도 부름
    
    HTTP 헤더란 HTTP통신에 필요한 부가 정보
    
    콜론을 기준으로 헤더이름:헤더값 으로 구성된다.
    
    <img width="1024" height="238" alt="image" src="https://github.com/user-attachments/assets/1bdb8511-fe89-416d-84ac-9bde3361e892" />

    

- **`메시지 본문`** (message-body)
    
    요청 혹은 응답에 본문이 필요할경우 메시지 본문에 명시
    
    없을수도 있고, 다양한 컨텐츠 타입 사용될수 있음
    
    <img width="948" height="374" alt="image" src="https://github.com/user-attachments/assets/3114c856-b250-4224-bbff-9637adba556d" />
    

## HTTP 메서드

<img width="814" height="472" alt="image" src="https://github.com/user-attachments/assets/2007cb54-79b4-43e9-8f21-756f97754154" />

## HTTP 상태 코드

<img width="576" height="296" alt="image" src="https://github.com/user-attachments/assets/3c126adb-d0fa-4f44-a75f-b22ac98cc576" />
