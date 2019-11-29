---
title: 후오비 코리아 API 문서

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://www.huobi.kr/ko-kr/api/'>API Key 생성</a>
includes:

search: false
---

# 1. 소개

### API 소개

안녕하세요. 후오비 코리아 API 안내입니다.
귀하는 API를 활용하여 후오비 코리아의 모든 마켓 데이터 및 거래, 계정 관리 Endpoints에 접근할 수 있습니다.

API key 발급 신청과 변경은 API 관리 메뉴를 통해 진행할 수 있으며 경로는 다음과 같습니다. 
> 후오비 코리아(www.huobi.co.kr) 로그인 > 마이페이지 > API 관리
API 관련하여 궁금증 또는 문의사항이 있을 경우 ‘자주 묻는 질문(FAQ)’을 참고하거나 ‘후오비 코리아 API 커뮤니티 텔레그램(http://bit.ly/2jXMzEN) 채팅방’을 통해 상담 받을 수 있습니다.

   

## 마켓 메이커 프로그램

<font color="#d73e48">우수한 Maker 전략으로 많은 거래량이 많은 회원님들께서 장기적으로 마켓 메이커 프로젝트에 참여하는 것을 환영합니다.</font>

1. 신청 조건
    - 후오비 코리아 계정에 30 BTC 이상 자산 보유자(BTC 환산 기준)
1. 신청 방법
    - 다음 3가지 내용을 작성하여 <span style="color:#d73e48">[MM-kr@huobi.com](mailto:MM-kr@huobi.com)</span> 이메일 주소로 보내주시기 바랍니다.
    - 마켓 메이커 신청 시 작성 항목
      1. 후오비 코리아 UID: 페이백 또는 레퍼럴과 관련되어 있지 않은 UID
      1. 기타 거래 플랫폼 Maker 거래량 캡쳐 화면: 예) 30일간 체결량 등
      1. 마켓 메이킹 진행 방안에 대해 간단히 기재해주세요. (구체적인 내용은 작성하지 않으셔도 됩니다.) 
1. 유의사항
    - 마켓 메이커 프로그램은 수수료 쿠폰 포인트 차감 및 거래량 관련 이벤트 등 어떠한 페이백 이벤트도 지원하지 않습니다.
    - 하위 계정은 기타 API에 접속할 수 없습니다. 접속 시도 시 시스템은 "error-code 403"를 반환합니다.
    

---
title: api_access
---

# 2. 액세스 설명
## Access URLs

1. REST API: <font color="#d73e48">wss://api-cloud.huobi.co.kr</font>
1. Websocket Feed(시세): <font color="#d73e48">wss://api-cloud.huobi.co.kr/ws</font>
1. Websocket Feed(자산 주문내역): <font color="#d73e48">wss://api-cloud.huobi.co.kr/ws/v1</font>

* 유의사항
<pre>
-	중국 이외의 IP를 통해 후오비 코리아 API로 접속해주시기 바랍니다.
- Proxy를 사용하여 접속 시 지연 시간이 길어지고 안정성이 저하될 수 있으므로 Proxy 접속 방식은 권장하지 않습니다.
</pre>


## 빈도 제한 규정
- 현물(api-cloud.huobi.co.kr) : 10초 이내에 최대 100 개의 https 요청을 보낼 수 있습니다. 더 높은 한계 속도가 필요하다고 생각되면 고객 지원에 문의하십시오. 
- API 통신을 무단 변경으로부터 보호하기 위해, 모든 비공용 API 호출에 서명이 필요합니다.

<pre>
단일 API Key 차원 제한. 시세 API 액세스는 서명이 필요없습니다.
</pre>



## 서명 인증
API 요청은 인터넷을 통한 전송 중 변조될 가능성이 있습니다.
Public API(기본 정보, 시세 데이터)를 제외한 Private API는 요청이 변조되지 않도록 하기 위해 반드시 귀하의 API Key로 서명 인증을 진행하여 매개 변수 또는 매개 변수 값이 변경되었는지 확인해야 합니다.
새로 생성된 모든 API Key에는 권한이 적용되어야 하고, 각 API Key는 해당 API에 적합한 권한이 있어야만 요청이 가능합니다.
권한 유형은 조회, 거래, 코인 출금으로 구분됩니다. 
API를 요청하기 전에 각 API 권한 유형을 보시고 회원님의 API Key에 적합한 권한을 확인해주시기 바랍니다. 

합법적인 요청은 다음과 같은 부분으로 구성됩니다.

- 메소드 요청 주소: 서버 접속 주소 api-cloud.huobi.co.kr, 예) api-cloud.huobi.co.kr/v1/order/orders.
- API Access Key(AccessKeyId): 회원님이 신청한 API Key에 대한 Access Key.
- 서명 방법(SignatureMethod): 회원님이 서명을 계산할 수 있는 Hash 기반의 프로토콜로 HmacSHA256를 사용합니다.
- 서명 버전(SignatureVersion): 서명 프로토콜 버전으로 API는 2를 사용합니다.
- 타임 스탬프(Timestamp): 회원님이 요청한 시간(UTC 시간대), 예) 2017-05-11 T16:22:06. 
쿼리 요청 시 해당 값을 포함하면 제3자가 회원님의 요청을 가로챌 수 없습니다.
- 필수 및 선택적 매개 변수: 각 메소드에는 API 호출 정의를 위한 필수 및 선택적 매개 변수 세트가 있습니다. 각 방법의 설명에서 이러한 매개 변수와 의미를 확인할 수 있습니다. 주의할 사항은 GET 방식으로 요청할 경우 각 메소드와 함께 제공되는 매개 변수에 서명 인증을 진행해야 합니다. POST 요청의 경우 각 메소드의 매개 변수는 서명 인증을 진행하지 않습니다. 즉, POST 요청 시 필요한 매개 변수는 AccessKeyId, SignatureMethod, SignatureVersion, Timestamp 총 4개 입니다. 나머지 매개 변수는 body에 배치됩니다.
- 서명: 유효하고 변조되지 않았음을 보장하기 위해 서명에 의해 계산된 값입니다.


## API Key 생성

본 <a href="https://www.huobi.co.kr/api">링크(https://www.huobi.co.kr/api)</a>로 접속하여 API Key를 발급 받을 수 있습니다.

API Key는 다음 2가지로 구성됩니다.

- `Access Key`: API Access Key
- `Secret Key`: 서명 인증 암호화에 사용되는 Key (신청 시에만 확인할 수 있습니다.)

<pre>
API Key 발급 시 IP 주소를 연동할 수 있습니다. IP 주소를 연동하지 않은 API Key의 유효기간은 90일입니다.
API Key는 거래, 코인 입출금 등을 포함한 모든 작업 권한을 가지고 있습니다.
이 2개의 키는 보안상 절대 타인에게 공개하시면 안됩니다.
</pre>


1. ### 서명 절차
    1) 서명 계산 시 유의사항
        - 서명 계산 진행 시 먼저 요청 전에 반드시 절차를 지켜주세요. 다음은 어떤 주문 건의 상세내역을 조회하는 예시입니다.

    2) 주문 건 상세내역 조회
    ```js
    https://api-cloud.huobi.co.kr/v1/order/orders? 
    AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
    &SignatureMethod=HmacSHA256
    &SignatureVersion=2
    &Timestamp=2017-05-11T15:19:30
    &order-id=1234567890
    ```

    #### 1. 요청 메소드(GET 또는 POST) 입력 후 줄바꿈 개행 문자 "\n" 추가해주세요.
    <pre>
    GET\n
    </pre>

    #### 2. 소문자로 접속 주소 입력 후 줄바꿈 개행 문자 "\n" 추가해주세요.
    <pre>
    api-cloud.huobi.co.kr\n
    </pre>

    #### 3. Access 메소드 입력 후 줄바꿈 개행 문자 "\n" 추가해주세요.
    <pre>
    /v1/order/orders\n
    </pre>

    #### 4. ASCII 코드 순서에 따라 매개 변수 명칭을 정렬해주세요.
    > 예) 인코딩 진행 후 매개 변수 정렬은 다음과 같습니다.

    ```js
    AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
    order-id=1234567890
    SignatureMethod=HmacSHA256
    SignatureVersion=2
    Timestamp=2017-05-11T15%3A19%3A30
    ```
    
    - UTF-8 방식으로 인코딩되고 URI 인코딩 됩니다. 16진수 문자는 반드시 대문자로 진행되어야 합니다.
    예를 들어 ":"는 "%3A"로 인코딩되고 빈칸은 "%20"으로 인코딩됩니다. 
    타임 스탬프(Timestamp)는 YYYY-MM-DDThh:mm:ss 형식 및 URI 인코딩으로 추가해야 합니다.

    #### STEP5. 정렬 완료 이후
    ```js
    AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
    SignatureMethod=HmacSHA256
    SignatureVersion=2
    Timestamp=2017-05-11T15%3A19%3A30
    order-id=1234567890
    ```

    #### STEP6. 위의 순서대로 각 매개 변수를 문자 “&”로 연결해주세요.
    ```js
    AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
    SignatureMethod=HmacSHA256&SignatureVersion=2
    Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890
    ```

    #### STEP7. 계산을 위해 서명할 마지막 문자열은 다음과 같습니다.
    ```js
    GET\n
    api-cloud.huobi.co.kr\n
    /v1/order/orders\n
    AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
    SignatureMethod=HmacSHA256&SignatureVersion=2
    Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890
    ```

    #### STEP8. 이전 단계에서 생성된 “요청 문자열”과 회원님 Key(Secret Key)를 사용하여 디지털 서명 생성해주세요.
    ```js
    4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o=
    ```

    - 위에서 발생한 요청 문자열과 API 개인 Key를 2개의 매개 변수로 사용하여 HmacSHA256 해시 함수를 호출후 해시 값을 얻습니다.
    - 해시 값은 base-64로 인코딩되고 생성된 값은 API 호출을 위한 디지털 서명으로 사용됩니다.

    #### STEP9. 생성된 디지털 서명을 요청 path 매개 변수에 추가해주세요.

    최종적으로 서버에 전송된 API 요청은 다음과 같습니다.

    ```js
    https://api-cloud.huobi.co.kr/v1/order/orders?
    AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&order-id=1234567890&
    SignatureMethod=HmacSHA256&
    SignatureVersion=2&
    Timestamp=2017-05-11T15%3A19%3A30&
    Signature=4F65x5A2bLyMWVQj3Aqp%2BB4w%2BivaA7n5Oi2SuYtCJ9o%3D
    ```

    - 필요한 모든 인증 매개 변수를 API 호출의 경로 매개 변수에 추가해주세요. 
    - URL 인코딩 후 path 매개 변수에 디지털 서명을 추가하십시오. 매개 변수 이름은 "Signature"입니다.

### 요청 형식

모든 API 요청은 GET 또는 POST로 전송됩니다. GET 요청의 경우 모든 매개 변수는 path 매개 변수에 있습니다. POST 요청의 경우 모든 매개 변수는 요청 본문(body)에 JSON 형식으로 적용합니다.

### 응답 형식

모든 API 응답은 JSON 형식입니다. JSON 상단에는 요청 상태와 속성을 나타내는 필드 "status", "ch", "ts"가 있으며 실제 응답은 "data"필드에 있습니다.

### 응답 내용 형식

> 응답 내용의 양식은 다음과 같습니다.

```json
{
"status": "ok", 
"ch": "market.btcusdt.kline.1day", 
"ts": 1499223904680, 
"data": // per API response data in nested JSON object 
}
```

| 매개 변수 | 데이터 유형 | 설명                                                         |
| -------- | ----------- | ------------------------------------------------------------ |
| status   | string      | API 응답 상태                      |
| ch       | string      | API 데이터에 해당하는 데이터 스트림. 일부 API에는 해당 데이터 스트림이 없으므로 이 필드를 반환하지 않습니다. |
| ts       | int         | API가 응답한 베이징 시간으로 조정된 타임 스탬프(단위: ms(millisecond)) |
| data     | object      | API 응답 데이터                    |

## 오류 정보

### 1) 마켓 API 오류 정보

| 오류 코드         | 설명          |
| ----------------- | ------------- |
| bad-request       | 잘못된 요청     |
| invalid-parameter | 매개 변수 오류 |
| invalid-command   | 명령 오류     |

> 코드에 대한 자세한 설명은 해당`err-msg`를 참조하십시오.

### 2) 거래 API 오류 정보

| 오류 코드                                                    | 설명                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| base-symbol-error                                            | 존재하지 않는 거래쌍입니다.                                  |
| base-currency-error                                          | 존재하지 않는 코인입니다.                                    |
| base-date-error                                              | 일자 형식 오류                                               |
| account for id `12,345` and user id `6,543,210` does not exist | `account-id` 오류입니다. GET `/v1/account/accounts` API를 이용하여 조회해주세요. |
| account-frozen-balance-insufficient-error                    | 잔액 부족                                                    |
| account-transfer-balance-insufficient-error                 | 이체할 잔액 부족                          |
| bad-argument                                                 | 잘못된 매개 변수                                |
| api-signature-not-valid                                      | API 서명 오류                                                |
| gateway-internal-error                                       | 시스템이 사용 중입니다. 잠시 후 다시 시도해주세요.                 |
| ad-ethereum-addresss                                         | 유효한 이더리움 주소를 입력해주세요.                         |
| order-accountbalance-error                                   | 계정 잔액 부족                                               |
| order-limitorder-price-error                                 | 지정가 주문 가격 제한을 초과하였습니다.                      |
| order-limitorder-amount-error                                | 지정가 주문 수량 제한을 초과하였습니다.                      |
| order-orderprice-precision-error                             | 주문 가격이 소수점 자릿수 제한을 초과하였습니다.               |
| order-orderamount-precision-error                            | 주문 수량이 소수점 자릿수 제한을 초과하였습니다.               |
| order-marketorder-amount-error                               | 주문 수량 제한을 초과였습니다.                             |
| order-queryorder-invalid                                     | 해당 주문은 존재하지 않습니다.                     |
| order-orderstate-error                                       | 주문 상태 오류                                               |
| order-datelimit-error                                        | 조회 가능한 제한 시간을 초과하였습니다.                          |
| order-update-error                                           | 주문 업데이트 오류                                         |

##  SDK 및 코드 예시

**SDK（추천）**

[Java](https://github.com/HuobiRDCenter/huobi_Java)

[Python3](https://github.com/HuobiRDCenter/huobi_Python)

[C++](https://github.com/HuobiRDCenter/huobi_Cpp)

**다른 코드 예시**

https://github.com/huobiapi?tab=repositories

* ## 자주 묻는 질문 Q & A

  ### 1) 연결이 끊기거나 데이터 분실의 경우

  - Japan Cloud 서버를 사용해 주세요.

  ### 2) 서명 실패의 경우

  - API Key의 유효성 여부, 복사된 값의 정확성 여부, IP의 화이트 리스트 여부를 확인해주세요.
  - 타임 스탬프(Time Stamp)가 UTC 시간으로 설정되어 있는지 확인해주세요.
  - Parameter가 문자 순서에 따라 정렬되었는지 확인해주세요.
  - 코딩을 확인해주세요.
  - 서명이 base64 코딩인지 확인해주세요.
  - Get이 form으로 제출되었는지 확인해주세요.
  - Post의 URL이 서명 필드를 보유했는지 확인해주세요.
  - Post의 데이터 형식이 json 형식인지 확인해주세요.
  - 서명 결과를 URI코딩인지 확인해 주세요.

  ### 3) Login-required로 응답된 경우

  - Accountid가 Accounts 인터페이스로 응답 되었는지 확인해주세요.
  - Get 요청이 Parameter를 ASCII 코드 표 순서대로 정렬했는지 확인해주세요.

  ### 4) gateway-internal-error 응답된 경우

  - POST 요청이 header에서 Content-Type:application/json 설정된 것인지 확인해주세요.


---
title: reference_data
---

# 3. 기본 정보

## 모든 거래쌍 정보 수집

본 API는 후오비 코리아에서 지원하는 모든 거래쌍 정보를 반환합니다.

```shell
curl "https://api-cloud.huobi.co.kr/v1/common/symbols"
```

### HTTP 요청

- GET `/v1/common/symbols`

### 요청 매개 변수

본 API는 매개 변수가 필요하지 않습니다.

> Response:

```json
  "data": [
    {
        "base-currency": "btc",
        "quote-currency": "usdt",
        "price-precision": 2,
        "amount-precision": 4,
        "symbol-partition": "main",
        "symbol": "btcusdt"
    }
    {
        "base-currency": "eth",
        "quote-currency": "usdt",
        "price-precision": 2,
        "amount-precision": 4,
        "symbol-partition": "main",
        "symbol": "ethusdt"
    }
  ]
```

### 응답 데이터

| 매개 변수        | 데이터 유형 | 설명                                                  |
| ---------------- | ----------- | ----------------------------------------------------- |
| base-currency    | string      | 기본 통화의 심볼명                                    |
| quote-currency   | string      | 기축 통화의 심볼명                                    |
| price-precision  | integer     | 코인 가격의 소수점 자릿수                             |
| amount-precision | integer     | 코인 수량의 소수점 자릿수                             |
| symbol-partition | string      | 거래 범위, 가능한 값: [main, innovation, bifurcation] |


## 모든 코인 회득

이 API에서 모든 후오비 코리아가 지원하는 코인이 반환됩니다. 

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

<font color="#d73e48">반환 된 "data" 개체는 각각 지원되는 통화를 나타내는 문자열 배열입니다.</font>

## 현재 시스템 시간 획득

본 API는 현재 시스템 시간 정보(UTC/GMT+08:00)가 반환됩니다. *시간 단위: ms(millisecond)

```shell
curl "https://api-cloud.huobi.co.kr/v1/common/timestamp"
```

### HTTP 요청

- GET `/v1/common/timestamp`

### 요청 매개 변수

이 API에서 모든 파라미터를 받지 않습니다.

> Response:

```json
  "data": 1494900087029
```

### 응답 데이터

반환된 "data"는 정수이며, ms(millisecond) 단위의 UTC/GMT+08:00 타임 스탬프 값입니다.


---
title: market_data
---

# 4. 시세 데이터

## K-line 차트 정보(캔들 도표)

본 API는 K-라인의 내역을 반환합니다.

### HTTP 요청

- GET `/market/history/kline`

```shell
curl "https://api-cloud.huobi.co.kr/market/history/kline?period=1day&size=200&symbol=btcusdt"
```

### 요청 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명                                                        | 값의 범위                                                 |
| -------- | ----------- | --------- | ------ | ----------------------------------------------------------- | --------------------------------------------------------- |
| symbol   | string      | true      | N/A     | 거래쌍                                                      | btcusdt, ethbtc 등  |
| period   | string      | true      | N/A     | 각 차트의 시간 간격의 단위를 반환합니다. | 1min, 5min, 15min, 30min, 60min, 1day, 1mon, 1week, 1year |
| size     | integer     | false     | 150    | 반환한 K챠트 개수                                           | [1, 2000]                                                 |

<font color="#d73e48">현재 REST API는 UTC/GMT+08:00만 지원합니다. 만약 기존 지정한 시간대 데이터가 필요하시면 Websocket API 중의 챠트 API 참고 하세요.</font>


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
| id        | integer     | UTC/GMT+08:00의 타임 스탬프로 조정(단위: 초)하고 K-라인의 아이디로 사용. |
| amount    | float       | 기본 통화 기준으로 측정된 거래량                                   |
| count     | integer     | 거래 건수                                                    |
| open      | float       | 이 단계 시작가                                               |
| close     | float       | 이 단계의 종가                                                 |
| low       | float       | 이 단계 최저가                                               |
| high      | float       | 이 단계 최고가                                               |
| vol       | float       | 기축 통화 기준으로 측정하는 거래량                                 |



## 시세 집계（Ticker）

본 API는 시세 정보를 수집하여 지난 24시간의 거래 집계 정보를 제공합니다.

### HTTP 요청

- GET `/market/detail/merged`

```shell
curl "https://api-cloud.huobi.co.kr/market/detail/merged?symbol=ethusdt"
```

### 요청 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명   | 값의 범위       |
| -------- | ----------- | --------- | ------ | ------ | --------------- |
| symbol   | string      | true      | N/A     | 거래쌍 | btcusdt, ethbtc 등 |

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
| id        | integer     | N/A                                     |
| amount    | float       | 기본 통화 기준으로 측정된 거래량             |
| count     | integer     | 거래 건수                              |
| open      | float       | 이 단계의 시작가                         |
| close     | float       | 이 단계의 종가                           |
| low       | float       | 이 단계 최저가                         |
| high      | float       | 이 단계 최고가                         |
| vol       | float       | 기축 통화 기준으로 측정하는 거래량           |
| bid       | object      | 현재 최고 매도가 [price, quote volume] |
| ask       | object      | 현재 최저 매수가 [price, quote volume] |


## 모든 거래쌍의 최신 시세

- 24시간 간격으로 모든 거래쌍의 시세 수집
- 본 API는 모든 거래쌍에 대한 시세 정보를 반환하므로 데이터 양이 많습니다.

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
        "high":0.045255,      // 최저가
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

매개 변수      | 데이터 유형 | 설명
--------- | --------- | -----------
amount    | float     | 기본 통화 기준으로 측정된 거래량 
count     | integer   | 거래 건수 
open      | float     | 이 단계의 시작가
close     | float     | 이 단계의 종가
low       | float     | 이 단계 최저가
high      | float     | 이 단계 최고가
vol       | float     | 기축 통화 기준으로 측정하는 거래량 
symbol    | string    | 거래쌍 예) btcusdt, ethbtc

## Market 누적주문량 정보

본 API에서 지정된 거래쌍에 대한 현재 Market 누적주문량 정보를 반환합니다.

### HTTP 요청

- GET `/market/depth`

```shell
curl "https://api-cloud.huobi.co.kr/market/depth?symbol=btcusdt&type=step2"
```

### 요청 매개 변수

매개 변수      | 데이터 유형 | 필수 여부 | 기본값 | 설명 | 값의 범위 |
--------- | --------- | -------- | ------| ---- | --- | --- 
symbol    | string    | true     | N/A    | 거래쌍 | btcusdt, ethbtc 등 |
depth     | integer   | false    | 20    | 누적 주문량 반환 | 5，10，20 |
type      | string    | true     | step0 | 누적주문량 가격 집계 (자세한 내용은 아래 참고) | step0，step1，step2，step3，step4，step5 |

type값이 ‘step0’인 경우，‘depth’의 기본값이 20이 아니고 150입니다.

**매개 변수 유형의 각 값에 대한 설명**

값      | 설명 
--------- | ---------
step0     | 집계하지 않음 
step1     | 집계 수준 = 정밀도 * 10
step2     | 집계 수준 = 정밀도 * 100
step3     | 집계 수준 = 정밀도 * 1000
step4     | 집계 수준 = 정밀도 * 10000
step5     | 집계 수준 = 정밀도 * 100000

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

반환된 JSON 최상위 데이터 개체의 이름은 일반적인 'data'대신 'tick'입니다.

매개 변수      | 데이터 유형  | 설명
--------- | --------- | -----------
ts        | integer   | UTC/GMT+08:00의 타임 스탬프로 조정 (단위: ms(milliseconds)) 
version   | integer   | 내부 필드 
bids      | object    | 현재 모든 매수 주문 
asks      | object    | 현재 모든 매도 주문

## 최근 Market 거래 기록

본 API는 지정된 거래쌍에 대한 최신 거래 기록을 반환합니다.

### HTTP 요청

- GET `/market/trade`

```shell
curl "https://api-cloud.huobi.co.kr/market/trade?symbol=ethusdt"
```

### 요청 매개 변수

매개 변수      | 데이터 유형 | 필수 여부 | 기본값 | 설명
--------- | --------- | -------- | ------- | -----------
symbol    | string    | true     | N/A      | 거래쌍，예) btcusdt, ethbtc 

> Response:

```json
{
    "id": 600848670,
    "ts": 1489464451000,
    "data": [
      {
        "id": 600848670,
        "price": 7962.62,
        "amount": 0.0122,
        "direction": "buy",
        "ts": 1489464451000
      }
    ]
}
```

### 응답 데이터

반환된 JSON 최상위 데이터 개체의 이름은 일반적인 'data'대신 'tick'입니다.
매개 변수       | 데이터 유형 | 설명
--------- | --------- | -----------
id        | integer   | 유일한 거래 id 
amount    | float     | 거래화폐을 단위으로 하는 거래량 
price     | float     | 마켓화폐을 단위으로 하는 체결가격 
ts        | integer   | UTC/GMT+08:00의 타임 스탬프으로 조정, 단위는 밀리초. 
direction | string    | 거래방향：“buy”  혹은 “sell”, “buy”는 매수，“sell” 는 매도. 

## 근일 체결 기록

이 API에서 지정한 거래쌍의 근일 모든 체결 기록이 반환 됩니다. 

### HTTP 요청

- GET `/market/history/trade`

```shell
curl "https://api-cloud.huobi.co.kr/market/history/trade?symbol=ethusdt&size=2"
```

### 요청 매개 변수

 매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명                                 
--------- | --------- | -------- | ------- | -----------
symbol    | string    | true     | N/A      | 거래쌍，예) btcusdt, ethbtc 
size      | integer   | false    | 1       | 반환되는 체결 기록수, 최대값 2000 

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
            "id":3161878751418918529341,
            "price":94.690000000000000000,
            "direction":"sell"
         },
         {  
            "amount":73.771000000000000000,
            "ts":1544390317905,
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
            "id":3161877698918918522622,
            "price":94.710000000000000000,
            "direction":"buy"
         }
      ]
   }
]
```

### 응답 데이터

반환한 데이터 대상은 하나의 object array이고 각 array의 요소는 UTC/GMT+08:00의 타임 스탬프(단위는 밀리초)으로 조정한 모든 체결기록이며 이런 체결 기록은 array 형식으로 나타납니다.

매개 변수      | 데이터 유형 | 설명
--------- | --------- | -----------
id        | integer   | 고유 거래 ID 
amount    | float     | 기본 통화 기준으로 측정된 거래량 
price     | float     | 기축 통화를 단위로 하는 체결 가격 
ts        | integer   | UTC/GMT+08:00의 타임 스탬프로 조정 (단위: ms(milliseconds))
direction | string    | 거래: “buy” 또는 “sell”, “buy” 매수，“sell” 매도 

## 최근 24시간 시세 데이터

본 API는 최근 24시간 시세 데이터의 합계를 반환합니다.

### HTTP 요청

- GET `/market/detail`

```shell
curl "https://api-cloud.huobi.co.kr/market/detail?symbol=ethusdt"
```

### 요청 매개 변수

 매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명                              
--------- | --------- | -------- | ------- | -----------
symbol    | string    | true     | N/A      | 거래쌍，예)  btcusdt, ethbtc 

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

반환된 JSON 최상위 데이터 개체의 이름은 일반적인 'data'대신 'tick'입니다.

매개 변수      | 데이터 유형 | 설명
--------- | --------- | -----------
id        | integer   | 고유 거래 ID
amount    | float     | 기본 통화 기준으로 측정된 거래량 
count     | integer   | 거래 건수 
open      | float     | 이 단계의 시작가 
close     | float     | 이 단계의 종가 
low       | float     | 이 단계 최저가 
high      | float     | 이 단계 최고가 
vol       | float     | 기축 통화 기준으로 측정하는 거래량 
version   | integer   | 내부 데이터 



---
title: account
---

# 5. 계정 관련

Access 계정과 연결된 API에는 서명 인증이 필요합니다.

## 계정 정보 

- API Key 권한：읽기

- 사용자`account-id`의 모든 계정 ID 및 관련 정보를 조회합니다.

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
    {
      "id": 100002,
      "type": "margin",
      "subtype": "btcusdt",
      "state": "working"
    },
    {
      "id": 100003,
      "type": "otc",
      "subtype": "",
      "state": "working"
    }
  ]
}
```

### 응답 데이터

| 매개 변수 | 데이터 유형 | 필수 여부 | 설명 | 값의 범위 |
| ----- | ---- | ------ | ----- | ----  |
| id    | long | true | account-id |    |
| state | string | true | 계정 상태 | working：정상, lock：계정이 잠금됨. |
| type  | string | true | 계정 유형 | spot: 자산 계정, otc: C2C 계정, point: 포인트 카드계정 |



## 계정 잔액

- API Key 권한：읽기

- 지정한 계정의 잔액 조회 가능합니다. 아래 계정 지원:

- spot：현물 계정, otc：C2C 계정, point：포인트 카드 계정

### HTTP 요청

- GET `/v1/account/accounts/{account-id}/balance`

### 요청 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 설명   |
| ---------- | ---- | ------ | --------------- | ---- | ---- |
| account-id | string | true | account-id을 path에 작성 ，GET /v1/account/accounts 으로 회득 가능함. |

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
    ],
    "user-id": 10000
  }
}
```

### 응답 데이터

| 매개 변수 | 데이터 유형 | 필수 여부 | 설명    | 값의 범위  |
| ----- | ----- | ------ | ----- | ----- |
| id    | long  | true   | 계정 ID |      |
| state | string  | true | 계정 상태 | working：정상, lock： 계정 잠금 처리 |
| type  | string  | true | 계정 유형 | spot: 현물 계정, otc: C2C 계정, point: 포인트 카드 계정 |

list 필드 설명

| 매개 변수 | 데이터 유형 | 필수 여부 | 설명   | 값의 범위 |
| :------- | ---- | ------ | ---- |  :----- |
| balance  | true | string | 잔액 |    |
| currency | true | string | 코인 |    |
| type     | true | string | 유형 | trade: 거래 잔액，frozen: 동결 자산 |


---
title: wallet
---

# 6. 지갑（입금과 출금）

<font color="red">지갑 관련 API 액세스하려면 서명 인증이 필요합니다.</font>

## 암호화폐 출금

- API Key 권한：출금
- 후오비 코리아 <a href='https://www.huobi.co.kr/ko-kr/address/'>웹사이트</a>에서 지원하는 코인 주소만 지원합니다.

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

| 매개 변수  | 유형 | 필수 여부 | 설명     |값의 범위 |
| ---------- | ---- | ------ | ------ | ---- |
| address | string | true   | 출금 주소 | 웹사이트에서 지원하는 [코인 주소](https://www.hbg.com/zh-cn/withdraw_address/) 중의 주소만 지원합니다.  |
| amount     | string | true | 출금 수량 |      |
| currency | string | true | 자산 유형                                                    | btc, ltc, bch, eth, etc 등 (후오비 코리아에서 지원하는 코인) |
| fee     | string | false | 수수료 |     |
| addr-tag      | string     | false | XRP, XEM, BTS, STEEM, EOS, XMR 암호화폐 주소 Tag | 형식, class "123"의 정수형 문자열 |


> Response:

```json
{
  "data": 700
}
```

### 응답 데이터


| 매개 변수 | 데이터 유형 | 필수 여부 | 설명   |
| ---- | ----- | ---- | ---- | ---- |
| data | long | false | 출금 ID |      |


## 출금 취소

- API Key 권한：출금

### HTTP 요청

- POST ` /v1/dw/withdraw-virtual/{withdraw-id}/cancel`

### 요청 매개 변수

| 매개 변수   | 유형 |  필수 여부  | 설명 | 값의 범위 |
| ----------- | ---- | ---- | ------------ | ---- |
| withdraw-id | long | true | 출금 ID | path에 작성 |      |


> Response:

```json
{
  "data": 700
}
```

### 응답 데이터


| 매개 변수 | 데이터 유형 | 필수 여부 | 설명    |
| ------------- | --------- | ----------- | ------- | --------- |
| data          | long     | false        | 출금 ID |           |

## 입출금 내역

- API Key 권한：읽기

- 입출금 기록 조회.

### HTTP 요청

- GET `/v1/query/deposit-withdraw`

### 요청 매개 변수

| 매개 변수 | 데이터 유형 | 필수 여부 | 설명 | 기본값  | 값의 범위 |
| ----------- | ---- | ---- | ------------ | ---- | ---- |
| currency | string | false | 코인종류 |  | 매개 변수가 없는 경우 모든 코인이 반환됩니다. |
| type | string | true | 입금 또는 출금 |     | deposit 혹은 withdraw |
| from   | string | false | 쿼리 시작 ID | 매개 변수가 없는 경우 기본값은 direct 관련입니다. direct가 'prev'인 경우 from은 1이고 과거순으로 오름차순 정렬됩니다. direct가 'next'인 경우 from은 최신 기록 ID이며 최신순으로 내림차순 정렬됩니다.    |     |
| size   | string | false | 쿼리 레코드 크기 | 100   |1-500     |
| direct | string | false | 레코드 정렬 direct 반환 | 매개 변수가 없을 시 기본값은 “prev”(올림차순) |“prev”(올림차순) or “next” (내림차순) |
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


| 매개 변수 |  데이터 유형 | 필수 여부 | 설명 | 값의 범위 |
|-----|-----|-----|-----|------|
|   id  |  long  |  true  |   | |
|   type  |  string  |  true  | 유형 | 'deposit', 'withdraw' |
| currency  |  string  |  true  | 코인 | |
| tx-hash | string | true | 거래hash | |
| amount | long | true | 개수 | |
| address | string | true | 주소 | |
| address-tag | string | true | 주소 tag | |
| fee | long | true | 수수료 | |
| state | string | true | 상태 | 상태는 아래의 도표를 참조 |
| created-at | long | true | 생성시간 | |
| updated-at | long | true | 최종 업데이트 시간 | |


- 암호화폐 입금 상태 정의：

|상태|설명|
|--|--|
|unknown|알 수 없는 상태|
|confirming|컨펌 중|
|confirmed|컨펌 완료|
|safe|입금 완료|
|orphan| 입금 대기|

- 암호화폐 출금 상태 정의：

| 상태 | 설명  |
|--|--|
| submitted | 제출 완료 |
| reexamine | 심사 중 |
| canceled  | 출금 취소 완료 |
| pass    | 심사 승인 |
| reject  | 심사 거절 |
| pre-transfer | 처리 중 |
| wallet-transfer | 입금 완료 |
| wallet-reject   | 지갑 전송 거절 |
| confirmed      | 블록 컨펌 완료 |
| confirm-error  | 블록 컨펌 오류 |
| repealed       | 출금 취소 완료 |


---
title: trading
---

# 7. 거래

거래 관련 API 액세스 하는 경우 서명 인증이 필요합니다.

## 주문

- API Key 권한：거래
- 후오비 코리아에 새로운 주문을 생성하여 매칭을 진행합니다.

### HTTP 요청

- POST ` /v1/order/orders/place`

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

매개 변수 | 데이터 유형 | 필수 여부 | 기본값 | 설명
---------  | --------- | -------- | ------- | -----------
account-id | string    | true     | N/A      | 계정 ID, GET /v1/account/accounts API로 조회, 현물 거래는 ‘spot’ 계정의 account-id 사용
symbol     | string    | true     | N/A      | 거래쌍, 예) btcusdt, ethbtc
type       | string    | true     | N/A      | 주문 유형 - buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sellioc, buy-limit-maker, sell-limit-maker 포함(아래 설명 참조)
amount     | string    | true     | N/A      | 주문 거래량 
price      | string    | false    | N/A      | limit order의 체결 가격 
source     | string    | false    | api     | 현물거래는 “api” 입력 

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

- 반환된 기본 데이터 object는 주문 번호에 해당하는 문자열입니다.

## 주문 취소

- API Key 권한：거래
- 본 API는 주문 취소 요청을 보냅니다.

- 본 API는 취소요청만 제출하며 실제 취소결과는 API 상태, 일치 상태 및 기타 API로 확인해야 합니다.

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

- 반환된 기본 데이터 object는 주문 번호에 해당하는 문자열입니다.


## 미체결 주문 조회

- API Key 권한：읽기
- 제출 후, 체결되지 않은 주문 건에 대한 조회

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

### HTTP 요청

- GET `/v1/order/openOrders`


### 요청 매개 변수

매개 변수 | 데이터 유형 | 필수여부 | 기본값 | 설명
---------  | --------- | -------- | ------- | -----------
account-id | string    | true    | N/A      | 계정 ID, GET /v1/account/accounts API로 조회, 현물 거래는 ‘spot’ 계정의 account-id 사용
symbol     | string    | ture    | N/A      | 거래쌍, 예) btcusdt, ethbtc 
side       | string    | false    | both    | 단 방향만 반환하는 주문을 지정합니다. 가능한 값은 buy, sell이며 기본값은 양방향으로 반환됩니다.
size       | int       | false    | 10      | 주문 건수를 반환합니다. 최대값: 2,000


"account-id"와 "symbol"은 같이 지정되거나 지정되지 않아야 합니다. 같이 지정되지 않은 경우 주문번호를 기준으로 내림차순 정렬된 최대 500개의 미체결 주문을 반환합니다.

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

매개 변수          | 데이터 유형 | 설명
---------           | --------- | -----------
id                  | integer   | 주문 ID
symbol              | string    | 거래쌍, 예) btcusdt, ethbtc
price               | string    | 지정가 주문 가격 제한
created-at          | int       | 주문 생성 시 UTC/GMT+08:00의 타임 스탬프로 조정 (단위: ms(milliseconds))
type                | string    | 주문 유형 
filled-amount       | string    | 주문 건에서 체결완료된 수량
filled-cash-amount  | string    | 주문 건에서 체결완료된 금액
filled-fees         | string    | 차감된 총 거래 수수료
source              | string    | 현물 거래는 “api” 추가
state               | string    | 주문 상태, submitted, partical-filled, cancelling 포함

## 대량 주문 취소(open orders)

- API Key 권한：거래
- 본 API는 대량 주문 취소 요청을 보냅니다.

본 API는 취소요청만 제출하며 실제 취소결과는 API 상태, 일치 상태 및 기타 API로 확인해야 합니다.

### HTTP 요청

- POST ` /v1/order/orders/batchCancelOpenOrders`


### 요청 매개 변수

| 매개 변수 | 유형 | 필수 여부     | 설명           | 기본값  | 값의 범위 |
| -------- | ---- | ------ | ------------ | ---- | ---- |
| account-id | string  | true | 계정ID     |     |      |
| symbol     | string | true | 거래쌍     |      | 단일 거래 쌍 문자열, 매개변수가 없을 경우 미체결된 주문이 반환됩니다.|
| side | string | false | 주문 활성화 지시 |      |   “buy” 또는 “sell”, 매개변수가 없을 경우 미체결된 주문이 반환됩니다.   |
| size | int | false | 필요한 레코드 수  |  100 |   [0,100]   |


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


| 매개 변수 | 데이터 유형 | 필수 여부 | 설명    | 값의 범위 |
| ---- | ---- | ------ | ----- | ---- |
| success-count | int | true | 취소 완료 주문 건수 |     |
| failed-count | int | true | 취소 실패 주문 건수 |     |
| next-id | long | true | 취소 가능한 다음 주문번호 |    |

## 주문서 대량 철회

API Key 권한：거래

해당 API는 동시에 여러개 주문서(id로 식별) 취소시 요청 합니다.

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

| 매개 변수 | 필수 여부 | 유형   | 설명   | 기본값  | 값의 범위 |
| ---- | ---- | ---- | ----  | ---- | ---- |
| order-ids | true | list | 철회 오더 ID리스트 |  | 한차례에 50개 오더 id를 초과 못합니다 |


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
      }
    ]
  }
}
```

### 응답 데이터

| 필드 이름 | 데이터 유형 | 설명|
| ---- | ----- | ---- |
| data | map | 주문서 철회 결과

## 주문서 내역 조회

API Key 권한：읽기

해당 API는 해당 주문서의 최신상태와 상세내역을 반환 합니다.

### HTTP 요청

- GET `/v1/order/orders/{order-id}`


### 요청 매개 변수

| 매개 변수   | 필수 여부 | 유형  | 설명   | 기본값  | 값의 범위 |
| -------- | ---- | ------ | -----  | ---- | ---- |
| order-id | true | string | 주문서ID，path에 입력 |      |      |


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
    "canceled-at": 0,
    "exchange": "huobi",
    "batch": ""
  }
}
```

### 응답 데이터

| 필드 이름    | 필수 여부 | 데이터 유형 | 설명   | 값의 범위    |
| ----------------- | ----- | ------ | -------  | ----  |
| account-id        | true  | long   | 계정 ID    |       |
| amount            | true  | string | 주문서 수량          |    |
| canceled-at       | false | long   | 주문서 철회 시간 |     |
| created-at        | true  | long   | 주문서 생성 시간 |   |
| field-amount      | true  | string | 체결된 수량  |     |
| field-cash-amount | true  | string | 체결된 총금액   |      |
| field-fees        | true  | string | 체결된 수수료(매수시 거래 화폐,매도시 메켓화폐) |     |
| finished-at       | false | long   | 주문서가 최종상태시 시간,체결 시간이 아니며 "주문서 철회"상태를 포함 |     |
| id                | true  | long   | 주문서 ID |     |
| price             | true  | string | 주문 가격     |     |
| source            | true  | string | 주문 경로 | api |
| state             | true  | string | 주문서 상태 | submitting , submitted 제출 완료, partial-filled 부분체결, partial-canceled 부분철회, filled 체결 완료, canceled 철회 완료 |
| symbol            | true  | string | 거래쌍 | btcusdt, ethbtc, rcneth ... |
| type              | true  | string | 주문서 유형 | buy-market：시가 매수, sell-market：시가 매도, buy-limit： 지정가 매수, sell-limit：지정가 매도, buy-ioc：IOC 매수 오더, sell-ioc：IOC 매도 오더 |

## 체결 완료 내역

API Key 권한：읽기

해당 API는 체결 완료 주문서 내역을 반환 합니다. 

### HTTP 요청

- GET `/v1/order/orders/{order-id}/matchresults`



### 요청 매개 변수

| 매개 변수 | 필수 여부 | 유형  | 설명  | 기본값  | 값의 범위 |
| -------- | ---- | ------ | -----  | ---- | ---- |
| order-id | true | string | 주문서ID，path에 입력 |      |      |


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
      "created-at": 1494901400435
    }
    ...
  ]
}
```

### 응답 데이터

<aside class="notice">반환한 주요 데이터 객체는 하나의 객체array입니다.그중에 매개 원소는 거래 결과 입니다.</aside>
| 필드 이름   | 필수 여부 | 데이터 유형 | 설명   | 값의 범위    |
| ------------- | ---- | ------ | -------- | -------- |
| created-at    | true | long   | 체결  시간   |    |
| filled-amount | true | string | 체결  수량   |    |
| filled-fees   | true | string | 체결  수수료  |     |
| id            | true | long   | 주문서 체결 리스트 ID |     |
| match-id      | true | long   | 매칭 ID     |     |
| order-id      | true | long   | 주문서 ID |      |
| price         | true | string | 체결 가격 |    |
| source        | true | string | 주문 경로 | api      |
| symbol        | true | string | 거래쌍 | btcusdt, ethbtc, rcneth ...  |
| type          | true | string | 주문서 유형 | buy-market：시가 매수, sell-market：시가 매도, buy-limit：지정가 매수, sell-limit：지정가 매도, buy-ioc：IOC 매수 오더, sell-ioc：IOC 매도 오더 |

## 오더 리스트 검색

API Key 권한：읽기

해당 API는 검색 조건에 의하여 주문 리스트를 반환합니다

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

| 매개 변수 | 필수 여부 | 유형     | 설명   | 기본값  | 값의 범위  |
| ---------- | ----- | ------ | ------  | ---- | ----  |
| symbol     | true  | string | 거래쌍   |      |btcusdt, ethbtc, rcneth ...  |
| types      | false | string | 주문서의 유형조합을 조회시 ','부호로 분리 |      | buy-market：시가 매수, sell-market：시가 매도, buy-limit：지정가 매수, sell-limit：지정가 매도, buy-ioc：IOC 매수 오더, sell-ioc：IOC 매도 오더 |
| start-date | false | string | 시작 일자 조회,일자 양식은 yyyy-mm-dd 입니다. 주문서 생성시간으로 조회 | -180 days     | [-180 days, end-date] （6월10일부터 시작하여 start-date와 end-date의 조회는 최대 2일이며 범위 초과시 오류코드를 반환합니다 |
| end-date   | false | string | 완료일자 조회, 일자 양식 yyyy-mm-dd 입니다. 주문서 생성기간으로 조회 | today     | [start-date, today] （6월10일부터 시작하여 start-date와 end-date의 조회 범위는 최대 2일이며 범위 초과시 오류코드를 반환합니다 |
| states     | true  | string | 오더 상태조합 조회,부호','분할  |      | submitted 제출 완료, partial-filled 부분 체결 완료, partial-canceled 부분 체결 철회, filled 체결 완료, canceled 철회 완료 |
| from       | false | string | 최초 ID 조회   |      |    |
| direct     | false | string | 조회 방향   |      | prev 이전，시간(혹은 ID) 순차적 순서；next 이후，시간(혹은 ID) 역차적 순서    |
| size       | false | string | 조회 기록 사이즈      | 100     |  [1, 1000]       |


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
      "canceled-at": 0,
      "exchange": "huobi",
      "batch": ""
    }
    ...
  ]
}
```

### 응답 데이터

| 매개 변수  | 필수 여부 | 데이터 유형 | 설명   | 값의 범위  |
| ----------------- | ----- | ------ | ----------------- | ----  |
| account-id        | true  | long   | 계정 ID    |     |
| amount            | true  | string | 주문 수량 |   |
| canceled-at       | false | long   | 철회 신청 접수 시간  |    |
| created-at        | true  | long   | 주문서 생성 시간 |    |
| field-amount      | true  | string | 체결 수량 |    |
| field-cash-amount | true  | string | 체결 총금액 |    |
| field-fees        | true  | string | 체결 수수료(매수시 거래 화폐, 매도시 마켓화폐) |       |
| finished-at       | false | long   | 최종 체결 시간  |   |
| id                | true  | long   | 주문서 ID |    |
| price             | true  | string | 주문 가격 |    |
| source            | true  | string | 주문 경로 | api  |
| state             | true  | string | 주문서 상태 | submitting , submitted 제출 완료, partial-filled 부분 체결, partial-canceled 부분 체결 철회, filled 체결 완료, canceled 철회 완료 |
| symbol            | true  | string | 거래쌍 | btcusdt, ethbtc, rcneth ... |
| type              | true  | string | 주문서 유형 | submit-cancel：철회 신청 제출 완료  ,buy-market：시가 매수, sell-market：시가 매도, buy-limit：지정가 매수, sell-limit：지정가 매도, buy-ioc：IOC 매수 오더, sell-ioc：IOC 매도 오더 |

### start-date, end-date 관련 오류 코드 （6월10일 부로 유효）

|오류 코드|해당 오류 환경|
|------------|----------------------------------------------|
|invalid_interval| start date 는 작기 end date; 혹은 start date 와 end date 시간차이는 크기 2일 이여야 합니다. |
|invalid_start_date|start date는 180일 이전 일자；혹은start date는 미래 일자|
|invalid_end_date|end date 는 180일 이전 일자；혹은 end date는 미래 일자|


## 최근 48시간내 주문 리스트 검색

API Key 권한：읽기

해당 API는 검색 조건에 의하여 최근 48시간내 주문 리스트를 반환 합니다.

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

| 매개 변수 | 필수 여부 | 유형     | 설명   | 기본값  | 값의 범위  |
| ---------- | ----- | ------ | ------  | ---- | ----  |
| symbol     | false  | string | 거래쌍   |all      |btcusdt, ethbtc, rcneth ...  |
| start-time      | false | long | 처음 시간 조회(포함) |48시간  이전 시각      |UTC time in millisecond |
| end-time | false | long | 종료시간 조회(포함) | 조회 시각     |UTC time in millisecond |
| direct   | false | string | 주문서 조회 방향（주：조회 결과 총세목 수량이 size필드 제한을 초과시 작용 합니다.；만약 조회 결과 총 세목 수량이 size필드 제한 내일 경우 direct필드는 작용이 없습니다.） | next     |prev, next   |
| size     | false  | int | 매번 반환한 총세목 |100      | [10,1000] |



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

| 매개 변수  | 필수 여부 | 데이터 유형 | 설명   | 값의 범위  |
| ----------------- | ----- | ------ | ----------------- | ----  |
| account-id        | true  | long   | 계정ID  |     |
| amount            | true  | string | 주문서 수량 |   |
| canceled-at       | false | long   | 주문 철회 신청을 접수한 시간 |    |
| created-at        | true  | long   | 주문서 생성 시간 |    |
| field-amount      | true  | string | 체결 수량 |    |
| field-cash-amount | true  | string | 체결 총금액 |    |
| field-fees        | true  | string | 체결 수수료(매수시 거래 화폐,매도시 마켓 화폐) |       |
| finished-at       | false | long   | 최종 체결 시간 |   |
| id                | true  | long   | 주문서 ID |    |
| price             | true  | string | 주문 가격 |    |
| source            | true  | string | 주문 경로 | api  |
| state             | true  | string | 주문서 상태 | partial-canceled 부분 체결 철회, filled 체결 완료, canceled 철회 완료 |
| symbol            | true  | string | 거래쌍 | btcusdt, ethbtc, rcneth ... |
| type              | true  | string | 주문서 유형 | buy-market：시가 매수, sell-market：시가 매도, buy-limit：지정가 매수, sell-limit：지정가 매도, buy-ioc：IOC 매수 오더, sell-ioc：IOC매도 오더, buy-limit-maker, sell-limit-maker |
| next-time            | false  | long |다음 조회 시작시간(요청한 필드"direct"가 "prev" 일때 유효 합니다), 다음 조회 종료시간(요청한 필드"direct"가 "next" 일때 유효 합니다),주：조회 결과 총 세목 수량이 size필드 제한 내일 경우  반환한 필드가 있습니다. |UTC time in millisecond   |

## 현재 및 과거 주문 체결 리스트

API Key 권한：읽기

해당 API는 검색 조건에 의하여 현재 및 과거 주문 체결 리스트를 조회 합니다.

### HTTP 요청

- GET `/v1/order/matchresults`


### 요청 매개 변수

| 매개 변수 | 필수 여부 | 유형  | 설명   | 기본값  | 값의 범위   |
| ---------- | ----- | ------ | ------ | ---- | ----------- |
| symbol     | true  | string | 거래쌍 | N/A |  btcusdt, ethbtc, rcneth ...  |
| types      | false | string | 주문서    유형 조합 조회,부호 ','로 분리 |      | buy-market：시가 매수, sell-market：시가 매도, buy-limit：지정가 매수, sell-limit：지정가 매도, buy-ioc：IOC 매수 오더, sell-ioc：IOC 매도 오더 |
| start-date | false | string | 시작 일자 조회,일자 양식:yyyy-mm-dd | -61 days     | [-61day, today] (6월 10일부터 시작하며 start-date와 end-date 의 조회 범위는 최대 2일 이며 범위 초과시 API는 오류 코드를 반환 합니다.) |
| end-date   | false | string | 종료 일자 조회,일자 양식:yyyy-mm-dd |   today   | [start-date, today] (6월 10일부터 시작하며 start-date와 end-date 의 조회 범위는 최대 2일 이며 범위 초과시 API는 오류 코드를 반환 합니다.) |
| from       | false | string | 조회 처음 ID |   주문서 체결 ID(최대치)   |     |
| direct     | false | string | 조회 방향 |   기본적으로next， 체결 리스트 ID는 내림차순   | prev 이전，시간(혹은 ID) 순차적 순서；next 이후，시간(혹은 ID) 역차적 순서 |
| size       | false | string | 조회 리스트 크기 |   100   | [1，100]  |


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
      "created-at": 1494901400435
    }
    ...
  ]
}
```

### 응답 데이터

<aside class="notice">반환한 주요 데이터 대상은 하나의 대상 array입니다.그 중에 매개 원소는 거래 결과 입니다.</aside>
| 매개 변수 | 필수 여부 | 데이터 유형 | 설명   | 값의 범위  |
| ------------- | ---- | ------ | -------- | ------- |
| created-at    | true | long   | 체결 시간 |    |
| filled-amount | true | string | 체결 수량 |    |
| filled-fees   | true | string | 체결 수수료 |    |
| id            | true | long   | 주문 체결 리스트 ID |    |
| match-id      | true | long   | 매칭 ID   |    |
| order-id      | true | long   | 주문서ID |    |
| price         | true | string | 체결 가격 |    |
| source        | true | string | 주문 경로 | api   |
| symbol        | true | string | 거래쌍   | btcusdt, ethbtc, rcneth ...  |
| type          | true | string | 주문서유형  | buy-market：시가 매수, sell-market：시가 매도, buy-limit：지정가 매수, sell-limit：지정가 매도, buy-ioc：IOC 매수 오더, sell-ioc：IOC 매도 오더 |

### start-date, end-date 관련 오류 코드

|오류 코드|해당 오류 환경|
|------------|----------------------------------------------|
|invalid_interval| start date 는 작기 end date; 혹은 start date 와 end date 시간차이는 크기 2일 이여야 합니다. |
|invalid_start_date|start date는 61일 이전 일자；혹은 start date는 미래 일자|
|invalid_end_date|end date는 61일 이전 일자；혹은 end date는 미래 일자|


---
title: websocket_market_data
---

# 8. Websocket 시세 데이터

## 소개

### Access URL

**후오비 코리아 시세 요청 API 주소**

**`wss://api-cloud.huobi.co.kr/ws`**

-	중국 이외의 IP를 통해 후오비 코리아 API로 접속해주시기 바랍니다.

### 데이터 압축

- WebSocket API에서 반환된 모든 데이터는 GZIP로 압축되어 있으며 데이터 수신 후 클라이언트의 압축을 해제해야 합니다.

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

일괄적으로 반환되는 데이터는 다음과 같습니다.

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

- 구독 요청

```json
{
  "sub": "market.ethbtc.kline.1min",
  "id": "id1"
}
```

### 매개 변수

매개 변수 | 데이터 유형 | 필수여부 | 설명                      | 값의 범위 
--------- | --------- | -------- | -----------                      | -----------
symbol    | string    | true     | 거래 코드 | 모든 거래 지원, 예) btcusdt, bccbtc
period     | string    | true     | K차트  주기 | 1분, 5분, 15분, 30분, 60분, 1일, 1달, 1주, 1년

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

매개 변수     | 데이터 유형 | 설명
--------- | --------- | -----------
id        | integer   | unix 시간, K-차트 ID로도 사용
amount    | float     | 거래 수량 
count     | integer   | 거래 건수
open      | float     | 시작가
close     | float     | 종가 (K-차트가 마지막 1개일 경우 최신 거래 가격 기준) 
low       | float     | 최저가
high      | float     | 최고가
vol       | float     | 총 거래대금 (각 거래가격 * 거래량)



### 요청 데이터

K-차트 데이터를 일괄 요청 시 다음 추가 매개 변수를 제공해야합니다.

```json
{
  "req": "market.$symbol.kline.$period",
  "id": "id generated by client",
  "from": "from time in epoch seconds",
  "to": "to time in epoch seconds"
}
```

매개 변수 | 데이터 유형 | 필수 여부 | 설명                                | 기본값 | 값의 범위 
--------- | --------- | -------- | -------------                          | -----------      | -----------
from      | integer   | false    | 시작시간 (epoch time in second) | 1501174800(2017-07-28T00:00:00+08:00)  | [1501174800, 2556115200]
to        | integer   | false    | 종료시간(epoch time in second) | 2556115200(2050-01-01T00:00:00+08:00)  | [1501174800, 2556115200] or ($from, 2556115200] if "from" is set

## depth of market 시세 데이터

depth of the market 변화시 최신 depth of the market 데이터를 전송합니다.

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

매개 변수 | 데이터 유형 | 필수여부 | 설명 | 기본값  | 값의 범위 
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | 거래 코드             | N/A                    | All supported trading symbols, e.g. btcusdt, bccbtc
type      | string    | true     | depth유형 조합 |  step0                 | step0, step1, step2, step3, step4, step5

**"type" depth 유형 조합**

Value     | Description
--------- | ---------
step0     | 통합하지 않음
step1     | 통합 수준 = 정밀도 * 10
step2     | 통합 수준 = 정밀도 * 100
step3     | 통합 수준 = 정밀도 * 1000
step4     | 통합 수준 = 정밀도 * 10000
step5     | 통합 수준 = 정밀도 * 100000

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
  "ch": "market.btcusdt.depth.step0",
  "ts": 1489474082831,
  "tick": {
    "bids": [
    [9999.3900,0.0098], // [price, amount]
    [9992.5947,0.0560]
    // more Market Depth data here
    ],
    "asks": [
    [10010.9800,0.0099],
    [10011.3900,2.0000]
    //more data here
    ]
  }
}
```

### 데이터 업데이트 필드 목록

매개 변수     | 데이터 유형 | 설명
--------- | --------- | -----------
bids      | object    | The current all bids in format [price, quote volume]
asks      | object    | The current all asks in format [price, quote volume]



### 요청 데이터

depth of market 데이터를 일괄 요청합니다.

```json
{
  "req": "market.btcusdt.depth.step0",
  "id": "id10"
}
```

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

매개 변수 | 데이터 유형 | 필수 여부 | 설명 | 기본값 | 값의 범위 
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | 거래 코드 | N/A | 모든 거래 지원, 예) btcusdt, bccbtc

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
                "price": 401.74,
                "direction": "buy"
            }
            // more Trade Detail data here
        ]
  }
}
```

### 데이터 업데이트 필드 리스트

매개 변수 | 데이터 유형 | 설명
--------- | --------- | -----------
id        | integer   | 고유 거래 ID
amount    | float     | 거래 수량
price     | float     | 거래 가격
ts        | integer   | 거래 시간 (UNIX epoch time in millisecond)
direction | string    | 활성 거래자 (taker 주문 입장): 매수 또는 매도

### 요청 데이터

- 한 번에 거래 세부 사항 데이터를 수집하기 위한 지원 데이터 요청 방법 (가장 최근 300개의 거래 내역을 받습니다.)

```json
{
  "req": "market.btcusdt.trade.detail",
  "id": "id11"
}
```

## Market 개요

### 구독 주제

- 본 주제는 24시간 내에 Market에 대한 최신 개요를 제공합니다.

`market.$symbol.detail`

> Subscribe request

```json
{
  "sub": "market.btcusdt.detail",
  "id": "id1"
}
```

### 매개 변수

매개 변수 | 데이터 유형 | 필수 여부 | 설명 | 기본값 | 값의 범위 
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | 거래 코드  | N/A   | 모든 거래 지원, 예) btcusdt, bccbtc

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

필드     | 데이터 유형 | 설명
--------- | --------- | -----------
id        | integer   | unix시간，동시에 메시지 ID로 사용 
ts        | integer   | unix 시스템 시간 
amount    | float     | 24시간내 거래량 
count     | integer   | 24시간내 체결건수 
open      | float     | 24시간 시작가 
close     | float     | 종가 
low       | float     | 24시간내 최저가 
high      | float     | 24시간내 최고가 
vol       | float     | 24시간 총 거래대금 

### 요청 데이터

한 번에 Market 요약 데이터를 얻기 위한 지원 데이터 요청 방법은 다음과 같습니다.

```json
{
  "req": "market.btcusdt.detail",
  "id": "id11"
}
```


---
title: websocket_asset_and_order
---

# 9. Websocket자산 및 주문서

## 소개

### Access URL

**WebSocket 자산 및 주문**

**`wss://api-cloud.huobi.co.kr/ws/v1`**

중국 이외의 IP를 통해 후오비 코리아 API로 접속해주시기 바랍니다.

### 데이터 압축

WebSocket API에서 반환된 모든 데이터는 GZIP로 압축되어 있으며 데이터 수신 후 클라이언트의 압축을 해제해야 합니다.

### Heartbeat Message

회원님의 WebSocket 클라이언트가 후오비 코리아 WebSocket 서버에 연결되면 서버는 정기적으로(현재 5초로 설정) `ping` 메세지를 발송하며 메시지에 아래와 같은 데이터를 포함합니다.

> {"ping": 1492420473027}

회원님의 Websocket클라이언트가  Heartbeat Message를 접수후 `pong` 메시지를 반환하여야 하며 메시지에 같은 데이터를 포함 하여야 합니다:

> {"pong": 1492420473027}

WebSocket 서버가 `pong` 메세지를 수신하지 못하고 `ping` 메세지를 2회 연속 보낼 경우 서버는 클라이언트와의 연결을 종료합니다.

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

### 구독취소

구독 취소 양식은 다음과 같습니다.

```json
{
  "op": "unsub",
  "topic": "accounts",
  "cid": "client generated id"
}
```

구독 취소 확인 완료

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

* accounts.list
* orders.list
* orders.detail

요청 데이터의 형식은 다음과 같습니다.

**데이터 요청 제한 규정**

빈도 제한 규정은 연결(connect) 대신 API Key를 기반으로 합니다. 요청 빈도가 제한 초과 시 WebSocket 클라이언트는 "too many request" 오류 코드를 수신 받습니다. 다음은 각 주제에 대한 현재 주파수 제한 설정입니다.

* accounts.list: once every 25 seconds
* orders.list AND orders.detail: once every 5 seconds

### 인증

자산 및 주문 주체에 대한 인증 요청 데이터 형식은 다음과 같습니다.

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

  매개 변수             | 데이터 유형 | 설명 |
  ------------------ |----   |  ----------------------------------------------------- |  ----------------------------------------------------- 
  op                 | string | 필수, 작업명, 인증 고정값은 auth |
  cid                | string | 선택, 클라이언트 요청 고유 ID |
  AccessKeyId        | string | 필수, 요청한 API Key의 API Access Key |
  SignatureMethod    | string | 선택, 서명 방법, 회원 서명 계산의 hash 기반 프로토콜, 여기서는 HmacSHA256를 사용합니다 |
  SignatureVersion   |string | 필수, 서명 프로토콜 버젼, 여기서는 버전 2를 사용합니다 |
  Timestamp          |string | 선택, 회원님이 요청한 시간(UTC 시간대), 쿼리 요청 시 해당 값을 포함하면 제3자가 회원님의 요청을 가로챌 수 없습니다. 예) 2017-05-11 T16:22:06. |
  Signature          |string | 필수, 서명이 유효하고 변조되지 않았음을 보장하기 위해 서명에 의해 계산된 값입니다. |

> **참고**
>
> - [http://alphaex-api.github.io/api_access/#%EC%84%9C%EB%AA%85-%EC%9D%B8%EC%A6%9D]를 참조하여 유효한 서명을 생성해주세요.
> - 요청 메소드는 서명 계산에서 고정된 GET 값을 갖습니다.

## 구독 계정 업데이트

API Key 권한：읽기

구독 계정의 자산 변화 업데이트.

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

매개 변수 | 데이터 유형 | 필수 여부 | 기본값      | 설명                                       | 값의 범위 
--------- | --------- | -------- | -------------         | -----------                                       | -----------
model     | string    | false    | 0                     | 동결 잔액 포함 여부      | 1 to include frozen balance, 0 to not

만약 사용가능한 총잔액 구독시 0과 1로 별도로 Websocket연결 하여야 합니다.

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

### 테이터 업데이트 필드 리스트

필드     | 데이터 유형 | 설명
--------- | --------- | -----------
event     | string    | 자산 변동 통지 관련 설명, 예를 들면 주문서 생성(order.place), 주문 체결(order.match), 주문 체결 환불（order.refund), 주문 철회(order.cancel), 쿠폰 포인트 삭감 수수료（order.fee-refund), 기타 자산 변동(other) 
account-id| integer   |  
currency  | string    | 코인 종류 
type      | string    | 계정 유형 
balance   | string    | 계정 잔액 (구독 mode=0 일때 해당 잔액은 사용가능한 잔액 입니다；구독 mode=1时，해당 잔액은 총잔액 입니다）

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

매개 변수 | 데이터 유형 | 필수여부 | 기본값 | 설명 | 값의 범위 
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | N/A                    | 거래 코드   | 모든 거래 지원, 예) btcusdt, bccbtc

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

필드               | 데이터 유형 | 설명
---------           | --------- | -----------
seq-id              | integer   | 시리얼 넘버(연속된 번호가 아님) 
order-id            | integer   | 주문서 id 
symbol              | string    | 거래쌍 
account-id          | string    | 계정 id 
order-amount        | string    | 주문서 수량 
order-price         | string    | 주문 가격 
created-at          | int       | 주문 생성 시간 (UNIX epoch time in millisecond) 
order-type          | string    | 주문서 유형, 유효치: buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker 
order-source        | string    | 주문 경로, 유효치: sys, web, api, app 
order-state         | string    | 주문 상태, 유효치: submitted, partical-filled, cancelling, filled, canceled, partial-canceled 
role                | string    | 체결 role: taker or maker 
price               | string    | 체결 가격 
filled-amount       | string    | 단일 체결 수량 
filled-cash-amount  | string    | 단일 미체결 수량 
filled-fees         | string    | 단일 체결 금액 
unfilled-amount     | string    | 단인 체결 수수료（매수시 거래 화폐, 매도시 마켓 화폐） 

## 구독 주문서 업데이트 (NEW)

API Key 권한：읽기

**현재 회원 주문서 업데이트 PUSH“orders.symbol”에 비하여 새로 증가 된 “orders.symbol.update”는 딜레이가 작고 메시지가 더 정확합니다. API 회원께서는 새로운 주제를 구독하여 “orders.symbol”를 PUSH 하기 바라는 바 입니다.（기존 구독 주제  “orders.symbol”는 별도로 통지 하기 전까지 Websocket API에서 보류 중입니다.）**

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

매개 변수 | 데이터 유형 | 필수 여부 | 기본값   | 설명                                       | 값의 범위 
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | N/A                    | 거래 코드              | 모든 거래 지원, 예) btcusdt, bccbtc



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
    "order-state": "filled"
  }
}
```

### 데이트 업데이트 필드 목록

필드               | 데이터 유형 | 설명
---------           | --------- | -----------
match-id              | integer   | 최근 매칭 번호（order-state = submitted, canceled, partial-canceled 일때 match-id 는 메시지 넘버 입니다. order-state = filled, partial-filled 일때 match-id 는 최근 매칭 번호 입니다.） 
order-id            | integer   | 주문서 번호 
symbol              | string    | 거래 코드 
order-state         | string    | 주문서 상태, 유효치: submitted, partical-filled, cancelling, filled, canceled, partial-canceled 
role                | string    | 최근 체결 role（order-state = submitted, canceled, partial-canceled 일때 role은 디폴트 taker입니다. order-state = filled, partial-filled 일때 role 값은taker 혹은 maker 입니다.） 
price               | string    | 현재가（order-state = submitted 일때 price 는 주문 가격 입니다. order-state = canceled, partial-canceled 일때 price 는 0입니다. order-state = filled, partial-filled 일때 price는 최근 체결가 입니다. role = taker 이고 해당 주문서가 여러 다른 회원의 주문서와 매칭시 price 는 여러 체결된 평균가 입니다.） 
filled-amount       | string    | 최근 체결 수량 
filled-cash-amount  | string    | 최근 체결 금액 
unfilled-amount     | string    | 최근 미체결 수량（order-state = submitted 일때 unfilled-amount 는 처음 주문량 입니다. order-state = canceled OR partial-canceled 일때 unfilled-amount 는 미체결 수량 입니다. order-state = filled 일때 만약 order-type = buy-market, unfilled-amount 는 극히 작은 수 일수 있습니다. 만약 order-type <> buy-market 일때 unfilled-amount 는 0 입니다. order-state = partial-filled AND role = taker 일때 unfilled-amount 는 미체결 수량 입니다. order-state = partial-filled AND role = maker 일때 unfilled-amount 는 0 입니다.（추후 해당 환경하에 미체결 수량을 지원할 예정입니다,시간은 따로 통보 토록 하겠습니다.） 


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
WebSocket API와 접속 후 Server에 다음 양식과 같은 데이터를 발송하여 계정 데이터를 조회 합니다：

매개 변수  | 데이터 유형 |  설명|
----------| --------| ------------------------------------------------------- | ------------------------------------------------------- 
op         |string  | 필수, 작업 명칭, 고정값은 req |
cid        |string  | 선택, Client 유일한 ID 요청 |
topic      |string   |필수, 고정값은 accounts.list |

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

매개 변수                |데이터 유형 |    설명|
-------------------- |--------| ------------------------------------ | ------------------------------------ 
id                   |long    | 계정ID |
type              |string   |계정 유형|
state           |string     |계정 상태|
list               |string   |계정 리스트|
currency                |string   |부계정 코인 종류|
type           |string     |부계정 유형|
balance           |string     |부계정 잔액|

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

매개 변수  | 데이터 유형 | 필수 여부 | 기본값 | 설명                                   | 값의 범위 
---------  | --------- | -------- | ------- | -----------                                   | ----------
account-id | int       | true     | N/A      | 계정ID                  | N/A
symbol     | string    | true     | N/A      | 거래쌍             | 모든 거래 지원, 예) btcusdt, bccbtc
types      | string    | false    | N/A      | 조회한 주문서 유형 조합, 부호 ',' 로 분리 | buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc
states     | string    | false    | N/A      | 조회한 주문서 상태 조합, 부호 ',' 로 분리 | submitted, partial-filled, partial-canceled, filled, canceled
start-date | string    | false    | -61d    | 조회 시작일자, 일자 양식 yyyy-mm-dd | N/A
end-date   | string    | false    | today   | 조회 종료일자, 일자 양식 yyyy-mm-dd | N/A
from       | string    | false    | N/A      | 조회 처음 ID          | N/A
direct     | string    | false    | next    | 조회 방향     | next, prev
size       | int       | false    | 100     | 조회 내역 크기       | [1, 100]

### 데이터 업데이트 필드 목록

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

필드                 | 데이터 유형 | 설명 |
-------------------- |--------| ------------------------------------ | ------------------------------------ 
id                   |long    | 주문서 ID |
symbol               |string   |거래쌍|
account-id           |long     |계정ID|
amount               |string   |주문서 수량|
price                |string   |주문 가격|
created-at           |long     |주문 생성 시간|
type                 |string   |주문서 유형, 주문서 유형 설명을 참고 하세요.|
filled-amount        |string   |체결 수량|
filled-cash-amount   |string   |체결 총금액|
filled-fees          |string   |체결 수수료|
finished-at          |string   |체결 종료 시간|
source               |string   |주문 경로, 주문 경로 설명을 참고 하세요.|
state                |string   |주문서 상태, 주문서 상태 설명을 참고 하세요.|
cancel-at            |long     |주문 철회 시간|


## 주문서 ID로 주문서 요청

API Key 권한：읽기

주문서 ID로 주문서 데이터 요청

### 요청 데이터

`order.detail`

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

매개 변수  | 데이터 유형 | 필수여부 | 설명       | 
----------| ----------| --------| ------------------------------------------------ |--------| ---------- | ---------- 
op         | string       | true   | 작업 명칭, 고정치는 req    |
cid        | string       | true   | Client 유일한 ID 요청        |
topic      | string      | false   | 고정치는 orders.detail  |
order-id   | string       | true   | 오더 ID    |                                                


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
필드                 | 데이터 유형 |    설명|
-------------------- |--------| ------------------------------------ | ------------------------------------ 
id                   |long    | 주문서 ID |
symbol               |string   | 거래쌍 |
account-id           |long     | 계정 ID |
amount               |string   | 주문서 수량 |
price                |string   | 주문 가격 |
created-at           |long     | 주문 생성 시간 |
type                 |string   | 주문서 유형，주문서 유형 설명을 참고 하세요. |
filled-amount        |string   | 체결 수량 |
filled-cash-amount   |string   | 체결 총금액 |
filled-fees          |string   | 체결 수수료 |
finished-at          |string   | 체결 종료 시간 |
source               |string   | 주문 경로, 주문 경로 설명을 참고 하세요. |
state                |string   | 주문서 상태, 주문서 상태 설명을 참고 하세요. |
cancel-at            |long     | 주문 철회 시간 |


