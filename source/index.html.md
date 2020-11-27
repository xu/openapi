---
title: 후오비 코리아 API 문서

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://www.huobi.kr/zh-cn/api/'>API Key 생성</a>
includes:

search: false
---


# 소개      

## API 소개

안녕하세요. 후오비 코리아 API 소개입니다.

본 문서는 후오비 코리아 공식 API 문서입니다. 지속적으로 업데이트 중이오니 참조하시기 바랍니다.

상단 메뉴를 클릭하여 다른 유형 업무의 API로 바꿀 수 있고 ,우측 상단의 언어 버튼을 클릭하여 원하시는 언어로 선택할 수 있습니다.  

본 문서 우측에는 request 파라미터와 response 결과 예시를 제공합니다.



**후오비 코리아 REST API URL**

**https://api-cloud.huobi.co.kr**



**후오비 코리아 Websocket Feed（시세）**

**`wss://api-cloud.huobi.co.kr/ws`**



**전문 트레이더를 위한 전용 REST API URL** 

**https://krapi.huobi.pro** **(AWS Tokyo 리전 전용)**

* 사전 신청 및 선별된 회원 대상으로 전용 URL 서비스를 제공합니다. 

* 자세한 내용은 `전문 트레이더를 위한 API 서비스 안내`(https://support.huobi.co.kr/hc/ko/articles/360035235091) 에서 확인하세요.

  

## 마켓 메이커 프로그램

우수한 Maker 전략과 함께 거래량이 많은 회원님들께서 마켓 메이커 프로그램에 참여하기 바랍니다. 후오비 코리아 계정에 10BTC 이상 자산을 보유중인 회원님께서는 아래 정보를 메일로 보내 주세요:

- Huobi Korea 마켓 메이커 프로그램 신청 이메일:[MM-kr@huobi.com](mailto:MM-kr@huobi.com) 

1. 후오비 코리아 UID: 페이백 또는 레퍼럴과 관련되어 있지 않은 UID
2. 타 암호화폐 거래소 Maker 거래량 캡쳐 화면: 예) 최근 30일간 체결량, VIP 등급 등
3. 마켓 메이킹 진행 계획에 대해 간단히 기재해주세요. 구체적인 내용은 작성하지 않으셔도 됩니다.

<aside class="notice">
마켓 메이커 프로그램은 수수료 쿠폰 포인트 차감 및 거래량 관련 이벤트 등 어떠한 페이백 이벤트도 지원하지 않습니다.
</aside>

## API 문의 센터

API 관련하여 궁금증 또는 문의사항이 있을 경우 `자주 묻는 질문(FAQ)`을 참고하거나 `후오비 코리아 API 커뮤니티 텔레그램(http://bit.ly/2lY4o7v) `채팅방’을 통해 상담 받을 수 있습니다.




# Quick Start

## 액세스 Preparation

API를 이용하기 위해서는 먼저 API key를 발급받아야 합니다. API Key를 발급받은 후 권한을 설정하고 본 문서를 참조하여 시스템 개발 및 거래하시기 바랍니다.

본 <a href='https://www.huobi.kr/zh-cn/api/'>링크</a> 에서 API Key를 생성 합니다.

매개 마스터 계정은 API Key 5개 그룹을 생성하고 매개 Api Key는 읽기, 거래, 출금 등 3가지 권한을 설정 할 수 있습니다.  

권한 설명:

- 읽기 권한: 읽기 권한은 데이터 조회 API에 사용 됩니다. 예: 주문서 조회, 체결 조회 등.
- 거래 권한: 거래 권한은 주문, 철회, 전환 등 API에 사용됩니다.
- 출금 권한: 출금 권한은 출금 주문 생성, 출금 주문 취소 작업에 사용됩니다.  

생성한 다음 다음 정보를 백업 해 두세요:

- `Access Key`  API 액세스 키
  
- `Secret Key`  서명 인증 암호화에 사용하는 키(신청시에만 노출)

<aside class="notice">
API Key 생성 시, IP주소 연동이 가능하며 IP주소를 연동하지 않은 API Key의 유효 기간은 90일입니다. 보안상 IP주소를 연동하는 것을 추천합니다.
</aside>
<aside class="warning">
<red><b>warning</b></red>: Access Key, Secret Key 2가지 키는 계정 보안에 밀접히 연관되어 있습니다. 어떠한 경우에도 2가지 키를 <b>동시에</b> 타인에게 전달하지 마세요. API Key 누설은 자산 손실을 가져올 수 있습니다.(출금 권한을 오픈하지 않은 경우에도 마찬가지입니다.) API Key가 누설 되었다면 즉시 삭제하시기 바랍니다.
</aside> 

## SDK 및 코드 예시

**SDK（추천）**

[Java](https://github.com/huobiapi/huobi_Java)

[Python3](https://github.com/huobiapi/huobi_Python)

[C++](https://github.com/huobiapi/huobi_Cpp)

**다른 코드 예시**

https://github.com/huobiapi?tab=repositories

## API 유형

후오비 코리아는 2가지 종류의 API를 제공합니다. 사용자의 환경과 선호에 따라 시세, 거래, 출금 등 조회 방식을 선택하세요. 

### REST API

REST, 즉 Representational State Transfer라는 용어의 약자입니다. 지금 유행하는 HTTP 기반의 일종 통신 방식이고 매개 URL은 한 개의 리소스를 상징합니다.

거래 혹은 자산 출금 등 일회성 작업은 REST API로 작업하는 것을 제안합니다.

### WebSocket API

WebSocket는 HTML5 새로운 규약(Protocol) 입니다. 이는 클라이언트와 서버 사이 Full Duplex 통신이고 간단한 connect를 통하여 클라이언트과 서버의 연결을 생성합니다. 서버는 업무 규정에 따라 메시지를 클라이언트에 push 합니다.

시세와 매수 매도 depth 등 정보는 WebSocket API를 사용하는 것을 제안합니다.

**API 인증**

REST API, WebSocket API 두가지 API는 Public API와 Private API 두 가지 유형이 있습니다.

Public API는 기본 정보와 시세 데이터에 사용됩니다. Public API는 인증 없이 사용 가능합니다.

Private API는 체결, 자산 관리와 계정 관리에 사용됩니다. 매개 사유 요청은 API Key로 서명 인증이 필요합니다.

## URLs 액세스
api-cloud.huobi.co.kr와 krapi.huobi.pro(AWS Tokyo 리전 전용) 두가지 URL의 딜레이 속도를 비교하시고 딜레이가 낮은 URL을 사용하시기 바랍니다.   

**REST API**

**`https://api-cloud.huobi.co.kr`** 

**Websocket Feed（시세）**

**`wss://api-cloud.huobi.co.kr/ws`**

**Websocket Feed（자산과 주문서）**

**`wss://api-cloud.huobi.co.kr/ws/v1`**  

<aside class="notice">
딜레이가 높고 안정성이 낮기 때문에 프록시 방식으로 후오비 코리아 API를 액세스 하는것을 추천하지 않습니다.
</aside>
<aside class="notice">
API 서버의 안정성을 위하여 AWS Tokyo 리전 클라우드 서버로 액세스하는 것을 추천합니다. 만약 중국 내 서버를 사용하면 액세스 안정성을 보장하지 못합니다. 
</aside> 

## 서명 인증

### 서명 인증

API 요청은 인터넷을 통한 전송 중 변조 될 가능성이 있습니다.
Public API(기본 정보, 시세 데이터)를 제외한 Private API는 요청이 변조되지 않도록 하기 위해 반드시 귀하의 API Key로 서명 인증을 진행하여 매개 변수 또는 매개 변수 값이 변경되었는지 확인해야 합니다.
새로 생성된 모든 API Key에는 권한이 적용되어야 하고, 각 API Key는 해당 API에 적합한 권한이 있어야만 요청이 가능합니다.
권한 유형은 조회, 거래, 코인 출금으로 구분됩니다. 
API를 요청하기 전에 각 API 권한 유형을 확인하여, 회원님의 API Key에 적합한 권한을 확인하시기 바랍니다. 

합법적인 요청은 다음과 같은 부분으로 구성됩니다.

- 메소드 요청 주소: 서버 접속 주소 api-cloud.huobi.co.kr, 예) api-cloud.huobi.co.kr/v1/order/orders.
- API Access Key(AccessKeyId): 회원님이 신청한 API Key에 대한 Access Key.
- 서명 방법(SignatureMethod): 회원님이 서명을 계산할 수 있는 Hash 기반의 프로토콜로 HmacSHA256를 사용합니다.
- 서명 버전(SignatureVersion): 서명 프로토콜 버전으로 API는 2를 사용합니다.
- 타임 스탬프(Timestamp): 회원님이 요청한 시간(UTC 시간대), 예) 2017-05-11 T16:22:06. 
  쿼리 요청 시 해당 값을 포함하면 제3자가 회원님의 요청을 가로챌 수 없습니다.
- 필수 및 선택적 매개 변수: 각 메소드에는 API 호출 정의를 위한 필수 및 선택적 매개 변수 세트가 있습니다. 각 방법의 설명에서 이러한 매개 변수와 의미를 확인할 수 있습니다. 
  - GET 방식으로 요청할 경우 각 메소드와 함께 제공되는 매개 변수에 서명 인증을 진행해야 합니다. 
  - POST 요청의 경우 각 메소드의 매개 변수는 서명 인증을 진행하지 않고  body에 넣어야 합니다.
- 서명: 유효하고 변조되지 않았음을 보장하기 위해 서명에 의해 계산된 값입니다.

### 서명 절차

HMAC로 서명 계산시 서로 다른 내용으로 계산하면 결과가 완전히 다르기 때문에 서명 계산 진행을 규범 하여야 합니다. 서명 계산 진행 시 요청 전에 반드시 절차를 지켜주세요. 다음은 어떤 주문 건의 상세내역을 조회하는 예시입니다.

주문 건 상세내역 조회 시 전체 URL을 요청해야 합니다.

`https://api-cloud.huobi.co.kr/v1/order/orders?`

`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx`

`&SignatureMethod=HmacSHA256`

`&SignatureVersion=2`

`&Timestamp=2017-05-11T15:19:30`

`&order-id=1234567890`

#### 1. 요청 메소드(GET 또는 POST) 입력 후 줄바꿈 개행 문자 "\n" 추가해주세요.


`GET\n`

#### 2. 소문자로 접속 주소 입력 후 줄바꿈 개행 문자 "\n" 추가해주세요.

`
api-cloud.huobi.co.kr\n
`

#### 3. Access 메소드 입력 후 줄바꿈 개행 문자 "\n" 추가해주세요.

`
/v1/order/orders\n
`

#### 4. 파라미터를 URI코딩 하고 ASCII 코드 순서에 따라 매개 변수 명칭을 정렬해주세요.

예) 인코딩 진행 후 매개 변수 정렬은 다음과 같습니다.

`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx`

`order-id=1234567890`

`SignatureMethod=HmacSHA256`

`SignatureVersion=2`

`Timestamp=2017-05-11T15%3A19%3A30`

<aside class="notice">
UTF-8 방식으로 인코딩되고 URI 인코딩 됩니다. 16진수 문자는 반드시 대문자로 진행되어야 합니다.예를 들어 ":"는 "%3A"로 인코딩되고 빈칸은 "%20"으로 인코딩됩니다. 
</aside>
<aside class="notice">
타임 스탬프(Timestamp)는 YYYY-MM-DDThh:mm:ss 형식 및 URI 인코딩으로 추가해야 합니다.
</aside>

정렬 완료 이후

`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx`

`SignatureMethod=HmacSHA256`

`SignatureVersion=2`

`Timestamp=2017-05-11T15%3A19%3A30`

`order-id=1234567890`

#### 5. 위의 순서대로 각 매개 변수를 문자 “&”로 연결해주세요.


`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890`

#### 6. 계산을 위해 서명할 마지막 문자열은 다음과 같습니다.

`GET\n`

`api-cloud.huobi.co.kr\n`

`/v1/order/orders\n`

`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890`


#### 7. 이전 단계에서 생성된 “요청 문자열”과 회원님 Key(Secret Key)를 사용하여 디지털 서명을 생성해주세요.


1. 위에서 발생한 요청 문자열과 API 개인 Key를 2개의 매개 변수로 사용하여 HmacSHA256 해시 함수를 호출후 해시 값을 얻습니다.

2. 해시 값은 base-64로 인코딩되고 생성된 값은 API 호출을 위한 디지털 서명으로 사용됩니다.

`4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o=`

#### 8. 생성된 디지털 서명을 요청 path 매개 변수에 추가해주세요.

1. 필요한 모든 인증 매개 변수를 API 호출의 경로 매개 변수에 추가해주세요. 
2. URL 인코딩 후 path 매개 변수에 디지털 서명을 추가하십시오. 매개 변수 이름은 "Signature"입니다.

최종적으로 서버에 전송된 API 요청은 다음과 같습니다.

`https://api-cloud.huobi.co.kr/v1/order/orders?AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&order-id=1234567890&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&Signature=4F65x5A2bLyMWVQj3Aqp%2BB4w%2BivaA7n5Oi2SuYtCJ9o%3D`


## 서브 계정

서브 계정은 자산 분리와 거래를 할 수 있고 마스터계정과 서브계정 간의 자산 이동이 가능합니다; 서브계정은 계정 내에서만 거래 가능하고 서브계정 사이에 자산을 이동할 수 없으며 마스터 계정만 자산 이동 권한이 있습니다.

서브 계정은 독립적 로그인 비밀번호와 API Key를 사용하고 웹에서 마스터계정으로 관리 가능합니다.

한 개의 마스터 계정은 총 200개의 서브 계정을 생성할  수 있고, 하나의 서브 계정은 Api Key 5개조를 생성할 수 있으며 각 Api Key는 읽기, 거래 2가지 권한을 설정할 수 있습니다.

서브 계정의 API Key는 IP주소를 연동하고 유효기간은 마스터계정의 API Key와 동일합니다.

 <a href='https://www.huobi.co.kr/zh-cn/sub-account/'>링크</a> 를 클릭하여 서브 계정 생성 및 관리가 가능합니다.  

서브 계정은 모든 Public API에 접근 가능합니다(기본 정보와 시세를 포함). 서브 계정이 접근 가능한 Private API는 다음과 같습니다:

| API                                                          | 설명                       |      |
| ------------------------------------------------------------ | -------------------------- | ---- |
| [POST /v1/order/orders/place](#fd6ce2a756)                   | 주문서 생성 및 진행        |      |
| [POST /v1/order/orders/{order-id}/submitcancel](#4e53c0fccd) | 주문서 철회                |      |
| [POST /v1/order/orders/batchcancel](#ad00632ed5)             | 대량 주문서 철회           |      |
| [POST /v1/order/orders/batchCancelOpenOrders](#open-orders)  | 현재 위탁 주문서 철회      |      |
| [GET /v1/order/orders/{order-id}](#92d59b6aad)               | 주문서 내역 조회           |      |
| [GET /v1/order/orders](#d72a5b49e7)                          | 현재,과거 주문 조회        |      |
| [GET /v1/order/openOrders](#95f2078356)                      | 현재 위탁 주문서 조회      |      |
| [GET /v1/order/matchresults](#0fa6055598)                    | 체결 조회                  |      |
| [GET /v1/order/orders/{order-id}/matchresults](#56c6c47284)  | 주문서 체결 내역 조회      |      |
| [GET /v1/account/accounts](#bd9157656f)                      | 현재 고객의 모든 계정 조회 |      |
| [GET /v1/account/accounts/{account-id}/balance](#870c0ab88b) | 지정한 계정의 잔액 조회    |      |

<aside class="notice">
서브 계정은 기타 API에 접근이 불가 합니다. 접근 시 시스템은 “error-code 403”을 반환 합니다.
</aside>

## 업무 dictionary

### 거래페어

거래페어는 거래 화폐와 마켓 화폐가 있습니다. BTC/USDT를 예로 들면 BTC는 거래 화폐, USDT는 마켓 화폐 입니다.

거래 화폐에 대응한 필드는 base-currency 입니다.

마켓 화폐에 대응한 필드는 quote-currency 입니다.

### 계정

유형이 다른 업무는 서로 다른 계정에 대응 하여야 하고  account-id는 서로 다른 유형의 업무 계정의 유일한 마크ID 입니다.

account-id는 /v1/account/accounts에서 가져오고 account-type에 따라 계정을 나눕니다.

계정 유형 포함: 

* spot: 현물 계정  
* point: 수수료 쿠폰 계정

### 주문서, 체결 관련 ID 설명 
* order-id : 주문서의 유일한 번호  
* client-order-id : 고객이 자체 정의한 ID, 본 ID는 주문시 전송하고 주문 성공시 반환하는 order-id와 대응 됩니다, ID 유효기간은 24시간 입니다.  
* match-id : 주문서가 매칭 과정에서 순서 번호 
* trade-id : 체결 유일한 번호

### 주문서 유형
현재 후오비의 주문서 유형은 매수 매도 방향 및 주문서 작업 유형이 있습니다, 예시: buy-market, buy는 매수 매도 방향, market는 작업 유형.    

매수 매도 방향:

* buy : 매수  
* sell: 매도 

주문서 유형:  

* limit : 지정가 주문, 본 유형 주문은 주문 가격과 주문 수량을 지정합니다.  
* market : 시장가 주문, 본 유형 주문은 주문 금액 혹은 주문 수량만 지정하고 주문 가격은 지정 하지 않아도 됩니다. 주문서가 매칭시 직접 상대방과 거래, 금액 혹은 수량이 최소 체결 금액 혹은 체결 수량보다 적을때까지 진행.
* limit-maker : 지정가 주문서, 본 주문서는 매칭시 maker이고 주문서가 거래되면 본 주문서는 매칭 거절 합니다.
* ioc : 즉시 체결 혹은 취소(immediately or cancel), 본 주문서는 매칭 진행 후 직접 체결 되지 않으면 직접 취소 됩니다.(부분 체결 후 남은 부분은 취소 됩니다) 
* stop-limit : Stop-limit 주문서, 시가보다 높거나 낮은 가격의 주문서, 주문서가 사전에 설정한 스톱 가격에 도달시 매칭 대열에 들어갑니다.  

### 주문서 상태
* submitted : 체결 대기, 본 상태 주문서는 매칭 대열에 포함 된 상태 입니다.
* partial-filled : 부분 체결, 본 상태 주문서는 매칭 대열에 포함 되고 주문서의 일정한 부분은 이미 체결 되였고 남은 부분은 체결 대기중인 상태입니다.
* filled : 체결 완료, 본 상태 주문서는 매칭 대열에 포함 되지 않고 주문서의 전부가 이미 체결 된 상태 입니다.
* partial-canceled : 부분 체결 철회. 본 상태 주문서는 매칭 대열에 포함 않 되고 본 상태는 partial-filled에서 온 것입니다. 주문 수량이 부분 체결 되였지만  철회 된 상태 입니다.
* canceled : 철회 완료. 본 상태 주문서는 매칭 대열에 포함 되지 않습니다. 본 상태 주문서는 체결 수량이 없이 철회 완료 된 상태 입니다.
* canceling : 철회중. 본 상태 주문서는 철회 과정에 있는 주문서 입니다. 주문서가 최종적으로 매칭 대열에서 삭제 되여야만 실제로 철회 됩니다, 때문에 본 상태는 중간 과정 상태 입니다.


# 액세스 설명

## API 설명

| API 설명    | 연결 분류                    | 설명                                                         |
| ----------- | ---------------------------- | ------------------------------------------------------------ |
| 기초 유형   | /v1/common/*                 | 기초 유형 API. 코인 종류, 거래페어, 타임스탬프 등을 포함     |
| 시세 유형   | /market/*                    | Public 시세 유형 API. 거래, depth, 시세 등을 포함            |
| 계정 유형   | /v1/account/*  /v1/subuser/* | 계정 유형 API. 계정 정보, 서브 유저 등을 포함                |
| 주문서 유형 | /v1/order/*                  | 주문서 유형 API. 주문, 철회, 주문서 조회, 체결 조회 등을 포함 |

본 API 분류는 대략적인 내용을 정리 하였습니다. 일부 API는 본 규칙을 따르지 않았으니 구체적인 수요에 따라 API 문서를 확인하시기 바랍니다.

## TPS (Rate limit) 빈도 제한 규정

* 한 개의 API Key는 1초에 10번 요청을 보낼 수 있습니다.
* API Key 없이 API 액세스 할때 하나의 IP는 1초에 10번 요청을 보낼 수 있습니다.

예시:

* 자산 오더 유형 API 액세스는 API Key에 따라 빈도 제한을 합니다: 1초 10번
* 시세 유형 API 액세스는 IP에 따라 빈도를 제한합니다: 1초 10번


## 요청 형식

모든 API 액세스는 restful 이고 2가지 방법만 지원합니다: GET와 POST

* GET요청: GET 요청의 경우 모든 매개 변수는 path 매개 변수에 있습니다.
* POST요청:POST 요청의 경우 모든 매개 변수는 요청 본문(body)에 JSON 형식으로 적용합니다. 

## 응답 형식

모든 API 응답은 JSON 형식입니다. JSON 상단에는 4가지 필드가 있습니다: `status`, `ch`,  `ts` ,및`data`.앞 3개 필드는 요청 상태와 속성을 나타내는 필드 이고 실제 응답은 `data`필드에 있습니다.

아래는 응답 형식의 예시 입니다:

```json
{
  "status": "ok",
  "ch": "market.btcusdt.kline.1day",
  "ts": 1499223904680,
  "data": // per API response data in nested JSON object
}
```

| 매개 변수 | 데이터 유형 | 설명                                                         |
| --------- | ----------- | ------------------------------------------------------------ |
| status    | string      | API 응답 상태                                                |
| ch        | string      | API 데이터에 해당하는 데이터 스트림. 일부 API에는 해당 데이터 스트림이 없으므로 이 필드를 제공하지 않습니다. |
| ts        | int         | API가 응답한 베이징 시간으로 조정된 타임 스탬프(단위: ms(millisecond)) |
| data      | object      | API 응답 데이터                                              |

## 오류 정보

### 마켓 API 오류 정보

| 오류 코드         | 설명           |
| ----------------- | -------------- |
| bad-request       | 잘못된 요청    |
| invalid-parameter | 매개 변수 오류 |
| invalid-command   | 명령 오류      |

### 거래 API 오류 정보

| 오류 코드                                                    | 설명                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| base-symbol-error                                            | 존재하지 않는 거래페어입니다.                                |
| base-currency-error                                          | 존재하지 않는 코인입니다.                                    |
| base-date-error                                              | 일자 형식 오류                                               |
| account for id `12,345` and user id `6,543,210` does not exist | `account-id` 오류입니다. GET `/v1/account/accounts`API를 이용하여 조회해주세요. |
| account-frozen-balance-insufficient-error                    | 잔액 부족으로 동결 불가                                      |
| account-transfer-balance-insufficient-error                  | 이체할 잔액 부족                                             |
| bad-argument                                                 | 잘못된 매개 변수                                             |
| api-signature-not-valid                                      | API 서명 오류                                                |
| gateway-internal-error                                       | 시스템이 사용 중입니다. 잠시 후 다시 시도해주세요.           |
| ad-ethereum-addresss                                         | 유효한 이더리움 주소를 입력해주세요.                         |
| order-accountbalance-error                                   | 계정 잔액 부족                                               |
| order-limitorder-price-error                                 | 지정가 주문 가격 제한을 초과하였습니다.                      |
| order-limitorder-amount-error                                | 지정가 주문 수량 제한을 초과하였습니다.                      |
| order-orderprice-precision-error                             | 주문 가격이 소수점 자릿수 제한을 초과하였습니다.             |
| order-orderamount-precision-error                            | 주문 수량이 소수점 자릿수 제한을 초과하였습니다.             |
| order-marketorder-amount-error                               | 주문 수량 제한을 초과였습니다.                               |
| order-queryorder-invalid                                     | 해당 주문은 존재하지 않습니다.                               |
| order-orderstate-error                                       | 주문 상태 오류                                               |
| order-datelimit-error                                        | 조회 가능한 제한 시간을 초과하였습니다.                      |
| order-update-error                                           | 주문 업데이트 오류                                           |

## 제안 사항

1. 시세 유형 데이터 정보는 WebSocket 방식으로 실시간 데이터를 받고 받은 데이터를 캐시 저장 하는것을 제안 합니다.
2. 최근 체결가를 얻는것은 /market/trade API 혹은 WebSocket를 사용하여 market.$symbol.trade.detail 를 구독하여 실시간 체결 가격을 얻는것을 제안합니다.
3. 주문 체결 push는 orders.$symbol.update을 사용할 것을 제안합니다, 본 주제는 순차성이 있고 딜레이가 적습니다.
4. 자산 주문 정보는 WebSocket의 accounts를 구독하고 Rest API를 사용하여 자산 데이터를 알람 요청하여 업데이트 하세요.

# 자주 묻는 질문 Q & A


## 액세스, 인증 관련

### Q1: 매개 회원은 몇개의 Api Key를 신청 가능 한가요?
A: 한 개의 마스터 계정은 Api Key 5개 그룹을 생성 할 수 있고, 각 Api Key는 읽기, 거래, 출금 3가지 권한을 설정할 수 있습니다.

한 개의 마스터 계정은 서브계정 200개를 생성 할 수 있고, 각 서브 계정은 Api Key 5개 그룹을 생성할 수 있고, 각 Api Key는 읽기, 거래 2가지 권한을 설정할 수 있습니다.

다음은 3가지 권한에 대한 설명입니다:

- 읽기 권한: 읽기 권한은 데이터 조회 API에 사용 됩니다: 주문서 조회, 체결 조회 등.
- 거래 권한: 거래 권한은 주문, 철회, 전환 등 API에 사용 됩니다.
- 출금 권한: 출금 권한은 출금 주문 생성, 출금 주문 취소 작업에 사용 됩니다.  

### Q2：오프라인, 타임 아웃 상황이 자주 나타나는 경우
A：아래 경우를 점검하세요:

1. api-cloud.huobi.co.kr 도메인으로 후오비 코리아 API에 액세스하였는지.
2. 일본 클라우드 서버를 사용하였는지.

### Q3：WebSocket이 자주 연결이 끊기는 경우
A：자주 나타나는 원인:

1. Pong을 회신하지 않음. WebSocket 연결은 서버에서 발송하는 Ping메시지를 받은 다음 Pong을 회신하여 안정적 연결을 보장 합니다.
2. 인터넷 상황 때문에 클라이언트에서 Pong 메시지를 발송하였지만 서버에서 받지 못하였을 경우.
3. 인터넷 상황 때문에 연결이 끊기는 경우.
4. WebSocket 연결이 끊기는 경우 재 연결 매커니즘을 생성하여 heartbeat(Ping/Pong) 메시지 정확히 발송한 다음 이외로 연결이 끊기면 시스템이 자동 재연결 하도록 진행.

### Q4：서명 인증이 자주 response 실패 하는 경우
A：Secret Key를 사용하여 서명하기 전 문자열과 다음 문자열의 다른점을 대조하여 사용하세요.

`GET\n`

`api-cloud.huobi.co.kr\n` 

`/v1/account/accounts\n`

`AccessKeyId=rfhxxxxx-950000847-boooooo3-432c0&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2019-10-28T07%3A28%3A38`

대조할 때 다음을 주의 하세요:

1. 서명 필드는 ASCII코드로 정열. 기본 필드 예시:

`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx`

`order-id=1234567890`

`SignatureMethod=HmacSHA256` 

`SignatureVersion=2`  

`Timestamp=2017-05-11T15%3A19%3A30` 

정열한 다음:

`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx`

`SignatureMethod=HmacSHA256`

`SignatureVersion=2`

`Timestamp=2017-05-11T15%3A19%3A30`

`order-id=1234567890`

2. 사인 문자열은 URI 코딩을 해야합니다. 예시:

- 부호 `:`는 `%3A`으로 코딩, space는  `%20`으로 코딩
- 타임 스탬프 양식: `YYYY-MM-DDThh:mm:ss` , URI 코딩을 한다음 `2017-05-11T15%3A19%3A30`  

3. 서명은 base64 코딩을 해야합니다.

4. Get 요청 필드는 서명 문자열에 있어야 합니다

5. 시간은 UTC시간을  YYYY-MM-DDTHH:mm:ss으로 전환합니다.

6. local 시간이 표준 시간과 차이가 있는지 여부를 확인.(차이는 1분 이내 여야 합니다)

7. WebSocket 인증 메시지를 발송하는 경우 메시지는 URI 코딩을 하지 않아도 됩니다.

8. 서명 할때 Host는 API 액세스 할때 Host와 같아야 합니다.

9. Api Key 와 Secret Key 에 숨겨진 특수부호가 있는지 확인해야 합니다. 서명에 영향이 있습니다.

후오비 코리아는 Java, Python3, C++ 3가지 언어의 SDK를 지원합니다. 코딩 언어에 따라 사용 여부를 판단하시고 참고 하시기 바랍니다.

<a href='https://github.com/HuobiRDCenter'>SDK다운</a>   

<a href='https://github.com/HuobiRDCenter/huobi_Python/blob/master/example/python_signature_demo.md'>Python 서명 코드 예시</a>   

<a href='https://github.com/HuobiRDCenter/huobi_Java/blob/master/java_signature_demo.md'>JAVA 서명 코드 예시</a>  

<a href='https://github.com/HuobiRDCenter/huobi_Cpp/blob/master/examples/cpp_signature_demo.md'>C++ 서명 코드 예시</a>  

### Q5：API 액세스 경우 gateway-internal-error 오류 발생 원인

A: 아래의 경우를 확인하시기 바랍니다:

1. account-id 가 정확한 지, `GET /v1/account/accounts` API에서 반환한 데이터를 확인하시기 바랍니다.

2. 인터넷 문제일 가능성이 있으니 잠시 후 다시 시도 하시기 바랍니다.

3. 발송한 데이터 양식이 정확한지 확인하시기 바랍니다.(JSON 양식 수요).

4. POST요청은 header에 `Content-Type:application/json`서명이 필요합니다.

### Q6：API 액세스 경우 login-required 오류 발생 원인
A：아래의 경우를 확인하시기 바랍니다:

1. AccessKey 필드를 URL에 넣어야 합니다.

2. account-id 가 정확한 지, `GET /v1/account/accounts` API에서 반환한 데이터를 확인하시기 바랍니다.

3. POST 요청은 body 내용을 서명에 넣지 않아도 됩니다.

4. GET 요청은 필드를 ASCII코드 순서로 정렬해야 합니다.

## 시세 관련
### Q1：시세 데이터 업데이트 주기
A：시세 데이터의 업데이트 주기는 1회/초 입니다. RESTful 조회 혹은 Websocket 푸쉬 모두 1회/초 입니다. 만약 실시간 매수 매도 데이터가 필요하면 WebSocket를 사용하여 market.$symbol.bbo 주제를 구독하세요. 본 주제는 매수 매도 데이터를 실시간으로 푸쉬 합니다.

### Q2：24시간 시세 API(/market/detail) 데이터 API의 체결량은 마이너스 성장 가능 여부
A：/market/detail API의 체결량, 체결 금액은 24시간 roll-data 입니다.(구간은 24시간) 후구간의 누적 체결량, 누적 체결액이 전구간보다 작은 상황이 있을 수 있습니다.

### Q3：최신 체결 가격을 얻는 방법
A：REST API `GET /market/trade` 를 사용하여 최신 체결 데이터를 받으세요. 혹은 WebSocket를 사용하여 market.$symbol.trade.detail 주제를 구독하여 제공되는 데이터에서 price 를 받을 수 있습니다.

### Q4：K-line의 계산 시작시간
A： K라인 주기는 UTC/GMT+08:00을 기준으로 시작하여 계산합니다. 예) 일 K-line은 UTC/GMT+08:00 0:00시  -  UTC/GMT+08:00 다음날 0:00시

## 거래 관련
### Q1：account-id？
A： account-id는 고객의 서로 다른 계정 ID에 대응 됩니다. /v1/account/accounts API를 통하여 조회하고 account-type으로 구체적인 계정을 나눕니다.

계정 유형 포함:

- spot 현물 계정 
- point 수수료 쿠폰 계정


### Q2：client-order-id？
A： client-order-id는 주문 요청 표시의 필드로 유형은 문자열이고 글자수는 64 입니다. 본 id는 고객이 자체 생성하고 유효기간은 24시간 입니다.

### Q3：주문 수량, 금액, 자릿수 제한, 소수점뒤 자리수 정보 조회

A： Rest API `GET /v1/common/symbols`를 사용하여 관련 거래페어의 정보를 얻습니다. 주문 시 최소 주문 수량과 최소 주문 금액 사이를 구별하세요. 

자주 발생하는 오류 예시:

- order-value-min-error: 주문 금액이 최소 거래 금액보다 작음 
- order-orderprice-precision-error : 지정가 가격 소수점자리 오류 
- order-orderamount-precision-error : 주문 수량 소수점자리 오류  
- order-limitorder-price-max-error : 지정가 가격이 최대 가격보다 높음 
- order-limitorder-price-min-error : 지정가 가격이 최소 가격보다 낮음  
- order-limitorder-amount-max-error : 지정가 주문 수량이 최대량보다 높음  
- order-limitorder-amount-min-error : 지정가 주문 수량이 최소량보다 낮음

### Q4：WebSocket 주문서 업데이트 푸쉬 주제 orders.$symbol 와 orders.$symbol.update의 차이점
A： 다른점은 다음과 같습니다:

1. order.$symbol 주제는 기존 푸쉬 주제로 일정한 시간이 지난 다음 주제의 업그레이드와 사용을 정지 합니다.order.$symbol.update 주제를 추천 합니다.
2. 새로운 주제 orders.$symbol.update는 순차성이 있어 데이터가 엄격히 매칭 체결 순서에 따라 푸쉬 되고 실시간이고 딜레이가 낮습니다.
3. 중복 데이터 푸쉬를 줄이고 더 빠른 속도를 위하여 orders.$symbol.update 푸쉬에 기존 주문서 수량, 가격 정보를 넣지 않습니다. 만약 본 데이터가 필요하면 주문시 local에서 주문 정보를 관리하거나 푸쉬 메시지를 받은 다음 Rest API로 조회하세요.

### Q5： 주문서가 체결 완료되었다는 메시지를 받은 다음 다시 주문하면 잔액 부족으로 표시
A: 주문의 신속성과 낮은 딜레이를 보장하기 위하여 주문 푸쉬 결과는 매칭이 이루어진 다음 직접 푸쉬 합니다. 이때 주문은 잔액 결산을 미완성한 상태 입니다.  

정확히 주문을 보장하도록 다음 방법을 추천합니다:

1. 자산 푸쉬 주제 `accounts` 으로 자산 변경 메시지을 받아서 자금이 이미 결산 되었는지 확인합니다.

2. 주문서 푸쉬 메시지를 받은 다음 Rest API를 사용하여 계정 잔액을 확인하고, 계정 자금이 충족한지 여부를 확인합니다.

3. 계정에 충분한 자금을 마련합니다.

### Q6: 매칭 결과에 filled-fees와 filled-points의 차이점
A: 매칭 결과에 체결 수수료는 일반 수수료와 수수료 쿠폰 2가지로 나뉩니다. 2가지 유형은 동시에 진행되지 않습니다.

1. 일반 수수료는 체결시 HT차감, 수수료 쿠폰 차감을 오픈하지 않고 고유 코인으로 수수료를  차감하는 경우입니다. 예: BTC/USDT 거래페어에서 BTC를 매수하고 filled-fees 필드가 null이 아니면 일반 수수료를 차감합니다, 단위는 BTC.
2. 수수료 쿠폰은 체결 시 HT차감, 수수료 쿠폰 차감을 오픈하고 HT 혹은 수수료 쿠폰으로 수수료를 차감하는 경우입니다. 예: BTC/USDT 거래페어에서 BTC를 매수하고 HT수수료 쿠폰이 충족할 경우 filled-fees가 null이고 filled-points가 null이 아니면 HT 혹은 수수료 쿠폰으로 수수료를 차감합니다. 차감 단위는 fee-deduct-currency 필드를 참고하세요.

### Q7: 매칭 결과에 match-id와 trade-id의 차이점
A: match-id는 주문서가 매칭중에서 순서 번호이고 trade-id는 체결시 순번 입니다. 한 개의 match-id에는 여러개 trade-id(체결시)가 있거나 trade-id가 없는 경우(주문서 생성, 철회 주문)도 있습니다.

### Q8: 시장 동태에 따라 매수 혹은 매도 주문 생성시 한정가 오류

A: 후오비 코리아는 최근 체결가에서 위아래의 일정한 범위 내 한정가 보호 조치를 하고 있습니다. 유동성이 적은 코인은 시장 동태 데이터에 따라 주문하는 경우 한정가 보호 조치를 발동할 수 있습니다. ws 푸쉬에 따른 체결가 + 시장 동태 데이터 정보에 따라 주문하시기 바랍니다.


## 계정 입출금 관련
### Q1：출금 오더 생성시 api-not-support-temp-addr 오류 생성
A：보안상 API로 출금 오더 생성시 출금 주소 리스트에 등록된 주소로만 출금 가능합니다. API 로 출금 주소 리스트에 주소를 추가 할 수 없고 웹 혹은 앱에서 주소를 추가한 다음 API로 작업 할 수 있습니다.

### Q2：USDT 출금시 Invaild-Address 오류 발생
A：USDT 코인은 전형적인 여러 가지 체인 지원 가능한 코인입니다. 출금 주문서 생성하는 경우 chain 필드를 관련 주소 유형에 넣어야 합니다. 아래 리스트는 체인과 chain 필드의 대응 관계입니다:

| 체인            | chain 필드 |
| --------------- | ---------- |
| ERC20 (기본 값) | usdterc20  |
| OMNI            | usdt       |
| TRX             | trc20usdt  |

chain 필드가 NULL이면 체인이 ERC20이고 혹은 필드 값은 `usdterc20`로 노출 됩니다.

OMNI혹은 TRX 체인으로 출금하면 chain필드는 usdt 혹은 trc20usdt으로 요청하세요. chain 필드는 `GET /v2/reference/currencies` API를 참고하세요.


### Q3：코인 출금 생성시 fee 필드 요청 방식
A： `GET /v2/reference/currencies` API의 조회 값을 참고하세요. 조회 데이터에 withdrawFeeType는 코인 출금 수수료 유형 입니다. 유형에 따라 대응 필드를 선택하고 출금 수수료를 설정하세요.

코인 출금 수수료 유형에 다음을 포함:  

- transactFeeWithdraw : 1회 코인 출금 수수료(고정 유형에만 유효, withdrawFeeType=fixed) 
- minTransactFeeWithdraw : 최소1회 코인 출금 수수료(구간 유형에만 유효, withdrawFeeType=circulated) 
- maxTransactFeeWithdraw : 최대 1회 코인 출금 수수료(구간 유형과 상한이 있는 비례 유형에만 유효, withdrawFeeType=circulated or ratio)
- transactFeeRateWithdraw :  1회 코인 출금 수수료(비례 유형에만 유효, withdrawFeeType=ratio)

### Q4：코인 출금 한도 조회
A：/v2/account/withdraw/quota API 조회 값을 참고하세요. 조회 데이터는 1회 한도, 당일 한도, 현재 한도, 총 출금 한도와 잔여 출금 한도 데이터가 포함됩니다.

메모: 고액의 코인 출금 수요가 있고 코인 출금 한도 관련 상향 조정을 원하시면 후오비 코리아 세일즈팀 (이메일 주소:API-kr@huobi.com)에 요청하세요.


## 기술 지원
아직도 도움이 필요하시다면 후오비 코리아에 연락주세요: 

1. 후오비 코리아 API 커뮤니티 텔레그램(http://bit.ly/2lY4o7v)

2. 이메일 주소: API-kr@huobi.com

   고객님의 문제점을 신속히 이해하고 해결하기 위하여 아래와 같은 양식으로 메일을 보내주세요.  

`1. UID`  
`2. AccessKey`(API Key)    
`3. Full URL`  
`4. 요청 필드`    
`5. 요청 시간`    
`6. API 조회 데이터`    
`7. 설명:(작업 절차, 필드에 대한 문의 내용, 문제 발생 빈도)`    
`8. 서명 전 문자열(서명 인증 오류 시 필수)`   

다음은 양식 예시 입니다:

`1. UID：123456`   
`2. AccessKey：rfhxxxxx-950000847-boooooo3-432c0`  
`3. Full URL： https://api-cloud.huobi.co.kr/v1/account/accounts?&SignatureVersion=2&SignatureMethod=HmacSHA256&Timestamp=2019-11-06T03%3A25%3A39&AccessKeyId=rfhxxxxx-950000847-boooooo3-432c0&Signature=HhJwApXKpaLPewiYLczwfLkoTPnFPHgyF61iq0iTFF8%3D`  
`4. 요청 필드：없음`    
`5. 요청 시간: 2019-11-06 11:26:14`   
`6. API 조회 오리지널 테이터:{"status":"error","err-code":"api-signature-not-valid","err-msg":"Signature not valid: Incorrect Access key [Access key错误]","data":null}`  
`7. 문제 설명: API 액세스 시 발생한 오류`  
`8. 서명 전 문자열(서명 인증 오류 시 필수)`    
`GET\n`  
`api-cloud.huobi.co.kr\n`  
`/v1/account/accounts\n`   
`AccessKeyId=rfhxxxxx-950000847-boooooo3-432c0&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2019-11-06T03%3A26%3A13`   

주의: Access Key는 고객의 신분을 증명하고 계정 보안에는 영향이 없습니다. 하지만 Secret Key 는 절대 타인에게 공유하면 안됩니다, 만약 실수로 Secret Key가 노출되였다면 API Key를 [삭제](https://www.huobi.co.kr/zh-cn/api/)하여 손실을 막기 바랍니다.



# 기본 정보

## 모든 거래페어 정보 수집

본 API는 후오비 코리아에서 지원하는 모든 거래페어 정보를 제공합니다.

```shell
curl "https://api-cloud.huobi.co.kr/v1/common/symbols"
```


### HTTP 요청

- GET `/v1/common/symbols`

### 요청 매개 변수

본 API는 매개 변수가 필요하지 않습니다.

> Responds:

```json
  "data": [
   {"base-currency":"etc",
    "quote-currency":"usdt",
    "price-precision":6,
    "amount-precision":4,
    "symbol-partition":"default",
    "symbol":"etcusdt",
    "state":"online",
    "value-precision":8,
    "min-order-amt":0.001,
    "max-order-amt":10000,
    "min-order-value":0.0001
    },
    {
    "base-currency":"ltc",
    "quote-currency":"usdt",
    "price-precision":6,
    "amount-precision":4,
    "symbol-partition":"main",
    "symbol":"ltcusdt",
    "state":"online",
    "value-precision":8,
    "min-order-amt":0.001,
    "max-order-amt":10000,
    "min-order-value":100,
    "leverage-ratio":4
    }
  ]
```

### 응답 데이터

| 매개 변수        | 데이터 유형 | 설명                                                         |
| ---------------- | ----------- | ------------------------------------------------------------ |
| base-currency    | string      | 기본 통화의 심볼명                                           |
| quote-currency   | string      | 기축 통화의 심볼명                                           |
| price-precision  | integer     | 코인 가격의 소수점 자릿수(소수점 뒤 자리수)                  |
| amount-precision | integer     | 코인 수량의 소수점 자릿수(소수점 뒤 자리수)                  |
| symbol-partition | string      | 거래 범위, 가능한 값: [main, innovation]                     |
| symbol           | string      | 거래페어                                                     |
| state            | string      | 거래페어 상태; 가능한 값:[online，offline,suspend] online - 거래페어 온라인; offline - 거래페어 오프라인, 거래 불가: suspend -- 거래 잠정 중지 |
| value-precision  | integer     | 거래 금액의 소수점 자릿수(소수점 뒤 자리수)                  |
| min-order-amt    | long        | 최소 주문량(주문량은 주문 유형이 지정가 주문 혹은 sell-market시 오더 API에 'amount' 전송) |
| max-order-amt    | long        | 거래페어 최대 주문량                                         |
| min-order-value  | long        | 최소 주문 금액(주문 금액은 주문 유형이 지정가 주문시 주문 API에 전송한(amount * price). 주문 유형이 buy-market시 주문 API에 전송한(amount). |

## 모든 코인 회득

본 API에서 모든 후오비 코리아가 지원하는 코인이 제공됩니다. 


```shell
curl "https://api-cloud.huobi.co.kr/v1/common/currencys"
```

### HTTP 요청

- GET `/v1/common/currencys`


### 요청 매개 변수

본 API는 매개 변수가 필요하지 않습니다.


> Response:

```json
  "data": [
    "usdt",
    "eth",
    "etc"
  ]
```

### 응답 데이터

<aside class="notice">조회 된 "data" 개체는 각각 지원되는 통화를 나타내는 문자열 배열입니다.</aside>
## APIv2 체인 참고 정보

본 노드는 각 코인 및 블록체인의 static state 정보 조회에 사용됩니다.(공용 데이터)

### HTTP 요청

- GET `/v2/reference/currencies`

```shell
curl "https://api-cloud.huobi.co.kr/v2/reference/currencies?currency=usdt"
```

### 요청 매개 변수

| 매개 변수      | 필수 여부 | 유형    | 설명             | 값의 범위                                                   |
| -------------- | --------- | ------- | ---------------- | ----------------------------------------------------------- |
| currency       | false     | string  | 코인 종류        | btc, ltc, bch, eth, etc ...(`GET /v1/common/currencys`참고) |
| authorizedUser | false     | boolean | 이미 인증한 고객 | true or false (필드를 요청하지 않으면 기본값은 true 입니다) |

> Response:

```json
{
    "code":200,
    "data":[
        {
            "chains":[
                {
                    "chain":"trc20usdt",
                    "depositStatus":"allowed",
                    "maxTransactFeeWithdraw":"1.00000000",
                    "maxWithdrawAmt":"280000.00000000",
                    "minDepositAmt":"100",
                    "minTransactFeeWithdraw":"0.10000000",
                    "minWithdrawAmt":"0.01",
                    "numOfConfirmations":999,
                    "numOfFastConfirmations":999,
                    "withdrawFeeType":"circulated",
                    "withdrawPrecision":5,
                    "withdrawQuotaPerDay":"280000.00000000",
                    "withdrawQuotaPerYear":"2800000.00000000",
                    "withdrawQuotaTotal":"2800000.00000000",
                    "withdrawStatus":"allowed"
                },
                {
                    "chain":"usdt",
                    "depositStatus":"allowed",
                    "maxWithdrawAmt":"19000.00000000",
                    "minDepositAmt":"0.0001",
                    "minWithdrawAmt":"2",
                    "numOfConfirmations":30,
                    "numOfFastConfirmations":15,
                    "transactFeeRateWithdraw":"0.00100000",
                    "withdrawFeeType":"ratio",
                    "withdrawPrecision":7,
                    "withdrawQuotaPerDay":"90000.00000000",
                    "withdrawQuotaPerYear":"111000.00000000",
                    "withdrawQuotaTotal":"1110000.00000000",
                    "withdrawStatus":"allowed"
                },
                {
                    "chain":"usdterc20",
                    "depositStatus":"allowed",
                    "maxWithdrawAmt":"18000.00000000",
                    "minDepositAmt":"100",
                    "minWithdrawAmt":"1",
                    "numOfConfirmations":999,
                    "numOfFastConfirmations":999,
                    "transactFeeWithdraw":"0.10000000",
                    "withdrawFeeType":"fixed",
                    "withdrawPrecision":6,
                    "withdrawQuotaPerDay":"180000.00000000",
                    "withdrawQuotaPerYear":"200000.00000000",
                    "withdrawQuotaTotal":"300000.00000000",
                    "withdrawStatus":"allowed"
                }
            ],
            "currency":"usdt",
            "instStatus":"normal"
        }
        ]
}

```

### 응답 데이터


| 매개 변수               | 필수 여부 | 데이터 유형 | 설명                                                         | 값의 범위              |
| ----------------------- | --------- | ----------- | ------------------------------------------------------------ | ---------------------- |
| code                    | true      | int         | 상태 코드                                                    |                        |
| message                 | false     | string      | 오류 설명(값이 있는 경우)                                    |                        |
| data                    | true      | object      |                                                              |                        |
| { currency              | true      | string      | 코인 종류                                                    |                        |
| { chains                | true      | object      |                                                              |                        |
| chain                   | true      | string      | 체인 명칭                                                    |                        |
| numOfConfirmations      | true      | int         | Confirmation에 필요한 확인 차수(확인 차수에 도달하면 출금 허가) |                        |
| numOfFastConfirmations  | true      | int         | Fast Confirmation에 필요한 확인 차수(확인 차수에 도달하면 거래는 가능하지만 출금은 금지) |                        |
| minDepositAmt           | true      | string      | 매회 최소 코인 입금 금액                                     |                        |
| depositStatus           | true      | string      | 코인 입금 상태                                               | allowed,prohibited     |
| minWithdrawAmt          | true      | string      | 매회 최소 코인 출금 금액                                     |                        |
| maxWithdrawAmt          | true      | string      | 매회 최대 코인 출금 금액                                     |                        |
| withdrawQuotaPerDay     | true      | string      | 당일 코인 출금 한도                                          |                        |
| withdrawQuotaPerYear    | true      | string      | 올해 코인 출금 한도                                          |                        |
| withdrawQuotaTotal      | true      | string      | 코인 출금 총액                                               |                        |
| withdrawPrecision       | true      | int         | 코인 출금 소수점 자리                                        |                        |
| withdrawFeeType         | true      | string      | 코인 출금 수수료 유형(특정 코인은 특정 체인에서의 수수료, 유형은 유일) | fixed,circulated,ratio |
| transactFeeWithdraw     | false     | string      | 1회 코인 출금 수수료( 고정 유형에만 유효, withdrawFeeType=fixed) |                        |
| minTransactFeeWithdraw  | false     | string      | 최소 1회 코인 출금 수수료(구간 유형에만 유효, withdrawFeeType=circulated) |                        |
| maxTransactFeeWithdraw  | false     | string      | 1회 최대 코인 출금 수수료(구간 유형과 상한이 있는 비례 유형에만 유효, withdrawFeeType=circulated or ratio) |                        |
| transactFeeRateWithdraw | false     | string      | 1회 코인 출금 수수료(비례 유형에만 유효, withdrawFeeType=ratio) |                        |
| withdrawStatus}         | true      | string      | 코인 출금 상태                                               | allowed,prohibited     |
| instStatus }            | true      | string      | 코인 상태                                                    | normal,delisted        |

### 상태 코드

| 상태 코드 | 오류 정보                           | 오류 설명    |
| --------- | ----------------------------------- | ------------ |
| 200       | success                             | 요청 성공    |
| 500       | error                               | 시스템 오류  |
| 2002      | invalid field value in "field name" | 무효 필드 값 |

## 현재 시스템 시간 조회

본 API는 현재 시스템 시간 정보(UTC/GMT+08:00)가 제공됩니다. *시간 단위: ms(millisecond)

```shell
curl "https://api-cloud.huobi.co.kr/v1/common/timestamp"
```

### HTTP 요청

- GET `/v1/common/timestamp`

### 요청 매개 변수

본 API에서 모든 파라미터를 받지 않습니다.

> Response:

```json
  "data": 1494900087029
```

# 시세 데이터

## K-line 차트 정보(캔들 도표)

본 API는 K-라인의 내역을 제공합니다.

### HTTP 요청

- GET `/market/history/kline`

```shell
curl "https://api-cloud.huobi.co.kr/market/history/kline?period=1day&size=200&symbol=btcusdt"
```

### 요청 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명                                     | 값의 범위                                                 |
| --------- | ----------- | --------- | ------ | ---------------------------------------- | --------------------------------------------------------- |
| symbol    | string      | true      | N/A    | 거래페어                                 | btcusdt, ethbtc 등                                        |
| period    | string      | true      | N/A    | 각 차트의 시간 간격의 단위를 제공합니다. | 1min, 5min, 15min, 30min, 60min, 1day, 1mon, 1week, 1year |
| size      | integer     | false     | 150    | K챠트 개수                               | [1, 2000]                                                 |

<aside class="notice">현재 REST API는 UTC/GMT+08:00만 지원합니다. 만약 기존 지정한 시간대 데이터가 필요하시면 Websocket API 중의 차트 API를 참고하세요.</aside>
<aside class="notice">hb10값인 경우, symbol은 “hb10”으로 요청.</aside>
<aside class="notice">K라인 주기는 UTC/GMT+08:00을 기준으로 시작하여 계산합니다. 예) 일 K-line은 UTC/GMT+08:00 0:00시  -  UTC/GMT+08:00 다음날 0:00시.</aside>
> Response:

```json
[
  {
    "id": 1499184000,
    "amount": 37593.0266,
    "count": 0,
    "open": 1935.2000,
    "close": 1879.0000,
    "low": 1856.0000,
    "high": 1940.0000,
    "vol": 71031537.97866500
  }
]
```

### 응답 데이터

| 매개 변수 | 데이터 유형 | 설명                                                         |
| --------- | ----------- | ------------------------------------------------------------ |
| id        | integer     | UTC/GMT+08:00의 타임 스탬프로 조정(단위: 초)하고 K-라인의 아이디로 사용 |
| amount    | float       | 기본 통화 기준으로 측정된 거래량                             |
| count     | integer     | 거래 건수                                                    |
| open      | float       | 이 단계 시작가                                               |
| close     | float       | 이 단계의 종가                                               |
| low       | float       | 이 단계 최저가                                               |
| high      | float       | 이 단계 최고가                                               |
| vol       | float       | 기축 통화 기준으로 측정하는 거래량                           |



## 시세 집계（Ticker）

본 API는 시세 정보를 수집하여 지난 24시간의 거래 집계 정보를 제공합니다.

### HTTP 요청

- GET `/market/detail/merged`

```shell
curl "https://api-cloud.huobi.co.kr/market/detail/merged?symbol=ethusdt"
```

### 요청 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명     | 값의 범위                                        |
| --------- | ----------- | --------- | ------ | -------- | ------------------------------------------------ |
| symbol    | string      | true      | NA     | 거래페어 | btcusdt, ethbtc...(`GET /v1/common/symbols`참고) |


> Response:

```json
{
  "id":1499225271,
  "ts":1499225271000,
  "close":1885.0000,
  "open":1960.0000,
  "high":1985.0000,
  "low":1856.0000,
  "amount":81486.2926,
  "count":42122,
  "vol":157052744.85708200,
  "ask":[1885.0000,21.8804],
  "bid":[1884.0000,1.6702]
}
```

### 응답 데이터

| 매개 변수 | 데이터 유형 | 설명                                   |
| :-------- | :---------- | :------------------------------------- |
| id        | integer     | N/A                                    |
| amount    | float       | 기본 통화 기준으로 측정된 거래량       |
| count     | integer     | 거래 건수                              |
| open      | float       | 이 단계의 시작가                       |
| close     | float       | 이 단계의 종가                         |
| low       | float       | 이 단계 최저가                         |
| high      | float       | 이 단계 최고가                         |
| vol       | float       | 기축 통화 기준으로 측정하는 거래량     |
| bid       | object      | 현재 최고 매도가 [price, quote volume] |
| ask       | object      | 현재 최저 매수가 [price, quote volume] |


## 모든 거래 페어의 최신 시세 Tickers

24시간 간격으로 모든 거래 페어의 시세 수집

<aside class="notice">본 API는 모든 거래 페어에 대한 시세 정보를 제공하므로 데이터 양이 많습니다.</aside>
### HTTP 요청

- GET `/market/tickers`

```shell
curl "https://api-cloud.huobi.co.kr/market/tickers"
```



### 요청 매개 변수

본 API는 매개 변수가 필요하지 않습니다.

> Response:

```json
[  
    {  
        "open":0.044297,      // 개장가
        "close":0.042178,     // 종가
        "low":0.040110,       // 최고가
        "high":0.045255,      // 최저가
        "amount":12880.8510,  
        "count":12838,
        "vol":563.0388715740,
        "symbol":"ethbtc"
    },
    {  
        "open":0.008545,
        "close":0.008656,
        "low":0.008088,
        "high":0.009388,
        "amount":88056.1860,
        "count":16077,
        "vol":771.7975953754,
        "symbol":"ltcbtc"
    }
]
```

### 응답 데이터

핵심 응답 내용은 개체 열이며, 각 개체에는 다음 필드가 포함됩니다.

| 매개 변수 | 데이터 유형 | 설명                               |
| --------- | ----------- | ---------------------------------- |
| amount    | float       | 기본 통화 기준으로 측정된 거래량   |
| count     | integer     | 거래 건수                          |
| open      | float       | 이 단계의 시작가                   |
| close     | float       | 이 단계의 종가                     |
| low       | float       | 이 단계 최저가                     |
| high      | float       | 이 단계 최고가                     |
| vol       | float       | 기축 통화 기준으로 측정하는 거래량 |
| symbol    | string      | 거래 페어 예) btcusdt, ethbtc      |

## Market 누적주문량 정보

본 API에서 지정된 거래 페어에 대한 현재 Market 누적 주문량 정보를 제공합니다.

### HTTP 요청

- GET `/market/depth`

```shell
curl "https://api-cloud.huobi.co.kr/market/depth?symbol=btcusdt&type=step2"
```

### 요청 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명                                            | 값의 범위                                        |      |
| --------- | ----------- | --------- | ------ | ----------------------------------------------- | ------------------------------------------------ | ---- |
| symbol    | string      | true      | N/A    | 거래 페어                                       | btcusdt, ethbtc 등(`GET /v1/common/symbols`참고) |      |
| depth     | integer     | false     | 20     | 누적 주문량 제공                                | 5，10，20                                        |      |
| type      | string      | true      | step0  | 누적 주문량 가격 집계 (자세한 내용은 아래 참고) | step0，step1，step2，step3，step4，step5         |      |

<aside class="notice">type값이 ‘step0’인 경우，‘depth’의 기본값은 20이 아니고 150입니다. </aside>
**매개 변수 유형의 각 값에 대한 설명**

| 값    | 설명                        |
| ----- | --------------------------- |
| step0 | 집계하지 않음               |
| step1 | 집계 수준 = 정밀도 * 10     |
| step2 | 집계 수준 = 정밀도 * 100    |
| step3 | 집계 수준 = 정밀도 * 1000   |
| step4 | 집계 수준 = 정밀도 * 10000  |
| step5 | 집계 수준 = 정밀도 * 100000 |

> Response:

```json
{
    "version": 31615842081,
    "ts": 1489464585407,
    "bids": [
      [7964, 0.0678], // [price, amount]
      [7963, 0.9162],
      [7961, 0.1],
      [7960, 12.8898],
      [7958, 1.2],
      ...
    ],
    "asks": [
      [7979, 0.0736],
      [7980, 1.0292],
      [7981, 5.5652],
      [7986, 0.2416],
      [7990, 1.9970],
      ...
    ]
  }
```

### 응답 데이터

<aside class="notice">조회 된 JSON 최상위 데이터 개체의 이름은 일반적인 'data'대신 'tick'입니다.</aside>
| 매개 변수 | 데이터 유형 | 설명                                                        |
| --------- | ----------- | ----------------------------------------------------------- |
| ts        | integer     | UTC/GMT+08:00의 타임 스탬프로 조정 (단위: ms(milliseconds)) |
| version   | integer     | 내부 필드                                                   |
| bids      | object      | 현재 모든 매수 주문                                         |
| asks      | object      | 현재 모든 매도 주문                                         |

## 최근 Market 거래 기록

본 API는 지정한 거래 페어의 최신 거래 데이터를 제공합니다.

### HTTP 요청

- GET `/market/trade`

```shell
curl "https://api-cloud.huobi.co.kr/market/trade?symbol=ethusdt"
```

### 요청 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명                                               |
| --------- | ----------- | --------- | ------ | -------------------------------------------------- |
| symbol    | string      | true      | NA     | btcusdt, ethbtc...（`GET /v1/common/symbols`참고） |

> Response:

```json
{
    "id": 600848670,
    "ts": 1489464451000,
    "data": [
      {
        "id": 600848670,
        "trade-id": 102043494568,
        "price": 7962.62,
        "amount": 0.0122,
        "direction": "buy",
        "ts": 1489464451000
      }
    ]
}
```

### 응답 데이터

<aside class="notice">조회 된 JSON 최상위 데이터 개체의 이름은 일반적인 'data'대신 'tick'입니다.</aside>
| 매개 변수 | 데이터 유형 | 설명                                                        |
| --------- | ----------- | ----------------------------------------------------------- |
| id        | integer     | 유일한 거래 id(삭제 예정)                                   |
| trade-id  | integer     | 유일한 거래 ID（NEW）                                       |
| amount    | float       | 거래화폐을 단위으로 하는 거래량                             |
| price     | float       | 마켓화폐을 단위으로 하는 체결가격                           |
| ts        | integer     | UTC/GMT+08:00의 타임 스탬프으로 조정, 단위는 밀리초.        |
| direction | string      | 거래방향：“buy”  혹은 “sell”, “buy”는 매수，“sell” 는 매도. |

## 최근 체결 기록

본 API에서 지정한 거래 페어의 최근 모든 체결 기록이 제공됩니다. 

### HTTP 요청

- GET `/market/history/trade`

```shell
curl "https://api-cloud.huobi.co.kr/market/history/trade?symbol=ethusdt&size=2"
```

### 요청 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명                                             |
| --------- | ----------- | --------- | ------ | ------------------------------------------------ |
| symbol    | string      | true      | NA     | btcusdt, ethbtc...(`GET /v1/common/symbols`참고) |
| size      | integer     | false     | 1      | 제공되는 체결 기록수, 최대값 2000                |

> Response:

```json
[  
   {  
      "id":31618787514,
      "ts":1544390317905,
      "data":[  
         {  
            "amount":9.000000000000000000,
            "ts":1544390317905,
            "trade-id": 102043483472,
            "id":3161878751418918529341,
            "price":94.690000000000000000,
            "direction":"sell"
         },
         {  
            "amount":73.771000000000000000,
            "ts":1544390317905,
            "trade-id": 102043483473
            "id":3161878751418918532514,
            "price":94.660000000000000000,
            "direction":"sell"
         }
      ]
   },
   {  
      "id":31618776989,
      "ts":1544390311353,
      "data":[  
         {  
            "amount":1.000000000000000000,
            "ts":1544390311353,
            "trade-id": 102043494568,
            "id":3161877698918918522622,
            "price":94.710000000000000000,
            "direction":"buy"
         }
      ]
   }
]
```

### 응답 데이터

<aside class="notice">제공 된 데이터 대상은 하나의 object array이고 각 array의 요소는 UTC/GMT+08:00의 타임 스탬프(단위는 밀리초)으로 조정한 모든 체결기록이며, 체결 기록은 array 형식으로 표시됩니다.</aside>
| 매개 변수 | 데이터 유형 | 설며                                                        |
| --------- | ----------- | ----------------------------------------------------------- |
| id        | integer     | 유일한 거래 id(삭제 예정)                                   |
| trade-id  | integer     | 유일한 거래ID（NEW）                                        |
| amount    | float       | 기본 통화 기준으로 측정된 거래량                            |
| price     | float       | 기축 통화를 단위로 하는 체결 가격                           |
| ts        | integer     | UTC/GMT+08:00의 타임 스탬프로 조정 (단위: ms(milliseconds)) |
| direction | string      | 거래: “buy” 또는 “sell”, “buy” 매수，“sell” 매도            |

## 최근 24시간 시세 데이터

본 API는 최근 24시간 시세 데이터의 합계를 제공합니다.

### HTTP 요청

- GET `/market/detail`

```shell
curl "https://api-cloud.huobi.co.kr/market/detail?symbol=ethusdt"
```

### 요청 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명                                             |
| --------- | ----------- | --------- | ------ | ------------------------------------------------ |
| symbol    | string      | true      | NA     | btcusdt, ethbtc...(`GET /v1/common/symbols`참고) |

> Response:

```json
{  
   "amount":613071.438479561,
   "open":86.21,
   "close":94.35,
   "high":98.7,
   "id":31619471534,
   "count":138909,
   "low":84.63,
   "version":31619471534,
   "vol":5.6617373443873316E7
}
```

### 응답 데이터

<aside class="notice">조회 된 JSON 최상위 데이터 개체의 이름은 일반적인 'data'대신 'tick'입니다.</aside>
| 매개 변수 | 데이터 유형 | 설명                               |
| --------- | ----------- | ---------------------------------- |
| id        | integer     | 고유 거래 ID                       |
| amount    | float       | 기본 통화 기준으로 측정된 거래량   |
| count     | integer     | 거래 건수                          |
| open      | float       | 이 단계의 시작가                   |
| close     | float       | 이 단계의 종가                     |
| low       | float       | 이 단계 최저가                     |
| high      | float       | 이 단계 최고가                     |
| vol       | float       | 기축 통화 기준으로 측정하는 거래량 |
| version   | integer     | 내부 데이터                        |

# 계정 관련

<aside class="notice">Access 계정과 연결된 API에는 서명 인증이 필요합니다.</aside>
## 계정 정보 

API Key 권한: 읽기

사용자`account-id`의 모든 계정 ID 및 관련 정보를 조회합니다.

### HTTP 요청

- GET `/v1/account/accounts`

### 요청 매개 변수

없음.

> Response:

```json
{
  "data": [
    {
      "id": 100001,
      "type": "spot",
      "subtype": "",
      "state": "working"
    }
  ]
}
```

### 응답 데이터

| 매개 변수 | 데이터 유형 | 필수 여부 | 설명       | 값의 범위                               |
| --------- | ----------- | --------- | ---------- | --------------------------------------- |
| id        | long        | true      | account-id |                                         |
| state     | string      | true      | 계정 상태  | working：정상, lock：계정이 잠금됨      |
| type      | string      | true      | 계정 유형  | spot: 자산 계정, point: 포인트 쿠폰계정 |


## 계정 잔액

API Key 권한: 읽기

지정한 계정의 잔액 조회가 가능합니다. 아래 계정 지원:

spot：현물 계정, point：수수료 쿠폰 계정

### HTTP 요청

- GET `/v1/account/accounts/{account-id}/balance`

### 요청 매개 변수

| 매개 변수  | 데이터 유형 | 필수 여부 | 설명                                                         |
| ---------- | ----------- | --------- | ------------------------------------------------------------ |
| account-id | string      | true      | account-id을 path에 작성 ，`GET /v1/account/accounts` 으로 조회 가능함 |

> Response:

```json
{
  "data": {
    "id": 100009,
    "type": "spot",
    "state": "working",
    "list": [
      {
        "currency": "usdt",
        "type": "trade",
        "balance": "5007.4362872650"
      },
      {
        "currency": "usdt",
        "type": "frozen",
        "balance": "348.1199920000"
      }
    ]
  }
}
```

### 응답 데이터

| 매개 변수 | 데이터 유형 | 필수 여부 | 설명      | 값의 범위                                |
| --------- | ----------- | --------- | --------- | ---------------------------------------- |
| id        | long        | true      | 계정 ID   |                                          |
| state     | string      | true      | 계정 상태 | working：정상, lock： 계정 잠금 처리     |
| type      | string      | true      | 계정 유형 | spot: 현물 계정, point: 수수료 쿠폰 계정 |
| list      | false       | Array     |           |                                          |

list 필드 설명

| 매개 변수 | 데이터 유형 | 필수 여부 | 설명 | 값의 범위                           |
| :-------- | ----------- | --------- | ---- | :---------------------------------- |
| balance   | true        | string    | 잔액 |                                     |
| currency  | true        | string    | 코인 |                                     |
| type      | true        | string    | 유형 | trade: 거래 잔액，frozen: 동결 자산 |

## 계정 Serial

API Key 권한: 일기

본 노드는 고객 계정 ID에 따라 계정 Serial을 제공합니다.

### HTTP Request

- GET `/v1/account/history`

### 요청 매개 변수

| 매개 변수      | 필수 여부 | 데터 유형 | 설명                                                         | 기본 값              | 값의 범위                                                    |
| -------------- | --------- | --------- | ------------------------------------------------------------ | -------------------- | ------------------------------------------------------------ |
| account-id     | true      | string    | 계정 번호, `GET /v1/account/accounts`참고                    |                      |                                                              |
| currency       | false     | string    | 코인 종류, 즉btc, ltc, bch, eth, etc ...(`GET /v1/common/currencys`참고) |                      |                                                              |
| transact-types | false     | string    | 변화 유형, 다선형                                            | all                  | trade (거래), transact-fee（거래 수수료）, deduction（수수료 차감）, transfer（전환）, deposit-withdraw（입출금）, withdraw-fee（출금 수수료）, other-types（기타） |
| start-time     | false     | long      | start-time unix time in millisecond. transact-time을 key로 검색. 조회 간격이 1시간이고 최대 30일전 조회 | ((end-time) – 1hour) | [((end-time) – 1hour), (end-time)]                           |
| end-time       | false     | long      | end-time unix time in millisecond. transact-time을 key로 검색. 조회 간격이 1시간이고 최대 30일전 조회 | current-time         | [(current-time) – 29days,(current-time)]                     |
| sort           | false     | string    | 검색 방향                                                    | asc                  | asc or desc                                                  |
| size           | false     | int       | 최대 사이즈                                                  | 100                  | [1,500]                                                      |

> Response:

```json
{
    "status": "ok",
    "data": [
        {
            "account-id": 5260185,
            "currency": "btc",
            "transact-amt": "0.002393000000000000",
            "transact-type": "transfer",
            "record-id": 89373333576,
            "avail-balance": "0.002393000000000000",
            "acct-balance": "0.002393000000000000",
            "transact-time": 1571393524526
        },
        {
            "account-id": 5260185,
            "currency": "btc",
            "transact-amt": "-0.002393000000000000",
            "transact-type": "transfer",
            "record-id": 89373382631,
            "avail-balance": "0E-18",
            "acct-balance": "0E-18",
            "transact-time": 1571393578496
        }
    ]
}
```

### 응답 데이터

| 매개 변수     | 데이터 유형 | 설명                                     | 값의 범위 |
| ------------- | ----------- | ---------------------------------------- | --------- |
| status        | string      | 상태 코드                                |           |
| data          | object      |                                          |           |
| { account-id  | long        | 계정 번호                                |           |
| currency      | string      | 코인 종류                                |           |
| transact-amt  | string      | 변화 금액(입금은 플러스 출금은 마이너스) |           |
| transact-type | string      | 변화 유형                                |           |
| avail-balance | string      | 사용 가능 잔액                           |           |
| acct-balance  | string      | 계정 잔액                                |           |
| transact-time | long        | 거래 시간(DB 기록 시간)                  |           |
| record-id }   | string      | DB기록 번호(전체에서 유일)               |           |



## 자산 이동(마스터계정-서브계정)

API Key 권한: 거래

마스트 계정과 서브 계정 사이 이동

### HTTP 요청

- POST ` /v1/subuser/transfer`

### 요청 매개 변수

| 매개 변수 | 필수 여부 | 데이터 유형 | 설명                                                         | 값의 범위                                                    |      |
| --------- | --------- | ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| sub-uid   | true      | long        | 서브 계정uid                                                 | -                                                            |      |
| currency  | true      | string      | 코인 종류, 즉: btc, ltc, bch, eth, etc ...(`GET /v1/common/currencys`참고) | -                                                            |      |
| amount    | true      | decimal     | 전환 금액                                                    | -                                                            |      |
| type      | true      | string      | 이동 유형                                                    | master-transfer-in(서브계정에서 마스터계정으로 코인 이동) / master-transfer-out (마스터계정에서 서브계정으로 코인 이동) / master-point-transfer-in (서브계정에서 마스터계정으로 수수료 쿠폰 이동) / master-point-transfer-out(마스터계정에서 서브계정으로 수수료 쿠폰 이동) |      |

> Response:

```json
{
  "data":123456,
  "status":"ok"
}
```

### 응답 데이터

| 매개 변수 | 필수 여부 | 데이터 유형 | 길이 | 설명          | 값의 범위       |      |
| --------- | --------- | ----------- | ---- | ------------- | --------------- | ---- |
| data      | true      | int         | -    | 이동 주문서id | -               |      |
| status    | true      |             | -    | 상태          | "OK" or "Error" |      |

### 오류 코드

| error_code                                  | 설명                                        | 유형   |      |
| ------------------------------------------- | ------------------------------------------- | ------ | ---- |
| account-transfer-balance-insufficient-error | 계정 잔액 부족                              | string |      |
| base-operation-forbidden                    | 작업 금지(마스터-서브계정 관계 오류인 경우) | string |      |

## 서브 계정 잔액(합계)

API Key 권한: 읽기

마스터계정에서 본 계정에 속한 모든 서브계정의 코인 잔액의 합계 조회

### HTTP 요청

- GET `/v1/subuser/aggregate-balance`

### 요청 매개 변수

없음

> Response:

```json
{
  "status": "ok",
  "data": [
      {
        "currency": "eos",
        "type": "spot",
        "balance": "1954559.809500000000000000"
      },
      {
        "currency": "btc",
        "type": "spot",
        "balance": "0.000000000000000000"
      },
      {
        "currency": "usdt",
        "type": "spot",
        "balance": "2925209.411300000000000000"
      },
      ...
   ]
}
```

### 응답 데이터

| 매개 변수 | 필수 여부 | 데이터 유형 | 길이 | 설명 | 값의 범위       |      |
| --------- | --------- | ----------- | ---- | ---- | --------------- | ---- |
| status    | true      |             | -    | 상태 | "OK" or "Error" |      |
| data      | true      | list        | -    |      | -               |      |

- data 

| 매개 변수 | 필수 여부 | 데이터 유형 | 길이 | 설명                                                         | 값의 범위                                |      |
| --------- | --------- | ----------- | ---- | ------------------------------------------------------------ | ---------------------------------------- | ---- |
| currency  | YES       | string      | -    | 서브 계정 코인 명칭                                          | -                                        |      |
| type      | YES       | string      | -    | 계정 유형                                                    | spot: 현물 계정, point: 수수료 쿠폰 계정 |      |
| balance   | YES       | string      | -    | 서브 계정에서 해당 코인의 모든 잔액(사용 가능 잔액과 동결 잔액의 합계) | -                                        |      |


## 서브 계정 잔액

API Key 권한: 읽기

마스터계정에서 개별 코인의 서브계정 잔액 조회

### HTTP 요청

- GET `/v1/account/accounts/{sub-uid}`

### 요청 매개 변수

| 매개 변수 | 필수 여부 | 데이터 유형 | 길이 | 설명            | 값의 범위 |      |
| --------- | --------- | ----------- | ---- | --------------- | --------- | ---- |
| sub-uid   | true      | long        | -    | 서브 계정의 UID | -         |      |

> Response:

```json
{
  "status": "ok",
  "data": [
    {
      "id": 9910049,
      "type": "spot",
      "list": 
      [
        {
          "currency": "btc",
          "type": "trade",
          "balance": "1.00"
        },
        {
          "currency": "eth",
          "type": "trade",
          "balance": "1934.00"
        }
      ]
    },
    {
      "id": 9910050,
      "type": "point",
      "list": []
    }
  ]
}
```

### 응답 데이터


| 매개 변수 | 필수 여부 | 데이터 유형 | 길이 | 설명         | 값의 범위                               |      |
| --------- | --------- | ----------- | ---- | ------------ | --------------------------------------- | ---- |
| id        | -         | long        | -    | 서브계정 UID | -                                       |      |
| type      | -         | string      | -    | 계정 유형    | spot: 현물계정, point: 수수료 쿠폰 계정 |      |
| list      | -         | object      | -    | -            | -                                       |      |

- list
  
| 매개 변수 | 필수 여부 | 데이터 유형 | 길이 | 설명      | 값의 범위                           |      |
| --------- | --------- | ----------- | ---- | --------- | ----------------------------------- | ---- |
| currency  | -         | string      | -    | 코인 명칭 | -                                   |      |
| type      | -         | string      | -    | 계정 유형 | trade: 거래 계정, frozen: 동결 계정 |      |
| balance   | -         | decimal     | -    | 계정 잔액 | -                                   |      |

# 지갑（입금과 출금）

<aside class="notice">지갑 관련 API에 액세스 하려면 서명 인증이 필요합니다.</aside>
## APIv2 입금 주소 조회

본 노드는 특정 코인이 블록 체인에서 코인 입금 주소를 조회 하는데 사용 합니다.

API Key 권한: 읽기

<aside class="notice"> 코인 입금 주소 조회는 잠정 IOTA 코인을 지원하지 않습니다. </aside>
### HTTP 요청

- GET ` /v2/account/deposit/address`

```shell
curl "https://api-cloud.huobi.co.kr/v2/account/deposit/address?currency=btc"
```

### 요청 데이터

| 매개 변수 | 필수 여부 | 데이터 유형 | 설명      | 값의 범위                                                   |
| --------- | --------- | ----------- | --------- | ----------------------------------------------------------- |
| currency  | true      | string      | 코인 종류 | btc, ltc, bch, eth, etc ...(`GET /v1/common/currencys`참고) |

> Response:

```json
{
    "code": 200,
    "data": [
        {
            "currency": "btc",
            "address": "1PSRjPg53cX7hMRYAXGJnL8mqHtzmQgPUs",
            "addressTag": "",
            "chain": "btc"
        }
    ]
}
```

### 응답 데이터


| 매개 변수  | 필수 여부 | 데이터 유형 | 설명                        | 값의 범위 |
| ---------- | --------- | ----------- | --------------------------- | --------- |
| code       | true      | int         | 상태 코드                   |           |
| message    | false     | string      | 오류 설명(값이 있는 경우)） |           |
| data       | true      | object      |                             |           |
| {currency  | true      | string      | 코인                        |           |
| address    | true      | string      | 입금 주소                   |           |
| addressTag | true      | string      | 입금 주소 태그              |           |
| chain }    | true      | string      | 체인                        |           |

### 상태 코드

| 상태 코드 | 오류 정보                            | 오류 설명      |
| --------- | ------------------------------------ | -------------- |
| 200       | success                              | 액세스 성공    |
| 500       | error                                | 시스템 오류    |
| 1002      | unauthorized                         | 권한 없음      |
| 1003      | invalid signature                    | 사인 인증 실패 |
| 2002      | invalid field value in "field name"  | 불법 필드 값   |
| 2003      | missing mandatory field "field name" | 필드 누락      |

## APIv2 코인 출금 한도 조회

본 노드는 코인 출금 한도 조회하는데 사용합니다.

API Key 권한: 읽기

### HTTP 요청

- GET ` /v2/account/withdraw/quota`

```shell
curl "https://api-cloud.huobi.co.kr/v2/account/withdraw/quota?currency=btc"
```

### 요청 매개 변수

| 매개 변수 | 필수 여부 | 필드 유형 | 설명          | 값의 범위                                                   |
| --------- | --------- | --------- | ------------- | ----------------------------------------------------------- |
| currency  | true      | string    | 암호화폐 종류 | btc, ltc, bch, eth, etc ...(`GET /v1/common/currencys`참고) |

> Response:

```json
{
    "code": 200,
    "data": 
        {
            "currency": "btc",
            "chains": [
                {
                    "chain": "btc",
                    "maxWithdrawAmt": "200.00000000",
                    "withdrawQuotaPerDay": "200.00000000",
                    "remainWithdrawQuotaPerDay": "200.000000000000000000",
                    "withdrawQuotaPerYear": "700000.00000000",
                    "remainWithdrawQuotaPerYear": "700000.000000000000000000",
                    "withdrawQuotaTotal": "7000000.00000000",
                    "remainWithdrawQuotaTotal": "7000000.000000000000000000"
                }
        }
    ]
}
```

### 응답 데이터


| 매개 변수                  | 필수 여부 | 데이터 유형 | 설며                      | 값의 범위 |
| -------------------------- | --------- | ----------- | ------------------------- | --------- |
| code                       | true      | int         | 상태 코드                 |           |
| message                    | false     | string      | 오류 설명(값이 있는 경우) |           |
| data                       | true      | object      |                           |           |
| currency                   | true      | string      | 코인                      |           |
| chains                     | true      | object      |                           |           |
| { chain                    | true      | string      | 체인                      |           |
| maxWithdrawAmt             | true      | string      | 매회 최대 출금 한도       |           |
| withdrawQuotaPerDay        | true      | string      | 당일 출금 한도            |           |
| remainWithdrawQuotaPerDay  | true      | string      | 당일 남은 출금 금액       |           |
| withdrawQuotaPerYear       | true      | string      | 올해 출금 한도            |           |
| remainWithdrawQuotaPerYear | true      | string      | 올해 남은 출금 금액       |           |
| withdrawQuotaTotal         | true      | string      | 총 출금 한도              |           |
| remainWithdrawQuotaTotal } | true      | string      | 총 남은 출금 한도         |           |

### 상태 코드

| 상태 코드 | 오류 정보                           | 오류 설명             |
| --------- | ----------------------------------- | --------------------- |
| 200       | success                             | 액세스 성공           |
| 500       | error                               | 시스템 오류           |
| 1002      | unauthorized                        | 권한 없음             |
| 1003      | invalid signature                   | 사인 인증 실패        |
| 2002      | invalid field value in "field name" | 유효하지 않은 필드 값 |


## 암호화폐 출금

API Key 권한：출금

<aside class="notice">후오비 코리아 <a href='https://www.huobi.co.kr/zh-CN/address//'>웹사이트</a>에서 지원하는 코인 주소만 지원합니다.</aside>
### HTTP 요청

- POST ` /v1/dw/withdraw/api/create`

```json
{
  "address": "0xde709f2102306220921060314715629080e2fb77",
  "amount": "0.05",
  "currency": "eth",
  "fee": "0.01"
}
```

### 요청 매개 변수

| 매개 변수 | 유형   | 필수 여부 | 설명                                                         | 값의 범위                                                    |
| --------- | ------ | --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| address   | string | true      | 출금 주소                                                    | 웹사이트에서 지원하는 [코인 주소](https://www.huobi.co.kr/zh-CN/address/) 중의 주소만 지원합니다. |
| amount    | string | true      | 출금 수량                                                    |                                                              |
| currency  | string | true      | 자산 유형                                                    | btc, ltc, bch, eth, etc 등 (`GET /v1/common/currencys`참고)  |
| fee       | string | false     | 수수료                                                       |                                                              |
| chain     | false  | string    | `GET /v2/reference/currencies`참고,예)USDT를 OMNI에 출금하면 파라미터를"usdt"로 설정하고 USDT를 TRX에 출금하면 파라미터를"trc20usdt"로 설정하며 기타 코인 출금은 본 파라미터가 없어도 됩니다. |                                                              |
| addr-tag  | string | false     | XRP, XEM, BTS, STEEM, EOS, XMR 암호화폐 주소 Tag             | 형식, class "123"의 정수형 문자열                            |

> Response:

```json
{
  "data": 700
}
```

### 응답 데이터

| 매개 변수 | 데이터 유형 | 필수 여부 | 설명    |
| --------- | ----------- | --------- | ------- |
| data      | long        | false     | 출금 ID |


## 출금 취소

API Key 권한：출금

### HTTP 요청

- POST ` /v1/dw/withdraw-virtual/{withdraw-id}/cancel`

### 요청 매개 변수

| 매개 변수   | 유형 | 필수 여부 | 설명    | 값의 범위   |
| ----------- | ---- | --------- | ------- | ----------- |
| withdraw-id | long | true      | 출금 ID | path에 작성 |


> Response:

```json
{
  "data": 700
}
```

### 응답 데이터

| 매개 변수 | 데이터 유형 | 필수 여부 | 설명    |
| --------- | ----------- | --------- | ------- |
| data      | long        | false     | 출금 ID |

## 입출금 내역

API Key 권한: 읽기

입출금 기록 조회

### HTTP 요청

- GET `/v1/query/deposit-withdraw`

### 요청 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 설명                    | 기본값                                                       | 값의 범위                                                   |
| --------- | ----------- | --------- | ----------------------- | ------------------------------------------------------------ | ----------------------------------------------------------- |
| currency  | string      | false     | 코인종류                |                                                              | btc, ltc, bch, eth, etc ...(`GET /v1/common/currencys`참고) |
| type      | string      | true      | 입금 또는 출금          |                                                              | deposit 혹은 withdraw                                       |
| from      | string      | false     | 쿼리 시작 ID            | 매개 변수가 없는 경우 기본값은 direct 관련입니다. direct가 'prev'인 경우 from은 1이고 과거순으로 오름차순 정렬됩니다. direct가 'next'인 경우 from은 최신 기록 ID이며 최신순으로 내림차순 정렬됩니다. |                                                             |
| size      | string      | false     | 쿼리 레코드 크기        | 100                                                          | 1-500                                                       |
| direct    | string      | false     | 레코드 정렬 direct 반환 | 매개 변수가 없을 시 기본값은 “prev”(올림차순)                | “prev”(올림차순) or “next” (내림차순)                       |

> Response:

```json
{
  "data":
    [
      {
        "id": 1171,
        "type": "deposit",
        "currency": "xrp",
        "tx-hash": "ed03094b84eafbe4bc16e7ef766ee959885ee5bcb265872baaa9c64e1cf86c2b",
        "amount": 7.457467,
        "address": "rae93V8d2mdoUQHwBDBdM4NHCMehRJAsbm",
        "address-tag": "100040",
        "fee": 0,
        "state": "safe",
        "created-at": 1510912472199,
        "updated-at": 1511145876575
      },
      ...
    ]
}
```

### 응답 데이터

| 매개 변수   | 데이터 유형 | 필수 여부 | 설명               | 값의 범위             |
| ----------- | ----------- | --------- | ------------------ | --------------------- |
| id          | long        | true      |                    |                       |
| type        | string      | true      | 유형               | 'deposit', 'withdraw' |
| currency    | string      | true      | 코인               |                       |
| tx-hash     | string      | true      | 거래hash           |                       |
| amount      | long        | true      | 개수               |                       |
| address     | string      | true      | 주소               |                       |
| address-tag | string      | true      | 주소 tag           |                       |
| fee         | long        | true      | 수수료             |                       |
| state       | string      | true      | 상태               | 상태는 아래 도표 참조 |
| created-at  | long        | true      | 생성시간           |                       |
| updated-at  | long        | true      | 최종 업데이트 시간 |                       |


- 암호화폐 입금 상태 정의：

| 상태       | 설명            |
| ---------- | --------------- |
| unknown    | 알 수 없는 상태 |
| confirming | 컨펌 중         |
| confirmed  | 컨펌 완료       |
| safe       | 입금 완료       |
| orphan     | 입금 대기       |

- 암호화폐 출금 상태 정의：

| 상태            | 설명           |
| --------------- | -------------- |
| submitted       | 제출 완료      |
| reexamine       | 심사 중        |
| canceled        | 출금 취소 완료 |
| pass            | 심사 승인      |
| reject          | 심사 거절      |
| pre-transfer    | 처리 중        |
| wallet-transfer | 입금 완료      |
| wallet-reject   | 지갑 전송 거절 |
| confirmed       | 블록 컨펌 완료 |
| confirm-error   | 블록 컨펌 오류 |
| repealed        | 출금 취소 완료 |

# 거래

<aside class="notice">거래 관련 API에 액세스하는 경우 서명 인증이 필요합니다.</aside>
## 주문

API Key 권한：거래

후오비 코리아에 새로운 주문을 생성하여 매칭을 진행합니다.

### HTTP 요청

- POST ` /v1/order/orders/place`

```json
{
  "account-id": "100009",
  "amount": "10.1",
  "price": "100.1",
  "source": "api",
  "symbol": "ethusdt",
  "type": "buy-limit",
  "client-order-id": "a0001"
}
```

### 요청 매개 변수

매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명 
---------  | --------- | -------- | ------- | -----------
account-id | string    | true     | NA      | 계정 ID, `GET /v1/account/accounts` API를 참고. 현물 거래는 ‘spot’ 계정의 account-id 사용 
symbol     | string    | true     | NA      | 거래 페어, 예)btcusdt, ethbtc...（`GET /v1/common/symbols`참고） 
type       | string    | true     | NA      | 주문 유형 - buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sellioc, buy-limit-maker, sell-limit-maker 포함(아래 설명 참조), buy-stop-limit, sell-stop-limit 
amount     | string    | true     | NA      | 주문 거래량(시장가 매수 시 본 필드는 주문 거래액 입니다.) 
price      | string    | false    | NA      | limit order의 체결 가격 
source     | string    | false    | api     | 현물거래는 “api” 입력 
client-order-id| string    | false    | NA     | 고객이 자체 설정한 주문 넘버(최대 길이는 64개 문자이고 24시간 동안 유효합니다.) 


**buy-limit-maker**

“주문 가격” >= “마켓 최저 매도가”인 경우 주문 제출 후 시스템에서 거절됩니다.

“주문 가격” < “마켓 최저 매도가”인 경우 주문 제출 후, 시스템에서 정상 접수됩니다.

**sell-limit-maker**

“주문 가격” <= “마켓 최고 매수가”인 경우 주문 제출 후 시스템에서 거절됩니다.

“주문 가격” > “마켓 최고 매수가”인 경우 주문 제출 후 시스템에서 정상 접수됩니다.

> Response:

```json
{  
  "data": "59378"
}
```

### 응답 데이터

제공된 기본 데이터 object는 주문 번호에 해당하는 문자열입니다.

예)client order ID(24시간내) 중복 사용시 노드는 기존 주문서의 client order ID를 제공합니다.

## 주문 철회

API Key 권한：거래

본 API는 주문취소 요청을 보냅니다.

<aside class="warning">본 API는 취소 요청만 제출하며 실제 취소결과는 API 상태, 일치 상태 및 기타 API로 확인해야 합니다.</aside>
### HTTP 요청

- POST ` /v1/order/orders/{order-id}/submitcancel`


### 요청 매개 변수

| 매개 변수 | 필수 여부 | 데이터 유형 | 설명                     |
| --------- | --------- | ----------- | ------------------------ |
| order-id  | true      | string      | 주문내역 ID，path에 작성 |


> Response:

```json
{  
  "data": "59378"
}
```

### 응답 데이터

제공된 기본 데이터 object는 주문 번호에 해당하는 문자열입니다.

### 오류 코드

> Response:

```json
{
  "status": "error",
  "err-code": "order-orderstate-error",
  "err-msg": "주문서 상태 오류",
  "order-state":-1 // 현재 주문서 상태
}
```

제공된 필드 리스트에서 order-state의 가능한 값:

order-state           |  Description
---------       | -----------
-1| order was already closed in the long past (order state = canceled, partial-canceled, filled, partial-filled)
5| partial-canceled
6| filled
7| canceled
10| cancelling

## 주문서 철회(client order ID에 한하여)

API Key 권한: 거래

본 API는 주문 철회 요청을 보냅니다.

<aside class="warning">본 API는 취소요청만 제출하며 실제 취소결과는 API 상태, 일치 상태 및 기타 API로 확인해야 합니다.</aside>
### HTTP 요청

- POST ` /v1/order/orders/submitCancelClientOrder`

```json
{
  "client-order-id": "a0001"
}
```


### 요청 매개 변수

| 매개 변수 | 필수 여부 | 데이터 유형 | 설명         |
| -------- | ---- | ------ | ------------ |
| client-order-id | true | string | 고객 자체 설정한 주문 넘버 |


> Response:

```json
{  
  "data": "10"
}
```
### 응답 데이터

매개 변수          | 데이터 유형 | 설명 
---------           | --------- | -----------
data                  | integer   | 철회 상태 코드 

Status Code           |  Description
---------       | -----------
-1| order was already closed in the long past (order state = canceled, partial-canceled, filled, partial-filled)
0| client-order-id not found
5| partial-canceled
6| filled
7| canceled
10| cancelling


## 미체결 주문 조회

API Key 권한: 읽기

제출 후, 체결되지 않은 주문 건에 대한 조회

```json
{
   "account-id": "100009",
   "symbol": "ethusdt",
   "side": "buy"
}
```

### HTTP 요청

- GET `/v1/order/openOrders`


### 요청 매개 변수

매개 변수 | 데이터 유형 | 필수 여부 | 기본 값 | 설명 
---------  | --------- | -------- | ------- | -----------
account-id | string    | true    | NA      | 계정 ID, `GET /v1/account/accounts`참고. 현물 거래는 ‘spot’ 계정의 account-id 사용 
symbol     | string    | ture    | NA      | 거래 페어, 예) btcusdt, ethbtc...（`GET /v1/common/symbols`참고） 
side       | string    | false    | both    | 단 방향만 제공하는 주문을 지정합니다. 가능한 값은 buy, sell이며 기본값은 양방향으로 제공됩니다. 
from       | string | false | |조회 시작 ID
direct     | string | false('from'값을 설정 하였으면 본 필드'direct'는 필수(ture)입니다.) | |조회방향(prev - 시작ID를 올림차순 검색; next - 시작ID를 내림차순 검색)
size       | int       | false    | 100      | 주문 건수를 제공합니다. 최대값: 500 

> Response:

```json
{  
  "data": [
    {
      "id": 5454937,
      "symbol": "ethusdt",
      "account-id": 30925,
      "amount": "1.000000000000000000",
      "price": "0.453000000000000000",
      "created-at": 1530604762277,
      "type": "sell-limit",
      "filled-amount": "0.0",
      "filled-cash-amount": "0.0",
      "filled-fees": "0.0",
      "source": "web",
      "state": "submitted"
    }
  ]
}
```

### 응답 데이터

| 매개 변수          | 데이터 유형 | 설명                                                         |
| ------------------ | ----------- | ------------------------------------------------------------ |
| id                 | integer     | 주문 ID                                                      |
| symbol             | string      | 거래 페어, 예) btcusdt, ethbtc                               |
| price              | string      | limit order의 거래 가격                                      |
| created-at         | int         | 주문 생성 시 UTC/GMT+08:00의 타임 스탬프로 조정 (단위: ms(milliseconds)) |
| type               | string      | 주문 유형                                                    |
| filled-amount      | string      | 주문 건에서 체결완료된 수량                                  |
| filled-cash-amount | string      | 주문 건에서 체결완료된 금액                                  |
| filled-fees        | string      | 차감된 총 거래 수수료                                        |
| source             | string      | 현물 거래는 “api” 추가                                       |
| state              | string      | 주문 상태, submitted, partical-filled, cancelling,created 포함 |
| stop-price         | string      | Stop limit 발동 가격                                         |
| operator           | string      | Stop limit 발동 가격 operator                                |

## 대량 주문 취소(open orders)

API Key 권한: 거래

본 API는 대량 주문 취소 요청을 보냅니다.

<aside class="warning">본 API는 취소요청만 제출하며 실제 취소결과는 API 상태, 매칭 상태 및 기타 API로 확인해야 합니다.</aside>
### HTTP 요청

- POST ` /v1/order/orders/batchCancelOpenOrders`


### 요청 매개 변수

| 매개 변수 | 필수 여부 | 데이터 유형 | 설명         | 기본값 | 값의 범위 |
| -------- | ---- | ------ | ------------ | ---- | ---- |
| account-id | true  | string | 계정ID, `GET /v1/account/accounts`참고 |     |      |
| symbol     | false | string | 거래 페어 리스트(최대 10개 symbols, 거래쌍 사이 부호","으로 나뉨), btcusdt, ethbtc...(`/v1/common/symbols`참고) |  all    |     |
| side | false | string | 주문 활성화 지시 |  | “buy” 또는 “sell”, 매개변수가 없을 경우 미체결된 주문이 제공됩니다. |
| size | false | int | 필요한 레코드수 |  100 |   [0,100]   |


> Response:

```json
{
  "status": "ok",
  "data": {
    "success-count": 2,
    "failed-count": 0,
    "next-id": 5454600
  }
}
```


### 응답 데이터


| 매 변수 | 필수 여부 | 데이터 유형 | 설명 |
| ---- | ---- | ------ | ----- |
| success-count | true | int | 취소 완료 주문 건수 |
| failed-count | true | int | 취소 실패 주문 건수 |
| next-id | true | long | 취소 가능한 다음 주문번호 |

## 주문서 대량 철회

API Key 권한：거래

해당 API는 동시에 여러 개 주문서(id로 식별) 취소 시 요청합니다.

### HTTP 요청

- POST ` /v1/order/orders/batchcancel`

```json
{
  "order-ids": [
    "1", "2", "3"
  ]
}
```

### 요청 매개 변수

| 매개 변수 | 필수 여부 | 유형 | 설명 | 기본 값 | 값의 범위 |
| ---- | ---- | ---- | ----  | ---- | ---- |
| order-ids | true | list | 철회 오더 ID리스트 |  |한차례에 50개 오더 id를 초과 못합니다|


> Response:

```json
{  
  "data": {
    "success": [
      "1",
      "3"
    ],
    "failed": [
      {
        "err-msg": "记录无效",
        "order-id": "2",
        "err-code": "base-record-invalid"
        "order-state":-1 // 当前订单状态
      }
    ]
  }
}
```

### 응답 데이터

| 필드 이름 | 데이터 유형 | 설명|
| ---- | ----- | ---- |
| data | map | 주문서 철회 결과

### 오류 코드

> Response:

```json
{
  "status": "ok",
  "data": {
    "success": ["123","456"],
    "failed": [
      {
        "err-msg": "订单状态错误",
        "order-id": "12345678",
        "err-code": "order-orderstate-error",
        "order-state":-1 // 当前订单状态
      }
    ]
  }
}
```

제공된 필드 리스트에서 order-state 값의 범위-

order-state           |  Description
---------       | -----------
-1| order was already closed in the long past (order state = canceled, partial-canceled, filled, partial-filled)
5| partial-canceled
6| filled
7| canceled
10| cancelling

## 주문서 내역 조회

API Key 권한：읽기

해당 API는 해당 주문서의 최신상태와 상세내역을 제공합니다.

### HTTP 요청

- GET `/v1/order/orders/{order-id}`

### 요청 매개 변수

| 매개 변수 | 필수 여부 | 유형   | 설명                  | 기본값 | 값의 범위 |
| --------- | --------- | ------ | --------------------- | ------ | --------- |
| order-id  | true      | string | 주문서ID，path에 입력 |        |           |


> Response:

```json
{  
  "data": 
  {
    "id": 59378,
    "symbol": "ethusdt",
    "account-id": 100009,
    "amount": "10.1000000000",
    "price": "100.1000000000",
    "created-at": 1494901162595,
    "type": "buy-limit",
    "field-amount": "10.1000000000",
    "field-cash-amount": "1011.0100000000",
    "field-fees": "0.0202000000",
    "finished-at": 1494901400468,
    "user-id": 1000,
    "source": "api",
    "state": "filled",
    "canceled-at": 0
  }
}
```

### 응답 데이터

| 필드 이름         | 필수 여부 | 데이터 유형 | 설명                                                         | 값의 범위                                                    |
| ----------------- | --------- | ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| account-id        | true      | long        | 계정 ID                                                      |                                                              |
| amount            | true      | string      | 주문서 수량                                                  |                                                              |
| canceled-at       | false     | long        | 주문서 철회 시간                                             |                                                              |
| created-at        | true      | long        | 주문서 생성 시간                                             |                                                              |
| field-amount      | true      | string      | 체결된 수량                                                  |                                                              |
| field-cash-amount | true      | string      | 체결된 총금액                                                |                                                              |
| field-fees        | true      | string      | 체결된 수수료(매수시 거래 화폐,매도시 메켓화폐)              |                                                              |
| finished-at       | false     | long        | 주문서가 최종상태시 시간,체결 시간이 아니며 "주문서 철회"상태를 포함 |                                                              |
| id                | true      | long        | 주문서 ID                                                    |                                                              |
| price             | true      | string      | 주문 가격                                                    |                                                              |
| source            | true      | string      | 주문 경로                                                    | api                                                          |
| state             | true      | string      | 주문서 상태                                                  | submitted 제출 완료, partial-filled 부분체결, partial-canceled 부분철회, filled 체결 완료, canceled 철회 완료, created |
| symbol            | true      | string      | 거래 페어                                                    | btcusdt, ethbtc, rcneth ...                                  |
| type              | true      | string      | 주문서 유형                                                  | buy-market：시장가 매수, sell-market：시장가 매도, buy-limit： 지정가 매수, sell-limit：지정가 매도, buy-ioc：IOC 매수 오더, sell-ioc：IOC 매도 오더 buy-limit-maker, sell-limit-maker, buy-stop-limit，sell-stop-limit |
| stop-price        | false     | string      | Stop-limit 발동 가격                                         |                                                              |
| operator          | false     | string      | Stop-limit 발동 가격 operator                                | gte,lte                                                      |


## 주문서 내역 조회(client order ID)

API Key 권한：읽기

해당 API는 해당 주문서의 최신상태와 상세내역을 제공합니다.

### HTTP 요청

- GET `/v1/order/orders/getClientOrder`

```json
{
  "clientOrderId": "a0001"
}
```

### 요청 매개 변수

| 매개 변수 | 필수 여부 | 유형 | 설명 | 기본 값 | 값의 범위 |
| -------- | ---- | ------ | -----  | ---- | ---- |
| clientOrderId | true | string | 고객이 자체 설정한 주문 넘버 |      |      |

> Response:

```json
{  
  "data": 
  {
    "id": 59378,
    "symbol": "ethusdt",
    "account-id": 100009,
    "amount": "10.1000000000",
    "price": "100.1000000000",
    "created-at": 1494901162595,
    "type": "buy-limit",
    "field-amount": "10.1000000000",
    "field-cash-amount": "1011.0100000000",
    "field-fees": "0.0202000000",
    "finished-at": 1494901400468,
    "user-id": 1000,
    "source": "api",
    "state": "filled",
    "canceled-at": 0
  }
}
```

### 응답 데이터

| 매개 변수 | 필수 여부 | 데이터 유형 | 설명 | 값의 범위 |
| ----------------- | ----- | ------ | -------  | ----  |
| account-id        | true  | long   | 계정 ID  |       |
| amount            | true  | string | 주문서 수량       |    |
| canceled-at       | false | long   | 주문서 철회 시간 |     |
| created-at        | true  | long   | 주문서 생성 시간 |   |
| field-amount      | true  | string | 체결된 수량 |     |
| field-cash-amount | true  | string | 체결된 총액 |      |
| field-fees        | true  | string | 체결된 수수료(매수시 거래 화폐,매도시 메켓화폐) |     |
| finished-at       | false | long   | 주문서가 최종상태시 시간,체결 시간이 아니며 "주문서 철회"상태를 포함 |     |
| id                | true  | long   | 주문서ID |     |
| price             | true  | string | 주문 가격  |     |
| source            | true  | string | 주문 경로 | api |
| state             | true  | string | 주문서 상태 | submitted 제출 완료, partial-filled 부분체결, partial-canceled 부분철회, filled 체결 완료, canceled 철회 완료, created |
| symbol            | true  | string | 거래 페어 | btcusdt, ethbtc, rcneth ... |
| type              | true  | string | 주문서 유형 | buy-market：시장가 매수, sell-market：시장가 매도, buy-limit： 지정가 매수, sell-limit：지정가 매도, buy-ioc：IOC 매수 오더, sell-ioc：IOC 매도 오더 buy-limit-maker, sell-limit-maker, buy-stop-limit，sell-stop-limit |
| stop-price              | false  | string | Stop-limit 발동 가격 | |
| operator              | false  | string | Stop-limit 발동 가격 operator | gte,lte |

client order ID가 없는 경우 아래 오류 데이터를 표시합니다.
{
    "status": "error",
    "err-code": "base-record-invalid",
    "err-msg": "record invalid",
    "data": null
}

## 체결 완료 내역

API Key 권한: 읽기

본 API는 체결 완료 주문서 내역을 제공합니다.

### HTTP 요청

- GET `/v1/order/orders/{order-id}/matchresults`



### 요청 매개 변수

| 매개 변수 | 필수 여부 | 유형   | 설명                  | 기본값 | 값의 범위 |
| --------- | --------- | ------ | --------------------- | ------ | --------- |
| order-id  | true      | string | 주문서ID，path에 입력 |        |           |


> Response:

```json
{  
  "data": [
    {
      "id": 29553,
      "order-id": 59378,
      "match-id": 59335,
      "trade-id": 100282808529,
      "symbol": "ethusdt",
      "type": "buy-limit",
      "source": "api",
      "price": "100.1000000000",
      "filled-amount": "9.1155000000",
      "filled-fees": "0.0182310000",
      "created-at": 1494901400435,
      "role": "maker",
      "filled-points": "0.0",
      "fee-deduct-currency": ""
    }
    ...
  ]
}
```

### 응답 데이터

<aside class="notice">제공 된 주요 데이터 객체는 하나의 객체array입니다.그중에 매개 원소는 거래 결과 입니다.</aside>
| 매개 변수 | 필수 여부 | 데이터 유형 | 설명 | 값의 범위 |
| ------------- | ---- | ------ | -------- | -------- |
| created-at    | true | long   | 체결시간 |    |
| filled-amount | true | string | 체결 수량 |    |
| filled-fees   | true | string | 체결 수수료가 NULL혹은 0이면 기타 코인을 삭감, filled-points와fee-deduct-currency 필드로 판단 |     |
| id            | true | long   | 주문 체결 기록ID |     |
| match-id      | true | long   | 매칭ID, 주문서 매칭 순서ID |     |
| order-id      | true | long   | 주문서ID, 체결 주문서ID |      |
| trade-id      | false | integer   | Unique trade ID (NEW)유일한 체결 번호, 체결시 생성되는 유일한 번호ID |     |
| price         | true | string | 체결 가격 |    |
| source        | true | string | 주문 경로 | api      |
| symbol        | true | string | 거래 페어 | btcusdt, ethbtc, rcneth ...  |
| type          | true | string | 주문서 유형 | buy-market：시장가 매수, sell-market：시장가 매도, buy-limit： 지정가 매수, sell-limit：지정가 매도, buy-ioc：IOC 매수 오더, sell-ioc：IOC 매도 오더 buy-limit-maker, sell-limit-maker, buy-stop-limit，sell-stop-limit |
| role      | true | string   | 체결 역할 |maker,taker      |
| filled-points      | true | string   | 차감 수량(ht 혹은hbpoint) |     |
| fee-deduct-currency      | true | string   | 차감 유형 |NULL이면 차감 수수료는 거래 코인; "ht"이면 차감 수수료는 HT; "hbpoint"이면 차감 수수료는 수수료 쿠폰     |

## 오더 리스트 검색

API Key 권한：읽기

본 API는 검색 조건에 의하여 주문 리스트를 제공합니다

### HTTP 요청

- GET `/v1/order/orders`

```json
{
   "account-id": "100009",
   "amount": "10.1",
   "price": "100.1",
   "source": "api",
   "symbol": "ethusdt",
   "type": "buy-limit"
}
```


### 요청 매개 변수

| 매개 변수 | 필수 여부 | 유형   | 설명 | 기본 값 | 값의 범위 |
| ---------- | ----- | ------ | ------  | ---- | ----  |
| symbol     | true  | string | 거래 페어 |      |btcusdt, ethbtc...（`GET /v1/common/symbols`참고）  |
| types      | false | string | 주문서의 유형조합을 조회시 ','부호로 분리                    |      | buy-market：시장가 매수, sell-market：시장가 매도, buy-limit： 지정가 매수, sell-limit：지정가 매도, buy-ioc：IOC 매수 오더, sell-ioc：IOC 매도 오더 buy-limit-maker, sell-limit-maker, buy-stop-limit，sell-stop-limit |
| start-date | false | string | 시작 일자 조회,일자 양식은 yyyy-mm-dd 입니다. 주문서 생성시간으로 조회 | -1d 조회 마지막 일자 하루전 | 값의 범위 [((end-date) – 1), (end-date)] , 조회 범위는 최소 2일이고 180일전 기록 조회 가능합니다. 철회 완료된 주문서는 7일전 기록 조회 가능합니다. (state="canceled") |
| end-date   | false | string | 완료일자 조회, 일자 양식 yyyy-mm-dd 입니다. 주문서 생성기간으로 조회 | today     | 값의 범위 [(today-179), today] , 조회 범위는 최소 2일이고 180일전 기록 조회 가능합니다. 철회 완료된 주문서는 7일전 기록 조회 가능합니다.(state="canceled") |
| states     | true  | string | 오더 상태조합 조회,부호','분할                               |      | submitted 제출 완료, partial-filled 부분체결, partial-canceled 부분철회, filled 체결 완료, canceled 철회 완료, created |
| from       | false | string | 최초 ID 조회                                                 |      |    |
| direct     | false | string | 조회 방향 |      | prev 이전，시간(혹은 ID) 순차적 순서；next 이후，시간(혹은 ID) 역차적 순서 |
| size       | false | string | 조회 기록 사이즈 | 100     |  [1, 100]       |



> Response:

```json
{  
  "data": [
    {
      "id": 59378,
      "symbol": "ethusdt",
      "account-id": 100009,
      "amount": "10.1000000000",
      "price": "100.1000000000",
      "created-at": 1494901162595,
      "type": "buy-limit",
      "field-amount": "10.1000000000",
      "field-cash-amount": "1011.0100000000",
      "field-fees": "0.0202000000",
      "finished-at": 1494901400468,
      "user-id": 1000,
      "source": "api",
      "state": "filled",
      "canceled-at": 0
    }
    ...
  ]
}
```

### 응답 데이터

| 매개 변수 | 필수 여부 | 데이터 유형 | 설명 | 값의 범위 |
| ----------------- | ----- | ------ | ----------------- | ----  |
| account-id        | true  | long   | 계정 ID  |     |
| amount            | true  | string | 주문 수량 |   |
| canceled-at       | false | long   | 철회 신청 접수 시간                            |    |
| created-at        | true  | long   | 주문서 생성 시간 |    |
| field-amount      | true  | string | 체결 수량 |    |
| field-cash-amount | true  | string | 체결 총금액 |    |
| field-fees        | true  | string | 체결 수수료(매수시 거래 화폐, 각도시 마켓화폐) |       |
| finished-at       | false | long   | 최종 체결 시간                                 |   |
| id                | true  | long   | 주문서ID |    |
| price             | true  | string | 주문 가격 |    |
| source            | true  | string | 주문 경로 | api  |
| state             | true  | string | 주문서 상태 | submitting , submitted 제출 완료, partial-filled 부분 체결, partial-canceled 부분 체결 철회, filled 체결 완료, canceled 철회 완료, created |
| symbol            | true  | string | 거래 페어 | btcusdt, ethbtc, rcneth ... |
| type              | true  | string | 주문서 유형                                    | submit-cancel：철회 신청 제출 완료  ,buy-market：시가 매수, sell-market：시가 매도, buy-limit：지정가 매수, sell-limit：지정가 매도, buy-ioc：IOC 매수 오더, sell-ioc：IOC 매도 오더, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit |
| stop-price              | false  | string | Stop-limit 스탑리밋 가격 | |
| operator              | false  | string | Stop-limit 스탑리밋 가격 operator | gte,lte |

### start-date, end-date관련 오류 코드

|오류 코드|오류 환경|
|------------|----------------------------------------------|
|invalid_interval| start date 는 작기 end date; 혹은 start date 와 end date 시간차이는 작기 2일 이여야 합니다. |
|invalid_start_date| start date는 180일 이전 일자；혹은start date가 미래 일자     |
|invalid_end_date|end date 는 180일 이전 일자；혹은 end date가 미래 일자|

## 최근 48시간내 주문 리스트 검색

API Key 권한：읽기

해당 API는 검색 조건에 의하여 최근 48시간 내 주문 리스트를 제공합니다.

### HTTP 요청

- GET `/v1/order/history`

```json
{
   "symbol": "btcusdt",
   "start-time": "1556417645419",
   "end-time": "1556533539282",
   "direct": "prev",
   "size": "10"
}
```

### 요청 매개 변수

| 매개 변수  | 필수 여부 | 유형   | 설명                                                         | 기본값           | 값의 범위                                        |
| ---------- | --------- | ------ | ------------------------------------------------------------ | ---------------- | ------------------------------------------------ |
| symbol     | false     | string | 거래페어                                                     | all              | btcusdt, ethbtc...(`GET /v1/common/symbols`참고) |
| start-time | false     | long   | 처음 시간 조회(포함)                                         | 48시간 이전 시각 | UTC time in millisecond                          |
| end-time   | false     | long   | 종료시간 조회(포함)                                          | 조회 시각        | UTC time in millisecond                          |
| direct     | false     | string | 주문서 조회 방향（주：조회 결과 총세목 수량이 size필드 제한을 초과 시 작용합니다.；만약 조회 결과 총 세목 수량이 size필드 제한 내일 경우 direct필드는 작용하지 않습니다.） | next             | prev, next                                       |
| size       | false     | int    | 매번 반환한 총세목                                           | 100              | [10,1000]                                        |



> Response:

```json
{
    "status": "ok",
    "data": [
        {
            "id": 31215214553,
            "symbol": "btcusdt",
            "account-id": 4717043,
            "amount": "1.000000000000000000",
            "price": "1.000000000000000000",
            "created-at": 1556533539282,
            "type": "buy-limit",
            "field-amount": "0.0",
            "field-cash-amount": "0.0",
            "field-fees": "0.0",
            "finished-at": 1556533568953,
            "source": "web",
            "state": "canceled",
            "canceled-at": 1556533568911
        }
    ]
}
```

### 응답 데이터

| 매개 변수         | 필수 여부 | 데이터 유형 | 설명                                                         | 값의 범위                                                    |
| ----------------- | --------- | ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| account-id        | true      | long        | 계정ID                                                       |                                                              |
| amount            | true      | string      | 주문서 수량                                                  |                                                              |
| canceled-at       | false     | long        | 주문 철회 신청을 접수한 시간                                 |                                                              |
| created-at        | true      | long        | 주문서 생성 시간                                             |                                                              |
| field-amount      | true      | string      | 체결 수량                                                    |                                                              |
| field-cash-amount | true      | string      | 체결 총금액                                                  |                                                              |
| field-fees        | true      | string      | 체결 수수료(매수시 거래 화폐,매도시 마켓 화폐)               |                                                              |
| finished-at       | false     | long        | 최종 체결 시간                                               |                                                              |
| id                | true      | long        | 주문서 ID                                                    |                                                              |
| price             | true      | string      | 주문 가격                                                    |                                                              |
| source            | true      | string      | 주문 경로                                                    | api                                                          |
| state             | true      | string      | 주문서 상태                                                  | partial-canceled 부분 체결 철회, filled 체결 완료, canceled 철회 완료 |
| symbol            | true      | string      | 거래페어                                                     | btcusdt, ethbtc, rcneth ...                                  |
| stop-price        | false     | string      | Stop-limit 발동 가격                                         |                                                              |
| operator          | false     | string      | Stop-limit 발동 가격 operator                                | gte,lte                                                      |
| type              | true      | string      | 주문서 유형                                                  | buy-market：시가 매수, sell-market：시가 매도, buy-limit：지정가 매수, sell-limit：지정가 매도, buy-ioc：IOC 매수 오더, sell-ioc：IOC매도 오더, buy-limit-maker, sell-limit-maker |
| next-time         | false     | long        | 다음 조회 시작시간(요청한 필드"direct"가 "prev" 일때 유효합니다), 다음 조회 종료시간(요청한 필드"direct"가 "next" 일때 유효합니다),주：조회 결과 총 세목 수량이 size필드 제한 내일 경우 반환한 필드가 있습니다. | UTC time in millisecond                                      |


## 현재 및 과거 주문 체결 리스트

API Key 권한：읽기

본 API는 검색 조건에 의하여 현재 및 과거 주문 체결 리스트를 제공합니다.

### HTTP 요청

- GET `/v1/order/matchresults`


### 요청 매개 변수

| 매개 변수 | 필수 여부 | 유형 | 설명 | 기본 값 | 값의 범위 |
| ---------- | ----- | ------ | ------ | ---- | ----------- |
| symbol     | true  | string | 거래 페어 | NA | btcusdt, ethbtc...（`GET /v1/common/symbols`참고） |
| types      | false | string | 주문서    유형 조합 조회,부호 ','로 분리 |  all    | buy-market：시가 매수, sell-market：시가 매도, buy-limit：지정가 매수, sell-limit：지정가 매도, buy-ioc：IOC 매수 오더, sell-ioc：IOC 매도 오더, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit |
| start-date | false | string | 시작 일자 조회,일자 양식:yyyy-mm-dd      | -61 days                                   | 값의 범위[((end-date) – 1), (end-date)], start-date와 end-date 의 조회 범위는 최대 2일이고 최근 61일 데이터를 조회 가능합니다. |
| end-date   | false | string | 마지막 일자 조회,일자 양식:yyyy-mm-dd    |   today   | 값의 범위 [(today-60), today],start-date와 end-date 의 조회 범위는 최대 2일이고 최근 61일 데이터를 조회 가능합니다. |
| from       | false | string | 조회 처음 ID                             | 주문서 체결 ID(최대치)                     |     |
| direct     | false | string | 조회 방향                                | 기본적으로next， 체결 리스트 ID는 내림차순 | prev 이전，시간(혹은 ID) 순차적 순서；next 이후，시간(혹은 ID) 역차적 순서 |
| size       | false | string | 조회 리스트 크기                         |   100   | [1，100]  |

> Response:

```json
{  
  "data": [
    {
      "id": 29553,
      "order-id": 59378,
      "match-id": 59335,
      "symbol": "ethusdt",
      "type": "buy-limit",
      "source": "api",
      "price": "100.1000000000",
      "filled-amount": "9.1155000000",
      "filled-fees": "0.0182310000",
      "created-at": 1494901400435,
      "trade-id": 100282808529,
      "role": taker,
      "filled-points": "0.0",
      "fee-deduct-currency": ""
    }
    ...
  ]
}
```

### 응답 데이터

<aside class="notice">제공 된 주요 데이터 대상은 하나의 대상 array입니다. 그 중에 매개 원소는 거래 결과입니다.</aside>
| 매개 변수 | 필수 여부 | 데이터 유형 | 설명 | 값의 범위 |
| ------------- | ---- | ------ | -------- | ------- |
| created-at    | true | long   | 체결시간                              |    |
| filled-amount | true | string | 체결수량 |    |
| filled-fees   | true | string | 체결수수료 |    |
| id            | true | long   | 주문 체결 리스트 ID |    |
| match-id      | true | long   | 매칭 ID   |    |
| order-id      | true | long   | 주문서 ID |    |
| trade-id      | false | integer   | Unique trade ID (NEW)유일한 체결 번호 |      |
| price         | true | string | 체결 가격 |    |
| source        | true | string | 주문 경로 | api   |
| symbol        | true | string | 거래 페어 | btcusdt, ethbtc, rcneth ...  |
| type          | true | string | 주문서 유형 | buy-market：시가 매수, sell-market：시가 매도, buy-limit：지정가 매수, sell-limit：지정가 매도, buy-ioc：IOC 매수 오더, sell-ioc：IOC 매도 오더, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit |
| role      | true | string   | 체결 역할 |maker,taker      |
| filled-points      | true | string   | 차감 수량(hbpoint) |     |
| fee-deduct-currency      | true | string   | 차감 유형 |hbpoint     |

### start-date, end-date관련 오류 코드 

|오류 코드|오류 환경|
|------------|----------------------------------------------|
|invalid_interval| start date 는 작기 end date; 혹은 start date 와 end date 시간차가 크기 2일 입니다. |
|invalid_start_date| start date는 61일 이전 일자；혹은 start date는 미래 일자     |
|invalid_end_date|end date는 61일 이전 일자；혹은 end date는 미래 일자|


## 고객의 현재 수수료

Api고객 거래 페어 수수료 조회, 일괄적으로 최대 10개 거래 페어 조회, 서브 유저의 수수료는 마스터 유저와 일치성 유지.

API Key 권한: 읽기

```shell
curl "https://api-cloud.huobi.co.kr/v1/fee/fee-rate/get?symbols=btcusdt,ethusdt,ltcusdt"
```

### HTTP 요청

- GET `/v1/fee/fee-rate/get`

### 요청 매개 변수

매개 변수       | 데터 유형 | 필수 여부 | 기본 값 | 설명 | 값의 범위 
--------- | --------- | -------- | ------- | ------ | ------
symbols    | string    | true     | NA      | 거래 페어, 여러가지 입력 가능하고 부호","로 분리 |btcusdt, ethbtc...（`GET /v1/common/symbols`참고）>

> Response:

```json
{
  "status": "ok",
  "data": [
     {
        "symbol": "btcusdt",
        "maker-fee":"0.0001",
        "taker-fee":"0.0002"
     },
     {
        "symbol": "ethusdt",
        "maker-fee":"0.002",
        "taker-fee":"0.002"
    },
     {
        "symbol": "ltcusdt",
        "maker-fee":"0.0015",
        "taker-fee":"0.0018"
    }
  ]
}
```
### 응답 데이터

매개 변수      | 데이터 유형 | 필수 여부 | 설명 
--------- | --------- | -----------| -----------
status        | string  |Y | 응답 상태 
err-code    | string   |N  | 응답 코드 
err-msg     | string |N   | 응답 정보 
data|list|Y|거래 페어 수수료 리스트

### List
매개 변수| 데이터 유형 | 설명 
--------- | --------- | ------
symbol| string| 거래 페어 
maker-fee|  string| maker-fee 
taker-fee|  string| taker-fee 

### 응답 코드
응답 코드| 설명 | 데이터 유형 | 메모 
--------- | --------- | ------ | ------
base-symbol-error| 무효 거래 페어 | string| -
base-too-many-symbol| 최대 10개 거래 페어 지원 | string| -

# Websocket 시세 데이터

## 소개

### Access URL

**후오비 코리아 시세 요청 API 주소**

**`wss://api-cloud.huobi.co.kr/ws`**

- 중국 이외의 IP를 통해 후오비 코리아 API로 접속해주시기 바랍니다.

### 데이터 압축

- WebSocket API에서 제공된 모든 데이터는 GZIP로 압축되어 있으며 데이터 수신 후 클라이언트의 압축을 해제해야 합니다.

### **Heartbeat 메세지**

- 회원님의 WebSocket 클라이언트가 후오비 코리아 WebSocket 서버에 접속하면 서버는 정기적으로(현재 5초로 설정) `ping` 메세지를 발송하며 메시지에 아래와 같은 데이터를 포함합니다.

  `{"ping": 1492420473027}`

- 회원님의 WebSocket 클라이언트가 Heartbeat 메세지를 수신하면 동일한 데이터를 지닌 pong 메세지를 반환해야 합니다.

  `{"pong": 1492420473027}`

WebSocket 서버가 `pong` 메세지를 수신하지 못하고 `ping` 메세지를 2회 연속 보낼 경우 서버는 클라이언트와의 연결을 종료합니다.

### 구독 주제

WebSocket 서버에 성공적으로 연결되면 WebSocket 클라이언트는 특정 주제를 구독하기 위해 다음 요청을 보냅니다.

```json
{
  "sub": "market.btccny.kline.1min",
  "id": "id1"
}
```

```json
{
  "sub": "topic to sub",
  "id": "id generate by client"
}
```

구독이 정상적으로 완료될 경우 WebSocket 클라이언트는 확인을 받습니다.

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btccny.kline.1min",
  "ts": 1489474081631
}
```

이후 구독한 주제가 업데이트될 시 WebSocket 클라이언트는 서버가 발송한 업데이트 메시지를 받습니다.（push）

```json
{
  "ch": "market.btccny.kline.1min",
  "ts": 1489474082831,
  "tick": {
    "id": 1489464480,
    "amount": 0.0,
    "count": 0,
    "open": 7962.62,
    "close": 7962.62,
    "low": 7962.62,
    "high": 7962.62,
    "vol": 0.0
  }
}
```

### 구독 취소

구독 취소 양식은 다음과 같습니다:

```json
{
  "unsub": "market.btccny.trade.detail",
  "id": "id4"
}
```

```json
{
  "unsub": "topic to unsub",
  "id": "id generate by client"
}
```

구독 취소 확인 완료:

```json
{
  "id": "id4",
  "status": "ok",
  "unsubbed": "market.btccny.trade.detail",
  "ts": 1494326028889
}
```

### 요청 데이터

WebSocket 서버는 동시에 일괄적으로 요청 받는 데이터(Pull)에 대해 모두 지원합니다.

요청 데이터의 양식：

```json
{
  "req": "market.ethbtc.kline.1min",
  "id": "id10"
}
```

```json
{
  "req": "topic to req",
  "id": "id generate by client"
}
```

일괄적으로 제공되는 데이터는 다음과 같습니다.

```json
{
  "status": "ok",
  "rep": "market.btccny.kline.1min",
  "tick": [
    {
      "amount": 1.6206,
      "count":  3,
      "id":     1494465840,
      "open":   9887.00,
      "close":  9885.00,
      "low":    9885.00,
      "high":   9887.00,
      "vol":    16021.632026
    },
    {
      "amount": 2.2124,
      "count":  6,
      "id":     1494465900,
      "open":   9885.00,
      "close":  9880.00,
      "low":    9880.00,
      "high":   9885.00,
      "vol":    21859.023500
    }
  ]
}
```

## K차트 데이터

### 구독 주제

K-차트 데이터 생성 시 WebSocket 서버는 구독 주제 API를 통해 클라이언트로 발송됩니다.

`market.$symbol$.kline.$period$`

> 구독 요청

```json
{
  "sub": "market.ethbtc.kline.1min",
  "id": "id1"
}
```

### 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 설명       | 값의 범위                                                    |
| --------- | ----------- | --------- | ---------- | ------------------------------------------------------------ |
| symbol    | string      | true      | 거래 코드  | btcusdt, ethbtc...(`GET /v1/common/symbols`참고)             |
| period    | string      | true      | K차트 주기 | 1min, 5min, 15min, 30min, 60min, 4hour, 1day, 1mon, 1week, 1year |

> Response

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.ethbtc.kline.1min",
  "ts": 1489474081631
}
```

> Update example

```json
{
  "ch": "market.ethbtc.kline.1min",
  "ts": 1489474082831,
  "tick": {
    "id": 1489464480,
    "amount": 0.0,
    "count": 0,
    "open": 7962.62,
    "close": 7962.62,
    "low": 7962.62,
    "high": 7962.62,
    "vol": 0.0
  }
}
```

### 데이터 업데이트 필드 목록

| 매개 변수 | 데이터 유형 | 설명                                                  |
| --------- | ----------- | ----------------------------------------------------- |
| id        | integer     | unix 시간, K-차트 ID로도 사용                         |
| amount    | float       | 거래 수량                                             |
| count     | integer     | 거래 건수                                             |
| open      | float       | 시작가                                                |
| close     | float       | 종가 (K-차트가 마지막 1개일 경우 최신 거래 가격 기준) |
| low       | float       | 최저가                                                |
| high      | float       | 최고가                                                |
| vol       | float       | 총 거래대금 (각 거래가격 * 거래량)                    |

### 요청 데이터

K-차트 데이터를 일괄 요청 시 다음 추가 매개 변수를 제공해야 합니다:
(매번 최대 300개 제공)

```json
{
  "req": "market.$symbol.kline.$period",
  "id": "id generated by client",
  "from": "from time in epoch seconds",
  "to": "to time in epoch seconds"
}
```

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본값                                | 설명                             | 값의 범위                                                    |
| --------- | ----------- | --------- | ------------------------------------- | -------------------------------- | ------------------------------------------------------------ |
| from      | integer     | false     | 1501174800(2017-07-28T00:00:00+08:00) | 시작 시간 (epoch time in second) | [1501174800, 2556115200]                                     |
| to        | integer     | false     | 2556115200(2050-01-01T00:00:00+08:00) | 종료 시간(epoch time in second)  | [1501174800, 2556115200] or ($from, 2556115200] if "from" is set |

## depth of market 시세 데이터

본 주제는 최근 마켓 depth of market 스냅샷을 보냅니다. 스냅샷 빈도는 1회/s.

### 구독 주제

`market.$symbol.depth.$type`

> Subscribe request

```json
{
  "sub": "market.btcusdt.depth.step0",
  "id": "id1"
}
```

### 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명           | 값의 범위                                          |
| --------- | ----------- | --------- | ------ | -------------- | -------------------------------------------------- |
| symbol    | string      | true      | NA     | 거래 코드      | btcusdt, ethbtc...（`GET /v1/common/symbols`참고） |
| type      | string      | true      | step0  | depth유형 조합 | step0, step1, step2, step3, step4, step5           |

**"type" depth 유형 조합**

| Value | Description                          |
| ----- | ------------------------------------ |
| step0 | 통합하지 않음                        |
| step1 | Aggregation level = precision*10     |
| step2 | Aggregation level = precision*100    |
| step3 | Aggregation level = precision*1000   |
| step4 | Aggregation level = precision*10000  |
| step5 | Aggregation level = precision*100000 |

type 값이 ‘step0’인 경우 기본 depth은 150획.

type 값이 ‘step1’,‘step2’,‘step3’,‘step4’,‘step5’인 경우 기본 depth은 20획.

> Response

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcusdt.depth.step0",
  "ts": 1489474081631
}
```

> Update example

```json
{
  "ch": "market.htusdt.depth.step0",
  "ts": 1572362902027,
  "tick": {
    "bids": [
      [3.7721, 344.86],// [price, quote volume]
      [3.7709, 46.66] 
    ],
    "asks": [
      [3.7745, 15.44],
      [3.7746, 70.52]
    ],
    "version": 100434317651,
    "ts": 1572362902012
  }
}
```

### 데이터 업데이트 필드 목록

<aside class="notice">'tick'object 아래에 매수 매도 depth 리스트를 노출합니다.</aside>
| 매개 변수 | 데터 유형 | 설명                                                 |
| --------- | --------- | ---------------------------------------------------- |
| bids      | object    | 현재 모든 매수 오더 [price, quote volume]            |
| asks      | object    | 현재 모든 매도 오더 [price, quote volume]            |
| version   | integer   | 내부 필드                                            |
| ts        | integer   | UTC/GMT+08:00 타임스탬프, 시간 단위: ms(millisecond) |

<aside class="notice">symbol이 "hb10"인 경우 amount, count, vol의 값은 0입니다. </aside>
### 요청 데이터

depth of market 데이터를 일괄 요청합니다.

```json
{
  "req": "market.btcusdt.depth.step0",
  "id": "id10"
}
```

## 매수 매도 시세

bid, bid 수량, ask, ask 수량 중에서 데이터가 변화될 때 업데이트 내용을 push 합니다.

### 구독 주제

`market.$symbol.bbo`

> Subscribe request

```json
{
  "sub": "market.btcusdt.bbo",
  "id": "id1"
}
```

### 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명      | 값의 범위                                          |
| --------- | ----------- | --------- | ------ | --------- | -------------------------------------------------- |
| symbol    | string      | true      | NA     | 거래 코드 | btcusdt, ethbtc...（`GET /v1/common/symbols`참고） |

> Response

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcusdt.bbo",
  "ts": 1489474081631
}
```

> Update example

```json
{
  "ch": "market.btcusdt.bbo",
  "ts": 1489474082831,
  "tick": {
    "symbol": "btcusdt",
    "quoteTime": "1489474082811",
    "bid": "10008.31",
    "bidSize": "0.01",
    "ask": "10009.54",
    "askSize": "0.3"
  }
}
```

### 데이터 업데이트 필드 리스트

| 매개 변수 | 데이터 유형 | 설명                    |
| --------- | ----------- | ----------------------- |
| symbol    | string      | 거래 코드               |
| quoteTime | long        | 시장 동태 업데이트 시간 |
| bid       | float       | bid                     |
| bidSize   | float       | bid 수량                |
| ask       | float       | ask                     |
| askSize   | float       | ask 수량                |


## 거래 상세 조회

### 구독 주제

본 주제는 Market의 최신 거래에 대한 내역을 제공합니다.

`market.$symbol.trade.detail`

> Subscribe request

```json
{
  "sub": "market.btcusdt.trade.detail",
  "id": "id1"
}
```

### 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명      | 값의 범위                                          |
| --------- | ----------- | --------- | ------ | --------- | -------------------------------------------------- |
| symbol    | string      | true      | NA     | 거래 코드 | btcusdt, ethbtc...（`GET /v1/common/symbols`참고） |

> Response

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcusdt.trade.detail",
  "ts": 1489474081631
}
```

> Update example

```json
{
  "ch": "market.btcusdt.trade.detail",
  "ts": 1489474082831,
  "tick": {
        "id": 14650745135,
        "ts": 1533265950234,
        "data": [
            {
                "amount": 0.0099,
                "ts": 1533265950234,
                "id": 146507451359183894799,
                "tradeId": 102043494568,
                "price": 401.74,
                "direction": "buy"
            }
            // more Trade Detail data here
        ]
  }
}
```

### 데이터 업데이트 필드 리스트

| 매개 변수 | 데이터 유형 | 설명                                          |
| --------- | ----------- | --------------------------------------------- |
| id        | integer     | 고유 거래 ID(삭제 예정)                       |
| tradeId   | integer     | 고유 거래 ID(NEW)                             |
| amount    | float       | 거래 수량                                     |
| price     | float       | 거래 가격                                     |
| ts        | integer     | 거래 시간 (UNIX epoch time in millisecond)    |
| direction | string      | 활성 거래자 (taker 주문 입장): 매수 또는 매도 |

### 요청 데이터

한 번에 거래 세부사항 데이터를 수집하기 위한 지원 데이터 요청 방법 (가장 최근 300개의 거래 내역을 받습니다.)

```json
{
  "req": "market.btcusdt.trade.detail",
  "id": "id11"
}
```

## Market 개요

### 구독 주제

본 주제는 24시간 내 최근 마켓의 스냅샷을 보냅니다. 스냅샷 빈도는 10회/s를 초과하지 않습니다.

`market.$symbol.detail`

> Subscribe request

```json
{
  "sub": "market.btcusdt.detail",
  "id": "id1"
}
```

### 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명      | 값의 범위                                          |
| --------- | ----------- | --------- | ------ | --------- | -------------------------------------------------- |
| symbol    | string      | true      | NA     | 거래 코드 | btcusdt, ethbtc...（`GET /v1/common/symbols`참고） |

> Response

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcusdt.detail",
  "ts": 1489474081631
}
```

> Update example

```json
  "tick": {
    "amount": 12224.2922,
    "open":   9790.52,
    "close":  10195.00,
    "high":   10300.00,
    "ts":     1494496390000,
    "id":     1494496390,
    "count":  15195,
    "low":    9657.00,
    "vol":    121906001.754751
  }
```

### 데이터 업데이트 필드 리스트

| 필드   | 데이터 유형 | 설명                              |
| ------ | ----------- | --------------------------------- |
| id     | integer     | unix시간，동시에 메시지 ID로 사용 |
| ts     | integer     | unix 시스템 시간                  |
| amount | float       | 24시간내 거래량                   |
| count  | integer     | 24시간내 체결건수                 |
| open   | float       | 24시간 시작가                     |
| close  | float       | 종가                              |
| low    | float       | 24시간내 최저가                   |
| high   | float       | 24시간내 최고가                   |
| vol    | float       | 24시간 총 거래대금                |

### 요청 데이터

한 번에 Market 요약 데이터를 얻기위한 지원 데이터 요청 방법은 다음과 같습니다.

```json
{
  "req": "market.btcusdt.detail",
  "id": "id11"
}
```

# Websocket자산 및 주문서

## 소개

### Access URL

**Websocket자산 및 주문**

**`wss://api-cloud.huobi.co.kr/ws/v1`**  

중국 이외의 IP를 통해 후오비 코리아 API로 접속해주시기 바랍니다.

### 데이터 압축

WebSocket API에서 제공된 모든 데이터는 GZIP로 압축되어 있으며 데이터 수신 후 클라이언트의 압축을 해제해야 합니다.

### Heartbeat Message

 **2019/07/08 이후**

회원님의 WebSocket 클라이언트가 후오비 코리아 WebSocket 서버에 연결되면 서버는 정기적으로(현재 20초로 설정) `ping` 메세지를 발송하며 메시지에 아래와 같은 데이터를 포함합니다.

> {
>     "op":"ping",
>     "ts":1492420473027
> } (자산 주문서 푸쉬)

회원님의 Websocket클라이언트가  Heartbeat Message를 접수후 `pong` 메시지를 반환하여야 하며 메시지에 같은 데이터를 포함 하여야 합니다:

> {
>     "op":"pong",
>     "ts":1492420473027
> } （자산 주문서 푸쉬）

<aside class="warning">WebSocket 서버가 `pong` 메세지를 수신하지 못하고 `ping` 메세지를 3회 연속 보낼 경우 서버는 클라이언트와의 연결을 종료합니다.</aside>
**2019/07/08 이전**

회원님의 WebSocket 클라이언트가 후오비 코리아 WebSocket 서버에 연결되면 서버는 정기적으로(30초로 설정) `ping` 메세지를 발송하며 메시지에 아래와 같은 데이터를 포함합니다.

> {
>     "op":"ping",
>     "ts":1492420473027
> }

회원님의 Websocket클라이언트가  Heartbeat Message를 접수후 `pong` 메시지를 반환하여야 하며 메시지에 같은 데이터를 포함 하여야 합니다:

> {
>     "op":"pong",
>     "ts":1492420473027
> }

<aside class="warning">WebSocket 서버가 `pong` 메세지를 수신하지 못하고 `ping` 메세지를 임의로 1회 보낼 경우 서버는 클라이언트와의 연결을 종료합니다.</aside>
### 구독 주제

WebSocket 서버에 성공적으로 연결되면 WebSocket 클라이언트는 특정 주제를 구독하기 위해 다음 요청을 보냅니다.

```json
{
  "op": "operation type, 'sub' for subscription, 'unsub' for unsubscription",
  "topic": "topic to sub",
  "cid": "id generate by client"
}
```

구독이 정상적으로 완료될 경우 WebSocket 클라이언트는 확인을 받습니다.

```json
{
  "op": "operation type, refer to the operation which triggers this response",
  "cid": "id1",
  "error-code": 0, // 0 for no error
  "topic": "topic to sub if the op is sub",
  "ts": 1489474081631
}
```

이후 구독한 주제가 업데이트될 시 WebSocket 클라이언트는 서버가 발송한 업데이트 메시지를 받습니다.（push）

```json
{
  "op": "notify",
  "topic": "topic of this notify",
  "ts": 1489474082831,
  "data": {
    // data of specific topic update
  }
}
```

### 구독 취소

구독 취소 양식은 다음과 같습니다:

```json
{
  "op": "unsub",
  "topic": "accounts",
  "cid": "client generated id"
}
```

구독 취소 확인 완료:

```json
{
  "op": "unsub",
  "topic": "accounts",
  "cid": "id generated by client",
  "err-code": 0,
  "ts": 1489474081631
}
```

### 요청 데이터

WebSocket 서버는 동시에 일괄적으로 요청 받는 데이터(Pull)에 대해 모두 지원합니다.

WebSocket 서버와 성공적으로 연결 시 회원님은 다음 3가지 주제를 요청할 수 있습니다.

- accounts.list
- orders.list
- orders.detail

요청 데이터의 형식은 다음과 같습니다.

### 빈도 제한

**데이터 요청(sub) 제한 규정**

빈도 제한 규정은 연결(connect) 대신 API Key를 기반으로 합니다. 요청 빈도가 제한 초과 시 WebSocket 클라이언트는 "too many request" 오류 코드를 수신 받습니다. 규칙은 다음과 같습니다:

매개 연결은 매초 최대 50회 sub와 50회 unsub가 있습니다.

매개 연결 sub 총량은 100개이고 sub총량이 한도에 도달하면 sub 불가 합니다. 하지만 매번 unsub시 sub 총량에서 감소 합니다. 예: 30개 sub 후 unsub1개, 이 경우 sub 총량의 count는 29개, 71개 sub 사용 가능합니다.

**데이터 요청 (req) 제한 규정**

빈도 제한 규정은 연결(connect) 대신 API Key를 기반으로 합니다. 요청 빈도가 제한 초과 시 WebSocket 클라이언트는 "too many request" 오류 코드를 수신 받습니다. 다음은 각 주제에 대한 현재 주파수 제한 설정입니다.

* accounts.list: once every 2 seconds
* orders.list AND orders.detail: once every 5 seconds

### 인증

자산 및 주문 주체에 대한 인증 요청 데이터 형식은 다음과 같습니다:

```json
{
  "op": "auth",
  "AccessKeyId": "e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx",
  "SignatureMethod": "HmacSHA256",
  "SignatureVersion": "2",
  "Timestamp": "2017-05-11T15:19:30",
  "Signature": "4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o=",
}
```

**인증 요청 데이터 유형 설명**

| 매개 변수        | 데이터 유형 | 설명                                                         |      |
| ---------------- | ----------- | ------------------------------------------------------------ | ---- |
| op               | string      | 필수, 작업명, 인증 고정값은 auth                             |      |
| cid              | string      | 선택, 클라이언트 요청 고유 ID                                |      |
| AccessKeyId      | string      | 필수, 요청한 API Key의 API Access Key                        |      |
| SignatureMethod  | string      | 선택, 서명 방법, 회원 서명 계산의 hash 기반 프로토콜, 여기서는 HmacSHA256를 사용합니다 |      |
| SignatureVersion | string      | 필수, 서명 프로토콜 버젼, 여기서는 버전 2를 사용합니다       |      |
| Timestamp        | string      | 선택, 회원님이 요청한 시간(UTC 시간대), 쿼리 요청 시 해당 값을 포함하면 제3자가 회원님의 요청을 가로챌 수 없습니다. 예) 2017-05-11 T16:22:06. UTC 시간대 입니다, 유의 하시기 바랍니다. |      |
| Signature        | string      | 필수, 서명이 유효하고 변조되지 않았음을 보장하기 위해 서명에 의해 계산된 값입니다. |      |

> **참고**
>
> - [https://huobiapi.github.io/docs/spot/v1/cn/#c64cd15fdc] 를 참조하여 유효한 서명을 생성해주세요.
> - 요청 메소드는 서명 계산에서 고정된 `GET` 값을 갖습니다.

## 구독 계정 업데이트

API Key 권한：읽기

구독 계정의 자산 변화 업데이트

### 구독 주제

`accounts`

> Subscribe request

```json
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "topic": "accounts",
  "model": "0"
}
```

### 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본 값 | 설명                | 값의 범위                             |
| --------- | ----------- | --------- | ------- | ------------------- | ------------------------------------- |
| model     | string      | false     | 0       | 동결 잔액 포함 여부 | 1 to include frozen balance, 0 to not |

<aside class="notice">만약 사용 가능한 총 잔액 구독 시 0과 1로 별도로 Websocket을 연결해야 합니다.</aside>
> Response

```json
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "err-code": 0,
  "ts": 1489474081631,
  "topic": "accounts"
}
```

> Update example

```json
{
  "op": "notify",
  "ts": 1522856623232,
  "topic": "accounts",
  "data": {
    "event": "order.place",
    "list": [
      {
        "account-id": 419013,
        "currency": "usdt",
        "type": "trade",
        "balance": "500009195917.4362872650"
      }
    ]
  }
}

```

### 데이터 업데이트 필드 리스트

| 필드       | 데이터 유형 | 설명                                                         |
| ---------- | ----------- | ------------------------------------------------------------ |
| event      | string      | 자산 변동 통지 관련 설명, 예를 들면 주문서 생성(order.place), 주문 체결(order.match), 주문 체결 환불（order.refund), 주문 철회(order.cancel), 쿠폰 포인트 차감 수수료（order.fee-refund), 기타 자산 변동(other) |
| account-id | integer     |                                                              |
| currency   | string      | 코인 종류                                                    |
| type       | string      | 계정 유형, 서브 계정(trade)                                  |
| balance    | string      | 계정 잔액 (구독 mode=0 일때 해당 잔액은 사용 가능한 잔액 입니다；구독 mode=1시，해당 잔액은 총잔액 입니다） |

## 구독 주문서 업데이트

API Key 권한：읽기

구독 계정에 있는 주문서 업데이트

### 구독 주제

`orders.$symbol`

> Subscribe request

```json
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "topic": "orders.htusdt",
}
```

### 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본 값 | 설명      | 값의 범위                                                    |
| --------- | ----------- | --------- | ------- | --------- | ------------------------------------------------------------ |
| symbol    | string      | true      | NA      | 거래 코드 | btcusdt, ethbtc...（`GET /v1/common/symbols`참고）, 부호"*"지원. |

> Response

```json
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "err-code": 0,
  "ts": 1489474081631,
  "topic": "orders.htusdt"
}
```

> Update example

```json
{
  "op": "notify",
  "topic": "orders.htusdt",
  "ts": 1522856623232,
  "data": {
    "seq-id": 94984,
    "order-id": 2039498445,
    "symbol": "htusdt",
    "account-id": 100077,
    "order-amount": "5000.000000000000000000",
    "order-price": "1.662100000000000000",
    "created-at": 1522858623622,
    "order-type": "buy-limit",
    "order-source": "api",
    "order-state": "filled",
    "role": "taker|maker",
    "price": "1.662100000000000000",
    "filled-amount": "5000.000000000000000000",
    "unfilled-amount": "0.000000000000000000",
    "filled-cash-amount": "8301.357280000000000000",
    "filled-fees": "8.000000000000000000"
  }
}
```

### 데이터 업데이트 필드 목록

| 매개 변수          | 데이터 유형 | 설명                                                         |
| ------------------ | ----------- | ------------------------------------------------------------ |
| seq-id             | integer     | 시리얼 넘버(연속된 번호가 아님)                              |
| order-id           | integer     | 주문서 id                                                    |
| symbol             | string      | 거래 페어                                                    |
| account-id         | string      | 계정 id                                                      |
| order-amount       | string      | 주문서 수량                                                  |
| order-price        | string      | 주문 가격                                                    |
| created-at         | int         | 주문 생성 시간(UNIX epoch time in millisecond)               |
| order-type         | string      | 주문서 유형, 유효값: buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker,buy-stop-limit,sell-stop-limit |
| order-source       | string      | 주문 경로, 유효값:sys, web, api, app                         |
| order-state        | string      | 주문 상태, 유효값: submitted, partial-filled, filled, canceled, partial-canceled |
| role               | string      | 체결 role: taker or maker                                    |
| price              | string      | 체결 가격                                                    |
| filled-amount      | string      | 단일 체결 수량                                               |
| unfilled-amount    | string      | 단일 미체결 수량                                             |
| filled-cash-amount | string      | 단일 체결 금액                                               |
| filled-fees        | string      | 단일 체결 수수료(매수시 거래 화폐, 매도시 마켓 화폐)         |

## 구독 주문서 업데이트 (NEW)

API Key 권한：읽기

현재 회원 주문서 업데이트 PUSH“orders.symbol”에 비하여 새로 추가 된 “orders.symbol.update”는 딜레이가 작고 메시지가 더 정확합니다. API 회원께서는 새로운 주제를 구독하여 “orders.symbol”를 PUSH 하기 바라는 바 입니다.（기존 구독 주제  “orders.symbol”는 별도로 통지 하기 전까지 Websocket API에서 보류 중입니다.）

### 구독 주제

`orders.$symbol.update`

> Subscribe request

```json
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "topic": "orders.htusdt.update"
}
```

### 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명                                                         | 값의 범위 |
| --------- | ----------- | --------- | ------ | ------------------------------------------------------------ | --------- |
| symbol    | string      | true      | NA     | btcusdt, ethbtc...（`GET /v1/common/symbols`참고）, 부호"*" 지원. |           |



> Response

```json
{
  "op": "sub",
  "ts": 1489474081631,
  "topic": "orders.htusdt.update",
  "err-code": 0,
  "cid": "40sG903yz80oDFWr"  
}
```

> Update example

```json
{
  "op": "notify",
  "ts": 1522856623232,
  "topic": "orders.htusdt.update",  
  "data": {
    "unfilled-amount": "0.000000000000000000",
    "filled-amount": "5000.000000000000000000",
    "price": "1.662100000000000000",
    "order-id": 2039498445,
    "symbol": "htusdt",
    "match-id": 94984,
    "filled-cash-amount": "8301.357280000000000000",
    "role": "taker|maker",
    "order-state": "filled",
    "client-order-id": "a0001",
    "order-type": "buy-limit"
  }
}
```

### 데이트 업데이트 필드 목록

| 매개 변수          | 데터 유형 | 설명                                                         |
| ------------------ | --------- | ------------------------------------------------------------ |
| match-id           | integer   | 최근 매칭 번호（order-state = submitted, canceled, partial-canceled 일때 match-id 는 메시지 넘버 입니다. order-state = filled, partial-filled 일때 match-id 는 최근 매칭 번호 입니다.） |
| order-id           | integer   | 주문서 번호                                                  |
| symbol             | string    | 거래 코드                                                    |
| order-state        | string    | 주문서 상태, 유효값: submitted, partial-filled, filled, canceled, partial-canceled |
| role               | string    | 최근 체결 role（order-state = submitted, canceled, partial-canceled 일때 role은 디폴트 taker입니다. order-state = filled, partial-filled 일때 role 값은taker 혹은 maker 입니다.） |
| price              | string    | 현재가（order-state = submitted 일때 price 는 주문 가격 입니다. order-state = canceled, partial-canceled 일때 price 는 0입니다. order-state = filled, partial-filled 일때 price는 최근 체결가 입니다.) |
| filled-amount      | string    | 최근 체결 수량                                               |
| filled-cash-amount | string    | 최근 체결 금액                                               |
| unfilled-amount    | string    | 최근 미체결 수량（order-state = submitted 일때 unfilled-amount 는 처음 주문량 입니다. order-state = canceled OR partial-canceled 일때 unfilled-amount 는 미체결 수량 입니다. order-state = filled 일때 만약 order-type = buy-market, unfilled-amount 는 극히 작은 수 일수 있습니다. 만약 order-type <> buy-market 일때 unfilled-amount 는 0 입니다. order-state = partial-filled AND role = taker 일때 unfilled-amount 는 미체결 수량 입니다. order-state = partial-filled AND role = maker 일때 unfilled-amount 는 미체결 수량 입니다. |
| client-order-id    | string    | 고객 자체 정의 주문서 번호                                   |
| order-type         | string    | 주문서 유형, buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker,buy-stop-limit,sell-stop-limit 포함 |

## 회원 자산 데이터 요청

API Key 권한：읽기

해당 회원의 모든 계정 잔액 조회

### 요청 데이터

`accounts.list`

> Query request

```json
{
  "op": "req",
  "cid": "40sG903yz80oDFWr",
  "topic": "accounts.list",
}
```
WebSocket API와 접속 후 Server에 다음 양식과 같은 데이터를 발송하여 계정 데이터를 조회합니다：

| 매개 변수 | 데이터 유형 | 설명                          |      |
| --------- | ----------- | ----------------------------- | ---- |
| op        | string      | 필수, 작업 명칭, 고정값은 req |      |
| cid       | string      | 선택, Client 유일한 ID 요청   |      |
| topic     | string      | 필수, 고정값은 accounts.list  |      |

### 반환

> Successful

```json
    {
      "op": "req",
      "topic": "accounts.list",
      "cid": "40sG903yz80oDFWr",
      "err-code": 0,
      "ts": 1489474082831,
      "data": [
        {
          "id": 419013,
          "type": "spot",
          "state": "working",
          "list": [
            {
              "currency": "usdt",
              "type": "trade",
              "balance": "500009195917.4362872650"
            },
            {
              "currency": "usdt",
              "type": "frozen",
              "balance": "9786.6783000000"
            }
          ]
        },
        {
          "id": 35535,
          "type": "point",
          "state": "working",
          "list": [
            {
              "currency": "eth",
              "type": "trade",
              "balance": "499999894616.1302471000"
            },
            {
              "currency": "eth",
              "type": "frozen",
              "balance": "9786.6783000000"
            }
          ]
        }
      ]
    }
```

> Failed

```json
    {
      "op": "req",
      "topic": "foo.bar",
      "cid": "40sG903yz80oDFWr",
      "err-code": 12001, //Response codes,0  represent success;others value  is error,the list of Response codes is in appendix
      "err-msg": "detail of error message",
      "ts": 1489474081631
    }
```

| 매개 변수 | 데이터 유형 | 설명               |      |
| --------- | ----------- | ------------------ | ---- |
| id        | long        | 계정ID             |      |
| type      | string      | 계정 유형          |      |
| state     | string      | 계정 상태          |      |
| list      | string      | 계정 리스트        |      |
| currency  | string      | 서브계정 코인 종류 |      |
| type      | string      | 서브계정 유형      |      |
| balance   | string      | 서브계정 잔액      |      |

## 현재 및 과거 주문 리스트 요청

API Key 권한：읽기

설정한 조건에 의하여 현재 주문 및 과거 주문 조회.

### 요청 데이터

`order.list`

> Query request

```json
{
  "op": "req",
  "topic": "orders.list",
  "cid": "40sG903yz80oDFWr",
  "symbol": "htusdt",
  "states": "submitted,partial-filled"
}
```

### 매개 변수

| 매개 변수  | 데이터 유형 | 필수 여부 | 기본값 | 설명                                      | 값의 범위                                                    |
| ---------- | ----------- | --------- | ------ | ----------------------------------------- | ------------------------------------------------------------ |
| account-id | int         | true      | N/A    | 계정ID                                    | N/A                                                          |
| symbol     | string      | true      | N/A    | 거래 페어                                 | btcusdt, ethbtc...（`GET /v1/common/symbols`참고）           |
| types      | string      | false     | N/A    | 조회한 주문서 유형 조합, 부호 ',' 로 분리 | buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc |
| states     | string      | false     | N/A    | 조회한 주문서 상태 조합, 부호 ',' 로 분리 | submitted, partial-filled, partial-canceled, filled, canceled |
| start-date | string      | false     | -61d   | 조회 시작일자, 일자 양식 yyyy-mm-dd       | N/A                                                          |
| end-date   | string      | false     | today  | 조회 종료일자, 일자 양식 yyyy-mm-dd       | N/A                                                          |
| from       | string      | false     | N/A    | 조회 처음 ID                              | N/A                                                          |
| direct     | string      | false     | next   | 조회 방향                                 | next, prev                                                   |
| size       | int         | false     | 100    | 조회 내역 크기                            | [1, 100]                                                     |

### 데이터 업데이트 필드 리스트

> Successful

```json
{
  "op": "req",
  "topic": "orders.list",
  "cid": "40sG903yz80oDFWr",
  "err-code": 0,
  "ts": 1522856623232,
  "data": [
    {
      "id": 2039498445,
      "symbol": "htusdt",
      "account-id": 100077,
      "amount": "5000.000000000000000000",
      "price": "1.662100000000000000",
      "created-at": 1522858623622,
      "type": "buy-limit",
      "filled-amount": "5000.000000000000000000",
      "filled-cash-amount": "8301.357280000000000000",
      "filled-fees": "8.000000000000000000",
      "finished-at": 1522858624796,
      "source": "api",
      "state": "filled",
      "canceled-at": 0
    }
  ]
}
```

| 필드               | 데이터 유형 | 설명                                         |      |
| ------------------ | ----------- | -------------------------------------------- | ---- |
| id                 | long        | 주문서 ID                                    |      |
| symbol             | string      | 거래 페어                                    |      |
| account-id         | long        | 계정ID                                       |      |
| amount             | string      | 주문서 수량                                  |      |
| price              | string      | 주문 가격                                    |      |
| created-at         | long        | 주문 생성 시간                               |      |
| type               | string      | 주문서 유형, 주문서 유형 설명을 참고 하세요. |      |
| filled-amount      | string      | 체결 수량                                    |      |
| filled-cash-amount | string      | 체결 총금액                                  |      |
| filled-fees        | string      | 체결 수수료                                  |      |
| finished-at        | string      | 체결 종료 시간                               |      |
| source             | string      | 주문 경로, 주문 경로 설명을 참고 하세요.     |      |
| state              | string      | 주문서 상태, 주문서 상태 설명을 참고 하세요. |      |
| cancel-at          | long        | 주문 철회 시간                               |      |
| stop-price         | string      | Stop limit 발동 가격                         |      |
| operator           | string      | Stop limit 발동 가격 operator                |      |

## 주문서 ID로 주문서 요청

API Key 권한：읽기

주문서 ID로 주문서 데이터 요청

### 요청 데이터

`orders.detail`

> Query request

```json
{
  "op": "req",
  "topic": "orders.detail",
  "order-id": "2039498445",
  "cid": "40sG903yz80oDFWr"
}
```

### 매개 변수

| 매개 변수 | 데이터 유형 | 필수여부 | 설명                    |      |
| --------- | ----------- | -------- | ----------------------- | ---- |
| op        | string      | true     | 작업 명칭, 고정치는 req |      |
| cid       | string      | true     | Client 유일한 ID 요청   |      |
| topic     | string      | false    | 고정치는 orders.detail  |      |
| order-id  | string      | true     | 오더 ID                 |      |


### Response

> Successful

```json
{
  "op": "req",
  "topic": "orders.detail",
  "cid": "40sG903yz80oDFWr",
  "err-code": 0,
  "ts": 1522856623232,
  "data": {
    "id": 2039498445,
    "symbol": "htusdt",
    "account-id": 100077,
    "amount": "5000.000000000000000000",
    "price": "1.662100000000000000",
    "created-at": 1522858623622,
    "type": "buy-limit",
    "filled-amount": "5000.000000000000000000",
    "filled-cash-amount": "8301.357280000000000000",
    "filled-fees": "8.000000000000000000",
    "finished-at": 1522858624796,
    "source": "api",
    "state": "filled",
    "canceled-at": 0
  }
}
```
| 매개 변수          | 데이터 유형 | 설명                                         |      |
| ------------------ | ----------- | -------------------------------------------- | ---- |
| id                 | long        | 주문서 ID                                    |      |
| symbol             | string      | 거래 페어                                    |      |
| account-id         | long        | 계정 ID                                      |      |
| amount             | string      | 주문서 수량                                  |      |
| price              | string      | 주문 가격                                    |      |
| created-at         | long        | 주문 생성 시간                               |      |
| type               | string      | 주문서 유형，주문서 유형 설명을 참고하세요.  |      |
| filled-amount      | string      | 체결 수량                                    |      |
| filled-cash-amount | string      | 체결 총금액                                  |      |
| filled-fees        | string      | 체결 수수료                                  |      |
| finished-at        | string      | 체결 종료 시간                               |      |
| source             | string      | 주문 경로, 주문 경로 설명을 참고 하세요.     |      |
| state              | string      | 주문서 상태, 주문서 상태 설명을 참고 하세요. |      |
| cancel-at          | long        | 주문 철회 시간                               |      |
| stop-price         | string      | Stop-limit 발동 가격                         |      |
| operator           | string      | Stop-limit 발동 가격 operator                |      |

# Websocket자산 및 주문서(v2)

## 소개

### Access URL

**WebSocket 자산 및 주문(v2)**

**`wss://api-cloud.huobi.co.kr/ws/v2`**  

중국 외 서버로 후오비 코리아 API를 접속하시기 바랍니다.

### 데이터 압축

v1 버전과 달리 v2 버전은 응답 데이터에 대하여 GZIP을 압축하지 않았습니다.

### Heartbeat Message

회원님의 WebSocket 클라이언트가 후오비 코리아 WebSocket 서버에 연결되면 서버는 정기적으로(현재 20초로 설정) `ping` 메세지를 발송하며 메세지에 아래와 같은 데이터를 포함합니다.

```json
{
  "action": "ping",
  "data": {
    "ts": 1575537778295
  }
}
```

회원님의 Websocket 클라이언트가  Heartbeat Message를 접수 후 `pong` 메시지를 반환하여야 하며 메시지에 같은 데이터를 포함하여야 합니다:

```json
{
    "action": "ping",
    "data": {
          "ts": 1575537778295 // Ping 메시지의 ts값
    }
}
```

### `action`의 유효 값

| 유효 값    | 설명                                                         |
| ---------- | ------------------------------------------------------------ |
| sub        | 구독 데이터                                                  |
| req        | 데이터 요청                                                  |
| ping、pong | Heartbeat 데이터                                             |
| push       | 푸시 데이터, 서버에서 고객 클라이언트에 발송하는 데이터 유형 |

### 빈도 제한(Rate Limit)

이 버전은 사용자를 위한 다차원 빈도 제한 전략을 채택하며, 구체적인 전략은 다음과 같습니다:

- 한 개의 연결하는 유효 요청(req，sub，unsub을 포함하고 ping/pong 및 기타 무효 요청은 포함하지 않습니다)은 50회/초(초는 슬라이드 윈도로 제한)로 제한합니다. 이 제한은 초과하는 경우 "too many request" 오류 메시지를 반환합니다.
- 한 개의 API Key로 연결하는 개수를 10개로 제한합니다. 제한을 초과하는 경우 "too many connection" 오류 메시지를 반환합니다.
- 한 개의 IP로 연결하는 수량은 100회/초로 제한합니다. 제한을 초과하는 경우 "too many request" 오류 메시지를 반환합니다.

### 인증

인증 요청 양식은 다음과 같습니다:

```json
{
    "action": "req", 
    "ch": "auth",
    "params": { 
        "authType":"api",
        "accessKey": "sffd-ddfd-dfdsaf-dfdsafsd",
        "signatureMethod": "HmacSHA256",
        "signatureVersion": "2.1",
        "timestamp": "2019-09-01T18:16:16",
        "signature": "4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o="
    }
}

```

인증 성공시 제공하는 데이터 양식은 다음과 같습니다:

```json
{
  "action": "req",
  "code": 200,
  "ch": "auth",
  "data": {}
}
```

매개 변수 설명

| 매개 변수        | 필수 여부 | 데이터 유형 | 설명                                                         |
| ---------------- | --------- | ----------- | ------------------------------------------------------------ |
| action           | true      | string      | Websocket 데이터 작업 유형, 인증 고정값은 req                |
| ch               | true      | string      | 인증 주제, 인증 고정값은 auth                                |
| authType         | true      | string      | 인증 유형, 인증 고정값은 api. 주의: 이 파라미터는 서명 계산에 없습니다. |
| accessKey        | true      | string      | 회원이 신청한 API Key의  AccessKey                           |
| signatureMethod  | true      | string      | 선택, 서명 방법, 회원 서명 계산의 hash 기반 프로토콜, 여기서는 HmacSHA256를 사용합니다. |
| signatureVersion | true      | string      | 사인 프로토콜 버전, 고정값은 2.1                             |
| timestamp        | true      | string      | 선택, 회원님이 요청한 시간(UTC 시간대), 쿼리 요청 시 해당 값을 포함하면 제 3자가 회원님의 요청을 가로챌 수 없습니다. 예) 2017-05-11 T16:22:06. UTC 시간대 입니다, 유의 하시기 바랍니다. |
| signature        | true      | string      | 필수, 서명이 유효하고 변조되지 않았음을 보장하기 위해 서명에 의해 계산된 값입니다. |

### 서명 절차

v2.1서명은  v2.0서명과 절차가 비슷합니다, 차이점은 다음과 같습니다:

1. 서명에 필요한 문자열 생성 시 요청 방법은 GET로 하고 요청 주소는 /ws/v2로 고정합니다.
2. 서명에 필요한 고정 파라미터를 다음으로 교체: accessKey，signatureMethod，signatureVersion，timestamp
3. signatureVersion버전을 2.1로 업데이트

v2 버전 서명 절차는 본 [링크](https://huobiapi.github.io/docs/spot/v1/cn/#c64cd15fdc) 에서 확인하십시오.

서명 전 마지막으로 생성한 문자열은 다음과 같습니다:

```
GET\n
api-cloud.huobi.co.kr\n
/ws/v2\n
accessKey=0664b695-rfhfg2mkl3-abbf6c5d-49810&signatureMethod=HmacSHA256&signatureVersion=2.1&timestamp=2019-12-05T11%3A53%3A03
```

주의: JSON 요청에서 데이터는 URL 코딩을 하지 않아도 됩니다.

### 구독 주제

WebSocket 서버에 성공적으로 연결되면 WebSocket 클라이언트는 특정 주제를 구독하기 위해 다음 요청을 보냅니다.

```json
{
  "action": "sub",
  "ch": "accounts.update"
}
```

구독이 완료될 경우 Websocket 클라이언트는 다음과 같은 메시지를 받습니다:

```json
{
  "action": "sub",
  "code": 200,
  "ch": "accounts.update#0",
  "data": {}
}
```

### 요청 데이터

Websocket 서버에 연결 시 Websocket 클라이언트는 다음과 같은 일괄적 데이터를 발송합니다:

```json
{
    "action": "req", 
    "ch": "topic",
}
```

요청 완료 시 Websocket 클라이언트는 다음과 같은 정보를 받습니다:

```json
{
    "action": "req",
    "ch": "topic",
    "code": 200,
    "data": {} // 요청 데이터
}
```

### 오류 코드

다음은 WebSocket 자산과 주문 API의 오류 코드, 오류 메시지에 대한 설명입니다.

| 오류 코드 | 오류 메시지              | 설명                                                         |
| --------- | ------------------------ | ------------------------------------------------------------ |
| 200       | Success                  | 정확한 응답                                                  |
| 100       | timeout close            | timeout close                                                |
| 400       | Bad Request              | 요청 오류                                                    |
| 404       | Not Found                | 서비스를 찾을 수 없습니다.                                   |
| 429       | Too Many Requests        | 한 개의 설비로 최대 연결수 초과 혹은 IP 최대 연결수를 초과하였습니다. |
| 500       | system error             | 시스템 오류                                                  |
| 2000      | invalid.ip               | ip 무효                                                      |
| 2001      | nvalid.json              | json 요청 무효                                               |
| 2001      | invalid.authType         | 인증 방법 오류                                               |
| 2001      | invalid.action           | 구독 이벤트 무효                                             |
| 2001      | invalid.symbol           | 거래 페어 무효                                               |
| 2001      | invalid.ch               | 구독 무효                                                    |
| 2001      | invalid.exchange         | 거래소 code 무효                                             |
| 2001      | missing.param.auth       | 인증 파라미터 없음                                           |
| 2002      | invalid.auth.state       | 인증 미통과                                                  |
| 2002      | auth.fail                | 인증 실패                                                    |
| 2003      | query.account.list.error | 계정 리스트 조회 실패                                        |
| 4000      | too.many.request         | 클라이언트 요청 상한 초과                                    |
| 4000      | too.many.connection      | 같은 key 연결수 초과                                         |

## 주문서 업그레이드 구독

API Key 권한: 읽기

다음 임의의 이벤트에 의해 주문서의 푸시가 업그레이드 됩니다:<br>

- 계획 위탁 혹은 추적 위탁 실패 이벤트（eventType=trigger）<br>
- 계획 위탁 혹은 추적 위탁 작동전 주문 철회 이벤트（eventType=deletion）<br>
- 주문서 생성（eventType=creation）<br>
- 주문서 체결（eventType=trade）<br>
- 주문서 철회（eventType=cancellation）<br>

서로 다른 이벤트 유형의 푸시 메세지중 필드 리스트가 조금 다릅니다. 개발자는 다음 2가지 방식을 채택하여 응답 데이터를 받을 수 있습니다:<br>

- 모든 필드를 포함한 데이터 구조를 정하고 일부 필드를 NULL로 허용<br>
- 서로 다른 데이터 구조를 정하고 각 필드를 포함하고 공용 데이터 필드를 계승한 데이터 구조

### 구독 주제

` orders#${symbol}`

### 구독 파라미터

> Subscribe request

```json
{
	"action": "sub",
	"ch": "orders#btcusdt"
}

```

> Response

```json
{
	"action": "sub",
	"code": 200,
	"ch": "orders#btcusdt",
	"data": {}
}
```

| 매개 변수 | 유형   | 설명                        |
| --------- | ------ | --------------------------- |
| symbol    | string | 거래 코드(와일드카드* 지원) |

### 데이터 업그레이드 필드 리스트

> Update example

```json
{
	"action":"push",
	"ch":"orders#btcusdt",
	"data":
	{
		"orderSide":"buy",
		"lastActTime":1583853365586,
		"clientOrderId":"abc123",
		"orderStatus":"rejected",
		"symbol":"btcusdt",
		"eventType":"trigger",
		"errCode": 2002,
		"errMessage":"invalid.client.order.id (NT)"
	}
}
```

조건부 주문 작동 실패 후 –

| 매개 변수     | 유형   | 설명                                                         |
| ------------- | ------ | ------------------------------------------------------------ |
| eventType     | string | 이벤트 유형, 유효값: trigger(본 이벤트는 계획 위탁/추적 위탁에만 유효) |
| symbol        | string | 거래 코드                                                    |
| clientOrderId | string | 유저가 정한 주문번호                                         |
| orderSide     | string | 주문 방향, 유효값: buy,sell                                  |
| orderStatus   | string | 주문 상태, 유효값: rejected                                  |
| errCode       | int    | 주문 실패 오류 코드                                          |
| errMessage    | string | 주문 실패 오류 메시지                                        |
| lastActTime   | long   | 주문 실패 시간                                               |

> Update example

```json
{
	"action":"push",
	"ch":"orders#btcusdt",
	"data":
	{
		"orderSide":"buy",
		"lastActTime":1583853365586,
		"clientOrderId":"abc123",
		"orderStatus":"canceled",
		"symbol":"btcusdt",
		"eventType":"deletion"
	}
}
```

조건부 주문이 작동되기 전 주문이 철회가 되었을 시 –

| 매개 변수     | 유형   | 설명                                                         |
| ------------- | ------ | ------------------------------------------------------------ |
| eventType     | string | 이벤트 유형, 유효값:deletion(본 이벤트는 계획 위탁/추적 위탁에만 유효) |
| symbol        | string | 거래 코드                                                    |
| clientOrderId | string | 유저가 정한 주문번호                                         |
| orderSide     | string | 주문 방향, 유효값:buy,sell                                   |
| orderStatus   | string | 주문 상태, 유효값:canceled                                   |
| lastActTime   | long   | 주문 철회 시간                                               |

> Update example

```json
{
	"action":"push",
	"ch":"orders#btcusdt",
	"data":
	{
		"orderSize":"2.000000000000000000",
		"orderCreateTime":1583853365586,
		"accountId":992701,
		"orderPrice":"77.000000000000000000",
		"type":"sell-limit",
		"orderId":27163533,
		"clientOrderId":"abc123",
		"orderStatus":"submitted",
		"symbol":"btcusdt",
		"eventType":"creation"
	}
}
```

주문서 제출 후 –

| 매개 변수       | 유형   | 설명                                                         |
| --------------- | ------ | ------------------------------------------------------------ |
| eventType       | string | 이벤트 유형, 유효값:creation                                 |
| symbol          | string | 거래 코드                                                    |
| accountId       | long   | 계정ID                                                       |
| orderId         | long   | 주문ID                                                       |
| clientOrderId   | string | 유저가 정한 주문번호(있는 경우)                              |
| orderPrice      | string | 주문 가격                                                    |
| orderSize       | string | 주문 수량(시장가 매수는 무효)                                |
| orderValue      | string | 주문 금액(시장가 매수 주문에만 유효)                         |
| type            | string | 주문 유형, 유효값:buy-market, sell-market, buy-limit, sell-limit, buy-limit-maker, sell-limit-maker, buy-ioc, sell-ioc |
| orderStatus     | string | 주문 상태, 유효값:submitted                                  |
| orderCreateTime | long   | 주문 생성 시간                                               |

주의：<BR>

- 스탑리밋 주문은 작동 되기전 API는 주문서 생성을 푸쉬하지 않습니다;<br>
- Taker 주문서는 체결 전, API는 우선적으로 생성 이벤트를 푸쉬 합니다.<br>
- 스탑리밋 주문의 주문 유형은 오리지널 주문 유형인 “buy-stop-limit”혹은 “sell-stop-limit”이 아니고 “buy-limit”혹은 “sell-limit”으로 변경됩니다.<BR>

> Update example

```json
{
	"action":"push",
	"ch":"orders#btcusdt",
	"data":
	{
		"tradePrice":"76.000000000000000000",
		"tradeVolume":"1.013157894736842100",
		"tradeId":301,
		"tradeTime":1583854188883,
		"aggressor":true,
		"remainAmt":"0.000000000000000400000000000000000000",
		"orderId":27163536,
		"type":"sell-limit",
		"clientOrderId":"abc123",
		"orderStatus":"filled",
		"symbol":"btcusdt",
		"eventType":"trade"
	}
}
```

주문 체결 후 –

| 매개 변수     | 유형   | 설명                                                         |
| ------------- | ------ | ------------------------------------------------------------ |
| eventType     | string | 이벤트 유형, 유효값:trade                                    |
| symbol        | string | 거래 코드                                                    |
| tradePrice    | string | 체결 가격                                                    |
| tradeVolume   | string | 체결량                                                       |
| orderId       | long   | 주문ID                                                       |
| type          | string | 주문 유형, 유효값:buy-market, sell-market, buy-limit, sell-limit, buy-limit-maker, sell-limit-maker, buy-ioc, sell-ioc |
| clientOrderId | string | 유저가 정한 주문번호(있는 경우)                              |
| tradeId       | long   | 체결ID                                                       |
| tradeTime     | long   | 체결 시간                                                    |
| aggressor     | bool   | 주동 거래측 여부, 유효값: true (taker), false (maker)        |
| orderStatus   | string | 주문 상태, 유효값:partial-filled, filled                     |
| remainAmt     | string | 미체결 수량(시장가 매수 주문이 미체결 금액)                  |

주의:<BR>

- 스탑리밋 주문의 주문 유형은 오리지널 주문 유형인 “buy-stop-limit”혹은 “sell-stop-limit”이 아니고 “buy-limit”혹은 “sell-limit”으로 변경 됩니다.<BR>
- 한 개 taker 주문이 상대 측 여러 주문과 체결 후 생성되는 체결 내역(tradePrice, tradeVolume, tradeTime, tradeId, aggressor)은 결합하여 한 개로 푸시 하지 않고 각각 나누어 푸시합니다. <BR>

> Update example

```json
{
	"action":"push",
	"ch":"orders#btcusdt",
	"data":
	{
		"lastActTime":1583853475406,
		"remainAmt":"2.000000000000000000",
		"orderId":27163533,
		"type":"sell-limit",
		"clientOrderId":"abc123",
		"orderStatus":"canceled",
		"symbol":"btcusdt",
		"eventType":"cancellation"
	}
}
```

주문 철회 후 –

| 매개 변수     | 유형   | 설명                                                         |
| :------------ | :----- | :----------------------------------------------------------- |
| eventType     | string | 이벤트 유형, 유효값:cancellation                             |
| symbol        | string | 거래 코드                                                    |
| orderId       | long   | 주문ID                                                       |
| type          | string | 주문 유형, 유효값:buy-market, sell-market, buy-limit, sell-limit, buy-limit-maker, sell-limit-maker, buy-ioc, sell-ioc |
| clientOrderId | string | 유저가 정한 주문번호(있는 경우)                              |
| orderStatus   | string | 주문 상태, 유효값:partial-canceled, canceled                 |
| remainAmt     | string | 미체결 수량(시장가 매수 주문인 경우 미체결금액)              |
| lastActTime   | long   | 주문서 마지막 업데이트 시간                                  |

注：<BR>

- 스탑리밋 주문의 주문 유형은 오리지널 주문 유형인 “buy-stop-limit”혹은 “sell-stop-limit”이 아니고 “buy-limit”혹은 “sell-limit”으로 변경됩니다.<BR>

## 청산 후 체결 및 철회 업그레이드 구독

API Key 권한: 읽기

유저 주문이 체결 혹은 철회 경우에만 푸시 합니다. 주문이 체결되는 경우에는 하나씩 푸시합니다. 한 개 taker 주문이 여러 개 maker 주문과 체결되면 이 API는 업데이트 내용을 하나씩 푸시합니다. 하지만 고객이 받은 체결 메세지 순서는 실제 체결된 순서와 다를 수 있습니다. 만약 1개 주문서가 체결 및 철회가 동시에 발생하는 경우, 예를 들면 IOC 주문서는 체결 후 남은 부분은 자동으로 철회되는 경우 유저는 우선 철회 푸시를 받고 그다음 체결 푸시를 받습니다.<br>

만약 유저가 주문서 업데이트를 순서대로 푸시 받으려면 다른 채널을 구독하기 바랍니다.

orders#${symbol}。<br>

### 구독 주제

`trade.clearing#${symbol}#${mode}`

### 구독 파라미터

| 매개 변수 | 유형   | 필수 여부 | 설명                                                         |
| --------- | ------ | --------- | ------------------------------------------------------------ |
| symbol    | string | TRUE      | 거래 코드(와일드카드* 지원)                                  |
| mode      | int    | FALSE     | 푸시 모드(0 - 주문이 체결 되는 경우에만 푸시; 1 - 주문이 체결/철회 되는 경우에 전부 푸시; 디폴트:0 ) |

주의:<br>

매개 변수 mode를 입력하지 않거나 0을 입력한 경우 체결 이벤트만 푸시 합니다; 만약 1을 입력하면 체결 및 철회 이벤트를 푸시 합니다.<br>

> Subscribe request

```json
{
	"action": "sub",
	"ch": "trade.clearing#btcusdt#0"
}

```

> Response

```json
{
	"action": "sub",
	"code": 200,
	"ch": "trade.clearing#btcusdt#0",
	"data": {}
}
```

> Update example

```json
{
    "ch": "trade.clearing#btcusdt#0",
    "data": {
         "eventType": "trade",
         "symbol": "btcusdt",
         "orderId": 99998888,
         "tradePrice": "9999.99",
         "tradeVolume": "0.96",
         "orderSide": "buy",
         "aggressor": true,
         "tradeId": 919219323232,
         "tradeTime": 998787897878,
         "transactFee": "19.88",
         "feeDeduct ": "0",
         "feeDeductType": "",
         "feeCurrency": "btc",
         "accountId": 9912791,
         "source": "spot-api",
         "orderPrice": "10000",
         "orderSize": "1",
         "clientOrderId": "a001",
         "orderCreateTime": 998787897878,
         "orderStatus": "partial-filled"
    }
}
```

### 데이터 업그레이드 파라미터 리스트(주문이 체결된 후)

| 매개변수        | 유형   | 설명                                                         |
| --------------- | ------ | ------------------------------------------------------------ |
| eventType       | string | 이벤트 유형（trade）                                         |
| symbol          | string | 거래 코드                                                    |
| orderId         | long   | 주문ID                                                       |
| tradePrice      | string | 체결 가격                                                    |
| tradeVolume     | string | 체결량                                                       |
| orderSide       | string | 주문 방향, 유효값: buy, sell                                 |
| orderType       | string | 주문 유형, 유효값:buy-market, sell-market,buy-limit,sell-limit,buy-ioc,sell-ioc,buy-limit-maker,sell-limit-maker,buy-stop-limit,sell-stop-limit |
| aggressor       | bool   | 주동 거래측 여부, 유효값: true, false                        |
| tradeId         | long   | 거래 ID                                                      |
| tradeTime       | long   | 체결 시간, unix time in millisecond                          |
| transactFee     | string | 거래 수수료(양수) 혹은 거래수수료 페이백(음수)               |
| feeCurrency     | string | 거래 수수료 혹은 거래 수수료 페이백 코인(매수 주문 거래 수수료 코인은 거래 화폐이고 매도 주문 거래 수수료 코인은 마켓 화폐; 매수 주문 거래 수수료 페이백 코인은 마켓 화폐이고 매도 주문 거래 수수료 페이백 코인은 거래 화폐입니다) |
| feeDeduct       | string | 거래 수수료 삭감                                             |
| feeDeductType   | string | 거래 수수료 삭감 유형, 유효값:ht, point                      |
| accountId       | long   | 계정번호                                                     |
| source          | string | 주문서 소스                                                  |
| orderPrice      | string | 주문 가격(시장가 주문은 이 필드가 없습니다)                  |
| orderSize       | string | 주문 수량(시장가 매수 주문은 이 필드가 없습니다)             |
| orderValue      | string | 주문 금액(시장가 매수 주문인 경우에만 이 필드가 있습니다)    |
| clientOrderId   | string | 유저가 정한 주문 번호                                        |
| stopPrice       | string | 주문 유발 가격(스탑리밋 주문에만 이 필드가 있습니다)         |
| operator        | string | 주문 유발 방향(스탑리밋 주문에만 이 필드가 있습니다)         |
| orderCreateTime | long   | 주문 생성 시간                                               |
| orderStatus     | string | 주문 상태, 유효값:filled, partial-filled                     |

주의:<br>

- transactFee에서 거래 페이백 금액은 실시간으로 입금되지 않을 수 있습니다;<br>

### 데이터 업그레이드 파라미터 리스트(주문 철회 후)

| 매개 변수       | 유형   | 설명                                                         |
| :-------------- | :----- | :----------------------------------------------------------- |
| eventType       | string | 이벤트 유형(cancellation)                                    |
| symbol          | string | 거래 코드                                                    |
| orderId         | long   | 주문ID                                                       |
| orderSide       | string | 주문 방향, 유효값: buy, sell                                 |
| orderType       | string | 주문 유형, 유효값:buy-market, sell-market,buy-limit,sell-limit,buy-ioc,sell-ioc,buy-limit-maker,sell-limit-maker,buy-stop-limit,sell-stop-limit |
| accountId       | long   | 계정 번호                                                    |
| source          | string | 주문 소스                                                    |
| orderPrice      | string | 주문 가격(시장가 주문은 이 필드가 없습니다)                  |
| orderSize       | string | 주문 수량(시장가 매수 주문은 이 필드가 없습니다)             |
| orderValue      | string | 주문 금액(시장가 매수 주문에만 이 필드가 있습니다)           |
| clientOrderId   | string | 유저가 정한 주문 번호                                        |
| stopPrice       | string | 주문 유발 가격(스탑리밋 주문에만 이 필드가 있습니다)         |
| operator        | string | 주문 유발 방향(스탑리밋 주문에만 이 필드가 있습니다)         |
| orderCreateTime | long   | 주문 생성 시간                                               |
| remainAmt       | string | 미 체결량(시장가 매수 주문인 경우 이 필드는 미체결금액 입니다) |
| orderStatus     | string | 주문 상태, 유효값:canceled, partial-canceled                 |

## 구독 계정 변경

API Key 권한: 읽기

구독 계정에서 주문 업데이트

### 구독 주제

```
accounts.update#${mode}
```

다음 계정 변경 푸시 방법 중에서 임의로 선택 가능합니다.

1. 계정 잔액에 변동이 있을 경우에만 푸시합니다.
2. 계정 잔액에 변동이 있거나 사용 가능한 잔액이 변동되는 경우 푸시하고 별개로 푸시합니다.

### 구독 매개 변수

| 매개 변수 | 데이터 유형 | 설명                               |
| --------- | ----------- | ---------------------------------- |
| mode      | integer     | 푸시 방식, 유효값: 0, 1. 기본값: 0 |

구독 예시  

1.mode 요청하지 않음:
accounts.update 
계정 잔액이 변동 있는 경우에만 푸시;
2.mode=0으로 요청:
accounts.update#0 

계정 잔액에 변동 있는 경우에만 푸시;
3.mode=1으로 요청：  
accounts.update#1  
계정 잔액에 변동이 있거나 사용 가능한 잔액에 변동이 생기는 경우 푸시하고 별개로 푸시;  

주의: 유저의 구독 모드에 관계없이 구독 성공 후, 서버는 우선 현재 각 계정의 계정잔액과 사용가능 잔액을 푸시하고 그다음 계정 업그레이드를 푸시 합니다. 처음 각 계정 초기값을 푸시하는 경우 메시지에 changeType과 changeTime 값은 null 입니다.

> Subscribe request

```json
{
  "action": "sub",
  "ch": "accounts.update"
}
```

> Response

```json
{
  "action": "sub",
  "code": 200,
  "ch": "accounts.update#0",
  "data": {}
}
```

> Update example

```json
accounts.update#0：
{
  "action": "push",
  "ch": "accounts.update#0",
  "data": {
    "currency": "btc",
    "accountId": 123456,
    "balance": "23.111",
    "changeType": "transfer",
            "accountType":"trade",
    "changeTime": 1568601800000
  }
}

accounts.update#1：
{
  "action": "push",
  "ch": "accounts.update#1",
  "data": {
    "currency": "btc",
    "accountId": 33385,
    "available": "2028.699426619837209087",
    "changeType": "order.match",
            "accountType":"trade",
    "changeTime": 1574393385167
  }
}
{
  "action": "push",
  "ch": "accounts.update#1",
  "data": {
    "currency": "btc",
    "accountId": 33385,
    "balance": "2065.100267619837209301",
    "changeType": "order.match",
            "accountType":"trade",
    "changeTime": 1574393385122
  }
}
```

### 데이터 업데이트 필드 리스트

| 매개 변수   | 데이터 유형 | 설명                                                         |
| ----------- | ----------- | ------------------------------------------------------------ |
| currency    | string      | 코인 종류                                                    |
| accountId   | long        | 계정ID                                                       |
| balance     | string      | 계정 잔액(계정 잔액에 변동이 있는 경우에만 푸시)             |
| available   | string      | 사용 가능 잔액(사용 가능 잔액이 변화 되는 경우에만 푸시)     |
| changeType  | string      | 잔액 변동 유형, 유효값: order-place(주문서 생성)，order-match(주문서 체결), order-refund(주문서 체결 환불)，order-cancel(주문 철회), order-fee-refund(거래 수수료를 수수료 쿠폰으로 삭감)，other(기타 자산 변동) |
| accountType | string      | 계정 유형, 유효값: trade, frozen, loan, interest             |
| changeTime  | long        | 잔액 변동 시간, unix time in millisecond                     |

주의:<br>
계정 업그레이드는 입금 금액을 푸시 합니다, 여러 개 체결로 생성한 페이백은 통합하여 입금될 수 있습니다.<br>

<br>
<br>
<br>
<br>
<br>
