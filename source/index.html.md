---
title: Huobi Korea API Reference v1.0

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - json

toc_footers:
  - <a href='https://www.huobi.co.kr/apikey/'>Sign Up for a Huobi Korea API key </a>
  - Login is required for creating an API key

includes:

search: true
---

# Introduction

## API Introduction

Welcome to Huobi Korea API！  

This is the official Huobi Korea API document, and will be continue updating, please follow us to get latest news.

You can switch to different API business in the top menu, and switch to different language by clicking the button in the top right.

The example of request and response is showing in the right hand side.

## Market Maker Program

It is very welcome for market maker who has good market making strategy and large trading volume. If your Huobi Spot account or Contract account has at least 10 BTC, you can send your email to:

- [MM-kr@huobi.com](mailto:MM-kr@huobi.com) for Huobi Korea (spot) market maker

And provide below details:

1. UID (not linked to any rebate program in any accounts)
2. Screenshot of trading volume in other transaction platform (such as trading volume within 30 days, or VIP status)
3. A brief description of your market-making strategy 

<aside class="notice">
Market makers will not be able to use point cards, VIP rate, rebate or any other fee promotion.
</aside>

## Contact Us

If you have any questions or inquiries using API, please refer to the `Inquiry Q&A` or join the `Huobi Korea API Communication Telegram Group` (http://bit.ly/2lY4o7v) for consultation. 

# Quick Start

## Preparation

Before you use API, you need to login the website to create API Key with proper permissions.

You can manage your API Key <a href='https://www.huobi.co.kr/en-us/api/'>here</a>.

Every user can create at most 20 API Key, each of them has three permissions:

- Read permission: It is used to query the data, such as order query, trade query.
- Trade permission: It is used to create order, cancel order and transfer, etc.
- Withdraw permission: It is used to create withdraw order, cancel withdraw order, etc.

Please remember below information after creation:

- `Access Key`  It is used in API request
  
- `Secret Key`  It is used to generate the signature (only visible once after creation)

<aside class="notice">
The API Key can bind maximum 20 IP addresses, we strongly suggest you bind IP address for security purpose. The API Key without IP binding will be expired after 90 days.
</aside>
<aside class="warning">
<red><b>Warning</b></red>: These two keys are important to your account safety, please don't share <b>both</b> of them together to anyone else. If you find your API Key is disposed, please remove it immediately.
</aside> 

## SDK and Demo

**SDK (Suggested)**

[Java](https://github.com/huobiapi/huobi_Java) | [Python3](https://github.com/huobiapi/huobi_Python) | [C++](https://github.com/huobiapi/huobi_Cpp) | [C#](https://github.com/HuobiRDCenter/huobi_CSharp) | [Go](https://github.com/huobirdcenter/huobi_golang)

**Other Demos**

https://github.com/huobiapi?tab=repositories

## Testnet (Stopped)

The testnet has been alive for months, however the active user count is rather low and the cost is high, after considering carefully we decide to shutdown the testnet environment.

It is suggest you use live environment, which is more stable and has more liquidity.

## Interface Type

There are two types of interface, you can choose the proper one according to your scenario and preferences.

### REST API

REST (Representational State Transfer) is one of the most popular communication mechanism under HTTP, each URL represents a type of resource.

It is suggested to use Rest API for one-off operation, like trading and withdraw.

### WebSocket API

WebSocket is a new protocol in HTML5. It is full-duplex between client and server. The connection can be established by a single handshake, and then server can push the notification to client actively.

It is suggest to use WebSocket API to get data update, like market data and order update.

**Authentication**

Both API has two levels of authentication:

Public API: It is for basic information and market data. It doesn't need authentication.

Private API: It is for account related operation like trading and account management. Each private API must be authenticated with API Key.

## Access URLs
You can compare the network latency between two domain <u>api-cloud.huobi.co.kr</u> and <u>api-cloud.huobi.co.kr</u>(AWS Tokyo), and then choose the better one for you.

In general, the domain <u>api-aws.huobi.co.kr</u> is optimized for AWS client, the latency will be lower.

**REST API**

**`https://api.huobi.co.kr`**  

**`https://api-aws.huobi.co.kr`**  

**Websocket Feed (market data except MBP incremental)**

**`wss://api.huobi.co.kr/ws`**  

**`wss://api-aws.huobi.co.kr/ws`**  

**Websocket Feed (market data only MBP incremental)**

**`wss://api.huobi.co.kr/feed`**  

**`wss://api-aws.huobi.co.kr/feed`**  

**Websocket Feed (account and order)**

**`wss://api.huobi.co.kr/ws/v1`**  

**`wss://api-aws.huobi.co.kr/ws/v1`**     

<aside class="notice">
Please initiate API calls with non-China IP.
</aside>
<aside class="notice">
It is not recommended to use proxy to access Huobi API because it will introduce high latency and low stability.
</aside>
<aside class="notice">
It is recommended to access Huobi API from AWS Japan for better stability. If your server is in China mainland, it may be not stable.
</aside> 

## Authentication

### Overview

The API request may be tampered during internet, therefore all private API must be signed by your API Key (Secrete Key).

Each API Key has permission property, please check the API permission, and make sure your API key has proper permission.

A valid request consists of below parts:

- API Path: for example <u>api-cloud.huobi.co.kr/v1/order/orders</u>
- API Access Key: The 'Access Key' in your API Key
- Signature Method: The Hash method that is used to sign, it uses **HmacSHA256**
- Signature Version: The version for the signature protocol, it uses **2**
- Timestamp: The UTC time when the request is sent, e.g. 2017-05-11T16:22:06. It is useful to prevent the request to be intercepted by third-party.
- Parameters: Each API Method has a group of parameters, you can refer to detailed document for each of them. 
  - For GET request, all the parameters must be signed.
  - For POST request, the parameters needn't be signed and they should be put in request body.
- Signature: The value after signed, it is guarantee the signature is valid and the request is not be tempered.

### Signature Method

The signature may be different if the request text is different, therefore the request should be normalized before signing. Below signing steps take the order query as an example:

This is a full URL to query one order:

`https://api-cloud.huobi.co.kr/v1/order/orders?`

`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx`

`&SignatureMethod=HmacSHA256`

`&SignatureVersion=2`

`&Timestamp=2017-05-11T15:19:30`

`&order-id=1234567890`

#### 1. The request Method (GET or POST, WebSocket use GET), append line break “\n”


`GET\n`

#### 2. The host with lower case, append line break “\n”

`api-cloud.huobi.co.kr\n`

#### 3. The path, append line break “\n”

`
/v1/order/orders\n
`

#### 4. The parameters are URI encoded, and ordered based on ASCII

For example below is the original parameters:

`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx`

`order-id=1234567890`

`SignatureMethod=HmacSHA256`

`SignatureVersion=2`

`Timestamp=2017-05-11T15%3A19%3A30`

<aside class="notice">
Use UTF-8 encoding and URI encoded, the hex must be upper case. For example, The semicolon ':' should be encoded as '%3A', The space should be encoded as '%20'.
</aside>
<aside class="notice">
The 'timestamp' should be formated as 'YYYY-MM-DDThh:mm:ss' and URI encoded.
</aside>

Then above parameter should be ordered like below:


`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx`

`SignatureMethod=HmacSHA256`

`SignatureVersion=2`

`Timestamp=2017-05-11T15%3A19%3A30`

`order-id=1234567890`

#### 5. Use char  “&” to concatenate all parameters


`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890`

#### 6. Assemble the pre-signed text

`GET\n`

`api-cloud.huobi.co.kr\n`

`/v1/order/orders\n`

`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890`


#### 7. Use the pre-signed text and your Secret Key to generate a signature

- Use the pre-signed text in above step and your API Secret Key to generate hash code by HmacSHA256 hash function.
- Encode the hash code with base-64 to generate the signature.

`4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o=`

#### 8. Put the signature into request URL

- Put all the parameters in the URL
- Append the signature in the URL, with parameter name “Signature”.

Finally, the request sent to API should be:

`https://api-cloud.huobi.co.kr/v1/order/orders?AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&order-id=1234567890&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&Signature=4F65x5A2bLyMWVQj3Aqp%2BB4w%2BivaA7n5Oi2SuYtCJ9o%3D`

For WebSocket interface:

1. Fill the value according to required JSON schema
2. The value in JSON doesn't require URL encode

For example:

`{
    "action": "req", 
    "ch": "auth",
    "params": { 
        "authType":"api",
        "accessKey": "e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx",
        "signatureMethod": "HmacSHA256",
        "signatureVersion": "2.1",
        "timestamp": "2019-09-01T18:16:16",
        "signature": "4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o="
    }
}`

## Sub User

Sub user can be used to isolate the assets and trade, the assets can be transferred between parent and sub users. Sub user can only trade with the sub user. The assets can not be transferred between sub users, only parent user has the transfer permission.  

Sub user has independent login password and API Key, they are managed under parent user in website.

Each parent user can create **200** sub user, each sub user can create at most **5** API Key, each API key can have two permissions: **read** and **trade**.

The sub user API Key can also bind IP address, the expiry policy is the same with parent user.

You can access <a href='https://www.huobi.co.kr/en-us/sub-account/'>here</a> to create and manage sub user.

The sub user can access all public API (including basic information and market data), below are the private APIs that sub user can access:

| API                                                          | Description                              |
| ------------------------------------------------------------ | ---------------------------------------- |
| [POST /v1/order/orders/place](#fd6ce2a756)                   | Create and execute an order              |
| [POST /v1/order/orders/{order-id}/submitcancel](#4e53c0fccd) | Cancel an order                          |
| [POST /v1/order/orders/submitCancelClientOrder](#submit-cancel-for-an-order-based-on-client-order-id) | Cancel an Order based on client order ID |
| [POST /v1/order/orders/batchcancel](#ad00632ed5)             | Cancel multiple orders                   |
| [POST /v1/order/orders/batchCancelOpenOrders](#open-orders)  | Cancel the open orders                   |
| [GET /v1/order/orders/{order-id}](#92d59b6aad)               | Query a specific order                   |
| [GET /v1/order/orders](#d72a5b49e7)                          | Query orders with criteria               |
| [GET /v1/order/openOrders](#95f2078356)                      | Query open orders                        |
| [GET /v1/order/matchresults](#0fa6055598)                    | Query the order matching result          |
| [GET /v1/order/orders/{order-id}/matchresults](#56c6c47284)  | Query a specific order matching result   |
| [GET /v1/account/accounts](#bd9157656f)                      | Query all accounts in current user       |
| [GET /v1/account/accounts/{account-id}/balance](#870c0ab88b) | Query the specific account balance       |
| [POST /v1/futures/transfer](#e227a2a3e8)                     | Transfer with future account             |
| [GET /v1/account/history](#get-account-history)              | Query account history                    |
| [GET /v2/account/ledger](#get-account-ledger)                | Query account ledger                     |
| [POST /v1/account/transfer](#asset-transfer)                 | Asset Transfer                           |
| [GET /v2/point/account](#get-point-balance)                  | Query Point Balance                      |
| [POST /v2/point/transfer](#point-transfer)                   | Point Transfer                           |

<aside class="notice">
All other APIs couldn't be accessed by sub user, otherwise the API will return “error-code 403”。
</aside>

## Glossary

### Trading symbols

The trading symbols are consist of base currency and quote currency. Take the symbol `BTC/USDT` as an example, `BTC` is the base currency, and `USDT` is the quote currency.  

The corresponding field of the base currency is base-currency.

The corresponding field of the quote currency is quote-currency.

### Account

The `account-id` defines the Identity for different business type, it can be retrieved from API `/v1/account/accounts` , where the `account-type` is the business types.
The types include:

* spot: Spot account
* point: Point card account

### Identity

* order-id: The unique identity for order.
* client-order-id: The identity that defined by client. This id is included in order creation request, and will be returned as order-id. This id is valid within 24 hours. The allowed characters are letters (case sensitive), digit, underscore (_) and hyphen (-), no more than 64 chars.
* match-id : The identity for order matching.
* trade-id : The unique identity for the trade.

### Order Type
The order type is consist of trade direction and order classification. Take `buy-market` as an example, the trade direction is `buy`, the operation type is `market`.  

Trade direction includes:

* buy
* sell  

Order classification includes:

* limit: Both of the price and amount should be specified in order creation request.
* market : The price is not required in order creation request, you only need to specify either money or amount. The matching and trade will happen automatically according to the request.
* limit-maker: The price is specified in order creation request as market maker. It will not be matched in the matching queue.
* ioc: ioc stands for "immediately or cancel", it means the order will be canceled if it couldn't be matched. If the order is partially traded, the remaining part will be canceled.
* stop-limit: The price in order creation request is the trigger price. The order will be put into matching queue only when the market price reaches the trigger price.

### Order Status

* submitted: The order is submitted, and already in the matching queue, waiting for deal.
* partial-filled: The order is already in the matching queue and partially traded, and is waiting for further matching and trade.
* filled: The order is already traded and not in the matching queue any more.
* partial-canceled: The order is not in the matching queue any more. The status is transferred from 'partial-filled', the order is partially trade, but remaining is canceled.
* canceled: The order is not in the matching queue any more, and completely canceled. There is no trade associated with this order.
* canceling: The order is under canceling, but haven't been removed from matching queue yet.

# API Access

## Overview

Category| URL Path | Description 
--------- | --------- | -----------
Common |/v1/common/* | Common interface, including currency, currency pair, timestamp, etc 
Market Data |/market/*| Market data interface, including trading, depth, quotation, etc 
Account |/v1/account/*  /v1/subuser/* | Account interface, including account information, sub-account ,etc 
Order |/v1/order/* | Order interface, including order creation, cancellation, query, etc 

Above is a general category, it doesn't cover all API, you can refer to detailed API document according to your requirement.

## New Version Rate limit Rule

- Only those endpoints marked with rate limit value and a bracketed 'NEW' are applied with new rate limit rule.<br>
- The new version rate limit is applied on UID basis, which means, the overall access rate, from all API keys under same UID, to single endpoint, shouldn’t exceed the rate limit applied on that endpoint.<br>
- It is suggested to read HTTP Header `X-HB-RateLimit-Requests-Remain` and `X-HB-RateLimit-Requests-Expire` to get the remaining count of request and the expire time for current rate limit time window, then you can adjust the API access rate dynamically.<br>

## Rate Limiting Rule

Except those endpoints which are marked with new rate limit value separately, following rate limit rules are applicable -

* Each API Key is limited to 10 times per second
* If API Key is empty in request, then each IP is limited to 10 times per second

For example

* Order interface is limited by API Key: no more than 10 times within 1 sec
* Market data interface is limited by IP: no more than 10 times within 1 sec

## Request Format

The API is restful and there are two method: GET and POST.

* GET request: All parameters are included in URL
* POST request: All parameters are formatted as JSON and put int the request body

## Response Format

The response is JSON format.There are four fields in the top level: `status`, `ch`, `ts` and `data`. The first three fields indicate the general status, the business data is is under `data` field.

Below is an example of response:

```json
{
  "status": "ok",
  "ch": "market.btcusdt.kline.1day",
  "ts": 1499223904680,
  "data": // per API response data in nested JSON object
}
```

Field| Data Type | Description 
--------- | --------- | -----------
status    | string    | Status of API response 
ch        | string    | The data stream. It may be empty as some API doesn't have data stream 
ts        | int       | The UTC timestamp when API respond, the unit is millisecond 
data      | object    | The body data in response 

## Data Type

The JSON data type described in this document is defined as below:

- `string`: a sequence of characters that are quoted
- `int`: a 32-bit integer, mainly used for status code, size and count
- `long`: a 64-bit integer, mainly used for Id and timestamp
- `float`: a fraction represented in decimal format, mainly used for volume and price, recommend to use high precision decimal data types in program

## Best Practice

### Security

- It is strongly suggested to bind your IP with your API Key to ensure that your API Key can only be used in your machine. Furthermore, your API Key will be expired after 90 days if it is not binded with any IP.
- It is strongly suggested not to share your API Key with any body or third-party software, otherwise your personal information and asset may be stolen. If your expose your API Key by accident, please do delete the API Key and create a new one.

### General

**API Access**

- It is suggested not to use temporary domain or proxy, which may be not stable.
- It is suggested to use AWS Japan to access API for lower latency
- It is suggested to connect to domain `api-aws.huobi.co.kr` if your server is based on AWS, because this domain is optimized for AWS client, the latency will be lower.

**New Version Rate limit Rule**

- Only those endpoints marked with rate limit value separately are applied with new rate limit rule.
- It is suggested to read HTTP Header `X-HB-RateLimit-Requests-Remain` and `X-HB-RateLimit-Requests-Expire` to get the remaining count of request and the expire time for current rate limit time window, then you can adjust the API access rate dynamically.
- The overall access rate, from all API keys under same UID, to single endpoint, shouldn’t exceed the rate limit applied on that endpoint.

### Market

**Market data**

- It is suggested to use WebSocket interface to subscribe the market update and then cache the data locally, because WebSocket notification has lower latency and not have rate limit.
- It is suggested not to subscribe too many topics in a single websocket connection, it may generate more notifications and cause network latency and disconnection.

**Latest trade**

- It is suggested to subscribe WebSocket topic `market.$symbol.trade.detail`, the response field `price` represents the latest price, and it has lower latency.
- It is suggested to use `tradeId` to de-duplicate if you subscribe WebSocket topic `market.$symbol.trade.detail`.

**Depth**

- It is suggested to subscribe WebSocket topic `market.$symbol.bbo` if you only need the best bid and best offer.
- It is suggested to subscribe WebSocket topic `market.$symbol.depth.$type` if you need multiple bid and offer with normal latency.
- It is suggested to subscribe WebSocket topic `market.$symbol.mbp.$level` if you need multiple bid and offer with lower latency
- It is suggested to use `version` field to de-duplicate and discard the smaller data if you use Rest interface `/market/depth` and WebSocket topic `market.$symbol.depth.$type`. It is suggest to use `seqNum` to de-duplicate and discard the smaller data if yo subscribe WebSocket topic `market.$symbol.mbp.$levels`.

### Order

**Place an order (/v1/order/orders/place)**

- It is suggested to follow the symbol reference (`/v1/common/symbols`) to validate the amount and value before placing the older, otherwise you may place an invalid order and waste your time.
- It is suggested to provide an unique `client-order-id` field when placing the order, it is useful to track your orders status if you fail to get the order id response. Later you can use the `client-order-id` to match the WebSocket order notification or query order detail by interface `/v1/order/orders/getClientOrder`.

**Search history olders (/v1/order/orders)**

- It is recommended to use `start-time` and `end-time` to query, that are two timestamps with 13 digits (millisecond). The maximum query time window is 48 hours (2 days), the more precision you provide, the better performance you will get. You can query for multiple iterations. 

**Order update**

- It is suggested to subscribe WebSocket topic `orders.$symbol.update`, which has lower latency and more accurate sequence.
- It is suggested not to subscribe WebSocket topic `orders.$symbol`,which is replaced by `orders.$symbol.update`, and will be retired later.

### Account

**Asset update**

- It is suggested to subscribe both WebSocket topic `orders.$symbol.update` and `account.update#${mode}`. The former one tells the order status update and arrives earlier than the latter one, and the latter one confirms the final asset balance.
- It is suggested not to subscribe WebSocket topic `accounts`, which is replaced by `accounts.update#${mode}`, and will be retired later.

# Frequently Asked Questions

## API Announcements

Huobi will publish API announcement in advance for any API change, please subscribe our announcements so that you can get latest update. 

How to subscribe: Click <a href='https://support.huobi.co.kr/hc/ko/sections/360007543852-API-%EA%B3%B5%EC%A7%80%EC%82%AC%ED%95%AD'>link</a>, click "Follow" button in the top right of the page, then choose the type you want to follow. After you subscribe, the button will changed to "Following". If you don't have any account, you need to register first. 

## Access and Authentication

### Q1：How many API Keys one user can apply?
A:  Every user can create 5 API Keys, and each API Key can be granted with 3 permissions: **read**, **trade** and **withdraw**.
Each user could create up to 200 sub users, and each sub user could create 5 API Keys, each API key can be granted with 2 permissions: **read** and **trade**.

Below are the explanation for permissions:

1、Read permission: It is used to query data, for example, **query orders**, **query trades**. 

2、Trade permission: it is used to **place order**, **cancel order** and **transfer**.

3、Withdraw permission: it is used to **withdraw**, **cancel withdraw**.

### Q2：Why APIs are always disconnected or timeout?
A：Please follow below suggestions:

1、It is unstable if the client's server locates in China mainland, it is suggested to invoke API from a server at AWS Japan.

2、It is suggested to invoke API only to host <u>api-cloud.huobi.co.kr</u> or <u>api-was.huobi.co.kr</u>.

### Q3：Why the WebSocket is often disconnected?
A：Please check below possible reasons:

1、The client didn't respond 'Pong'. It is requird to respond 'Pong' after receive 'Ping' from server.

2、The server didn't receive 'Pong' successfully due to network issue.

3、The connection is broken due to network issue.

4、It is suggested to implement WebSocket re-connect mechanism. If Ping/Pong works well but the connection is broken, the application should be able to re-connect automatically.

### Q4：Why the signature authentication always fail?
A：Please compare  your signature text with below example: 

`GET\n`

`api-cloud.huobi.co.kr\n`

`/v1/account/accounts\n`

`AccessKeyId=rfhxxxxx-950000847-boooooo3-432c0&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2019-10-28T07%3A28%3A38`

Please check whether you follow below rules:

1、The parameter in signature text should be ordered by ASCII, for example below is the original parameters:

`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx`

`order-id=1234567890`

`SignatureMethod=HmacSHA256`

`SignatureVersion=2`

`Timestamp=2017-05-11T15%3A19%3A30`

They should be ordered like below:

`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx`

`SignatureMethod=HmacSHA256`

`SignatureVersion=2`

`Timestamp=2017-05-11T15%3A19%3A30`

`order-id=1234567890`

2、The signature text should be URI encoded, for example

- The semicolon `:`should be encoded as `%3A`, The space should be encoded as `%20`.
- The timestamp should be formatted as `YYYY-MM-DDThh:mm:ss` and after encoded it should be like `2017-05-11T15%3A19%3A30`  

3、The signature should be base64 encoded.

4、The parameter for Get request should be included in signature request.

5、The Timestamp should be UTC time and the format should be YYYY-MM-DDTHH:mm:ss.

6、The time difference between your timestamp and standard should be less than 1 minute.

7、The message body doesn't need URI encoded if you are using WebSocket for authentication.

8、The host in signature text should be the same as the host in your API request.

9、The hidden text in API Key and Secret Key may have impact on the signature.

Right now the official SDK supports 3 language: Java, Python3 and C++, you can choose the one that suitable for you. 

<a href='https://github.com/HuobiRDCenter'>Download SDK </a>    

<a href='https://github.com/HuobiRDCenter/huobi_Python/blob/master/example/python_signature_demo.md'>Python signature example</a>   

<a href='https://github.com/HuobiRDCenter/huobi_Java/blob/master/java_signature_demo.md'>JAVA signature example</a>   

<a href='https://github.com/HuobiRDCenter/huobi_Cpp/blob/master/examples/cpp_signature_demo.md'>C++ signature example</a>   

### Q5：Why the API return 'gateway-internal-error'?
A：Please check below possible reasons:

1、Check the `account-id`, it could be returned from `GET /v1/account/accounts`.

2、It may be due to network issue, please try again later.

3、The data format should be correct (standard JSON).

4、The `Content-Type` in POST header should be `application/json` .

### Q6：Why the API return 'login-required'?
A：Please check below possible reasons:

1、The parameter should include `AccessKeyId`.

2、The parameter should include `Signature`.

3、Check the `account-id` it could be returned from `GET /v1/account/accounts`.

4、It may happen if previous request reach the rate limitation.

### Q7: Why the API return HTTP 405 'method-not-allowed'?

A: It indicates the request path doesn't exist, please check the path spelling carefully. Due to the Nginx setting, the request path is case sensitive, please follow the path definition in document.

## Market Data

### Q1：What is the update frequency of market depth?
A：The data is updated **once per second**. But, the BBO (Best Bid/Offer) feed upon WebSocket subscription to `market.$symbol.bbo` is updating in tick by tick mode.

### Q2：Could the total volume of Last 24h Market Summary (GET /v1/market/detail) decrease?
A：Yes, it is possible that the accumulated volume and the accumulated value counted for current 24h window is smaller than previous.

### Q3：How to retrieve the last trade price in market?
A：It is suggested to request to `GET /v1/market/trade` to get last market price, or to subscribe WebSocket topic `market.$symbol.trade.detail` for getting the same.

### Q4：Which timezone the start time of candlesticks falls into?
A： The start time for candlesticks is based on Singapore time (GMT+8), for example, the duration for daily candlesticks is from 00:00:00 to 23:59:59 Singapore time.

## Order and Trade

### Q1：What is account-id?
A： The `account-id` defines the Identity for different business type, it can be retrieved from API `/v1/account/accounts` , where the `account-type` is the business types.

The types include:

- spot: Spot account
- point: Point card account

### Q2：What is client-order-id?
A： The `client-order-id` is an optional request parameter while placing order. It's string type which maximum length is 64. The client order id is generated by client, and is only valid within 24 hours.

### Q3：How to get the order size, price and decimal precision?
A： You can call API `GET /v1/common/symbols` to get the currency pair information, pay attention to the difference between the minimum amount and the minimum price.   

Below are common errors:

- order-value-min-error: The order price is less than minimum price
- order-orderprice-precision-error : The precision for limited order price is wrong 
- order-orderamount-precision-error : The precision for limited order amount is wrong
- order-limitorder-price-max-error : The limited order price is higher than the threshold
- order-limitorder-price-min-error : The limited order price is lower than the threshold
- order-limitorder-amount-max-error : The limited order amount is larger than the threshold
- order-limitorder-amount-min-error : The limited order amount is smaller than the threshold  

### Q4：What is the difference between two WebSocket topic 'orders.\$symbol' and 'orders.\$symbol.update'?
A： Below are the difference:

1、The topic `order.$symbol` is the legacy version, which will be no longer supported in the near future. It is strongly recommended to subscribe topic `orders.$symbol.update` instead for getting order updates.

2、The update message sequence of `orders.$symbol.update` strictly follows transaction time, with lower latency.

3、In order to reduce latency, the topic `orders.$symbol.update` doesn't include original order details and transaction fee etc. If you require the original order information or transaction fee details, you may query to corresponding REST API endpoint.

### Q5：Why I got insufficient balance error while placing an order just after a successful order matching?
A：The time order matching update being sent down, the clearing service of that order may be still in progress at backend. Suggest to follow either of below to ensure a successful order submission:

1、Subscribe to WebSocket topic `accounts` for getting account balance moves to ensure the completion of asset clearing.

2、Check account balance from REST endpoint to ensure sufficient available balance for the next order submission.

3、Leave sufficient balance in your account.

### Q6: What is the difference between 'filled-fees' and 'filled-points' in match result?
A: Transaction fee can be paid from either of below.

1、filled-fees: Filled-fee is also called transaction fee. It's charged from your income currency from the transaction. For example, if your purchase order of BTC/USDT got matched，the transaction fee will be based on BTC.

2、filled-points: If user enabled transaction fee deduction, the fee should be charged from either HT or Point. User could refer to field `fee-deduct-currency` to get the exact deduction type of the transaction.

### Q7: What is the difference between 'match-id' and 'trade-id' in matching result?
A: The `match-id` is the identity for order matching, while the `trade-id` is the unique identifier for each trade. One `match-id` may be correlated with multiple `trade-id`, or no `trade-id`(if the order is cancelled). For example, if a taker's order got matched with 3 maker's orders at the same time, it generates 3 trade IDs but only one match ID.

### Q8: Why the order submission could be rejected even though the order price is set as same as current best bid (or best ask)?
A: For some extreme illiquid trading symbols, the best quote price at particular time might be far away from last trade price. But the price limit is actually based on last trade price which could possibly exclude best quote price from valid range for any new order.

## Deposit and Withdraw

### Q1：Why the API returns error 'api-not-support-temp-addr' when withdrawing?
A：User has to include the address into the pre-defined address table on Huobi Korea official website before withdrawing through API.

### Q2：Why the API returns error 'Invaild-Address' when withdraw USDT?
A：USDT locates on multiple chains, therefore the withdraw order should clearly specify which chain the withdrawal goes to. See the table below:

| Chain           | Field Value |
| --------------- | --------------- |
| ERC20 (default) | `usdterc20`     |
| OMNI            | `usdt`          |
| TRX             | `trc20usdt`     |

If leaving the field empty, default target chain is `ERC20`, or you can explicitly set the chain to `usdterc20`.

If the target chain is `OMNI` or `TRX`, the field value should be `usdt` or `trc20usdt`.

The full chain name list for all currencies can be retrieved from endpoint `GET /v2/reference/currencies`.

### Q3：How to specify 'fee' when creating a withdraw request?

A：Please refer to the response from endpoint `GET /v2/reference/currencies`, where the field `withdrawFeeType` defining different fee types below: 

- transactFeeWithdraw : The withdraw fee per request (only applicable when withdrawFeeType=fixed).      
- minTransactFeeWithdraw : The minimum withdraw fee per request (only applicable when withdrawFeeType=circulated).
- maxTransactFeeWithdraw : The maximum withdraw fee per request (only applicable when withdrawFeeType=circulated or ratio).
- transactFeeRateWithdraw : The withdraw fee rate per request (only applicable when withdrawFeeType=ratio).

### Q4：How to query my withdraw quota?
A：Please refer to the response from endpoint `GET /v2/account/withdraw/quota`, where quota per request, daily quota, annual quota, overall quota are available.

Note: If you need to withdraw large amount which breaking the limitation, you can contact our official support ( API-kr@huobi.com) for assistance.

# Reference Data

## Introduction

Reference data APIs provide public reference information such as system status, market status, symbol info, currency info, chain info and server timestamp.

## Get system status

This endpoint allows users to get system status, Incidents and planned maintenance.

```shell
curl "https://status.huobigroup.com/api/v2/summary.json"
```

### HTTP Request

- GET `https://status.huobigroup.com/api/v2/summary.json`

### Request Parameters

No parameter is available for this endpoint.

> Response:

```json
{
  "page": {  // Basic information of huobi spot status page
    "id": "p0qjfl24znv5",  // page id
    "name": "Huobi",  // page name
    "url": "https://status.huobigroup.com", // page url
    "time_zone": "Etc/UTC", // time zone
    "updated_at": "2020-02-07T10:25:14.717Z" // page update time
  },
  "components": [  // System components and their status
    {
      "id": "h028tnzw1n5l",  // component id
      "name": "Deposit", // component name
      "status": "operational", // component status
      "created_at": "2019-12-05T02:07:12.372Z",  // component create time
      "updated_at": "2020-02-07T09:27:15.563Z", // component update time
      "position": 1,
      "description": null,
      "showcase": true,
      "group_id": "gtd0nyr3pf0k",  
      "page_id": "p0qjfl24znv5", 
      "group": false,
      "only_show_if_degraded": false
    }
  ],
  "incidents": [ // System fault incidents and their status
        {
            "id": "rclfxz2g21ly",  // incident id
            "name": "Market data is delayed",  // incident name
            "status": "investigating",  // incident stutus
            "created_at": "2020-02-11T03:15:01.913Z",  // incident create time
            "updated_at": "2020-02-11T03:15:02.003Z",   // incident update time
            "monitoring_at": null,
            "resolved_at": null,
            "impact": "minor",  // incident impact
            "shortlink": "http://stspg.io/pkvbwp8jppf9",
            "started_at": "2020-02-11T03:15:01.906Z",
            "page_id": "p0qjfl24znv5",
            "incident_updates": [ 
                {
                    "id": "dwfsk5ttyvtb",   
                    "status": "investigating",   
                    "body": "Market data is delayed", 
                    "incident_id": "rclfxz2g21ly",   
                    "created_at": "2020-02-11T03:15:02.000Z",    
                    "updated_at": "2020-02-11T03:15:02.000Z",  
                    "display_at": "2020-02-11T03:15:02.000Z",   
                    "affected_components": [  
                        {
                            "code": "nctwm9tghxh6",  
                            "name": "Market data",  
                            "old_status": "operational",  
                            "new_status": "degraded_performance"  
                        }
                    ],
                    "deliver_notifications": true,
                    "custom_tweet": null,
                    "tweet_id": null
                }
            ],
            "components": [  
                {
                    "id": "nctwm9tghxh6",   
                    "name": "Market data",  
                    "status": "degraded_performance", 
                    "created_at": "2020-01-13T09:34:48.284Z", 
                    "updated_at": "2020-02-11T03:15:01.951Z", 
                    "position": 8,
                    "description": null,
                    "showcase": false,
                    "group_id": null,
                    "page_id": "p0qjfl24znv5",
                    "group": false,
                    "only_show_if_degraded": false
                }
            ]
        }
    ],
      "scheduled_maintenances": [  // System scheduled maintenance events and their status
        {
            "id": "k7g299zl765l", // incident id
            "name": "Schedule maintenance", // incident name
            "status": "scheduled", // incident status
            "created_at": "2020-02-11T03:16:31.481Z",  // incident create time
            "updated_at": "2020-02-11T03:16:31.530Z",  // incident update time
            "monitoring_at": null,
            "resolved_at": null,
            "impact": "maintenance",  // incident impact
            "shortlink": "http://stspg.io/md4t4ym7nytd",
            "started_at": "2020-02-11T03:16:31.474Z",
            "page_id": "p0qjfl24znv5",
            "incident_updates": [  
                {
                    "id": "8whgr3rlbld8",  
                    "status": "scheduled", 
                    "body": "We will be undergoing scheduled maintenance during this time.", 
                    "incident_id": "k7g299zl765l",  
                    "created_at": "2020-02-11T03:16:31.527Z",  
                    "updated_at": "2020-02-11T03:16:31.527Z",  
                    "display_at": "2020-02-11T03:16:31.527Z",  
                    "affected_components": [  
                        {
                            "code": "h028tnzw1n5l",  
                            "name": "Deposit And Withdraw - Deposit",  
                            "old_status": "operational",  
                            "new_status": "operational" 
                        }
                    ],
                    "deliver_notifications": true,
                    "custom_tweet": null,
                    "tweet_id": null
                }
            ],
            "components": [  
                {
                    "id": "h028tnzw1n5l",  
                    "name": "Deposit", 
                    "status": "operational", 
                    "created_at": "2019-12-05T02:07:12.372Z",  
                    "updated_at": "2020-02-10T12:34:52.970Z",  
                    "position": 1,
                    "description": null,
                    "showcase": false,
                    "group_id": "gtd0nyr3pf0k",
                    "page_id": "p0qjfl24znv5",
                    "group": false,
                    "only_show_if_degraded": false
                }
            ],
            "scheduled_for": "2020-02-15T00:00:00.000Z",  // scheduled maintenance start time
            "scheduled_until": "2020-02-15T01:00:00.000Z"  // scheduled maintenance end time
        }
    ],
    "status": {  // The overall current status of the system
        "indicator": "minor",   // system indicator
        "description": "Partially Degraded Service"  // system description
    }
}
```

### Response Content

|Field | Data Type | Description
|--------- |  -----------|  -----------
|page    |                     | basic information of huobi spot status page
|{id        |  string                   | page id
|name      |      string                | page name
|url     |    string                  | page url
|time_zone     |     string                 | time zone
|updated_at}     |    string                  | page update time
|components  |                      | System components and their status
|[{id        |  string                    | component id
|name        |    string                  | component name, including Order submission, Order cancellation, Deposit etc.
|status        |    string                  | component status, value range: operational, degraded_performance, partial_outage, major_outage, under maintenance
|created_at        |    string                  | component create time
|updated_at        |    string                  | component update time
|.......}]        |                     | for details of other fields, please refer to the return example
|incidents  |           | System fault incident and their status. If there is no fault at present, it will return to null
|[{id        |       string               | incident id
|name        |      string                | incident name
|status        |     string                 | incident staus, value range: investigating, identified, monitoring, resolved
|created_at        |       string               | incident creat time
|updated_at        |      string                | incident update time
|.......}]        |                     | for details of other fields, please refer to the return example
|scheduled_maintenances|                     | System scheduled maintenance incident and status. If there is no scheduled maintenance at present, it will return to null
|[{id        |     string                 |  incident id
|name        |      string                | incident name
|status        |       string               | incident staus, value range: scheduled, in progress, verifying, completed
|created_at        |     string                 | incident creat time
|updated_at        |     string                 | incident update time
|scheduled_for       |      string                | scheduled maintenance start time
|scheduled_until       |     string                 | scheduled maintenance end time
|.......}]        |                     | for details of other fields, please refer to the return example
|status   |                       | The overall current status of the system
|{indicator        |    string                  | system indicator, value range: none, minor, major, critical, maintenance
|description}     |      string                | system description, value range: All Systems Operational, Minor Service Outager, Partial System Outage, Partially Degraded Service, Service Under Maintenance

## Get Market Status

The endpoint returns current market status<br>
The enum values of market status includes: 1 - normal (order submission & cancellation are allowed)，2 - halted (order submission & cancellation are prohibited)，3 - cancel-only(order submission is prohibited but order cancellation is allowed).<br>
Halt reason includes: 2 - emergency maintenance，3 - schedule maintenance.<br>

```shell
curl "https://api.huobi.co.kr/v2/market-status"
```

### HTTP Request

- GET `/v2/market-status`

### Request Parameter

None.

> Responds:

```json
{
    "code": 200,
    "message": "success",
    "data": {
        "marketStatus": 1
    }
}
```

### Response Content

|	Field	|	Data Type	|	Mandatory	|	Description	|
|	-----	|	---------	|	--------	|	-----------	|
|	code	|	integer	|	TRUE	|	Status code	|
|	message	|	string	|	FALSE	|	Error message (if any)	|
|	data	|	object	|	TRUE	|		|
|	{ marketStatus	|	integer	|	TRUE	|	Market status (1=normal, 2=halted, 3=cancel-only) 	|
|	haltStartTime	|	long	|	FALSE	|	Halt start time (unix time in millisecond) , only valid for marketStatus=halted or cancel-only	|
|	haltEndTime	|	long	|	FALSE	|	Estimated halt end time (unix time in millisecond) , only valid for marketStatus=halted or cancel-only; if this field is not returned during marketStatus=halted or cancel-only, it implicates the halt end time cannot be estimated at this time.	|
|	haltReason	|	integer	|	FALSE	|	Halt reason (2=emergency-maintenance, 3=scheduled-maintenance) , only valid for marketStatus=halted or cancel-only	|
|	affectedSymbols }	|	string	|	FALSE	|	Affected symbols, separated by comma. If affect all symbols just respond with value ‘all’. Only valid for marketStatus=halted or cancel-only	|

## Get all Supported Trading Symbol

This endpoint returns all Huobi Korea's supported trading symbol.

```shell
curl "https://api-cloud.huobi.co.kr/v1/common/symbols"
```

### HTTP Request

`GET /v1/common/symbols`

### Request Parameters

No parameter is needed for this endpoint.

> Responds:

```json
{
    "status": "ok",
    "data": [
        {
           "base-currency": "btc",
            "quote-currency": "usdt",
            "price-precision": 2,
            "amount-precision": 6,
            "symbol-partition": "main",
            "symbol": "btcusdt",
            "state": "online",
            "value-precision": 8,
            "min-order-amt": 0.0001,
            "max-order-amt": 1000,
            "min-order-value": 5,
            "limit-order-min-order-amt": 0.0001,
            "limit-order-max-order-amt": 1000,
            "sell-market-min-order-amt": 0.0001,
            "sell-market-max-order-amt": 100,
            "buy-market-max-order-value": 1000000,
            "leverage-ratio": 5,
            "super-margin-leverage-ratio": 3,
            "funding-leverage-ratio": 3,
            "api-trading": "enabled"
        },
    ......
    ]
}
```

### Response Content

| Parameter                  | Required | Data Type | Description                                                  |
| -------------------------- | -------- | --------- | ------------------------------------------------------------ |
| base-currency              | true     | string    | Base currency in a trading symbol                            |
| quote-currency             | true     | string    | Quote currency in a trading symbol                           |
| price-precision            | true     | integer   | Quote currency precision when quote price(decimal places)）  |
| amount-precision           | true     | integer   | Base currency precision when quote amount(decimal places)）  |
| symbol-partition           | true     | string    | Trading section, possible values: [main，innovation]         |
| symbol                     | true     | string    | symbol                                                       |
| state                      | true     | string    | The status of the symbol；Allowable values: [online，offline,suspend]. "online" - Listed, available for trading, "offline" - de-listed, not available for trading， "suspend"-suspended for trading, "pre-online" - to be online soon |
| value-precision            | true     | integer   | Precision of value in quote currency (value = price * amount) |
| min-order-amt              | true     | float     | Minimum order amount of limit order in base currency (to be obsoleted) |
| max-order-amt              | true     | float     | Max order amount of limit order in base currency (to be obsoleted) |
| limit-order-min-order-amt  | true     | float     | Minimum order amount of limit order in base currency (NEW)   |
| limit-order-max-order-amt  | true     | float     | Max order amount of limit order in base currency (NEW)       |
| sell-market-min-order-amt  | true     | float     | Minimum order amount of sell-market order in base currency (NEW) |
| sell-market-max-order-amt  | true     | float     | Max order amount of sell-market order in base currency (NEW) |
| buy-market-max-order-value | true     | float     | Max order value of buy-market order in quote currency (NEW)  |
| min-order-value            | true     | float     | Minimum order value of limit order and buy-market order in quote currency |
| max-order-value            | false    | float     | Max order value of limit order and buy-market order in usdt (NEW) |
| api-trading                | true     | string    | API trading enabled or not (possible value: enabled, disabled) |


## Get all Supported Currencies

This endpoint returns all Huobi Korea's supported trading currencies.

```shell
curl "https://api-cloud.huobi.co.kr/v1/common/currencys"
```

### HTTP Request

`GET /v1/common/currencys`

### Request Parameters

No parameter is needed for this endpoint.

> Response:

```json
  "data": [
    "usdt",
    "eth",
    "etc"
  ]
```

### Response Content

<aside class="notice">The returned "data" field contains a list of string with each string represents a suppported currency.</aside>
## APIv2 - Currency & Chains

API user could query static reference information for each currency, as well as its corresponding chain(s). (Public Endpoint)

### HTTP Request

`GET https://api.huobi.co.kr/v2/reference/currencies`

```shell
curl "https://api.huobi.co.kr/v2/reference/currencies?currency=usdt"
```

### Request Parameters

| Field Name     | Mandatory | Data Type | Description     | Value Range                                                  |
| -------------- | --------- | --------- | --------------- | ------------------------------------------------------------ |
| currency       | false     | string    | Currency        | btc, ltc, bch, eth, etc ...(available currencies in Huobi Korea) |
| authorizedUser | false     | boolean   | Authorized user | true or false (if not filled, default value is true)         |

> Response:

```json
{
    "code":200,
    "data":[
        {
            "chains":[
                {
                    "chain":"trc20usdt",
                    "displayName":"",
                    "baseChain": "TRX",
                    "baseChainProtocol": "TRC20",
                    "isDynamic": false,
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
                    "displayName":"",
                    "baseChain": "BTC",
                    "baseChainProtocol": "OMNI",
                    "isDynamic": false,
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
                    "displayName":"",
                    "baseChain": "ETH",
                    "baseChainProtocol": "ERC20",
                    "isDynamic": false,
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

### Response Content

| Field Name              | Mandatory | Data Type | Description                                                  | Value Range            |
| ----------------------- | --------- | --------- | ------------------------------------------------------------ | ---------------------- |
| code                    | true      | int       | Status code                                                  |                        |
| message                 | false     | string    | Error message (if any)                                       |                        |
| data                    | true      | object    |                                                              |                        |
| { currency              | true      | string    | Currency                                                     |                        |
| { chains                | true      | object    |                                                              |                        |
| chain                   | true      | string    | Chain name                                                   |                        |
| displayName             | true      | string    | Chain display name                                           |                        |
| baseChain               | false     | string    | Base chain name                                              |                        |
| baseChainProtocol       | false     | string    | Base chain protocol                                          |                        |
| isDynamic               | false     | boolean   | Is dynamic fee type or not (only applicable to withdrawFeeType = fixed) | true,false             |
| numOfConfirmations      | true      | int       | Number of confirmations required for deposit success (trading & withdrawal allowed once reached) |                        |
| numOfFastConfirmations  | true      | int       | Number of confirmations required for quick success (trading allowed but withdrawal disallowed once reached) |                        |
| minDepositAmt           | true      | string    | Minimal deposit amount in each request                       |                        |
| depositStatus           | true      | string    | Deposit status                                               | allowed,prohibited     |
| minWithdrawAmt          | true      | string    | Minimal withdraw amount in each request                      |                        |
| maxWithdrawAmt          | true      | string    | Maximum withdraw amount in each request                      |                        |
| withdrawQuotaPerDay     | true      | string    | Maximum withdraw amount in a day (Singapore timezone)        |                        |
| withdrawQuotaPerYear    | true      | string    | Maximum withdraw amount in a year                            |                        |
| withdrawQuotaTotal      | true      | string    | Maximum withdraw amount in total                             |                        |
| withdrawPrecision       | true      | int       | Withdraw amount precision                                    |                        |
| withdrawFeeType         | true      | string    | Type of withdraw fee (only one type can be applied to each currency) | fixed,circulated,ratio |
| transactFeeWithdraw     | false     | string    | Withdraw fee in each request (only applicable to withdrawFeeType = fixed) |                        |
| minTransactFeeWithdraw  | false     | string    | Minimal withdraw fee in each request (only applicable to withdrawFeeType = circulated or ratio) |                        |
| maxTransactFeeWithdraw  | false     | string    | Maximum withdraw fee in each request (only applicable to withdrawFeeType = circulated or ratio) |                        |
| transactFeeRateWithdraw | false     | string    | Withdraw fee in each request (only applicable to withdrawFeeType = ratio) |                        |
| withdrawStatus}         | true      | string    | Withdraw status                                              | allowed,prohibited     |
| instStatus }            | true      | string    | Instrument status                                            | normal,delisted        |

### Status Code

| Status Code | Error Message                       | Scenario            |
| ----------- | ----------------------------------- | ------------------- |
| 200         | success                             | Request successful  |
| 500         | error                               | System error        |
| 2002        | invalid field value in "field name" | Invalid field value |

## Get Current System Time

This endpoint returns the current timestamp, i.e. the number of **milliseconds** that have elapsed since 00:00:00 **UTC** on 1 January 1970. 

```shell
curl "https://api.huobi.co.kr/v1/common/timestamp"
```

### HTTP Request

`GET /v1/common/timestamp`

### Request Parameters

No parameter is needed for this endpoint.

> Response:

```json
  "data": 1494900087029
```

# Market Data

## Introduction

Market data APIs provide public market information such as varies of candlestick, depth and trade information.

Below is the error code, error message and description returned by Market data APIs.

| Error Code        | Error Message                       | Description                                      |
| ----------------- | ----------------------------------- | ------------------------------------------------ |
| invalid-parameter | invalid symbol                      | Parameter symbol is invalid                      |
| invalid-parameter | invalid period                      | Parameter period is invalid for candlestick data |
| invalid-parameter | invalid depth                       | Parameter depth is invalid for depth data        |
| invalid-parameter | invalid type                        | Parameter type is invalid                        |
| invalid-parameter | invalid size                        | Parameter size is invalid                        |
| invalid-parameter | invalid size,valid range: [1, 2000] | Parameter size range is invalide                 |
| invalid-parameter | request timeout                     | Request timeout please try again                 |

## Get Klines(Candles)

This endpoint retrieves all klines in a specific range.

### HTTP Request

GET `/market/history/kline`

```shell
curl "https://api-cloud.huobi.co.kr/market/history/kline?period=1day&size=200&symbol=btcusdt"
```

### Query Parameters

Parameter | Data Type | Required | Default | Description                 | Value Range
--------- | --------- | -------- | ------- | -----------                 | -----------
symbol    | string    | true     | NA      | The trading symbol to query | All trading symbol supported, e.g. btcusdt, bccbtc
period    | string    | true     | NA      | The period of each candle   | 1min, 5min, 15min, 30min, 60min, 4hour, 1day, 1mon, 1week, 1year
size      | integer   | false    | 150     | The number of data returns  | [1, 2000]

<aside class="notice">This API doesn't support customized period, refer to Websocket K line API to get the emurated period value.</aside>
<aside class="notice">To query HB10, put "hb10" at symbol position.</aside>
<aside class="notice">The start time for candlesticks is based on Singapore time (GMT+8), for example, the duration for daily candlesticks is from 00:00:00 to 23:59:59 Singapore time.</aside>
> The above command returns JSON structured like this:

```json
"data": [
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

### Response Content

Field     | Data Type | Description
--------- | --------- | -----------
id        | integer   | The UNIX timestamp in seconds as response id
amount    | float     | The aggregated trading volume in USDT
count     | integer   | The number of completed trades
open      | float     | The opening price
close     | float     | The closing price
low       | float     | The low price
high      | float     | The high price
vol       | float     | The trading volume in base currency

## Get Latest Aggregated Ticker

This endpoint retrieves the latest ticker with some important 24h aggregated market data.

### HTTP Request

GET `/market/detail/merged`

```shell
curl "https://api-cloud.huobi.co.kr/market/detail/merged?symbol=ethusdt"
```

### Request Parameters

Parameter | Data Type | Required | Default | Description                  | Value Range
--------- | --------- | -------- | ------- | -----------                  | --------
symbol    | string    | true     | NA      | The trading symbol to query  | All supported trading symbol, e.g. btcusdt, bccbtc.Refer to `/v1/common/symbols` 

> The above command returns JSON structured like this:

```json
"data": {
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

### Response Content

| Field  | Data Type | Description                                                  |
| ------ | --------- | ------------------------------------------------------------ |
| id     | long      | The internal identity                                        |
| amount | float     | Accumulated trading volume of last 24 hours (rotating 24h), in base currency |
| count  | integer   | The number of completed trades (rotating 24h)                |
| open   | float     | The opening price of last 24 hours (rotating 24h)            |
| close  | float     | The last price of last 24 hours (rotating 24h)               |
| low    | float     | The low price of last 24 hours (rotating 24h)                |
| high   | float     | The high price of last 24 hours (rotating 24h)               |
| vol    | float     | Accumulated trading value of last 24 hours (rotating 24h), in quote currency |
| bid    | object    | The current best bid in format [price, size]                 |
| ask    | object    | The current best ask in format [price, size]                 |

## Get Latest Tickers for All Pairs

This endpoint retrieves the latest tickers for all supported pairs.

<aside class="notice">The returned data object can contain large amount of tickers.</aside>
### HTTP Request

GET `/market/tickers`

```shell
curl "https://api-cloud.huobi.co.kr/market/tickers"
```

### Request Parameters

No parameters are needed for this endpoint.

> The above command returns JSON structured like this:

```json
"data": [  
    {  
        "open":0.044297,
        "close":0.042178,
        "low":0.040110,
        "high":0.045255,
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

### Response Content

Response content is an array of object, each object has below fields.

| Field   | Data Type | Description                                                  |
| ------- | --------- | ------------------------------------------------------------ |
| amount  | float     | The aggregated trading volume in last 24 hours (rotating 24h) |
| count   | integer   | The number of completed trades of last 24 hours (rotating 24h) |
| open    | float     | The opening price of a nature day (Singapore time)           |
| close   | float     | The last price of a nature day (Singapore time)              |
| low     | float     | The low price of a nature day (Singapore time)               |
| high    | float     | The high price of a nature day (Singapore time)              |
| vol     | float     | The aggregated trading value in last 24 hours (rotating 24h) |
| symbol  | string    | The trading symbol of this object, e.g. btcusdt, bccbtc      |
| bid     | float     | Best bid price                                               |
| bidSize | float     | Best bid size                                                |
| ask     | float     | Best ask price                                               |
| askSize | float     | Best ask size                                                |

## Get Market Depth

This endpoint retrieves the current order book of a specific pair.

### HTTP Request

GET `/market/depth`

```shell
curl "https://api-cloud.huobi.co.kr/market/depth?symbol=btcusdt&type=step1"
```

### Request Parameters

Parameter | Data Type | Required | Default Value         | Description                                       | Value Range
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | The trading symbol to query                       | Refer to `GET /v1/common/symbols` 
depth     | integer   | false    | 20                    | The number of market depth to return on each side | 5, 10, 20
type      | string    | true     | step0                 | Market depth aggregation level, details below     | step0, step1, step2, step3, step4, step5

<aside class="notice">when type is set to "step0", the default value of "depth" is 150 instead of 20.</aside>
**"type" Details**

Value     | Description
--------- | ---------
step0     | No market depth aggregation
step1     | Aggregation level = precision*10
step2     | Aggregation level = precision*100
step3     | Aggregation level = precision*500 
step4     | Aggregation level = precision*1000 

> The above command returns JSON structured like this:

```json
"tick": {
    "version": 31615842081,
    "ts": 1489464585407,
    "bids": [
      [7964, 0.0678],
      [7963, 0.9162],
      [7961, 0.1],
      [7960, 12.8898],
      [7958, 1.2],
      [7955, 2.1009],
      [7954, 0.4708],
      [7953, 0.0564],
      [7951, 2.8031],
      [7950, 13.7785],
      [7949, 0.125],
      [7948, 4],
      [7942, 0.4337],
      [7940, 6.1612],
      [7936, 0.02],
      [7935, 1.3575],
      [7933, 2.002],
      [7932, 1.3449],
      [7930, 10.2974],
      [7929, 3.2226]
    ],
    "asks": [
      [7979, 0.0736],
      [7980, 1.0292],
      [7981, 5.5652],
      [7986, 0.2416],
      [7990, 1.9970],
      [7995, 0.88],
      [7996, 0.0212],
      [8000, 9.2609],
      [8002, 0.02],
      [8008, 1],
      [8010, 0.8735],
      [8011, 2.36],
      [8012, 0.02],
      [8014, 0.1067],
      [8015, 12.9118],
      [8016, 2.5206],
      [8017, 0.0166],
      [8018, 1.3218],
      [8019, 0.01],
      [8020, 13.6584]
    ]
  }
```

### Response Content

<aside class="notice">The returned data object is under 'tick' object instead of 'data' object in the top level JSON</aside>
Field     | Data Type | Description
--------- | --------- | -----------
ts        | integer   | The UNIX timestamp in milliseconds adjusted to Singapore time
version   | integer   | Internal data
bids      | object    | The current all bids in format [price, size] 
asks      | object    | The current all asks in format [price, size] 


## Get the Last Trade

This endpoint retrieves the latest trade with its price, volume, and direction.

### HTTP Request

GET `/market/trade`

```shell
curl "https://api-cloud.huobi.co.kr/market/trade?symbol=ethusdt"
```

### Request Parameters

Parameter | Data Type | Required | Default Value         | Description                                       | Value Range
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | The trading symbol to query                       | Refer to `GET /v1/common/symbols`

> The above command returns JSON structured like this:

```json
"tick": {
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

### Response Content

<aside class="notice">The returned data object is under 'tick' object instead of 'data' object in the top level JSON</aside>
Parameter | Data Type | Description
--------- | --------- | -----------
id        | integer   | The unique trade id of this trade (to be obsoleted)
trade-id|integer| The unique trade id (NEW)
amount    | float     | The trading volume in base currency
price     | float     | The trading price in quote currency
ts        | integer   | The UNIX timestamp in milliseconds adjusted to Singapore time
direction | string    | The direction of the taker trade: 'buy' or 'sell'

## Get the Most Recent Trades

This endpoint retrieves the most recent trades with their price, volume, and direction.

### HTTP Request

GET `/market/history/trade`

```shell
curl "https://api-cloud.huobi.co.kr/market/history/trade?symbol=ethusdt&size=2"
```

### Request Parameters

Parameter | Data Type | Required | Default Value    | Description                   | Value Range
--------- | --------- | -------- | -------------    | ----------                    | -----------
symbol    | string    | true     | NA               | The trading symbol to query   | All supported trading symbol, e.g. btcusdt, bccbtc.Refer to `GET /v1/common/symbols`
size      | integer   | false    | 1                | The number of data returns    | [1, 2000]

> The above command returns JSON structured like this:

```json
"data": [  
   {  
      "id":31618787514,
      "ts":1544390317905,
      "data":[  
         {  
            "amount":9.000000000000000000,
            "ts":1544390317905,
            "id":3161878751418918529341,
            "trade-id": 102043495672,
            "price":94.690000000000000000,
            "direction":"sell"
         },
         {  
            "amount":73.771000000000000000,
            "ts":1544390317905,
            "id":3161878751418918532514,
            "trade-id": 102043495673,
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
            "trade-id": 102043495674,
            "price":94.710000000000000000,
            "direction":"buy"
         }
      ]
   }
}
```

### Response Content

<aside class="notice">The returned data object is an array represents one recent timestamp; each timestamp object again is an array represents all trades occurred at this timestamp.</aside>
Field     | Data Type | Description
--------- | --------- | -----------
id        | integer   | The unique trade id of this trade (to be obsoleted)
trade-id|integer| The unique trade id (NEW)
amount    | float     | The trading volume in base currency
price     | float     | The trading price in quote currency
ts        | integer   | The UNIX timestamp in milliseconds adjusted to Singapore time
direction | string    | The direction of the taker trade: 'buy' or 'sell'

## Get the Last 24h Market Summary

This endpoint retrieves the summary of trading in the market for the last 24 hours.

### HTTP Request

GET `/market/detail/`

```shell
curl "https://api-cloud.huobi.co.kr/market/detail?symbol=ethusdt"
```

### Request Parameters

Parameter | Data Type | Required | Default Value    | Description                   | Value Range
--------- | --------- | -------- | -------------    | ----------                    | -----------
symbol    | string    | true     | NA               | The trading symbol to query   | Refer to /v1/common/symbols

> The above command returns JSON structured like this:

```json
"tick": {  
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

### Response Content

<aside class="notice">The returned data object is under 'tick' object instead of 'data' object in the top level JSON</aside>
| Field   | Data Type | Description                                                  |
| ------- | --------- | ------------------------------------------------------------ |
| id      | integer   | The internal identity                                        |
| amount  | float     | The aggregated trading volume in USDT of last 24 hours (rotating 24h) |
| count   | integer   | The number of completed trades of last 24 hours (rotating 24h) |
| open    | float     | The opening price of last 24 hours (rotating 24h)            |
| close   | float     | The last price of last 24 hours (rotating 24h)               |
| low     | float     | The low price of last 24 hours (rotating 24h)                |
| high    | float     | The high price of last 24 hours (rotating 24h)               |
| vol     | float     | The trading volume in base currency of last 24 hours (rotating 24h) |
| version | integer   | Internal data                                                |


# Account

## Introduction

Account APIs provide account related (such as basic info, balance, history, point) query and transfer functionality.

<aside class="notice">All endpoints in this section require authentication</aside>
Below is the error code, error message and description returned by Account APIs.

| Error Code | Error Message                               | Description                                                  |
| ---------- | ------------------------------------------- | ------------------------------------------------------------ |
| 500        | system error                                | Server internal error                                        |
| 1002       | forbidden                                   | Operation is forbidden, such as the account Id and UID doesn't match |
| 2002       | "invalid field value in `currency`"         | Parameter currency is invalid                                |
| 2002       | "invalid field value in `transactTypes`"    | Parameter transactTypes is invalid (should be transfer)      |
| 2002       | "invalid field value in `sort`"             | Parameter sort is invalid (should be 'asc' or 'desc')        |
| 2002       | "value in `fromId` is not found in record”  | Value fromId doesn't exist                                   |
| 2002       | "invalid field value in `accountId`"        | Parameter accountId is invalid (should not be empty)         |
| 2002       | "value in `startTime` exceeded valid range" | Value startTime is later than current time or earlier than 180 days ago |
| 2002       | "value in `endTime` exceeded valid range")  | Value endTime is earlier than startTime, or 10 days later than startTime |

## Get all Accounts of the Current User

API Key Permission：Read<br>

Rate Limit (NEW): 100times/2s

This endpoint returns a list of accounts owned by this API user.

### HTTP Request

GET `/v1/account/accounts`

```shell
curl "https://api-cloud.huobi.co.kr/v1/account/accounts"
```

### Request Parameters

<aside class="notice">No parameter is available for this endpoint</aside>
> The above command returns JSON structured like this:

```json
  "data": [
    {
      "id": 100009,
      "type": "spot",
      "subtype": "",
      "state": "working"
    }
  ]
```

### Response Content

Field               | Data Type | Description              | Value Range
---------           | --------- | -----------              | -----------
id                  | integer   | Unique account id        | NA
state               | string    | Account state            | working, lock
type                | string    | The type of this account | spot, point 

## Get Account Balance of a Specific Account

API Key Permission：Read

Rate Limit (NEW): 100times/2s

This endpoint returns the balance of an account specified by account id.

### HTTP Request

GET `/v1/account/accounts/{account-id}/balance`

'account-id': The specified account id to get balance for, can be found by query '/v1/account/accounts' endpoint.

```shell
curl "https://api-cloud.huobi.co.kr/v1/account/accounts/100009/balance"
```

### Request Parameters

<aside class="notice">No parameter is needed for this endpoint</aside>
> The above command returns JSON structured like this:

```json
"data": {
    "id": 100009,
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
        "balance": "328048.1199920000"
      },
     {
        "currency": "etc",
        "type": "trade",
        "balance": "499999894616.1302471000"
      }
    ],
  }
}
```

### Response Content

Field               | Data Type | Description              | Value Range
---------           | --------- | -----------              | -----------
id                  | integer   | Unique account id        | NA
state               | string    | Account state            | working, lock
type                | string    | The type of this account | spot, point 
list                | object    | The balance details of each currency|

**Per list item content**

Field               | Data Type | Description                           | Value Range
---------           | --------- | -----------                           | -----------
currency            | string    | The currency of this balance          | NA
type                | string    | The balance type                      | trade, frozen
balance             | string    | The balance in the main currency unit | NA

## Get Asset Valuation

API Key Permission：Read

Rate Limit (NEW): 100times/2s

This endpoint the valuation of the total assets of the account in btc or fiat currency.

### HTTP Request

- GET `/v2/account/asset-valuation`

### Request Parameters

| Parameter         | Required | Data Type | Description                                                  | Default Value | Value Range                                                  |
| ----------------- | -------- | --------- | ------------------------------------------------------------ | ------------- | ------------------------------------------------------------ |
| accountType       | true     | string    | The type of this account                                     | NA            | spot                                                         |
| valuationCurrency | false    | string    | The valuation according to the certain fiat currency         | BTC           | BTC, CNY, USD, JPY, KRW, GBP, TRY, EUR, RUB, VND, HKD, TWD, MYR, SGD, AED, SAR (case sensitive) |
| subUid            | false    | long      | Sub User's UID. When sub user's UID is not specified, the response would include the records of  API key. | NA            |                                                              |

> The above command returns JSON structured like this:

```json
{
    "code": 200,
    "data": {
        "balance": "34.75",
        "timestamp": 1594901254363
    },
    "ok": true
}
```

### Response Content

| Parameter | Required | Data Type | Description                                          |
| --------- | -------- | --------- | ---------------------------------------------------- |
| balance   | true     | string    | The valuation according to the certain fiat currency |
| timestamp | true     | long      | Return time                                          |

## Asset Transfer

API Key Permission：Trade<br>

This endpoint allows parent user and sub user to transfer asset between accounts.<br>

Features now supported for both parent user and sub user include: <br>
1.transfer asset between spot account and individual isolated-margin account; <br>
2.transfer asset between individual isolated-margin accounts; <br>

Features now supported for parent user include: <br>
1.Transfer asset between parent user's spot account and sub user's spot account; <br>
2.Transfer asset from sub user’s spot account to another sub user’s spot account who under the same parent user;<br>

Features now supported for sub user include: <br>
1.Transfer asset from  authorized sub user’s spot account to another sub user’s spot account who under the same parent user.The authorization endpoint is `POST /v2/sub-user/transferability`. <br>
2.Transfer asset from sub user’s spot account to parent user’s spot account;<br>

Other transfer functions will be gradually launched later, please take note on API announcement in near future. <br>

### HTTP Request

- POST `/v1/account/transfer`

### Request Parameters

| Parameter         | Required | Data Type | Description                | Values                            |
| ----------------- | -------- | --------- | -------------------------- | --------------------------------- |
| from-user         | true     | long      | Transfer out user uid      | parent user uid, sub user uid     |
| from-account-type | true     | string    | Transfer out account type  | spot                              |
| from-account      | true     | long      | Transfer out account id    |                                   |
| to-user           | true     | long      | Transfer in user uid       | parent user uid, sub user uid     |
| to-account-type   | true     | string    | Transfer in account type   | spot                              |
| to-account        | true     | long      | Transfer in account id     |                                   |
| currency          | true     | string    | Currency name              | Refer to GET /v1/common/currencys |
| amount            | true     | string    | Amount of fund to transfer |                                   |

> Response:

```json
{
    "status": "ok",
    "data": {
        "transact-id": 220521190,
        "transact-time": 1590662591832
    }
}
```

### Response Content

| Field          | Required | Data Type | Description    | Values          |
| -------------- | -------- | --------- | -------------- | --------------- |
| status         | true     | string    | Request status | "ok" or "error" |
| data           | true     | list      |                |                 |
| {transact-id   | true     | int       | Transfer id    |                 |
| transact-time} | true     | long      | Transfer time  |                 |

## Get Account History

API Key Permission：Read

Rate Limit (NEW): 5times/2s

This endpoint returns the amount changes of specified user's account.

### HTTP Request

GET `/v1/account/history`

```shell
curl "https://api-cloud.huobi.co.kr/v1/account/history?account-id=5260185"
```

### Request Parameters

Parameter  | Required | Data Type | Description | Default Value                                  | Value Range
---------  | --------- | -------- | ------- | -----------                                   | ----------
account-id     | true  | string | Account Id, refer to `GET /v1/account/accounts` |     |  
currency      | false | string | Currency name |       | Refer to /v1/common/currencys 
transact-types | false | string | Amount change types (multiple selection allowed)  | all     |trade,etf, transact-fee, deduction, transfer, credit, liquidation, interest, deposit-withdraw, withdraw-fee, exchange, other-types 
start-time   | false | long | Far point of time of the query window (unix time in millisecond). Searching based on transact-time. The maximum size of the query window is 1 hour. The query window can be shifted within 30 days. | ((end-time) – 1hour)     | [((end-time) – 1hour), (end-time)]   
end-time     | false  | long | Near point of time of the query window (unix time in millisecond). Searching based on transact-time. The maximum size of the query window is 1 hour. The query window can be shifted within 30 days.  |  current-time    |[(current-time) – 29days,(current-time)]
sort     | false  | string | Sorting order  |  asc    |asc or desc
size     | false  | int | Maximum number of items in each response  |   100   |[1,500]
from-id | false | long | First record ID in this query (only valid for next page querying, see Note 2) |  |

> The above command returns JSON structured like this:

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

### Response Content

Field               | Data Type | Description              | Value Range
---------           | --------- | -----------              | -----------
status                 | string   | Status code        | 
data               | object    |             | 
{ account-id  | long   | Account ID|
currency               | string    | Currency|
transact-amt                 | string   | Amount change (positive value if income, negative value if outcome)        | 
transact-type                 | string   | Amount change types        | 
avail-balance                 | string   | Available balance        | 
acct-balance                | string   | Account balance        | 
transact-time                 | long   | Transaction time (database time)      | 
record-id }                 | string   | Unique record ID in the database      | 
next-id | long | First record ID in next page (only valid if exceeded page size, see Note 2) | 

Note 1:<br>

- If received ‘transaction-amt’ with ‘transact-type’ as ‘rebate’, it implicates a paid maker rebate.<br>
- A paid maker rebate could possibly include rebate from multiple trades.<br>

Note 2:<br>
Only when the number of items within the query window (between “start-time” and ”end-time”) exceeded the page limitation (defined by “size”), Huobi server returns “next-id”. Once received “next-id”, API user should –<br>
1) Be aware of that, some items within the query window were not returned due to the page size limitation.<br>
2) In order to get these items from Huobi server, adopt the “next-id” as “from-id” and submit another request, with other request parameters no change.<br>
3) As database record ID, “next-id” and “from-id” are for recurring query purpose and the ID itself does not have any business implication.<br>

## Get Account Ledger

API Key Permission：Read

This endpoint returns the amount changes of specified user's account.<br>

Phase 1 release only supports historical assets transfer querying (“transactType” = “transfer”).<br>

The maximum query window size set by “startTime” & “endTime” is 10-day, which means, maximum 10-day records are queriable per request.
The query window can be sliding within the latest 180 days, which means, by adjusting “startTime” & “endTime” to slide the query window in multiple requests, the records in latest 180 days are queriable.<br>

### HTTP Request

`GET https://api.huobi.co.kr/v2/account/ledger`

```shell
curl "https://api.huobi.co.kr/v2/account/ledger?account-id=5260185"
```

### Request Parameters

| Field Name    | Data Type | Mandatory | Description                                                  |
| ------------- | --------- | --------- | ------------------------------------------------------------ |
| accountId     | string    | TRUE      | Account ID                                                   |
| currency      | string    | FALSE     | Cryptocurrency (default value: all)                          |
| transactTypes | string    | FALSE     | Transaction types (multiple inputs are allowed; default value: all; enumerated values: transfer) |
| startTime     | long      | FALSE     | Farthest time (please refer to note 1 for valid range and default value) |
| endTime       | long      | FALSE     | Nearest time (please refer to note 2 for valid range and default value) |
| sort          | string    | FALSE     | Sorting order (enumerated values: asc, desc)                 |
| limit         | int       | FALSE     | Maximum number of items in one page (valid range:[1,500]; default value:100) |
| fromId        | long      | FALSE     | First record ID in this query (only valid for next page querying. please refer to note 3) |

Note 1:<br>
startTime valid range: [(endTime – 10days), endTime], unix time in millisecond<br>
startTime default value: (endTime – 10days)

Note 2:<br>
endTime valid range: [(current time – 180days), current time], unix time in millisecond<br>
endTime default value: current time

> The above command returns JSON structured like this:

```json
{
"code": 200,
"message": "success",
"data": [
    {
        "accountId": 5260185,
        "currency": "btc",
        "transactAmt": 1.000000000000000000,
        "transactType": "transfer",
        "transferType": "margin-transfer-out",
        "transactId": 0,
        "transactTime": 1585573286913,
        "transferer": 5463409,
        "transferee": 5260185
    },
    {
        "accountId": 5260185,
        "currency": "btc",
        "transactAmt": -1.000000000000000000,
        "transactType": "transfer",
        "transferType": "margin-transfer-in",
        "transactId": 0,
        "transactTime": 1585573281160,
        "transferer": 5260185,
        "transferee": 5463409
    }
]
}
```

### Response Content

| Field Name   | Data Type | Mandatory | Description                                                  | Value Rage                                                   |
| ------------ | --------- | --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| code         | integer   | TRUE      | Status code                                                  |                                                              |
| message      | string    | FALSE     | Error message (if any)                                       |                                                              |
| data         | object    | TRUE      | Sorting as user defined (in request parameter “sort”	)    |                                                              |
| { accountId  | integer   | TRUE      | Account ID                                                   |                                                              |
| currency     | string    | TRUE      | Cryptocurrency                                               |                                                              |
| transactAmt  | number    | TRUE      | Transaction amount (income positive, expenditure negative)   |                                                              |
| transactType | string    | TRUE      | Transaction type                                             |                                                              |
| transferType | string    | FALSE     | Transfer type	(only valid for transactType=transfer)      | otc-to-pro, pro-to-otc, futures-to-pro, pro-to-futures, dm-swap-to-pro (coin-margined-swap), dm-pro-to-swap (coin-margined-swap), margin-transfer-in, margin-transfer-out, lock-transfer-in, lock-transfer-out, user-lock-transfer-in, user-lock-transfer-out, master-transfer-in, master-transfer-out, sub-transfer-in, sub-transfer-out, agency-transfer-in, agency-transfer-out, pro-to-super-margin, super-margin-to-pro |
| transactId   | integer   | TRUE      | Transaction ID                                               |                                                              |
| transactTime | integer   | TRUE      | Transaction time                                             |                                                              |
| transferer   | integer   | FALSE     | Transferer’s account ID                                      |                                                              |
| transferee } | integer   | FALSE     | Transferee’s account ID                                      |                                                              |
| nextId       | integer   | FALSE     | First record ID in next page (only valid if exceeded page size. please refer to note 3.) |                                                              |

Note 3:<br>
Only when the number of items within the query window (between “startTime” and ”endTime”) exceeded the page limitation (defined by “limit”), Huobi server returns “nextId”. Once received “nextId”, API user should –<br>
1)	Be aware of that, some items within the query window were not returned due to the page size limitation.<br>
2)	In order to get these items from Huobi server, adopt the “nextId” as “fromId” and submit another request, with other request parameters no change.<br>
3)	As database record ID, “nextId” and “fromId” are for recurring query purpose and the ID itself does not have any business implication.<br>

## Get Point Balance

Via this endpoint, user should be able to query ‘termless’ point’s balance, as well as ‘terminable’ point’s balance including its group IDs and individual expiration date.<br>
Via this endpoint, user could only query point’s balance instead of any other cryptocurrency’s balance.<br>
Via this endpoint, parent user could query either parent user’s point balance, or sub user’s point balance.<br>
User can only exchange Huobi point via Huobi official web or app.<br>

API Key Permission：Read<br>
Rate Limit: 2times/s<br>
Callable by sub user<br>

### HTTP Request

`GET /v2/point/account`

### Request Parameters

|	Field	|	Data Type	|	Mandatory	|	Description	|
|	-----	|	--------	|	--------	|	----------	|
|	subUid	|	string	|	FALSE	|	Sub user’s UID (only valid for scenario of parent user querying sub user’s point balance) 	|

> Response:

```json
{
    "code": 200,
    "data": {
        "accountId": "14403739",
        "groupIds": [
            {
                "groupId": 26,
                "expiryDate": 1594396800000,
                "remainAmt": "0.3"
            }
        ],
        "acctBalance": "0.30000000",
        "accountStatus": "working"
    },
    "success": true
}
```

### Response Content

|	Field	|	Data Type	|	Mandatory	|	Description	|
|	-----	|	--------	|	--------	|	----------	|
|	code	|	integer	|	TRUE	|	Status code	|
|	message	|	string	|	FALSE	|	Error message (if any)	|
|	data	|	object	|	TRUE	|		|
|	{ accountId	|	string	|	TRUE	|	Account ID	|
|	accountStatus	|	string	|	TRUE	|	Account status (working, lock, fl-sys, fl-mgt, fl-end, fl-negative) 	|
|	acctBalance	|	string	|	TRUE	|	Account balance	|
|	groupIds	|	object	|	TRUE	|	Group ID list	|
|	{ groupId	|	long	|	TRUE	|	Group ID	|
|	expiryDate	|	long	|	TRUE	|	Expiration date (unix time in millisecond) 	|
|	remainAmt }}	|	string	|	TRUE	|	Remaining amount	|

Note:<br>
Group ID is the transaction ID generated while parent user exchanging the ‘terminable’ points.<br>
Group ID of ‘termless’ points is 0.<br>
Expiration date of ‘termless’ points is null.<br>

## Point Transfer

Via this endpoint, parent user should be able to transfer points between parent user and sub user, sub user should be able to transfer point to parent user. Both ‘termless’ and ‘terminable’ points are transferrable.<br>
Via this endpoint, user could only transfer ‘termless’ and ‘terminable’ points instead of any other cryptocurrencies.<br>
Parent user could transfer point between parent user and sub user in two ways.<br>
Sub user could only transfer point from sub user to parent user.<br>
Before parent user trying to transfer the terminable points back from sub user's account, parent user should query the sub user's point balance first in order to get the corresponding groupId.<br>

API Key Permission：Trade<br>
Rate Limit: 2times/s<br>
Callable by sub user<br>

### HTTP Request

`POST /v2/point/transfer`

### Request Parameters

|	Field	|	Data Type	|	Mandatory	|	Description	|
|	-----	|	--------	|	--------	|	----------	|
|	fromUid	|	string	|	TRUE	|	Transferer’s UID	|
|	toUid	|	string	|	TRUE	|	Transferee’s UID	|
|	groupId	|	long	|	TRUE	|	Group ID	|
|	amount	|	string	|	TRUE	|	Transfer amount (precision: maximum 8 decimal places)	|

Note:<br>

- If groupId=0, it implicates an ‘termless’ point transfer request.<br>

> Response:

```json
{
    "code": 200,
    "data": {
        "transactId": "74",
        "transactTime": 1594370136458
    },
    "success": true
}
```

### Response Content

|	Field	|	Data Type	|	Mandatory	|	Description	|
|	-----	|	--------	|	--------	|	----------	|
|	code	|	integer	|	TRUE	|	Status code	|
|	message	|	string	|	FALSE	|	Error message (if any)	|
|	data	|	object	|	TRUE	|		|
|	{ transactId	|	string	|	TRUE	|	Transaction ID	|
|	transactTime }	|	long	|	TRUE	|	Transaction time (unix time in millisecond) 	|


# Wallet (Deposit and Withdraw)

## Introduction

Wallet APIs provide query functionality for deposit address, withdraw address, withdraw quota, deposit and withdraw history, and also provide withdraw and cancel-withdraw functionality.

<aside class="notice">All endpoints in this section require authentication</aside>

Below is the response code, message and description returned by Wallet APIs.

| Response Code | Message                              | Description            |
| ------------- | ------------------------------------ | ---------------------- |
| 200           | success                              | Successful             |
| 500           | error                                | Server internal error  |
| 1002          | unauthorized                         | User is unautherized   |
| 1003          | invalid signature                    | API signature is wrong |
| 2002          | invalid field value in "field name"  | Parameter is invalid   |
| 2003          | missing mandatory field "field name" | Parameter is missing   |

Parent user and sub user could query deposit address of corresponding chain, for a specific crypto currency (except IOTA).

API Key Permission：Read<br>
Rate Limit (NEW): 20times/2s

<aside class="notice"> The endpoint does not support deposit address querying for currency "IOTA" at this moment </aside>

### HTTP Request

`GET https://api.huobi.co.kr/v2/account/deposit/address`

```shell
curl "https://api.huobi.co.kr/v2/account/deposit/address?currency=btc"
```

### Request Parameters

| Field Name | Data Type | Mandatory | Default Value | Description                                         |
| ---------- | --------- | --------- | ------------- | --------------------------------------------------- |
| currency   | string    | true      | N/A           | Crypto currency,refer to `GET /v1/common/currencys` |

> The above command returns JSON structured like this:

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

### Response Content

| Field Name | Data Type | Description            |
| ---------- | --------- | ---------------------- |
| code       | int       | Status code            |
| message    | string    | Error message (if any) |
| data       | object    |                        |
| { currency | string    | Crypto currency        |
| address    | string    | Deposit address        |
| addressTag | string    | Deposit address tag    |
| chain }    | string    | Block chain name       |

### Status Code

| Status Code | Error Message                        | Scenario                |
| ----------- | ------------------------------------ | ----------------------- |
| 200         | success                              | Request successful      |
| 500         | error                                | System error            |
| 1002        | unauthorized                         | Unauthorized            |
| 1003        | invalid signature                    | Signature failure       |
| 2002        | invalid field value in "field name"  | Invalid field value     |
| 2003        | missing mandatory field "field name" | Mandatory field missing |

## Query Withdraw Quota

Parent user could query withdraw quota for currencies

API Key Permission：Read<br>
Rate Limit (NEW): 20times/2s

### HTTP Request

`GET https://api.huobi.co.kr/v2/account/withdraw/quota`

```shell
curl "https://api.huobi.co.kr/v2/account/withdraw/quota?currency=btc"
```

### Request Parameters

| Field Name | Data Type | Mandatory | Default Value | Description                                         |
| ---------- | --------- | --------- | ------------- | --------------------------------------------------- |
| currency   | string    | true      | N/A           | Crypto currency,refer to `GET /v1/common/currencys` |

> The above command returns JSON structured like this:

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

### Response Content

| Field Name                 | Data Type | Description                             |
| -------------------------- | --------- | --------------------------------------- |
| code                       | int       | Status code                             |
| message                    | string    | Error message (if any)                  |
| data                       | object    |                                         |
| currency                   | string    | Crypto currency                         |
| chains                     | object    |                                         |
| { chain                    | string    | Block chain name                        |
| maxWithdrawAmt             | string    | Maximum withdraw amount in each request |
| withdrawQuotaPerDay        | string    | Maximum withdraw amount in a day        |
| remainWithdrawQuotaPerDay  | string    | Remaining withdraw quota in the day     |
| withdrawQuotaPerYear       | string    | Maximum withdraw amount in a year       |
| remainWithdrawQuotaPerYear | string    | Remaining withdraw quota in the year    |
| withdrawQuotaTotal         | string    | Maximum withdraw amount in total        |
| remainWithdrawQuotaTotal } | string    | Remaining withdraw quota in total       |

### Status Code

| Status Code | Error Message                       | Scenario            |
| ----------- | ----------------------------------- | ------------------- |
| 200         | success                             | Request successful  |
| 500         | error                               | System error        |
| 1002        | unauthorized                        | Unauthorized        |
| 1003        | invalid signature                   | Signature failure   |
| 2002        | invalid field value in "field name" | Invalid field value |

## Query withdraw address

API Key Permission: Read<br>

This endpoint allows parent user to query withdraw address available for API key.<br>

### HTTP Request

- GET `/v2/account/withdraw/address`

### Request Parameters

| Parameter | Required | Data Type | Description                                                  | Default Value                                                | Value Range                                                  |
| --------- | -------- | --------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| currency  | true     | string    | Crypto currency                                              |                                                              | btc, ltc, bch, eth, etc ...(refer to GET /v1/common/currencys) |
| chain     | false    | string    | Block chain name                                             | When chain is not specified, the reponse would include the records of ALL chains. |                                                              |
| note      | false    | string    | The note of withdraw address                                 | When note is not specified, the reponse would include the records of ALL notes. |                                                              |
| limit     | false    | int       | The number of items to return                                | 100                                                          | [1,500]                                                      |
| fromId    | false    | long      | First record ID in this query (only valid for next page querying; please refer to note) | NA                                                           |                                                              |

> Response:

```json
{
    "code": 200,
    "data": [
        {
            "currency": "usdt",
            "chain": "usdt",
            "note": "币安",
            "addressTag": "",
            "address": "15PrEcqTJRn4haLeby3gJJebtyf4KgWmSd"
        }
    ]
}
```

### Response Content

| Field Name | Mandatory | Data Type | Description                                                  | Value Range |
| ---------- | --------- | --------- | ------------------------------------------------------------ | ----------- |
| code       | true      | int       | Status code                                                  |             |
| message    | false     | string    | Error message (if any)                                       |             |
| data       | true      | object    |                                                              |             |
| { currency | true      | string    | Crypto currency                                              |             |
| chain      | true      | string    | Block chain name                                             |             |
| note       | true      | string    | The address note                                             |             |
| addressTag | false     | string    | The address tag，if any                                      |             |
| address }  | true      | string    | Withdraw address                                             |             |
| nextId     | false     | long      | First record ID in next page (only valid if exceeded page size) |             |

Note:<br>
Only when the number of items within the query window exceeded the page limitation (defined by “limit”), Huobi server returns “nextId”. Once received “nextId”, API user should –<br>
1) Be aware of that, some items within the query window were not returned due to the page size limitation.<br>
2) In order to get these items from Huobi server, adopt the “nextId” as “fromId” and submit another request, with other request parameters no change.<br>
3) “nextId” and “fromId” are for recurring query purpose and the ID itself does not have any business implication.<br>

## Create a Withdraw Request

API Key Permission：Withdraw<br>
Rate Limit (NEW): 20times/2s

Parent user creates a withdraw request from spot account to an external address (exists in your withdraw address list), which doesn't require two-factor-authentication.

<aside class="notice">If user has chosen fast withdraw preferred in  <a href='https://www.hbg.com/en-us/user_center/uc_setting/'>settings </a>, the withdraw requests submitted via this endpoint would choose 'fast withdraw' as preferred channel. </aside>
<aside class="notice">Only support the existed addresses in your  <a href='https://www.hbg.com/en-us/withdraw_address/'>withdraw address list</a>. The once-off withdraw address of IOTA couldn't be set in the list, thus IOTA withdrawal is not supported through API. </aside>

### HTTP Request

`POST https://api.huobi.co.kr/v1/dw/withdraw/api/create`

```shell
curl -X POST -H "Content-Type: application/json" "https://api.huobi.co.kr/v1/dw/withdraw/api/create" -d
'{
  "address": "0xde709f2102306220921060314715629080e2fb77",
  "amount": "0.05",
  "currency": "eth",
  "fee": "0.01"
}'
```

### Request Parameters

| Parameter | Data Type | Required | Default | Description                                                  |
| --------- | --------- | -------- | ------- | ------------------------------------------------------------ |
| address   | string    | true     | NA      | The desination address of this withdraw                      |
| currency  | string    | true     | NA      | Crypto currency,refer to `GET /v1/common/currencys`          |
| amount    | string    | true     | NA      | The amount of currency to withdraw                           |
| fee       | string    | true     | NA      | The fee to pay with this withdraw                            |
| chain     | string    | false    | NA      | Refer to`GET /v2/reference/currencies`.Set as "usdt" to withdraw USDT to OMNI, set as "trc20usdt" to withdraw USDT to TRX |
| addr-tag  | string    | false    | NA      | A tag specified for this address                             |

> The above command returns JSON structured like this:

```json
{  
  "data": 1000
}
```

### Response Content

<aside class="notice">The return data contains a single value instead of an object</aside>

| Field | Data Type | Description |
| ----- | --------- | ----------- |
| data  | integer   | Transfer id |

<aside class="notice">All new transfer id will be incremental to the previous ids. This allows search by transfer id sequences</aside>

## Cancel a Withdraw Request

API Key Permission：Withdraw<br>
Rate Limit (NEW): 20times/2s

Parent user cancels a previously created withdraw request by its transfer id.

### HTTP Request

`POST https://api.huobi.co.kr/v1/dw/withdraw-virtual/{withdraw-id}/cancel`

```shell
curl -X POST "https://api.huobi.co.kr/v1/dw/withdraw-virtual/1000/cancel"
```

'withdraw-id': the id returned when previously created a withdraw request

### Request Parameters

<aside class="notice">No parameter is needed for this endpoint</aside>

> The above command returns JSON structured like this:

```json
  "data": 700
```

### Response Content

<aside class="notice">The return data contains a single value instead of an object</aside>

| Parameter | Data Type | Description        |
| --------- | --------- | ------------------ |
| data      | integer   | Withdraw cancel id |

## Search for Existed Withdraws and Deposits

API Key Permission：Read<br>
Rate Limit (NEW): 20times/2s

Parent user and sub user searche for all existed withdraws and deposits and return their latest status.

### HTTP Request

`GET https://api.huobi.co.kr/v1/query/deposit-withdraw`

```shell
curl "https://api.huobi.co.kr/v1/query/deposit-withdraw?currency=xrp&type=deposit&from=5&size=12"
```

### Request Parameters

| Parameter | Data Type | Required | Description                     | Value Range                                      | Default Value                                                |      |
| --------- | --------- | -------- | ------------------------------- | ------------------------------------------------ | ------------------------------------------------------------ | ---- |
| currency  | string    | false    | The crypto currency to withdraw | NA                                               | When currency is not specified, the reponse would include the records of ALL currencies. |      |
| type      | string    | true     | Define transfer type to search  | deposit, withdraw, sub user can only use deposit |                                                              |      |
| from      | string    | false    | The transfer id to begin search | 1 ~ latest record ID                             | When 'from' is not specified, the default value would be 1 if 'direct' is 'prev' with the response in ascending order, the default value would be the ID of latest record if 'direct' is 'next' with the response in descending order. |      |
| size      | string    | false    | The number of items to return   | 1-500                                            | 100                                                          |      |
| direct    | string    | false    | the order of response           | 'prev' (ascending), 'next' (descending)          | 'prev'                                                       |      |

> The above command returns JSON structured like this:

```json
{
	"status": "ok",
	"data": [{
		"id": 24383070,
		"type": "deposit",
		"currency": "usdt",
		"chain": "usdterc20",
		"tx-hash": "16382690",
		"amount": 4.000000000000000000,
		"address": "0x138d709030b4e096044d371a27efc5c562889b9b",
		"address-tag": "",
		"fee": 0,
		"state": "safe",
		"created-at": 1571303815800,
		"updated-at": 1571303815826
	}]
}
```

### Response Content

| Field       | Data Type | Description                                                  |
| ----------- | --------- | ------------------------------------------------------------ |
| id          | integer   | Transfer id                                                  |
| type        | string    | Define transfer type to search, possible values: [deposit, withdraw] |
| currency    | string    | The crypto currency to withdraw                              |
| tx-hash     | string    | The on-chain transaction hash                                |
| chain       | string    | Block chain name                                             |
| amount      | float     | The number of crypto asset transfered in its minimum unit    |
| address     | string    | The deposit or withdraw target address                       |
| address-tag | string    | The user defined address tag                                 |
| fee         | float     | Withdraw fee                                                 |
| state       | string    | The state of this transfer (see below for details)           |
| error-code  | string    | Error code for withdrawal failure, only returned when the type is "withdraw" and the state is "reject", "wallet-reject" and "failed". |
| error-msg   | string    | Error description of withdrawal failure, only returned when the type is "withdraw" and the state is "reject", "wallet-reject" and "failed". |
| created-at  | integer   | The timestamp in milliseconds for the transfer creation      |
| updated-at  | integer   | The timestamp in milliseconds for the transfer's latest update |

**List of possible withdraw state**

| State           | Description                                       |
| --------------- | ------------------------------------------------- |
| verifying       | Awaiting verification                             |
| failed          | verification failed                               |
| submitted       | Withdraw request submitted successfully           |
| reexamine       | Under examination for withdraw validation         |
| canceled        | Withdraw canceled by user                         |
| pass            | Withdraw validation passed                        |
| reject          | Withdraw validation rejected                      |
| pre-transfer    | Withdraw is about to be released                  |
| wallet-transfer | On-chain transfer initiated                       |
| wallet-reject   | Transfer rejected by chain                        |
| confirmed       | On-chain transfer completed with one confirmation |
| confirm-error   | On-chain transfer faied to get confirmation       |
| repealed        | Withdraw terminated by system                     |

**List of possible deposit state**

| State      | Description                                        |
| ---------- | -------------------------------------------------- |
| unknown    | On-chain transfer has not been received            |
| confirming | On-chain transfer waits for first confirmation     |
| confirmed  | On-chain transfer confirmed for at least one block |
| safe       | Multiple on-chain confirmation happened            |
| orphan     | Confirmed but currently in an orphan branch        |

# Sub user management

## Introduction

Sub user management APIs provide sub user account management (creation, query, permission, transfer), sub user API key management (creation, update, query, deletion), sub user address (deposit, withdraw) query and balance query.

<aside class="notice">All endpoints in this section require authentication</aside>
Below is the error code, error message and description returned by Sub user management APIs

| Error Code | Error Message                                             | Description                                                  |
| ---------- | --------------------------------------------------------- | ------------------------------------------------------------ |
| 1002       | forbidden                                                 | Operation is forbidden, such as sub user creation is not allowed for current user |
| 1003       | unauthorized                                              | Signature is wrong                                           |
| 2002       | invalid field value                                       | Parameter is invalid                                         |
| 2014       | number of sub account in the request exceeded valid range | number of sub account exceeded                               |
| 2014       | number of api key in the request exceeded valid range     | number of API Key exceeded                                   |
| 2016       | invalid request while value specified in sub user states  | lock or unlock failure                                       |

## Sub user creation

This endpoint is used by the parent user to create sub users, up to 50 at a time

API Key Permission：Trade

### HTTP Request

- POST `/v2/sub-user/creation`

### Request Parameters

| Parameter   | Required | Data Type | Description                                                  | Default | Value Range                                                  |
| ----------- | -------- | --------- | ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| userList    | true     | object    |                                                              |         |                                                              |
| [{ userName | true     | string    | Sub user name, an important identifier of the sub user's identity, requires unique within the huobi platform | NA      | The combination of 6 to 20 letters and numbers, or only letters. Letter is not case sensitive. The first character has to be a letter. |
| note }]     | false    | string    | Sub user note, no unique requirements                        | NA      | Up to 20 characters, unlimited character types               |

> Request:

```json
{
"userList":
[
{
"userName":"test123",
"note":"huobi"
},
{
"userName":"test456",
"note":"huobi"
}
]
}
```

> Response:

```json
{
    "code": 200,
    "data": [
        {
    "userName": "test123",
    "note": "huobi",
    "uid": 123
      },
        {
    "userName": "test456",
    "note": "huobi",
    "errCode": "2002",
    "errMessage": "value in user name duplicated with existing record"
      }
    ]
}
```

### Response Content

| Parameter     | Required | Data Type | Description                                                  | Value Range |
| ------------- | -------- | --------- | ------------------------------------------------------------ | ----------- |
| code          | true     | int       | Status code                                                  |             |
| message       | false    | string    | Error message (if any)                                       |             |
| data          | true     | object    |                                                              |             |
| [{ userName   | true     | string    | Sub user name                                                |             |
| note          | false    | string    | Sub user note (only valid for sub-users with note)）         |             |
| uid           | false    | long      | Sub user UID (only valid for successfully created sub users) |             |
| errCode       | false    | string    | Error code for creation failure (only valid for sub users that failed to create) |             |
| errMessage }] | false    | string    | Cause of creation failure error (only valid for sub users that failed to create) |             |

## Get Sub User's List

Via this endpoint parent user is able to query a full list of sub user's UID as well as individual's status.

API Key Permission: Read

### HTTP Request

- GET `/v2/sub-user/user-list`

### Request Parameter

| Field  | Mandatory | Data Type | Description                                                  | Default Value | Possible Value |
| ------ | --------- | --------- | ------------------------------------------------------------ | ------------- | -------------- |
| fromId | FALSE     | long      | First record ID in next page (only valid if exceeded page size) |               |                |

> Response:

```json
{
    "code": 200,
    "data": [
        {
            "uid": 63628520,
            "userState": "normal"
        },
        {
            "uid": 132208121,
            "userState": "normal"
        }
    ]
}
```

### Response

| Field       | Mandatory | Data Type | Description                                                  | Possible Value |
| ----------- | --------- | --------- | ------------------------------------------------------------ | -------------- |
| code        | TRUE      | int       | Status code                                                  |                |
| message     | FALSE     | string    | Error message (if any)                                       |                |
| data        | TRUE      | object    | In ascending order of uid, each response contains maximum 100 records |                |
| { uid       | TRUE      | long      | Sub user’s UID                                               |                |
| userState } | TRUE      | string    | Sub user’s status                                            | lock, normal   |
| nextId      | FALSE     | long      | First record ID in next page (only valid if exceeded page size) |                |

## Lock/Unlock Sub User

API Key Permission：Trade<br>
Rate Limit (NEW): 20times/2s

This endpoint allows parent user to lock or unlock a specific sub user.

### HTTP Request

`POST https://api.huobi.co.kr/v2/sub-user/management`

### Request Parameters

| Parameter  | Data Type | Required | Description                                       | Value Range
| ---------  | --------- | -------- | -----------                                       | -----------
| subUid    | long  | true     | Sub user UID | NA
| action   | string    | true     | Action                   | lock,unlock 

> The above command returns JSON structured like this:

```json
{
  "code": 200,
	"data": {
     "subUid": 12902150,
     "userState":"lock"}
}
```

### Response Content

| Field               | Data Type | Description                           | Value Range
| ---------           | --------- | -----------                           | -----------
| subUid                 | long   | sub user UID                     | NA
| userState                | string    | The state of sub user             | lock,normal

## Get Sub User's Status

Via this endpoint, parent user is able to query sub user's status by specifying a UID.

API Key Permission: Read

### HTTP Request

- GET `/v2/sub-user/user-state`

### Request Parameter

| Field  | Mandatory | Data Type | Description    | Default Value | Possible Value |
| ------ | --------- | --------- | -------------- | ------------- | -------------- |
| subUid | TRUE      | long      | Sub user's UID |               |                |

> Response:

```json
{
    "code": 200,
    "data": {
        "uid": 132208121,
        "userState": "normal"
    }
}
```

### Response

| Field       | Mandatory | Data Type | Description            | Possible Value |
| ----------- | --------- | --------- | ---------------------- | -------------- |
| code        | TRUE      | int       | Status code            |                |
| message     | FALSE     | string    | Error message (if any) |                |
| data        | TRUE      | object    |                        |                |
| { uid       | TRUE      | long      | Sub user’s UID         |                |
| userState } | TRUE      | string    | Sub user’s status      | lock, normal   |

## Set Tradable Market for Sub Users

API Key Permission: Trade

Parent user is able to set tradable market for a batch of sub users through this endpoint.
By default, sub user’s trading permission in spot market is activated.

### HTTP Request

- POST `/v2/sub-user/tradable-market`

### Request Parameters

| Parameter   | Mandatory | Data Type | Length | Description                                               | Possible Value               |
| ----------- | --------- | --------- | ------ | --------------------------------------------------------- | ---------------------------- |
| subUids     | true      | string    | -      | Sub user's UID list (maximum 50 UIDs, separated by comma) | -                            |
| accountType | true      | string    | -      | Account type                                              | isolated-margin,cross-margin |
| activation  | true      | string    | -      | Account activation                                        | activated,deactivated        |

> Response:

```json
{
    "code": 200,
    "data": [
        {
            "subUid": "132208121",
            "accountType": "isolated-margin",
            "activation": "activated"
        }
    ]
}
```

### Response Content

| Parameter   | Mandatory | Data Type | Length | Description                                                  | Possible Value               |
| ----------- | --------- | --------- | ------ | ------------------------------------------------------------ | ---------------------------- |
| code        | true      | int       | -      | Status code                                                  |                              |
| message     | false     | string    | -      | Error message (if any)                                       |                              |
| data        | true      | object    |        |                                                              |                              |
| {subUid     | true      | string    | -      | Sub user's UID                                               | -                            |
| accountType | true      | string    | -      | Account type                                                 | isolated-margin,cross-margin |
| activation  | true      | string    | -      | Account activation                                           | activated,deactivated        |
| errCode     | false     | int       | -      | Error code in case of rejection (only valid when the requested UID being rejected) |                              |
| errMessage} | false     | string    | -      | Error message in case of rejection (only valid when the requested UID being rejected) |                              |

## Set Asset Transfer Permission for Sub Users

API Key Permission: Trade

Parent user is able to set asset transfer permission for a batch of sub users.
By default, the asset transfer from sub user’s spot account to parent user’s spot account is allowed.

### HTTP Request

- POST `/v2/sub-user/transferability`

### Request Parameters

| Parameter     | Mandatory | Data Type | Length | Description                                                  | Possible Value |
| ------------- | --------- | --------- | ------ | ------------------------------------------------------------ | -------------- |
| subUids       | true      | string    | -      | Sub user's UID list (maximum 50 UIDs, separated by comma)    | -              |
| accountType   | false     | string    | -      | Account type (if not available, adopt default value 'spot'） | spot           |
| transferrable | true      | bool      | -      | Transferrablility                                            | true,false     |

> Response:

```json
{
    "code": 200,
    "data": [
        {
            "accountType": "spot",
            "transferrable": true,
            "subUid": 13220823
        }
    ]
}
```

### Response Content

| Parameter     | Mandatory | Data Type | Length | Description                                                  | Possible Value |
| ------------- | --------- | --------- | ------ | ------------------------------------------------------------ | -------------- |
| code          | true      | int       | -      | Status code                                                  |                |
| message       | false     | string    | -      | Error message (if any)                                       |                |
| data          | true      | object    |        |                                                              |                |
| {subUid       | true      | long      | -      | Sub user's UID                                               | -              |
| accountType   | true      | string    | -      | Account type                                                 | spot           |
| transferrable | true      | bool      | -      | Transferrability                                             | true,false     |
| errCode       | false     | int       | -      | Error code in case of rejection (only valid when the requested UID being rejected) |                |
| errMessage}   | false     | string    | -      | Error code in case of rejection (only valid when the requested UID being rejected) |                |

## Get Sub User's Account List

Via this endpoint parent user is able to query account list of sub user by specifying a UID.

API Key Permission: Read

### HTTP Request

- GET `/v2/sub-user/account-list`

### Request Parameter

| Field  | Mandatory | Data Type | Description    | Default Value | Possible Value |
| ------ | --------- | --------- | -------------- | ------------- | -------------- |
| subUid | TRUE      | long      | Sub User's UID |               |                |

> Response:

```json
{
    "code": 200,
    "data": {
        "uid": 132208121,
        "list": [
            {
                "accountType": "isolated-margin",
                "activation": "activated"
            },
            {
                "accountType": "cross-margin",
                "activation": "deactivated"
            },
            {
                "accountType": "spot",
                "activation": "activated",
                "transferrable": true,
                "accountIds": [
                    {
                        "accountId": 12255180,
                        "accountStatus": "normal"
                    }
                ]
            }
        ]
    }
}
```

### Response

| Field             | Mandatory | Data Type | Description                                                  | Possible Value         |
| ----------------- | --------- | --------- | ------------------------------------------------------------ | ---------------------- |
| code              | TRUE      | int       | Status code                                                  |                        |
| message           | FALSE     | string    | Error message (if any)                                       |                        |
| data              | TRUE      | object    |                                                              |                        |
| { uid             | TRUE      | long      | Sub user’s UID                                               |                        |
| list              | TRUE      | object    |                                                              |                        |
| { accountType     | TRUE      | string    | Account type                                                 | spot                   |
| activation        | TRUE      | string    | Account’s activation                                         | activated, deactivated |
| transferrable     | FALSE     | bool      | Transfer permission (only valid for accountType=spot)        | true, false            |
| accountIds        | FALSE     | object    |                                                              |                        |
| { accountId       | TRUE      | string    | Account ID                                                   |                        |
| subType           | FALSE     | string    | Account sub type (only valid for accountType=isolated-margin) |                        |
| accountStatus }}} | TRUE      | string    | Account status                                               | normal, locked         |

## Sub user API key creation

This endpoint is used by the parent user to create the API key of the sub user

API Key Permission：Trade

### HTTP Request

- POST `/v2/sub-user/api-key-generation`

### Request Parameters

| Parameter   | Required | Data Type | Description                                                  | Default | Value Range                                                  |
| ----------- | -------- | --------- | ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| otpToken    | true     | string    | Google verification code of the parent user, the parent user must be bound to Google for verification on the web | NA      | 6 characters, pure numbers                                   |
| subUid      | true     | long      | Sub user UID                                                 | NA      |                                                              |
| note        | true     | string    | API key note                                                 | NA      | Up to 20 characters, free text                               |
| permission  | true     | string    | API key permissions                                          | NA      | Valid value: readOnly, trade; multiple inputs are allowed, separated by comma, i.e. readOnly, trade; readOnly is required permission for any API key, while trade permission is optional. |
| ipAddresses | false    | string    | IP address bind to the API key                               | NA      | At most 20 IPv4/IPv6 host address(es) and/or IPv4 network address(es) can bind with one API key, separated by comma. For example: 202.106.196.115, 202.106.96.0/24. Without any IP address binding, the API key will expire in 90 days. |

> Response:

```json
{
    "code": 200,
    "data": {
        "accessKey": "2b55df29-vf25treb80-1535713d-8aea2",
        "secretKey": "c405c550-6fa0583b-fb4bc38e-d317e",
        "note": "62924133",
        "permission": "trade,readOnly",
        "ipAddresses": "1.1.1.1,1.1.1.2"
    }
}
```

### Response Content

| Parameter     | Required | Data Type | Description            | Value Range |
| ------------- | -------- | --------- | ---------------------- | ----------- |
| code          | true     | int       | Status code            |             |
| message       | false    | string    | Error message (if any) |             |
| data          | true     | object    |                        |             |
| { note        | true     | string    | API key note           |             |
| accessKey     | true     | string    | access key             |             |
| secretKey     | true     | string    | secret key             |             |
| permission    | true     | string    | API key  permission    |             |
| ipAddresses } | true     | string    | API key IP addresses   |             |

## API key query

This  endpoint is used by the parent user to query their own API key information, and the parent user to query their sub user's API key information.

API Key Permission：Read

### HTTP Request

- GET `/v2/user/api-key`

### Request Parameters

| Parameter | Required | Data Type | Description                                                  | Default | Value Range |
| --------- | -------- | --------- | ------------------------------------------------------------ | ------- | ----------- |
| uid       | true     | long      | parent user uid , sub user uid                               | NA      |             |
| accessKey | false    | string    | The access key of the API key, if not specified, it will return all API keys belong to the UID. | NA      |             |

> Response:

```json
{
    "code": 200,
    "message": "success",
    "data": [
        {
            "accessKey": "4ba5cdf2-4a92c5da-718ba144-dbuqg6hkte",
            "status": "normal",
            "note": "62924133",
            "permission": "readOnly,trade",
            "ipAddresses": "1.1.1.1,1.1.1.2",
            "validDays": -1,
            "createTime": 1591348751000,
            "updateTime": 1591348751000
        }
    ]
}
```

### Response Content

| Parameter     | Required | Data Type | Description                | Value Range                             |
| ------------- | -------- | --------- | -------------------------- | --------------------------------------- |
| code          | true     | int       | Status code                |                                         |
| message       | false    | string    | Error message (if any)     |                                         |
| data          | true     | object    |                            |                                         |
| [{ accessKey  | true     | string    | access key                 |                                         |
| note          | true     | string    | API key note               |                                         |
| permission    | true     | string    | API key permission         |                                         |
| ipAddresses   | true     | string    | API key IP addresses       |                                         |
| validDays     | true     | int       | API key expire in (days)   | If it is -1, it means permanently valid |
| status        | true     | string    | API key status             | normal, expired                         |
| createTime    | true     | long      | API key creation time      |                                         |
| updateTime }] | true     | long      | API key last modified time |                                         |

## Sub user API key modification

This endpoint is used by the parent user to modify the API key of the sub user

API Key Permission：Trade

### HTTP Request

- POST `/v2/sub-user/api-key-modification`

### Request Parameters

| Parameter   | Required | Data Type | Description                                                  | Default | Value Range                                                  |
| ----------- | -------- | --------- | ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| subUid      | true     | long      | sub user uid                                                 | NA      |                                                              |
| accessKey   | true     | string    | Access key for sub user API key                              | NA      |                                                              |
| note        | false    | string    | API key note for sub user API key                            | NA      | Up to 255 characters                                         |
| permission  | false    | string    | API key permission for sub user API key                      | NA      | Valid value: readOnly, trade; multiple inputs are allowed, separated by comma, i.e. readOnly, trade; readOnly is required permission for any API key, while trade permission is optional. |
| ipAddresses | false    | string    | At most 20 IPv4/IPv6 host address(es) and/or IPv4 network address(es) can bind with one API key, separated by comma. For example: 202.106.196.115, 202.106.96.0/24. Without any IP address binding, the API key will expire in 90 days. | NA      |                                                              |

> Response:

```json
{
    "code": 200,
    "data": {
        "note": "test",
        "permission": "readOnly",
        "ipAddresses": "1.1.1.3"
    }
}
```

### Response Content

| Parameter     | Required | Data Type | Description                                                  | Value Range |
| ------------- | -------- | --------- | ------------------------------------------------------------ | ----------- |
| code          | true     | int       | Status code                                                  |             |
| message       | false    | string    | Error message (if any)                                       |             |
| data          | true     | object    |                                                              |             |
| { note        | true     | string    | API key note                                                 |             |
| permission    | true     | string    | API key permission                                           |             |
| ipAddresses } | true     | string    | IPv4/IPv6 host address(es) or IPv4 network address(es) bind to the API key |             |

## Sub user API key deletion

This endpoint is used by the parent user to delete the API key of the sub user.

API Key Permission：Trade

### HTTP Request

- POST `/v2/sub-user/api-key-deletion`

### Request Parameters

| Parameter | Required | Data Type | Description                     | Default | Value Range |
| --------- | -------- | --------- | ------------------------------- | ------- | ----------- |
| subUid    | true     | long      | sub user uid                    | NA      |             |
| accessKey | true     | string    | Access key for sub user API key | NA      |             |

> Response:

```json
{
    "code": 200,
    "data": null
}
```

### Response Content

| Parameter | Required | Data Type | Description              | Value Range |
| --------- | -------- | --------- | ------------------------ | ----------- |
| code      | true     | int       | Status code              |             |
| message   | false    | string    | Error message (if any)） |             |

## Transfer Asset between Parent and Sub Account

API Key Permission：Trade<br>
Rate Limit (NEW): 2times/2s

This endpoint allows user to transfer asset between parent and sub account.

### HTTP Request

`POST https://api.huobi.co.kr/v1/subuser/transfer`

```shell
curl -X POST "https://api.huobi.co.kr/v1/subuser/transfer" -H "Content-Type: application/json" -d '{"sub-uid": 12345, "currency": "btc", "amount": 123.5, "type": "master-transfer-in"}'
```

### Request Parameters

| Parameter | Data Type | Required | Description                                       | Value Range                                                  |
| :-------- | :-------- | :------- | :------------------------------------------------ | :----------------------------------------------------------- |
| sub-uid   | integer   | true     | The target sub account uid to transfer to or from | NA                                                           |
| currency  | string    | true     | The crypto currency to transfer                   | NA                                                           |
| amount    | decimal   | true     | The amount of asset to transfer                   | NA                                                           |
| type      | string    | true     | The type of transfer                              | master-transfer-in, master-transfer-out, master-point-transfer-in, master-point-transfer-out |

> Response:

```json
{
  "data":123456,
  "status":"ok"
}
```

### Response Content

The return data contains a single value instead of an object

| Field | Data Type | Description        |
| :---- | :-------- | :----------------- |
| data  | integer   | Unique transfer id |

## Query Deposit Address of Sub User

Parent user could query sub user's deposit address on corresponding chain, for a specific crypto currency (except IOTA).

API Key Permission：Read

The endpoint does not support deposit address querying for currency "IOTA" at this moment

### HTTP Request

`GET https://api.huobi.co.kr/v2/sub-user/deposit-address`

### Request Parameters

| Field Name | Data Type | Mandatory | Default Value | Description                                         |
| ---------- | --------- | --------- | ------------- | --------------------------------------------------- |
| subUid     | long      | true      | N/A           | Sub user UID                                        |
| currency   | string    | true      | N/A           | Crypto currency,refer to `GET /v1/common/currencys` |

> The above command returns JSON structured like this:

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

### Response Content

| Field Name | Data Type | Description            |
| ---------- | --------- | ---------------------- |
| code       | int       | Status code            |
| message    | string    | Error message (if any) |
| data       | object    |                        |
| { currency | string    | Crypto currency        |
| address    | string    | Deposit address        |
| addressTag | string    | Deposit address tag    |
| chain }    | string    | Block chain name       |

## Query Deposit History of Sub User

API Key Permission：Read

Parent user could query sub user's deposit history via this endpoint.

### HTTP Request

`GET https://api.huobi.co.kr/v2/sub-user/query-deposit`

### Request Parameters

|Field Name	|Data Type	|Mandatory	|Description												|
|-------		|-------		|-------		|-------													|
| subUid	| long		| ture		| Sub user UID												|
|currency	|string		|FALSE		|Cryptocurrency (default value: all)									|
|startTime	|long		|FALSE		|Farthest time (please refer to note 1 for valid range and default value)				|
|endTime	|long		|FALSE		|Nearest time (please refer to note 2 for valid range and default value)				|
|sort		|string		|FALSE		|Sorting order (enumerated values: asc, desc)							|
|limit		|int		|FALSE		|Maximum number of items in one page (valid range:[1,500]; default value:100)			|
|fromId	|long|FALSE|First record ID in this query (only valid for next page querying; please refer to note 3)	|

Note 1:<br>
startTime valid range: [(endTime – 30days), endTime]<br>
startTime default value: (endTime – 30days)<br>

Note 2:<br>
endTime valid range: Unlimited<br>
endTime default value: current time<br>

Note 3:<br>
Only when the number of items within the query window (between “startTime” and ”endTime”) exceeded the page limitation (defined by “limit”), Huobi server returns “nextId”. Once received “nextId”, API user should –<br>
1)	Be aware of that, some items within the query window were not returned due to the page size limitation.<br>
2)	In order to get these items from Huobi server, adopt the “nextId” as “fromId” and submit another request, with other request parameters no change.<br>
3)	“nextId” and “fromId” are for recurring query purpose and the ID itself does not have any business implication.<br>

> The above command returns JSON structured like this:

```json
{
    "code": 200,
    "data": [
        {
            "id": 33419472,
            "currency": "ltc",
            "chain": "ltc",
            "amount": 0.001000000000000000,
            "address": "LUuuPs5C5Ph3cZz76ZLN1AMLSstqG5PbAz",
            "state": "safe",
            "txHash": "847601d249861da56022323514870ddb96456ec9579526233d53e690264605a7",
            "addressTag": "",
            "createTime": 1587033225787,
            "updateTime": 1587033716616
        }
    ]
}
```

### Response Content

|	Field Name	|	Data Type	|	Mandatory	|	Description								|
|	-------		|	-------		|	-------		|	-------									|
|	code		|	integer		|	TRUE		|	Status code								|
|	message	|	string		|	FALSE		|	Error message (if any)							|
|	data		|	object		|	TRUE		|	                                                                                                 		|
|	{ id          	|	long		|	TRUE		|	Deposit id								|
|	currency	|	string		|	TRUE		|	Cryptocurrency							|
|	txHash  	|	string    	|	TRUE		|	The on-chain transaction hash                                                       	|
|	chain     	|	string		|	TRUE		|	Block chain name							|
|	amount	|	float      	|	TRUE		|	The number of crypto asset transferred                        		|
|	address	|	string		|	TRUE		|	The deposit source address			                               	|
|	addressTag	|	string    	|	FALSE		|	The user defined address tag                                               		|
|	state     	|	string    	|	TRUE		|	The state of this transfer (see below for details)                       	|
|	createTime	|	long    	|	TRUE		|	The timestamp in milliseconds for the transfer creation                      |
|	updateTime }   |	long    	|	TRUE 	            |	The timestamp in milliseconds for the transfer's latest update   |
|	nextId		|	long		|	FALSE		|	First record ID in next page (only valid if exceeded page size)	 |

**List of possible deposit state**

|State                | Description
|---------             |  -----------
|unknown         | On-chain transfer has not been received
|confirming      | On-chain transfer waits for first confirmation
|confirmed       | On-chain transfer confirmed for at least one block
|safe                  | Multiple on-chain confirmation happened
|orphan            | Confirmed but currently in an orphan branch

## Get the Aggregated Balance of all Sub-users

API Key Permission：Read<br>
Rate Limit (NEW): 2times/2s

This endpoint returns the aggregated balance from all the sub-users.

### HTTP Request

`GET https://api.huobi.co.kr/v1/subuser/aggregate-balance`

```shell
curl "https://api.huobi.co.kr/v1/subuser/aggregate-balance"
```

> The above command returns JSON structured like this:

```json
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
      }
   ]
```

### Request Parameters

<aside class="notice">No parameter is needed for this endpoint</aside>
### Response Content

<aside class="notice">The returned "data" object is a list of aggregated balances</aside>
| Field    | Data Type | Description                                                  |
| -------- | --------- | ------------------------------------------------------------ |
| currency | string    | The currency of this balance                                 |
| type     | string    | account type (spot, point)                                   |
| balance  | string    | The total balance in the main currency unit including all balance and frozen banlance |

## Get Account Balance of a Sub-User

API Key Permission：Read<br>
Rate Limit (NEW): 20times/2s

This endpoint returns the balance of a sub-user specified by sub-uid.

### HTTP Request

`GET https://api.huobi.co.kr/v1/account/accounts/{sub-uid}`

'sub-uid': The specified sub user id to get balance for.

```shell
curl "https://api.huobi.co.kr/v1/account/accounts/10758899"
```

### Request Parameters

<aside class="notice">No parameter is needed for this endpoint</aside>
> The above command returns JSON structured like this:

```json
"data": [
  {
    "id": 9910049,
    "type": "spot",
    "list": [
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
```

### Response Content

<aside class="notice">The returned "data" object is a list of accounts under this sub-user</aside>
| Field | Data Type | Description                          | Value Range      |
| ----- | --------- | ------------------------------------ | ---------------- |
| id    | integer   | Unique account id                    | NA               |
| type  | string    | The type of this account             | spot, otc, point |
| list  | object    | The balance details of each currency | NA               |

**Per list item content**

| Field    | Data Type | Description                           | Value Range   |
| -------- | --------- | ------------------------------------- | ------------- |
| currency | string    | The currency of this balance          | NA            |
| type     | string    | The balance type                      | trade, frozen |
| balance  | string    | The balance in the main currency unit | NA            |

# Trading

## Introduction

Trading APIs provide trading related functionality, including placing order, canceling order, order history query, trading history query, transaction fee query.

All endpoints in this section require authenticationThe parameter "account-id" and "source" should be set properly, refer to details in Request Parameters description below.

Below is the error code and description returned by Trading APIs

| Error Code                                                   | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| base-argument-unsupported                                    | The specified parameter is not supported                     |
| base-system-error                                            | Server internel error. For placing or canceling order, it is most related to cache issue, please try again later. |
| login-required                                               | Signature is missing                                         |
| parameter-required                                           | Parameter stop-price or operator is missing for stop-order type |
| base-record-invalid                                          | Failed to get data, please try again later                   |
| order-amount-over-limit                                      | The amount of order exceeds the limitation                   |
| base-symbol-trade-disabled                                   | The symbol is disabled for trading                           |
| base-operation-forbidden                                     | The operation is forbidden for current user or the symbol is not allowed to trade over OTC |
| account-get-accounts-inexistent-error                        | The account doesn't exist in current user                    |
| account-account-id-inexistent                                | The account id doesn't exist                                 |
| order-disabled                                               | The symbol is pending and not allowed to place order         |
| cancel-disabled                                              | The symbol is pending and not allowed to cancel order        |
| order-invalid-price                                          | The order price is invalid, usually exceeds the 10% of latest trade price |
| order-accountbalance-error                                   | The account balance is insufficient                          |
| order-limitorder-price-min-error                             | Sell price cannot be lower than specific price               |
| order-limitorder-price-max-error                             | Buy price cannot be higher than specific price               |
| order-limitorder-amount-min-error                            | Limit order amount can not be less than specific number      |
| order-limitorder-amount-max-error                            | Limit order amount can not be more than specific number      |
| order-etp-nav-price-min-error                                | Order price cannot be lower than specific percentage         |
| order-etp-nav-price-max-error                                | Order price cannot be higher than specific percentage        |
| order-orderprice-precision-error                             | Order price precision error                                  |
| order-orderamount-precision-error                            | Order amount precision error                                 |
| order-value-min-error                                        | Order value cannot be lower than specific value              |
| order-marketorder-amount-min-error                           | Market order sell amount cannot be less than specific amount |
| order-marketorder-amount-buy-max-error                       | Market order buy amount(value) cannot be more than specific amount(value) |
| order-marketorder-amount-sell-max-error                      | Market order sell amount cannot be more than specific amount |
| order-holding-limit-failed                                   | Exceed the holding limit of the currency                     |
| order-type-invalid                                           | Order type is invalid                                        |
| order-orderstate-error                                       | Order state is invalid                                       |
| order-date-limit-error                                       | Order query date exceed the limit                            |
| order-source-invalid                                         | Order source is invalid                                      |
| order-update-error                                           | Order update error                                           |
| order-user-cancel-forbidden                                  | IOC order is not allowed to cancel                           |
| order-price-greater-than-limit                               | Order price is higher than the limitation before market opens |
| order-price-less-than-limit                                  | Order price is lower than the limitation before market opens |
| order-stop-order-hit-trigger                                 | The stop orders triggered immediately are not allowed        |
| market-orders-not-support-during-limit-price-trading         | Market orders are not supported during limit-price trading   |
| price-exceeds-the-protective-price-during-limit-price-trading | The price exceeds the protective price during limit-price trading |
| invalid-client-order-id                                      | The client order id is duplicated (within last 24h)          |
| invalid-interval                                             | Query window is zero, negative or greater than limitation    |
| invalid-start-date                                           | The start date is invalid                                    |
| invalid-end-date                                             | The end date is invalid                                      |
| invalid-start-time                                           | The start time is invalid                                    |
| invalid-end-time                                             | The end time is invalid                                      |
| validation-constraints-required                              | The specified parameters is missing                          |
| not-found                                                    | The order id is not found                                    |
| base-not-found                                               | Too much invalid client order id in the past, try again after 1 hour |

## Place a New Order

API Key Permission：Trade

Rate Limit (NEW): 100times/2s

This endpoint place a new order and send to the exchange to be matched.

### HTTP Request

POST `/v1/order/orders/place`

```shell
curl -X POST -H "Content-Type: application/json" "https://api-cloud.huobi.co.kr/v1/order/orders/place" -d
'{
   "account-id": "100009",
   "amount": "10.1",
   "price": "100.1",
   "source": "api",
   "symbol": "ethusdt",
   "type": "buy-limit",
   "client-order-id": "a0001"
  }'
```

### Request Parameters

| Parameter       | Data Type | Required | Default  | Description                                                  | Value Range                                                  |
| --------------- | --------- | -------- | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| account-id      | string    | true     | NA       | The account id used for this trade                           | Refer to `GET /v1/account/accounts`                          |
| symbol          | string    | true     | NA       | The trading symbol to trade                                  | Refer to `GET /v1/common/symbols`                            |
| type            | string    | true     | NA       | The order type                                               | buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit |
| amount          | string    | true     | NA       | order size (for buy market order, it's order value)          | NA                                                           |
| price           | string    | false    | NA       | The order price (not available for market order)             | NA                                                           |
| source          | string    | false    | spot-api | When trade with spot use 'spot-api'                          | api                                                          |
| client-order-id | string    | false    | NA       | Client order ID (maximum 64-character length, to be unique within 24 hours) |                                                              |
| stop-price      | string    | false    | NA       | Trigger price of stop limit order                            |                                                              |
| operator        | string    | false    | NA       | operation charactor of stop price                            | gte – greater than and equal (>=), lte – less than and equal (<=) |

**buy-limit-maker ** 
When "Order Price"> = "Minimum Sell Price in the Market", the system will reject the order after it is submitted; 
When "Order price" <"Market Sell Price in the Market", after successful submission, this order will be accepted by the system. 
**sell-limit-maker**
When "Order Price" <= "Highest Market Buy Price", the system will refuse to accept this order when it is submitted; 
When "Order Price"> "Highest Market Buy Price", this order will be accepted by the system after successful submission.

> The above command returns JSON structured like this:

```json
  "data": "59378"
```

### Response Content

<aside class="notice">The returned data object is a single string which represents the order id</aside>
If client order ID duplicates with a previous order (within 24 hours), the endpoint reverts error message `invalid.client.order.id`.

## Place a Batch of Orders

API Key Permission：Trade<br>
Rate Limit (NEW): 50times/2s

A batch contains at most 10 orders

### HTTP Request

- POST ` /v1/order/batch-orders`

```json
 [
 	{
     "account-id": "123456",
     "price": "7801",
     "amount": "0.001",
     "symbol": "btcusdt",
     "type": "sell-limit",
     "client-order-id": "c1"
 	},
 	{
     "account-id": "123456",
     "price": "7802",
     "amount": "0.001",
     "symbol": "btcusdt",
     "type": "sell-limit",
     "client-order-id": "d2"
 	}
 ]
```

### Request Parameters

| Parameter       | Data Type | Required | Default  | Description                                                  |
| --------------- | --------- | -------- | -------- | ------------------------------------------------------------ |
| [{ account-id   | string    | true     | NA       | The account id, refer to `GET /v1/account/accounts`. Use 'spot' `account-id` for spot trading |
| symbol          | string    | true     | NA       | The trading symbol, i.e. btcusdt, ethbtc...(Refer to `GET /v1/common/symbols`) |
| type            | string    | true     | NA       | The type of order, including 'buy-market', 'sell-market', 'buy-limit', 'sell-limit', 'buy-ioc', 'sell-ioc', 'buy-limit-maker', 'sell-limit-maker' (refer to detail below), 'buy-stop-limit', 'sell-stop-limit'. |
| amount          | string    | true     | NA       | The order size (for buy market order, it's order value)      |
| price           | string    | false    | NA       | The order price (not available for market order)             |
| source          | string    | false    | spot-api | When trade with spot use 'spot-api'                          |
| client-order-id | string    | false    | NA       | Client order ID (maximum 64-character length, to be unique within 24 hours) |
| stop-price      | string    | false    | NA       | Trigger price of stop limit order                            |
| operator}]      | string    | false    | NA       | Operation character of stop price, use 'gte' for greater than and equal (>=), use 'lte' for less than and equal (<=) |

**buy-limit-maker**

If the order price is greater than or equal to the lowest selling price in the market, the order will be rejected.

If the order price is less than the lowest selling price in the market, the order will be accepted.

**sell-limit-maker**

If the order price is less than or equal to the highest buy price in the market, the order will be rejected.

If the order price is greater than the highest buy price in the market, the order will be accepted.

> Response:

```json
{
    "status": "ok",
    "data": [
        {
            "order-id": 61713400772,
            "client-order-id": "c1"
        },
        {
            "order-id": 61713400940,
            "client-order-id": "d2"
        }
    ]
}
```

### Response Content

| Field           | Data Type | Description                                 |
| --------------- | --------- | ------------------------------------------- |
| [{order-id      | integer   | The order id                                |
| client-order-id | string    | The client order id (if available)          |
| err-code        | string    | The error code (only for rejected order)    |
| err-msg}]       | string    | The error message (only for rejected order) |

If client order ID duplicates with a previous order (within 24 hours), the endpoint responds that previous order's Id and client order ID.

## Submit Cancel for an Order

API Key Permission：Trade

Rate Limit (NEW): 100times/2s

This endpoint submit a request to cancel an order.

<aside class="warning">This only submit the cancel request, the actual result of the canel request needs to be checked by order status or match result endpoints</aside>
### HTTP Request

POST `/v1/order/orders/{order-id}/submitcancel`

'order-id': the previously returned order id when order was created

```shell
curl -X POST "https://api-cloud.huobi.co.kr/v1/order/orders/59378/submitcancel"
```

### Request Parameters

| Parameter | Required | Data Type | Description |
| --------- | -------- | --------- | ----------- |
| order-id  | true     | string    | Order ID    |

> The above command returns JSON structured like this:

```json
  "data": "59378"
```

### Response Content

<aside class="notice">The returned data object is a single string which represents the order id</aside>
### Error Code

> Response:

```json
{
  "status": "error",
  "err-code": "order-orderstate-error",
  "err-msg": "Incorrect order state",
  "order-state":-1 // current order state
}
```

The possible values of "order-state" includes -

order-state           |  Description
---------       | -----------
-1| order was already closed in the long past (order state = canceled, partial-canceled, filled, partial-filled)
5| partial-canceled
6| filled
7| canceled
10| cancelling

## Submit Cancel for an Order (based on client order ID)

API Key Permission：Trade

Rate Limit (NEW): 100times/2s

This endpoint submit a request to cancel an order.

<aside class="warning">This only submit the cancel request, the actual result of the canel request needs to be checked by order status or match result endpoints</aside>
### HTTP Request

POST `/v1/order/orders/submitCancelClientOrder`

```shell
curl -X POST -H "Content-Type: application/json" "https://api-cloud.huobi.co.kr/v1/order/orders/submitCancelClientOrder" -d
'{
  "client-order-id": "a0001"
  }'
```

### Request Parameters

Parameter  | Data Type | Required | Default | Description
---------  | --------- | -------- | ------- | -----------
client-order-id     | string    | true     | NA      | Client order ID

> The above command returns JSON structured like this:

```json
  "data": "59378"
```

### Response Content

Field           | Data Type | Description
---------       | --------- | -----------
data         | integer | Cancellation status code

Status Code           |  Description
---------       | -----------
-1| order was already closed in the long past (order state = canceled, partial-canceled, filled, partial-filled)
0| client-order-id not found
5| partial-canceled
6| filled
7| canceled
10| cancelling


## Get All Open Orders

API Key Permission：Read

Rate Limit (NEW): 50times/2s

This endpoint returns all open orders which have not been filled completely.

```json
{
   "account-id": "100009",
   "symbol": "ethusdt",
   "side": "buy"
}
```

### HTTP Request

GET `/v1/order/openOrders`

```shell
curl "https://api-cloud.huobi.co.kr/v1/order/openOrders?account-id=100009&symbol=btcusdt&side=buy&size=5"
```

### Request Parameters

Parameter  | Data Type | Required | Default | Description                             | Value Range
---------  | --------- | -------- | ------- | -----------                             | -----------
account-id | string    | true    | NA      | The account id used for this trade      | Refer to `GET /v1/account/accounts`
symbol     | string    | true    | NA      | The trading symbol to trade             | Refer to `GET /v1/common/symbols`
side       | string    | false    | NA      | Filter on the direction of the trade    | buy, sell
from       | string    | false    | NA      |  start order ID the searching to begin with   |
direct       | string    | false (if field "from" is defined, this field "direct" becomes mandatory)   | NA      |  searching direction    | prev - in ascending order from the start order ID; next - in descending order from the start order ID
size       | int       | false    | 100      | The number of orders to return          | [1, 500]

> The above command returns JSON structured like this:

```json
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
```

### Response Content

Field               | Data Type | Description
---------           | --------- | -----------
id                  | integer   | order id
client-order-id | string | Client order id, can be returned from all open orders (if specified). 
symbol              | string    | The trading symbol to trade, e.g. btcusdt, bccbtc
price               | string    | The limit price of limit order
created-at          | int       | The timestamp in milliseconds when the order was created
type                | string    | The order type, possible values are: buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit
filled-amount       | string    | The amount which has been filled
filled-cash-amount  | string    | The filled total in quote currency
filled-fees         | string    | Transaction fee paid so far
source              | string    | The source where the order was triggered, possible values: sys, web, api, app
state               | string    | submitted, partial-filled, cancelling, created
stop-price    | string          | false 
operator       | string       | false  

## Submit Cancel for Multiple Orders by Criteria

API Key Permission：Trade

Rate Limit (NEW): 50times/2s

This endpoint submit cancellation for multiple orders at once with given criteria.

### HTTP Request

POST `/v1/order/orders/batchcancelopenorders`

```shell
curl -X POST -H 'Content-Type: application/json' "https://api-cloud.huobi.co.kr/v1/order/orders/batchCancelOpenOrders" -d
'{
  "account-id": "100009",
  "symbol": "btcusdt,btchusd",
  "side": "buy",
  "size": 5
}'
```

Parameter  | Data Type | Required | Default | Description                             | Value Range
---------  | --------- | -------- | ------- | -----------                             | -----------
account-id | string    | true    | NA      | The account id used for this cancel     | Refer to `GET /v1/account/accounts`
symbol     | string    | false    | NA      | The trading symbol list (maximum 10 symbols, separated by comma, default value all symbols)            | All supported trading symbol, e.g. btcusdt, bccbtc.Refer to `GET /v1/common/symbols`
side       | string    | false    | NA      | Filter on the direction of the trade    | buy, sell
size       | int       | false    | 100     | The number of orders to cancel          | [1, 100]

> The above command returns JSON structured like this:

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

### Response Content

Field           | Data Type | Description
---------       | --------- | -----------
success-count   | integer   | The number of cancel request sent successfully
failed-count    | integer   | The number of cancel request failed
next-id         | integer   | the next order id that can be cancelled

## Submit Cancel for Multiple Orders by IDs

API Key Permission：Trade

Rate Limit (NEW): 50times/2s

This endpoint submit cancellation for multiple orders at once with given ids.

### HTTP Request

POST `/v1/order/orders/batchcancel`

```shell
{
  "client-order-ids": [
    "5983466", "5722939", "5721027", "5719487"
  ]
}
```

### Request Parameters

 Parameter        | Data Type | Required | Description                                                  
 ---------------- | --------- | -------- | ------------------------------------------------------------ 
 order-ids        | list      | true     | The order ids to cancel. Max list size is 50.                
 client-order-ids | string[]  | false    | The client order ids to cancel (Either order-ids or client-order-ids can be filled in one batch request) 

> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "data": {
        "success": [
            "5983466"            
        ],
        "failed": [
            {
              "err-msg": "Incorrect order state",
              "order-state": 7,
              "order-id": "",
              "err-code": "order-orderstate-error",
              "client-order-id": "first"
            },
            {
              "err-msg": "Incorrect order state",
              "order-state": 7,
              "order-id": "",
              "err-code": "order-orderstate-error",
              "client-order-id": "second"
            },
            {
              "err-msg": "The record is not found.",
              "order-id": "",
              "err-code": "base-not-found",
              "client-order-id": "third"
            }
          ]
    }
}
```

### Response Content

| Field    | Data Type | Description                                                  |
| -------- | --------- | ------------------------------------------------------------ |
| {success | string[]  | Cancelled order list (Can be order ID list or client order list, upon the request) |
| failed}  | string[]  | Failed order list (Can be order ID list or client order list, upon the request) |

The failed id list has below fields

| Fields          | Data Type | Description                                                  |
| --------------- | --------- | ------------------------------------------------------------ |
| [{ order-id     | string    | The order id (if the request is based on order-ids)          |
| client-order-id | string    | The client order id (if the request is based on client-order-ids) |
| err-code        | string    | The error code (only applicable for rejected order)          |
| err-msg         | string    | The error message (only applicable for rejected order)       |
| order-state }]  | string    | Current order state (if available)                           |

The possible values of "order-state" includes -

order-state           |  Description
---------       | -----------
-1| order was already closed in the long past (order state = canceled, partial-canceled, filled, partial-filled)
5| partial-canceled
6| filled
7| canceled
10| cancelling


## Get the Order Detail of an Order

API Key Permission：Read

Rate Limit (NEW): 50times/2s

This endpoint returns the detail of one order. The API created order will not exist after cancelling 2 hours.

### HTTP Request

GET `/v1/order/orders/{order-id}`

```shell
curl "https://api-cloud.huobi.co.kr/v1/order/orders/59378"
```

### Request Parameters

| Parameter | Required | Data Type | Description |
| --------- | -------- | --------- | ----------- |
| order-id  | true     | string    | Order ID    |

> The above command returns JSON structured like this:

```json
{  
  "data": {
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

### Response Content

Field               | Data Type | Description
---------           | --------- | -----------
id                  | integer   | order id
client-order-id | string | Client order id ("client-order-id" (if specified) can be returned from all open orders.	"client-order-id"  (if specified) can be returned only from closed orders (state <> canceled) created within 7 days.	"client-order-id"  (if specified) can be returned only from closed orders (state = canceled) created within 24 hours.) 
symbol              | string    | The trading symbol to trade, e.g. btcusdt, bccbtc
account-id          | string    | The account id which this order belongs to
amount              | string    | The amount of base currency in this order
price               | string    | The limit price of limit order
created-at          | int       | The timestamp in milliseconds when the order was created
finished-at         | int       | The timestamp in milliseconds when the order was changed to a final state. This is not the time the order is matched.
canceled-at         | int       | The timestamp in milliseconds when the order was canceled, if not canceled then has value of 0
type                | string    | The order type, possible values are: buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit
filled-amount       | string    | The amount which has been filled
filled-cash-amount  | string    | The filled total in quote currency
filled-fees         | string    | Transaction fee paid so far
source              | string    | The source where the order was triggered, possible values: sys, web, api, app
state               | string    | Order state: submitted, partial-filled, filled, canceled
exchange            | string    | Internal data
batch               | string    | Internal data
stop-price|string|trigger price of stop limit order
operator|string|operation character of stop price



## Get the Order Detail of an Order (based on client order ID)

API Key Permission：Read

Rate Limit (NEW):50times/2s

This endpoint returns the detail of one order by specified client order id (within 24 hours). The API created order will not exist after cancelling 2 hours.

### HTTP Request

GET `/v1/order/orders/getClientOrder`

```shell
curl "https://api-cloud.huobi.co.kr/v1/order/orders/getClientOrder?clientOrderId=a0001"
```

### Request Parameters

Parameter  | Data Type | Required | Default | Description
---------  | --------- | -------- | ------- | -----------
clientOrderID     | string    | true     | NA      | Client order ID

> The above command returns JSON structured like this:

```json
{  
  "data": {
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

### Response Content

Field               | Data Type | Description
---------           | --------- | -----------
id                  | integer   | order id
client-order-id | string | Client order id (only those orders created within 24 hours can be returned.) 
symbol              | string    | The trading symbol to trade, e.g. btcusdt, bccbtc
account-id          | string    | The account id which this order belongs to
amount              | string    | The amount of base currency in this order
price               | string    | The limit price of limit order
created-at          | int       | The timestamp in milliseconds when the order was created
finished-at         | int       | The timestamp in milliseconds when the order was changed to a final state. This is not the time the order is matched.
canceled-at         | int       | The timestamp in milliseconds when the order was canceled, if not canceled then has value of 0
type                | string    | The order type, possible values are: buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit
filled-amount       | string    | The amount which has been filled
filled-cash-amount  | string    | The filled total in quote currency
filled-fees         | string    | Transaction fee paid so far
source              | string    | The source where the order was triggered, possible values: sys, web, api, app
state               | string    | Order state: submitted, partial-filled, filled, canceled
exchange            | string    | Internal data
batch               | string    | Internal data
stop-price|string|trigger price of stop limit order
operator|string|operation character of stop price

If the client order ID is not found, following error message will be returned:
{
    "status": "error",
    "err-code": "base-record-invalid",
    "err-msg": "record invalid",
    "data": null
}



## Get the Match Result of an Order

API Key Permission：Read<br>
Rate Limit (NEW): 50times/2s

This endpoint returns the match result of an order.

### HTTP Request

GET `/v1/order/orders/{order-id}/matchresults`

```shell
curl "https://api-cloud.huobi.co.kr/v1/order/orders/59378/matchresults"
```

### Request Parameters

| Parameter | Required | Data Type | Description |
| --------- | -------- | --------- | ----------- |
| order-id  | true     | string    | Order ID    |

> The above command returns JSON structured like this:

```json
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
      "role": maker,
      "filled-points": "0.0",
      "fee-deduct-currency": ""
    }
  ]
```

### Response Content

<aside class="notice">The return data contains a list and each item in the list represents a match result</aside>
Parameter           | Data Type | Description
---------           | --------- | -----------
id                  | integer   | Internal id
symbol              | string    | The trading symbol to trade, e.g. btcusdt, bccbtc
order-id            | string    | The order id of this order
match-id            | string    | The match id of this match
trade-id            | int    | Unique trade ID (NEW)
price               | string    | The limit price of limit order
created-at          | int       | The timestamp in milliseconds when the match and fill is done
type                | string    | The order type, possible values are: buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit
filled-amount       | string    | The amount which has been filled
filled-fees         | string    | Transaction fee (positive value). If maker rebate applicable, revert maker rebate value per trade (negative value). 
fee-currency | string | Currency of transaction fee or transaction fee rebate (transaction fee of buy order is based on base currency, transaction fee of sell order is based on quote currency; transaction fee rebate of buy order is based on quote currency, transaction fee rebate of sell order is based on base currency) 
source              | string    | The source where the order was triggered, possible values: sys, web, api, app
role                  | string   | the role in the transaction: taker or maker
filled-points      | string   | deduction amount (unit: in ht or hbpoint) 
fee-deduct-currency      | string   | deduction type. if blank, the transaction fee is based on original currency; if showing value as "ht", the transaction fee is deducted by HT; if showing value as "hbpoint", the transaction fee is deducted by HB point.    

Notes:<br>

- The calculated maker rebate value inside ‘filled-fees’ would not be paid immediately.<br>

## Search Past Orders

API Key Permission：Read<br>
Rate Limit (NEW): 50times/2s

This endpoint returns orders based on a specific searching criteria. The API created order will not exist after cancelling 2 hours.

- Upon user defined “start-time” AND/OR “end-time”, Huobi server will revert back historical orders whose order creation time falling into the period. The maximum time span between “start-time” and “end-time” is 48-hour. Farthest order searchable should be within recent 180 days. In case either “start-time” or “end-time” is defined, Huobi server will ignore “start-date” and “end-date” regardless they were filled or not.
- If user does neither define “start-time” nor “end-time”, but “start-date”/”end-date”, the order searching will be based on defined “date range”, as usual.
- If user does not define any of “start-time”/”end-time”/”start-date”/”end-date”, by default Huobi server will treat current time as “end-time”, and then revert back historical orders within recent 48 hours.

Huobi Korea suggests API users to search historical orders based on “time” filter instead of “date”. In the near future, Huobi Korea would remove “start-date”/”end-date” fields from the endpoint, through another notification.

### HTTP Request

GET `/v1/order/orders`

```shell
curl "https://api-cloud.huobi.co.kr/v1/order/orders?symbol=ethusdt&type=buy-limit&staet=filled"
```

### Request Parameters

| Parameter  | Data Type | Required | Default | Description                                                  | Value Range                                                  |
| ---------- | --------- | -------- | ------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| symbol     | string    | true     | NA      | The trading symbol                                           | All supported trading symbols, e.g. btcusdt, bccbtc          |
| types      | string    | false    | NA      | One or more types of order to include in the search, use comma to separate. | buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-stop-limit, sell-stop-limit |
| start-time | long      | false    | -48h    | Search starts time, UTC time in millisecond                  | Value range [((end-time) – 48h), (end-time)], maximum query window size is 48 hours, query window shift should be within past 180 days, query window shift should be within past 2 hours for cancelled order (state = "canceled") |
| end-time   | long      | false    | present | Search ends time, UTC time in millisecond                    | Value range [(present-179d), present], maximum query window size is 48 hours, query window shift should be within past 180 days, queriable range should be within past 2 hours for cancelled order (state = "canceled") |
| start-date | string    | false    | -1d     | Search starts date, in format yyyy-mm-dd                     | Value range [((end-date) – 1), (end-date)], maximum query window size is 2 days, query window shift should be within past 180 days, query window shift should be within past 2 hours for cancelled order (state = "canceled") |
| end-date   | string    | false    | today   | Search ends date, in format yyyy-mm-dd                       | Value range [(today-179), today], maximum query window size is 2 days, query window shift should be within past 180 days, queriable range should be within past 2 hours for cancelled order (state = "canceled") |
| states     | string    | true     | NA      | One or more states of order to include in the search, use comma to separate. | submitted, partial-filled, partial-canceled, filled, canceled, created |
| from       | string    | false    | NA      | Search order id to begin with                                | NA                                                           |
| direct     | string    | false    | both    | Search direction when 'from' is used                         | next, prev                                                   |
| size       | int       | false    | 100     | The number of orders to return                               | [1, 100]                                                     |

> The above command returns JSON structured like this:

```json
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
  ]
```

### Response Content

| Field              | Data Type | Description                                                  |
| ------------------ | --------- | ------------------------------------------------------------ |
| id                 | integer   | Order id                                                     |
| client-order-id    | string    | Client order id ("client-order-id" (if specified) can be returned from all open orders."client-order-id" (if specified) can be returned only from closed orders (state <> canceled) created within 7 days.	only those closed orders (state = canceled) created within 24 hours can be returned.) |
| account-id         | integer   | Account id                                                   |
| user-id            | integer   | User id                                                      |
| amount             | string    | The amount of base currency in this order                    |
| symbol             | string    | The trading symbol to trade, e.g. btcusdt, bccbtc            |
| price              | string    | The limit price of limit order                               |
| created-at         | int       | The timestamp in milliseconds when the order was created     |
| canceled-at        | int       | The timestamp in milliseconds when the order was canceled, or 0 if not canceled |
| finished-at        | int       | The timestamp in milliseconds when the order was finished, or 0 if not finished |
| type               | string    | The order type, possible values are: buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit |
| filled-amount      | string    | The amount which has been filled                             |
| filled-cash-amount | string    | The filled total in quote currency                           |
| filled-fees        | string    | Transaction fee paid so far                                  |
| source             | string    | The source where the order was triggered, possible values: sys, web, api, app |
| state              | string    | created, submitted, partial-filled, filled, canceled, partial-canceled |
| exchange           | string    | Internal data                                                |
| batch              | string    | Internal data                                                |
| stop-price         | string    | trigger price of stop limit order                            |
| operator           | string    | operation character of stop price                            |

### Error code for invalid start-date/end-date

|err-code| scenarios|
|--------|---------------------------------------------------------------|
|invalid_interval| Start date is later than end date; the date between start date and end date is greater than 2 days|
|invalid_start_date| Start date is a future date; or start date is earlier than 180 days ago.|
|invalid_end_date| end date is a future date; or end date is earlier than 180 days ago.|



## Search Historical Orders within 48 Hours

API Key Permission：Read

Rate Limit (NEW): 20times/2s

This endpoint returns orders based on a specific searching criteria.

### HTTP Request

GET `/v1/order/history`

```json
{
   "symbol": "btcusdt",
   "start-time": "1556417645419",
   "end-time": "1556533539282",
   "direct": "prev",
   "size": "10"
}
```

### Request Parameters

Parameter  | Required | Data Type | Description | Default Value                                  | Value Range
---------  | --------- | -------- | ------- | -----------                                   | ----------
symbol     | false  | string | The trading symbol to trade      |all      |All supported trading symbol, e.g. btcusdt, bccbtc.Refer to `GET /v1/common/symbols` |
start-time      | false | long | Start time (included)   |The time 48 hours ago      |UTC time in millisecond |
end-time | false | long | End time (included)  | The query time     |UTC time in millisecond |
direct   | false | string | Direction of the query. (Note: If the total number of items in the search result is within the limitation defined in “size”, this field does not take effect.)| next     |prev, next   |
size     | false  | int | Number of items in each response  |100      | [10,1000] |



> The above command returns JSON structured like this:

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

### Response Content

Field               | Data Type | Description
---------           | --------- | -----------
{account-id         | long      | Account ID
amount              | string    | Order size
canceled-at             | long   | Order cancellation time
created-at              | long    | Order creation time
field-amount              | string    | Executed order amount
field-cash-amount               | string    | Executed cash amount
field-fees          | string       | Transaction fee
finished-at         | long       | Last trade time
id         | long       | Order ID
client-order-id | string | Client order id ("client-order-id" (if specified) can be returned only from closed orders (state <> canceled) created within 48 hours, upon order creation time.	only those closed orders (state = canceled) created within 24 hours can be returned.) 
price                | string   | Order price
source       | string    | Order source
state  | string    | Order status ( filled, partial-canceled, canceled )
symbol         | string    | Trading symbol
stop-price|string|trigger price of stop limit order
operator|string|operation character of stop price
type}              | string    | Order type (buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit)
next-time               | long    | Next query “start-time” (in response of “direct” = prev), Next query “end-time” (in response of “direct” = next). Note: Only when the total number of items in the search result exceeded the limitation defined in “size”, this field exists. UTC time in millisecond. 


## Search Match Results

API Key Permission：Read

Rate Limit (NEW): 20times/2s

This endpoint returns the match results of past and open orders based on specific search criteria.

### HTTP Request

GET `/v1/order/matchresults`

```shell
curl "https://api-cloud.huobi.co.kr/v1/order/matchresults?symbol=ethusdt"
```

### Request Parameters

Parameter  | Data Type | Required | Default | Description                                   | Value Range
---------  | --------- | -------- | ------- | -----------                                   | ----------
symbol     | string    | true     | NA      | The trading symbol to trade                   | All supported trading symbol, e.g. btcusdt, bccbtc.Refer to `GET /v1/common/symbols`
types      | string    | false    | all      | The types of order to include in the search   | buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit
states     | string    | false    | NA      | The states of order to include in the search  | submitted, partial-filled, partial-canceled, filled, canceled
start-date | string    | false    | -1d    | Search starts date, in format yyyy-mm-dd      |Value range [((end-date) – 1), (end-date)], maximum query window size is 2 days, query window shift should be within past 61 days |
end-date   | string    | false    | today   | Search ends date, in format yyyy-mm-dd        |Value range [(today-60), today], maximum query window size is 2 days, query window shift should be within past 61 days|
from       | string    | false    | NA      | Search internal id to begin with                 | NA
direct     | string    | false    | both    | Search direction when 'from' is used          | next, prev
size       | int       | false    | 100     | The number of orders to return                | [1, 100]

> The above command returns JSON structured like this:

```json
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
      "role": "taker",
      "filled-points": "0.0",
      "fee-deduct-currency": ""
    }
  ]
```

### Response Content

<aside class="notice">The return data contains a list and each item in the list represents a match result</aside>
| Field               | Data Type | Description                                                  |
| ------------------- | --------- | ------------------------------------------------------------ |
| id                  | long      | Record id, non sequential, it can be used in "from" field for next request |
| symbol              | string    | The trading symbol to trade, e.g. btcusdt, bccbtc            |
| order-id            | long      | The order id of this order                                   |
| match-id            | long      | The match id of this match                                   |
| trade-id            | long      | Unique trade ID                                              |
| price               | string    | The limit price of limit order                               |
| created-at          | int       | The timestamp in milliseconds when the match and fill is done |
| type                | string    | The order type, possible values are: buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit |
| filled-amount       | string    | The amount which has been filled                             |
| filled-fees         | string    | Transaction fee (positive value). If maker rebate applicable, revert maker rebate value per trade (negative value). |
| fee-currency        | string    | Currency of transaction fee or transaction fee rebate (transaction fee of buy order is based on base currency, transaction fee of sell order is based on quote currency; transaction fee rebate of buy order is based on quote currency, transaction fee rebate of sell order is based on base currency) |
| source              | string    | The source where the order was triggered, possible values: sys, web, api, app |
| role                | string    | The role in the transaction: taker or maker.                 |
| filled-points       | string    | deduction amount (unit: in ht or hbpoint)                    |
| fee-deduct-currency | string    | deduction type: ht or hbpoint.                               |

Notes:<br>

- The calculated maker rebate value inside ‘filled-fees’ would not be paid immediately.<br>

### Error code for invalid start-date/end-date

|err-code| scenarios|
|--------|---------------------------------------------------------------|
|invalid_interval| Start date is later than end date; the date between start date and end date is greater than 2 days|
|invalid_start_date| Start date is a future date; or start date is earlier than 61 days ago.|
|invalid_end_date| end date is a future date; or end date is earlier than 61 days ago.|

## Get Current Fee Rate Applied to The User

This endpoint returns the current transaction fee rate applied to the user.

API Key Permission：Read

```shell
curl "https://api.huobi.co.kr/v2/reference/transact-fee-rate?symbols=btcusdt,ethusdt,ltcusdt"
```

### HTTP Request

`GET /v2/reference/transact-fee-rate`

### Request Parameters

Parameter | Data Type | Required | Default | Description                 | Value Range
--------- | --------- | -------- | ------- | -----------                 | -----------
symbols    | string    | true     | NA      | The trading symbols to query, separated by comma | Refer to `GET /v1/common/symbols`

> Response:

```json
 {
  "code": "200",
  "data": [
     {
        "symbol": "btcusdt",
        "makerFeeRate":"0.002",
        "takerFeeRate":"0.002",
        "actualMakerRate": "0.002",
        "actualTakerRate":"0.002
     },
     {
        "symbol": "ethusdt",
        "makerFeeRate":"0.002",
        "takerFeeRate":"0.002",
        "actualMakerRate": "0.002",
        "actualTakerRate":"0.002
     },
     {
        "symbol": "ltcusdt",
        "makerFeeRate":"0.002",
        "takerFeeRate":"0.002",
        "actualMakerRate": "0.002",
        "actualTakerRate":"0.002
     }
   ]
}
```

### Response Content

|	Field Name	|	Data Type	|	Description	|
--------- | --------- | -----------|
|	code	|	integer	|	Status code	|
|	message	|	string	|	Error message (if any)	|
|	data	|	object	|		|
|	{ symbol	|	string	|Trading symbol	|
|	makerFeeRate	|	string	|	Basic fee rate – passive side (positive value);If maker rebate applicable, revert maker rebate rate (negative value).	|
|	takerFeeRate	|	string	|	Basic fee rate – aggressive side	|
|	actualMakerRate	|	string	|	Deducted fee rate – passive side (positive value). If deduction is inapplicable or disabled, return basic fee rate.If maker rebate applicable, revert maker rebate rate (negative value).	|
|	actualTakerRate }	|	string	|	Deducted fee rate – aggressive side. If deduction is inapplicable or disabled, return basic fee rate.	|


# Websocket Market Data

## General

### Websocket URL

**Websocket Market Feed**

**`wss://api-cloud.huobi.co.kr/ws`**


### Data Format

All return data of websocket APIs are compressed with GZIP so they need to be unzipped.

### Heartbeat and Connection

```json
{"ping": 1492420473027} 
```

After connected to Huobi's Websocket server, the server will send heartbeat periodically (currently at 5s interval). The heartbeat message will have an integer in it, e.g.

```json
{"pong": 1492420473027} 
```

When client receives this heartbeat message, it should response with a matching "pong" message which has the same integer in it, e.g.

<aside class="warning">After the server sent two consecutive heartbeat messages without receiving at least one matching "pong" response from a client, then right before server sends the next "ping" heartbeat, the server will disconnect this client</aside>

### Subscribe to Topic

To receive data you have to send a "sub" message first.

```json
{
  "sub": "market.btcusdt.kline.1min",
  "id": "id1"
}
```

{
  "sub": "topic to sub",
  "id": "id generate by client"
}

After successfully subscribed, you will receive a response to confirm subscription

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcusdt.kline.1min",
  "ts": 1489474081631
}
```

Then, you will received message when there is update in this topic

### Unsubscribe

To unsubscribe, you need to send below message

```json
{
  "unsub": "market.btcusdt.trade.detail",
  "id": "id4"
}
```

{
  "unsub": "topic to unsub",
  "id": "id generate by client"
}

And you will receive a message to confirm the unsubscribe

```json
{
  "id": "id4",
  "status": "ok",
  "unsubbed": "market.btcusdt.trade.detail",
  "ts": 1494326028889
}
```

### Pull Data

While connected to websocket, you can also use it in pull style by sending message to the server.

To request pull style data, you send below message

{
  "req": "topic to req",
  "id": "id generate by client"
}

You will receive a response accordingly and immediately

### Rate Limit

**Rate limt of pull style query (req)**

The limitation of single connection is 100 ms.

### Error Code

Below is the error code, error message and description

| Error Code  | Error Message               | Description                            |
| ----------- | --------------------------- | -------------------------------------- |
| bad-request | invalid topic               | Parameter topic is invalid             |
| bad-request | invalid symbol              | Parammeter symbol is invalid           |
| bad-request | symbol trade not open now   | The market is not open for this symbol |
| bad-request | 429 too many request        | Request exceed limit                   |
| bad-request | unsub with not subbed topic | The topic is not subscribed            |
| bad-request | not json string             | The request is not JSON format         |
| bad-request | request timeout             | The request is time out                |

## Market Candlestick

This topic sends a new candlestick whenever it is available.

### Topic

`market.$symbol$.kline.$period$`

> Subscribe request

```json
{
  "sub": "market.ethbtc.kline.1min",
  "id": "id1"
}
```

### Topic Parameter

Parameter | Data Type | Required | Description                      | Value Range
--------- | --------- | -------- | -----------                      | -----------
symbol    | string    | true     | Trading symbol     | All supported trading symbol, e.g. btcusdt, bccbtc.Refer to `GET /v1/common/symbols`
period     | string    | true     | Candlestick interval   | 1min, 5min, 15min, 30min, 60min, 4hour, 1day, 1mon, 1week, 1year

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

### Update Content

Field     | Data Type | Description
--------- | --------- | -----------
id        | integer   | UNIX epoch timestamp in second as response id
amount    | float     | Aggregated trading volume during the interval (in base currency)
count     | integer   | Number of trades during the interval
open      | float     | Opening price during the interval
close     | float     | Closing price during the interval
low       | float     | Low price during the interval
high      | float     | High price during the interval
vol       | float     | Aggregated trading value during the interval (in quote currency)

<aside class="notice">When symbol is set to "hb10" or "huobi10", amount, count, and vol will always have the value of 0</aside>
### Pull Request

Pull request is supported with extra parameters to define the range. The maximum number of ticks in each response is 300.

```json
{
  "req": "market.$symbol.kline.$period",
  "id": "id generated by client",
  "from": "from time in epoch seconds",
  "to": "to time in epoch seconds"
}
```

Parameter | Data Type | Required | Default Value                          | Description      | Value Range
--------- | --------- | -------- | -------------                          | -----------      | -----------
from      | integer   | false    | 1501174800(2017-07-28T00:00:00+08:00)  | "From" time (epoch time in second)   | [1501174800, 2556115200]
to        | integer   | false    | 2556115200(2050-01-01T00:00:00+08:00)  | "To" time (epoch time in second)      | [1501174800, 2556115200] or ($from, 2556115200] if "from" is set

## Market Depth

This topic sends the latest market by price order book in snapshot mode at 1-second interval.

### Topic

`market.$symbol.depth.$type`

> Subscribe request

```json
{
  "sub": "market.btcusdt.depth.step0",
  "id": "id1"
}
```

### Topic Parameter

Parameter | Data Type | Required | Default Value         | Description                                       | Value Range
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | Trading symbol                   | Refer to `GET /v1/common/symbols`
type      | string    | true     | step0                 | Market depth aggregation level, details below     | step0, step1, step2, step3, step4, step5

**"type" Details**

Value     | Description
--------- | ---------
step0     | No market depth aggregation
step1     | Aggregation level = precision*10
step2     | Aggregation level = precision*100
step3     | Aggregation level = precision*1000
step4     | Aggregation level = precision*10000
step5     | Aggregation level = precision*100000

While type is set as ‘step0’, the market depth data supports up to 150 levels.
While type is set as ‘step1’, ‘step2’, ‘step3’, ‘step4’, or ‘step5’, the market depth data supports up to 20 levels.

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

### Update Content

<aside class="notice">Under 'tick' object there is a list of bids and a list of asks</aside>
Field     | Data Type | Description
--------- | --------- | -----------
bids      | object    | The current all bids in format [price, quote volume]
asks      | object    | The current all asks in format [price, quote volume]
version   | integer   | Internal data
ts        | integer   | The UNIX timestamp in milliseconds adjusted to Singapore time

<aside class="notice">When symbol is set to "hb10" amount, count, and vol will always have the value of 0</aside>
### Pull Request

Pull request is supported.

```json
{
  "req": "market.btcusdt.depth.step0",
  "id": "id10"
}
```

## Market By Price (incremental update)

User could subscribe to this channel to receive incremental update of Market By Price order book. Refresh message, the full image of the order book, is acquirable from the same channel, upon "req" request.

**MBP incremental channel & its REQ channel)**

**`wss://api.huobi.co.kr/feed`**
or
**`wss://api-aws.huobi.co.kr/feed`**

Suggested downstream data processing:<br>
1)	Subscribe incremental updates and start to cache them;<br>
2)	Request refresh message (with same number of levels), and base on its “seqNum” to align it with the cached incremental message which having a same “prevSeqNum”;<br>
3)	Start to continuously process incremental messages to build up MBP book;<br>
4)	The “prevSeqNum” of current incremental message must be same with “seqNum” of previous message, otherwise it implicates message loss which should require another round refresh message retrieval and alignment;<br>
5)	Once receiving a new price level from incremental message, that price level should be inserted into appropriate position of existing MBP book;<br>
6)	Once receiving an updated “size” at existing price level from incremental message, the level size should be replaced directly by the new value;<br>
7)	Once receiving a “size=0” at existing price level from incremental message, that price level should be removed from MBP book;<br>
8)	If one incremental message includes updates of multiple price levels, all of those levels should be updated simultaneously in MBP book.<br>

Currently Huobi Korea only supports 5-level/20-level MBP incremental channel and 150-level incremental channel, the differences between them are -<br>
1) Different depth of market.<br>
2) 5-level/20-level incremental MBP is a tick by tick feed, which means whenever there is an order book change at that level, it pushes an update; 150 levels incremental MBP feed is based on the gap between two snapshots at 100ms interval.<br>
3) While there is single side order book update, either bid or ask, the incremental message sent from 5-level/20-level MBP feed only contains that side update. <br>

```json
{
    "ch": "market.btcusdt.mbp.5",
    "ts": 1573199608679,
    "tick": {
        "seqNum": 100020146795,
        "prevSeqNum": 100020146794,
        "asks": [
            [645.140000000000000000, 26.755973959140651643]
        ]
    }
}
```

But the incremental message from 150 levels MBP feed contains not only that side update and also a blank object for another side.

```json
{
    "ch":"market.btcusdt.mbp.150",
    "ts":1573199608679,
    "tick":{
        "seqNum":100020146795,
        "prevSeqNum":100020146794,
        "bids":[ ],
        "asks":[
            [645.14,26.75597395914065]
        ]
    }
}
```

In the near future, Huobi Korea will align the update behavior of 150-level incremental channel with 5-level/20-level, which means while single side order book changed (either bid or ask), the update message will be no longer including a blank object for another side.<br>
4) While there is nothing change between two snapshots in past 100ms, the 150 levels incremental MBP feed still sends out a message which containing two blank objects – bids & asks. <br>

```json
{
    "ch":"market.zecusdt.mbp.150",
    "ts":1585074391470,
    "tick":{
        "seqNum":100772868478,
        "prevSeqNum":100772868476,
        "bids":[  ],
        "asks":[  ]
    }
}
```

But 5-level/20-level incremental channel won’t disseminate any update in such a case.<br>
In the near future, Huobi Korea  will align the update behavior of 150-level incremental channel with 5-level/20-level, which means while there is no order book change at all, the channel will be no longer disseminating blank objects any more.<br>
5) 5-level/20-level incremental channel only supports these symbols at this stage - btcusdt,ethusdt,xrpusdt,eosusdt,ltcusdt,etcusdt,adausdt,dashusdt,bsvusdt, while 150-level incremental channel supports all.<br>

REQ channel supports refreshment message for 5-level, 20-level, and 150-level.

### Subscribe incremntal updates

`market.$symbol.mbp.$levels`

> Sub request

```json
{
  "sub": "market.btcusdt.mbp.5",
  "id": "id1"
}
```

### Request refresh update

`market.$symbol.mbp.$levels`

> Req request

```json
{
  "req": "market.btcusdt.mbp.5",
  "id": "id2"
}
```

### Parameters

| Field Name | Data Type | Mandatory | Default Value | Description                                    | Value Range                                                  |
| ---------- | --------- | --------- | ------------- | ---------------------------------------------- | ------------------------------------------------------------ |
| symbol     | string    | true      | NA            | Trading symbol (wildcard inacceptable)         |                                                              |
| levels     | integer   | true      | NA            | Number of price levels (Valid value: 5,20,150) | Only support the number of price levels at 5, 20, or 150 at this point of time. |

> Response (Incremental update subscription)

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcusdt.mbp.5",
  "ts": 1489474081631 //system response time
}
```

> Incremental Update (Incremental update subscription)

```json
{
	"ch": "market.btcusdt.mbp.5",
	"ts": 1573199608679, //system update time
	"tick": {
		"seqNum": 100020146795,
		"prevSeqNum": 100020146794,
		"asks": [
			[645.140000000000000000, 26.755973959140651643] // [price, size]
		]
	}
}
```

> Response (Refresh update acquisition)

```json
{
	"id": "id2",
	"rep": "market.btcusdt.mbp.5",
	"status": "ok",
	"data": {
		"seqNum": 100020142010,
		"bids": [
			[618.37, 71.594], // [price, size]
			[423.33, 77.726],
			[223.18, 47.997],
			[219.34, 24.82],
			[210.34, 94.463]
    ],
		"asks": [
			[650.59, 14.909733438479636],
			[650.63, 97.996],
			[650.77, 97.465],
			[651.23, 83.973],
			[651.42, 34.465]
		]
	}
}
```

### Update Content

| Field Name | Data Type | Description                                                  |
| ---------- | --------- | ------------------------------------------------------------ |
| seqNum     | integer   | Sequence number of the message                               |
| prevSeqNum | integer   | Sequence number of previous message                          |
| bids       | object    | Bid side, (in descending order of “price”), ["price","size"] |
| asks       | object    | Ask side, (in ascending order of “price”), ["price","size"]  |

## Market By Price (refresh update)

User could subscribe to this channel to receive refresh update of Market By Price order book. The update interval is around 100ms.

### Subscription

`market.$symbol.mbp.refresh.$levels`

```json
{
"sub": "market.btcusdt.mbp.refresh.20",
"id": "id1"
}

```

### Parameters

| Field Name | Data Type | Mandatory | Default Value | Description                            | Value Range |
| ---------- | --------- | --------- | ------------- | -------------------------------------- | ----------- |
| symbol     | string    | true      | NA            | Trading symbol (wildcard inacceptable) |             |
| levels     | integer   | true      | NA            | Number of price levels                 | 5,10,20     |

> Response

```json
{
"id": "id1",
"status": "ok",
"subbed": "market.btcusdt.mbp.refresh.20",
"ts": 1489474081631 //system response time
}
```

> Response

```json
{
"ch": "market.btcusdt.mbp.refresh.20",
"ts": 1573199608679, //system update time
"tick": {

		"seqNum": 100020142010,
		"bids": [
			[618.37, 71.594], // [price, size]
			[423.33, 77.726],
			[223.18, 47.997],
			[219.34, 24.82],
			[210.34, 94.463], ... // rest levels omitted
   		],
		"asks": [
			[650.59, 14.909733438479636],
			[650.63, 97.996],
			[650.77, 97.465],
			[651.23, 83.973],
			[651.42, 34.465], ... // rest levels omitted
		]
}
}
```

### Update Content

| Field Name | Data Type | Description                                                  |
| ---------- | --------- | ------------------------------------------------------------ |
| seqNum     | integer   | Sequence number of the message                               |
| bids       | object    | Bid side, (in descending order of “price”), ["price","size"] |
| asks       | object    | Ask side, (in ascending order of “price”), ["price","size"]  |

## Best Bid/Offer

While any of best bid, best ask, best bid size, best ask size is changing, subscriber can receive BBO (Best Bid/Offer) update in tick by tick mode.

### Topic

`market.$symbol.bbo`

> Subscribe request

```json
{
  "sub": "market.btcusdt.bbo",
  "id": "id1"
}
```

### Topic parameter

Parameter | Data Type | Required | Default Value         | Description                                       | Value Range
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | Trading symbol                   | Refer to `GET /v1/common/symbols`

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

### Update Content

Field     | Data Type | Description
--------- | --------- | -----------
symbol     | string    | Trading symbol
quoteTime      | long    | Quote time
bid      | float    | Best bid
bidSize      | float    | Best bid size
ask      | float    | Best ask
askSize      | float    | Best ask size
seqId | int | Sequence number 


## Trade Detail

This topic sends the latest completed trade. It updates in tick by tick mode.

### Topic

`market.$symbol.trade.detail`

> Subscribe request

```json
{
  "sub": "market.btcusdt.trade.detail",
  "id": "id1"
}
```

### Topic Parameter

Parameter | Data Type | Required | Default Value         | Description                                       | Value Range
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | Trading symbol                     | Refer to `GET /v1/common/symbols`

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
                "tradeId": 102043495674,
                "price": 401.74,
                "direction": "buy"
            }
            // more Trade Detail data here
        ]
  }
}
```

### Update Content

Field     | Data Type | Description
--------- | --------- | -----------
id        | integer   | Unique trade id (to be obsoleted)
tradeId|integer| Unique trade id (NEW)
amount    | float     | Last trade volume
price     | float     | Last trade price
ts        | integer   | Last trade time (UNIX epoch time in millisecond)
direction | string    | Aggressive order side (taker's order side) of the trade: 'buy' or 'sell'

### Pull Request

Pull request (of maximum latest 300 trade records) is supported.

```json
{
  "req": "market.btcusdt.trade.detail",
  "id": "id11"
}
```

## Market Details

This topic sends the latest market stats with 24h summary. It updates in snapshot mode, in frequency of no more than 10 times per second.

### Topic

`market.$symbol.detail`

> Subscribe request

```json
{
  "sub": "market.btcusdt.detail",
  "id": "id1"
}
```

### Topic Parameter

Parameter | Data Type | Required | Default Value         | Description                                       | Value Range
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | Trading symbol                     | Refer to `GET /v1/common/symbols`

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

### Update Content

Field     | Data Type | Description
--------- | --------- | -----------
id        | integer   | UNIX epoch timestamp in second as response id
ts        | integer   | UNIX epoch timestamp in millisecond of this tick
amount    | float     | Aggregated trading volume in past 24H (in base currency)
count     | integer   | Number of trades in past 24H
open      | float     | Opening price in past 24H
close     | float     | Last price
low       | float     | Low price in past 24H
high      | float     | High price in past 24H
vol       | float     | Aggregated trading value in past 24H (in quote currency)

### Pull Request

Pull request is supported.

```json
{
  "req": "market.btcusdt.detail",
  "id": "id11"
}
```

# Websocket Asset and Order (to be obsolete)

## Introduction(to be obsolete)

### Websocket URL

**Websocket Asset and Order**

**`wss://api-cloud.huobi.co.kr/ws/v1`**


### Data Format

All return data of websocket APIs are compressed with GZIP so they need to be unzipped.

### Heartbeat and Connection

**After 2019/07/08**

After connected to Huobi's Websocket server, the server will send heartbeat periodically (at 20s interval). The heartbeat message will have an integer in it, e.g.

> {
    "op":"ping",
    "ts":1492420473027
}

When client receives this heartbeat message, it should response with a matching "pong" message which has the same integer in it, e.g.

> {
    "op":"pong",
    "ts":1492420473027
}

<aside class="warning">After the server sent THREE consective heartbeat messages without receiving at least one matching "pong" response from a client, then right before server sends the next "ping" heartbeat, the server will disconnect this client</aside>
**Prior to 2019/07/08**

After connected to Huobi's Websocket server, the server will send heartbeat periodically (at 30s interval). The heartbeat message will have an integer in it, e.g.

> {
    "op":"ping",
    "ts":1492420473027
}

When client receives this heartbeat message, it should response with a matching "pong" message which has the same integer in it, e.g.

> {
    "op":"pong",
    "ts":1492420473027
}

<aside class="warning">After the server sent two consective heartbeat message without receiving at least one matching "pong" response from a client, then right before server sends the next "ping" heartbeat, the server will disconnect this client</aside>
### Subscribe to Topic

To receive data you have to send a "sub" message first.

```json
{
  "op": "operation type, 'sub' for subscription, 'unsub' for unsubscription",
  "topic": "topic to sub",
  "cid": "id generate by client"
}
```

After successfully subscribed, you will receied a response to confirm subscription

```json
{
  "op": "operation type, refer to the operation which triggers this response",
  "cid": "id1",
  "error-code": 0, // 0 for no error
  "topic": "topic to sub if the op is sub",
  "ts": 1489474081631
}
```

Then, you will received message when there is update in this topic

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

### Unsubscribe

To unsubscribe, you need to send below message

```json
{
  "op": "unsub",
  "topic": "accounts",
  "cid": "client generated id"
}
```

And you will receive a message to confirm the unsubscribe

```json
{
  "op": "unsub",
  "topic": "accounts",
  "cid": "id generated by client",
  "err-code": 0,
  "ts": 1489474081631
}
```

### Pull Data

After successfully establishing a connection with the WebSocket API. There are 3 topics which are designed particularly for pull style data query. Those are

* accounts.list
* orders.list
* orders.detail

The details of how to user those three topic will be explain later in this documents.

### Rate Limit

**Rate limt of Subscription for a connection**

The limit is count againt per API key not per connection. When you reached the limit you will receive error with "too many request".

For a given single connection, 
1. maximum of 50 'sub' and 50‘unsub’ in one second. 
2. maximum of 100 sub allowed in total, and every 'unsub' would be deduct from total count of 'sub'. For example, there are 30 sub counts already, if 'unsub' once, then the total count of sub would be 29 for this given connection. When the limit of 100 'sub' reached, no more 'sub' would be allowed. 

**Rate limt of pull style query (req)**

The limit is count againt per API key not per connection. When you reached the limit you will receive error with "too many request".

* accounts.list: once every 2 seconds
* orders.list AND orders.detail: once every 5 seconds

### Authentication

Asset and Order topics require authentication. To authenticate yourself, send below message

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

**The format of Authentication data instruction**

  filed              |type   |  instruction|
  ------------------ |----   |  ----------------------------------------------------- |  ----------------------------------------------------- 
  op                 |string | required; the type of requested operator is auth|
  cid                |string | optional; the ID of Client request|
  AccessKeyId        |string | required; API access key , AccessKey is in APIKEY you applied|
  SignatureMethod    |string | required; the method of sign, user computes signature basing on the protocol of hash ,the api uses HmacSHA256|
  SignatureVersion   |string | required; the version of signature's protocol, the api uses 2|
  Timestamp          |string | required; timestamp, the time is you requests (UTC timezone), this value is to avoid that another people intercepts your request. for example ：2017-05-11T16:22:06 (UTC timezone)|
  Signature          |string |required; signature, the value is computed to make sure that the Authentication is valid and not tampered|

**Notice：**

- Refer to the [Authentication](#authentication) section to generate the signature
- The request method in signature's method is `GET`, WebSocket v1 path is `/ws/v1`
- The data in JSON request doesn't require URL encode

## Subscribe to Account Updates (to be obsoleted)

API Key Permission：Read

This topic publishes all balance updates of the current account.

### Topic

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

### Topic Parameter

Parameter | Data Type | Required | Default Value         | Description                                       | Value Range
--------- | --------- | -------- | -------------         | -----------                                       | -----------
model     | string    | false    | 0                     | Whether to include frozen balance                 | 1 to include frozen balance, 0 to not

<aside class="notice">You may subscribe to this topic with different model to get updates in both models</aside>
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

### Update Content

Field     | Data Type | Description
--------- | --------- | -----------
event     | string    | The event type which triggers this balance updates, including oder.place, order.match, order.refund, order.cancel, order.fee-refund, and other balance transfer event types
account-id| integer   | The account id of this individual balance
currency  | string    | The crypto currency of this balance
type      | string    | The type of this account, including trade, loan, interest
balance   | string    | The balance of this account, include frozen balance if "model" was set to 1 in subscription

Note:<br>

- A maker rebate would be paid in batch mode for multiple trades.<br>

## Subscribe to Order Updates (to be obsoleted)

API Key Permission：Read

This topic publishes all order updates of the current account.

### Topic

`orders.$symbol`

> Subscribe request

```json
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "topic": "orders.htusdt",
}
```

### Topic Parameter

Parameter | Data Type | Required | Default Value         | Description                                       | Value Range
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | Trading symbol                       | All supported trading symbols, e.g. btcusdt, bccbtc.Refer to `GET /v1/common/symbols`.support wildcard "*".

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

### Update Content

Field               | Data Type | Description
---------           | --------- | -----------
seq-id              | integer   | Sequence id
order-id            | integer   | Order id
symbol              | string    | Trading symbol
account-id          | string    | Account id
order-amount        | string    | Order amount (in base currency)
order-price         | string    | Order price
created-at          | int       | Order creation time (UNIX epoch time in millisecond)
order-type          | string    | Order type, possible values: buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker,buy-stop-limit,sell-stop-limit
order-source        | string    | Order source, possible values: sys, web, api, app
order-state         | string    | Order state, possible values: submitted, partial-filled, filled, canceled, partial-canceled
role                | string    | Order role in the trade: taker or maker
price               | string    | Order execution price
filled-amount       | string    | Order execution quantity (in base currency)
filled-cash-amount  | string    | Order execution value (in quote currency)
filled-fees         | string    | Transaction fee paid so far
unfilled-amount     | string    | Remaining order quantity

## Subscribe to Order Updates (to be obsoleted)

API Key Permission：Read

This topic publishes all order updates of the current account. By comparing with above subscription topic “orders.$symbol”, the new topic “orders.$symbol.update” should have lower latency but more sequential updates. API users are encouraged to subscribe to this new topic for getting order update ticks, instead of above topic “orders.$symbol”. (The current subscription topic “orders.$symbol” will be still kept in Websocket API service till further notice.)

### Topic

`orders.$symbol.update`

> Subscribe request

```json
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "topic": "orders.htusdt.update"
}
```

### Topic Parameter

Parameter | Data Type | Required | Default Value         | Description                                       | Value Range
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | Trading symbol                       | Refer to `GET /v1/common/symbols`. wild card (\*) is supported 



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

### Update Content

Field               | Data Type | Description
---------           | --------- | -----------
match-id              | integer   | Match id (While order-state = submitted, canceled, partial-canceled,match-id refers to sequence number; While order-state = filled, partial-filled, match-id refers to last match ID.)
order-id            | integer   | Order id
symbol              | string    | Trading symbol
order-state         | string    | Order state, possible values: submitted, partial-filled, filled, canceled, partial-canceled
role                | string    | Order role in the trade: taker or maker (While order-state = submitted, canceled, partialcanceled, a default value “taker” is given to this field; While order-state = filled, partial-filled, role can be either taker or maker.)
price               | string    | Last price (While order-state = submitted, price refers to order price; While order-state = canceled, partial-canceled, price is zero; While order-state = filled, partial-filled, price reflects the last execution price.)
filled-amount       | string    | Last execution quantity (in base currency)
filled-cash-amount  | string    | Last execution value (in quote currency)
unfilled-amount     | string    | Remaining order quantity (While order-state = submitted, unfilled-amount contains the original order size; While order-state = canceled OR partial-canceled, unfilled-amount contains the remaining order quantity; While order-state = filled, if order-type = buymarket, unfilled-amount could possibly contain a minimal value; if order-type <> buy-market, unfilled-amount is zero; While order-state = partial-filled AND role = taker, unfilled-amount is the remaining order quantity; While order-state = partial-filled AND role = maker, unfilled-amount is remaining order quantity.)
client-order-id | string | Client order ID
order-type | string | order type, including buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker,buy-stop-limit,sell-stop-limit


## Request Account Details(to be obsoleted)

API Key Permission：Read

Query all account data of the current user.

### Query Topic

`accounts.list`

> Query request

```json
{
  "op": "req",
  "cid": "40sG903yz80oDFWr",
  "topic": "accounts.list",
}
```

Parameter  | Data Type  |  Description|
----------| --------| -------------------------------------------------------|
op         |string  | Mandatory parameter; operation type "req"|
cid        |string  | optional parameter; id generate by client|
topic      |string   |mandatory parameter; topic to request "accounts.list"|

### Response

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

Field                |Data Type |    Description|
-------------------- |--------| ------------------------------------|
{ id                   |long    | account ID|
type              |string   |account type|
state           |string     |account status|
list               |string   |account list|
{currency                |string   |sub-account currency|
type           |string     |sub-account type|
balance }}           |string     |sub-account balance|

## Search Past Orders(to be obsoleted)

API Key Permission：Read

Search past and open orders based on searching criteria.

### Query Topic

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

### Request Parameters

Parameter  | Data Type | Required | Default | Description                                   | Value Range
---------  | --------- | -------- | ------- | -----------                                   | ----------
account-id | int       | true     | NA      | Account id                        | NA
symbol     | string    | true     | NA      | Trading symbol                | Refer to `GET /v1/common/symbols`
types      | string    | false    | NA      | Order type   | buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit
states     | string    | true    | NA      | Order state  | submitted, partial-filled, partial-canceled, filled, canceled, created
start-date | string    | false    | -61d    | Start date, in format yyyy-mm-dd      | NA
end-date   | string    | false    | today   | End date, in format yyyy-mm-dd        | NA
from       | string    | false    | NA      | Order id to begin with                 | NA
direct     | string    | false    | next    | Searching direction when 'from' is given          | next, prev
size       | string       | false    | 100     | Number of items in each return               | [1, 100]

### Response

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
Field                |Data Type |    Description|
-------------------- |--------| ------------------------------------|
id                   |long    | order ID|
symbol               |string   |trading symbol|
account-id           |long     |account ID|
amount               |string   |order size|
price                |string   |order price|
created-at           |long     |order creation time|
type                 |string   |order type, possible values: buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit|
filled-amount        |string   |filled amount|
filled-cash-amount   |string   |filled value|
filled-fees          |string   |transaction fee|
finished-at          |string   |trade time|
source               |string   |order source, possible values: sys, web, api, app|
state                |string   |order state, possible values: submitted, partial-filled, filled, canceled, partial-canceled|
cancel-at            |long     |order cancellation time|
stop-price|string|trigger price of stop limit order|
operator|string|opration character of stop price|


## Query Order by Order ID(to be obsoleted)

API Key Permission：Read

Get order details by a given order ID.

### Query Topic

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

### Request Parameters

Parameter  | Required | Data Type | Description |      Default              | Value Range
---------  | --------- | -------- | ------- | -----------                                   | ----------
op         |true       |string   |operation type "req"    |||                                
cid        |true       |string   |id generate by client        |||                                
topic      |false      |string   |topic to request "orders.detail"  |||          
order-id   |true       |string   |order ID    ||| 

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
Field                |Data Type |    Description|
-------------------- |--------| ------------------------------------|
id                   |long    | order ID|
symbol               |string   |trading symbol|
account-id           |long     |account ID|
amount               |string   |order size|
price                |string   |order price|
created-at           |long     |order creation time|
type                 |string   |order type, possible values: buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit|
filled-amount        |string   |filled amount|
filled-cash-amount   |string   |filled value|
filled-fees          |string   |transaction fee|
finished-at          |string   |trade time|
source               |string   |order source, possible values: sys, web, api, app|
state                |string   |order state, possible values: submitted, partial-filled, filled, canceled, partial-canceled|
cancel-at            |long     |order cancellation time|
stop-price|string|trigger price of stop limit order|
operator|string|opration character of stop price|

# Websocket Asset and Order (v2)

## Genral

### Access URL

**Websocket Asset and Order (v2)**

**`wss://api-cloud.huobi.co.kr/ws/v2`**  

Note: 
By comparing to api-cloud.huobi.co.kr, the network latency to api-aws.huobi.co.kr is lower, for those client's servers locating at AWS.

### Message Compression

Different with v1, the return data of websocket v2 are not compressed.

### Heartbeat

Once the Websocket connection is established, Huobi server will periodically send "ping" message at 20s interval, with an integer inside.

```json
{
  "action": "ping",
  "data": {
    "ts": 1575537778295
  }
}
```

Once the Websocket connection is established, Huobi server will periodically send "ping" message at 20s interval, with an integer inside.

```json
{
    "action": "ping",
    "data": {
          "ts": 1575537778295 // the same integer from "ping" message
    }
}
```

Once client's server receives "ping", it should respond "pong" message back with the same integer.

### Valid Values of `action`

| Valid Values | Description                                 |
| ------------ | ------------------------------------------- |
| sub          | Subscribe                                   |
| req          | Request                                     |
| ping,pong    | Heartbeat                                   |
| push         | Push (from Huobi server to client's server) |

### Rate Limit

There are multiple limitations for this version:

- The limitation of single connection for **valid** request (including req, sub, unsub, not including ping/pong or other invalid request) is **50 per second**. It will return "too many request" when the limit is exceeded.
- The limitation of single API Key is **10**. It will return "too many connection" when the limit is exceeded.
- The limitation of single IP is **100 per second**. It will return "too many request" when the limitation is exceeded.

### Authentication

Authentication request:

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
        "signature": "safsfdsjfljljdfsjfsjfsdfhsdkjfhklhsdlkfjhlksdfh"
    }
}

```

The response of success

```json
{
  "action": "req",
  "code": 200,
  "ch": "auth",
  "data": {}
}
```

Authentication request field list

| Field            | Mandatory | Data Type | Description                                         |
| ---------------- | --------- | --------- | --------------------------------------------------- |
| action           | true      | string    | Action type, valid value: "req"                     |
| ch               | true      | string    | Channel, valid value: "auth"                        |
| authType         | true      | string    | Authentication type, valid value: "api"             |
| accessKey        | true      | string    | Access key                                          |
| signatureMethod  | true      | string    | Signature method, valid value: "HmacSHA256"         |
| signatureVersion | true      | string    | Signature version, valid value: "2.1"               |
| timestamp        | true      | string    | Timestamp in UTC in format like 2017-05-11T16:22:06 |
| signature        | true      | string    | Signature                                           |

### Generating Signature 

The signature generation method v2.1 is similar with v2.0, with only following differences:

1. The request method should be "GET", to URL "/ws/v2".
2. The involved field names in v2 signature generation are: accessKey，signatureMethod，signatureVersion，timestamp
3. The valid value of signatureVersion is 2.1.

Please refer to detailed signature generation steps from: [https://alphaex-api.github.io/openapi/spot/v1/en/#authentication]

The final string involved in signature generation should be like below:

```
GET\n
api-cloud.huobi.co.kr\n
/ws/v2\n
accessKey=0664b695-rfhfg2mkl3-abbf6c5d-49810&signatureMethod=HmacSHA256&signatureVersion=2.1&timestamp=2019-12-05T11%3A53%3A03
```

The final string involved in signature generation should be like below:

Note: The data in JSON request doesn't require URL encode

### Subscribe a Topic to Continuously Receive Updates

> Sub request:

```json
{
	"action": "sub",
	"ch": "accounts.update"
}
```

Once the Websocket connection is established, Websocket client could send following request to subscribe a topic:

> Sub respose:

```json
{
	"action": "sub",
	"code": 200,
	"ch": "accounts.update#0",
	"data": {}
}
```

Upon success, Websocket client should receive a response below:

Below is the return code, return message and the description returend from Asset and Order WebSocket

| Return Code | Return Message           | Description                                       |
| ----------- | ------------------------ | ------------------------------------------------- |
| 200         | Success                  | Successful                                        |
| 100         | time out close           | The connection is timeout and closed              |
| 400         | Bad Request              | The request is invalid                            |
| 404         | Not Found                | The service is not found                          |
| 429         | Too Many Requests        | Connection number exceed limit                    |
| 500         | system error             | System internal error                             |
| 2000        | invalid.ip               | The IP is invalid                                 |
| 2001        | nvalid.json              | The JSON request is invalid                       |
| 2001        | invalid.action           | Parameter action is invalid                       |
| 2001        | invalid.symbol           | Parameter symbol is invalid                       |
| 2001        | invalid.ch               | Parameter channel is invalid                      |
| 2001        | missing.param.auth       | Parameter auth is missing                         |
| 2002        | invalid.auth.state       | Authentication state is invalid                   |
| 2002        | auth.fail                | Authentication failed                             |
| 2003        | query.account.list.error | Account query error                               |
| 4000        | too.many.request         | Request exceed limit                              |
| 4000        | too.many.connection      | Connection number exceed limit for single API Key |

## Subscribe Order Updates

API Key Permission: Read

An order update can be triggered by any of following:<br>

- Conditional order triggering failure (eventType=trigger)<br>
- Conditional order cancellation before trigger (eventType=deletion)<br>
- Order creation (eventType=creation)<br>
- Order matching (eventType=trade)<br>
- Order cancellation (eventType=cancellation)<br>

The field list in order update message can be various per event type, developers can design the data structure in either of two ways:<br>

- Define a data structure including fields for all event types, allowing a few of them null<br>
- Define different data structure for each event type to include specific fields, inheriting from a common data structure which has common fields

### Topic

` orders#${symbol}`

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

### Subscription Field

| Field  | Data Type | Description                            |
| ------ | --------- | -------------------------------------- |
| symbol | string    | Trading symbol (wildcard * is allowed) |

### Update Content

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

After conditional order triggering failure –

| Field         | Data Type | Description                                                  |
| ------------- | --------- | ------------------------------------------------------------ |
| eventType     | string    | Event type, valid value: trigger (only applicable for conditional order) |
| symbol        | string    | Trading symbol                                               |
| clientOrderId | string    | Client order ID                                              |
| orderSide     | string    | Order side, valid value: buy, sell                           |
| orderStatus   | string    | Order status, valid value: rejected                          |
| errCode       | int       | Error code for triggering failure                            |
| errMessage    | string    | Error message for triggering failure                         |
| lastActTime   | long      | Order trigger time                                           |

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

After conditional order being cancelled before triggering –

| Field         | Data Type | Description                                                  |
| ------------- | --------- | ------------------------------------------------------------ |
| eventType     | string    | Event type, valid value: deletion (only applicable for conditional order) |
| symbol        | string    | Trading symbol                                               |
| clientOrderId | string    | Client order ID                                              |
| orderSide     | string    | Order side, valid value: buy, sell                           |
| orderStatus   | string    | Order status, valid value: canceled                          |
| lastActTime   | long      | Order trigger time                                           |

```json
{
	"action":"push",
	"ch":"orders#btcusdt",
	"data":
	{
		"orderSize":"2.000000000000000000",
		"orderCreateTime":1583853365586,
		"accountld":992701,
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

After order is submitted –

| Field           | Data Type | Description                                                  |
| --------------- | --------- | ------------------------------------------------------------ |
| eventType       | string    | Event type, valid value: creation                            |
| symbol          | string    | Trading symbol                                               |
| accountId       | long      | account ID                                                   |
| orderId         | long      | Order ID                                                     |
| clientOrderId   | string    | Client order ID (if any)                                     |
| orderPrice      | string    | Order price                                                  |
| orderSize       | string    | Order size (inapplicable for market buy order)               |
| orderValue      | string    | Order value (only applicable for market buy order)           |
| type            | string    | Order type, valid value: buy-market, sell-market, buy-limit, sell-limit, buy-limit-maker, sell-limit-maker, buy-ioc, sell-ioc |
| orderStatus     | string    | Order status, valid value: submitted                         |
| orderCreateTime | long      | Order creation time                                          |

Note:<br>

- If a stop limit order is created but not yet triggered, the topic won’t send an update.<br>
- The topic will send creation update for taker's order before it being filled.<br>
- Stop limit order's type is no longer as “buy-stop-limit” or “sell-stop-limit”, but changing to “buy-limit” or “sell-limit”.<br>

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

After order matching –

| Field         | Data Type | Description                                                  |
| ------------- | --------- | ------------------------------------------------------------ |
| eventType     | string    | Event type, valid value: trade                               |
| symbol        | string    | Trading symbol                                               |
| tradePrice    | string    | Trade price                                                  |
| tradeVolume   | string    | Trade volume                                                 |
| orderId       | long      | Order ID                                                     |
| type          | string    | Order type, valid value: buy-market, sell-market, buy-limit, sell-limit, buy-limit-maker, sell-limit-maker, buy-ioc, sell-ioc |
| clientOrderId | string    | Client order ID (if any)                                     |
| tradeId       | long      | Trade ID                                                     |
| tradeTime     | long      | Trade time                                                   |
| aggressor     | bool      | Aggressor or not, valid value: true (taker), false (maker)   |
| orderStatus   | string    | Order status, valid value: partial-filled, filled            |
| remainAmt     | string    | Remaining amount (for buy-market order it's remaining value) |

Note:<br>

- Stop limit order's type is no longer as “buy-stop-limit” or “sell-stop-limit”, but changing to “buy-limit” or “sell-limit”.<br>
- If a taker’s order matching with multiple orders at opposite side simultaneously, the multiple trades will be disseminated over separately instead of merging into one trade.<br>

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

After order cancellation –

| Field         | Data Type | Description                                                  |
| ------------- | --------- | ------------------------------------------------------------ |
| eventType     | string    | Event type, valid value: cancellation                        |
| symbol        | string    | Trading symbol                                               |
| orderId       | long      | Order ID                                                     |
| type          | string    | Order type, valid value: buy-market, sell-market, buy-limit, sell-limit, buy-limit-maker, sell-limit-maker, buy-ioc, sell-ioc |
| clientOrderId | string    | Client order ID (if any)                                     |
| orderStatus   | string    | Order status, valid value: partial-canceled, canceled        |
| remainAmt     | string    | Remaining amount	(for buy-market order it's remaining value) |
| lastActTime   | long      | Last activity time                                           |

Note:<br>

- Stop limit order's type is no longer as “buy-stop-limit” or “sell-stop-limit”, but changing to “buy-limit” or “sell-limit”.<br>

## Subscribe Trade Details & Order Cancellation post Clearing

API Key Permission: Read

Only update when order transaction or cancellation. Order transaction update is in tick by tick mode, which means, if a taker’s order matches with multiple maker’s orders, the simultaneous multiple trades will be disseminated one by one. But, the update sequence of the multiple trades, may not be exactly same with the sequence of the transactions made. Also, if an order is auto cancelled immediately just after its partial fills, for example a typical IOC order, this channel would be possibly disseminate the cancellation update first prior to the trade. <br>

If user willing to receive order updates in exact same sequence with the original happening, it is recommended to subscribe order update channel orders#${symbol}.<br>

### Topic

`trade.clearing#${symbol}#${mode}`

### Subscription Field

| Field  | Data Type | Description                                                  |
| ------ | --------- | ------------------------------------------------------------ |
| symbol | string    | Trading symbol (wildcard * is allowed)                       |
| mode   | int       | Subscription mode (0 – subscribe only trade event; 1 – subscribe both trade and cancellation events; default value: 0) |

Note:<br>
About optional field ‘mode’: If not filled, or filled with 0, it implicates to subscribe trade event only. If filled with 1, it implicates to subscribe both trade and cancellation events.<br>

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

### Update Contents (after order matching)

| Field           | Data Type | Description                                                  |
| --------------- | --------- | ------------------------------------------------------------ |
| eventType       | string    | Event type (trade)                                           |
| symbol          | string    | Trading symbol                                               |
| orderId         | long      | Order ID                                                     |
| tradePrice      | string    | Trade price                                                  |
| tradeVolume     | string    | Trade volume                                                 |
| orderSide       | string    | Order side, valid value: buy, sell                           |
| orderType       | string    | Order type, valid value: buy-market, sell-market,buy-limit,sell-limit,buy-ioc,sell-ioc,buy-limit-maker,sell-limit-maker,buy-stop-limit,sell-stop-limit |
| aggressor       | bool      | Aggressor or not, valid value: true, false                   |
| tradeId         | long      | Trade ID                                                     |
| tradeTime       | long      | Trade time, unix time in millisecond                         |
| transactFee     | string    | Transaction fee (positive value) or Transaction rebate (negative value) |
| feeCurrency     | string    | Currency of transaction fee or transaction fee rebate (transaction fee of buy order is based on base currency, transaction fee of sell order is based on quote currency; transaction fee rebate of buy order is based on quote currency, transaction fee rebate of sell order is based on base currency) |
| feeDeduct       | string    | Transaction fee deduction                                    |
| feeDeductType   | string    | Transaction fee deduction type, valid value: ht, point       |
| accountId       | long      | Account ID                                                   |
| source          | string    | Order source                                                 |
| orderPrice      | string    | Order price (invalid for market order)                       |
| orderSize       | string    | Order size (invalid for market buy order)                    |
| orderValue      | string    | Order value (only valid for market buy order)                |
| clientOrderId   | string    | Client order ID                                              |
| stopPrice       | string    | Stop price (only valid for stop limit order)                 |
| operator        | string    | Operation character (only valid for stop limit order)        |
| orderCreateTime | long      | Order creation time                                          |
| orderStatus     | string    | Order status, valid value: filled, partial-filled            |

Notes:<br>

- The calculated maker rebate value inside ‘transactFee’ would not be paid immediately.<br>

### Update Contents (after order cancellation)

| Field           | Data Type | Description                                                  |
| --------------- | --------- | ------------------------------------------------------------ |
| eventType       | string    | Event type (cancellation)                                    |
| symbol          | string    | Trading symbol                                               |
| orderId         | long      | Order ID                                                     |
| orderSide       | string    | Order side, valid value: buy, sell                           |
| orderType       | string    | Order type, valid value: buy-market, sell-market,buy-limit,sell-limit,buy-ioc,sell-ioc,buy-limit-maker,sell-limit-maker,buy-stop-limit,sell-stop-limit |
| accountId       | long      | Account ID                                                   |
| source          | string    | Order source                                                 |
| orderPrice      | string    | Order price (invalid for market order)                       |
| orderSize       | string    | Order size (invalid for market buy order)                    |
| orderValue      | string    | Order value (only valid for market buy order)                |
| clientOrderId   | string    | Client order ID                                              |
| stopPrice       | string    | Stop price (only valid for stop limit order)                 |
| operator        | string    | Operation character (only valid for stop limit order)        |
| orderCreateTime | long      | Order creation time                                          |
| remainAmt       | string    | Remaining order amount (if market buy order, it implicates remaining order value) |
| orderStatus     | string    | Order status, valid value: canceled, partial-canceled        |

## Subscribe Account Change

API Key Permission: Read

The topic updates account change details.

### Topic

`accounts.update#${mode}`

Upon subscription field value specified, the update can be triggered by either of following events:

1、Whenever account balance is changed.

2、Whenever account balance or available balance is changed. (Update separately.)

### Subscription Field

| Field | Data Type | Description                                       |
| ----- | --------- | ------------------------------------------------- |
| mode  | integer   | Trigger mode, valid value: 0, 1, default value: 0 |

Samples  
1、Not specifying "mode":  
accounts.update  
Only update when account balance changed;  
2、Specify "mode" as 0:  
accounts.update#0  
Only update when account balance changed;  
3、Specify "mode" as 1:  
accounts.update#1  
Update when either account balance changed or available balance changed.  

Note:
The topic disseminates the current static value of individual accounts first, at the beginning of subscription, followed by account change updates. While disseminating the current static value of individual accounts, inside the message, field value of "changeType" and "changeTime" is null.

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

### Update Contents

| Field       | Data Type | Description                                                  |
| ----------- | --------- | ------------------------------------------------------------ |
| currency    | string    | Currency                                                     |
| accountId   | long      | Account ID                                                   |
| balance     | string    | Account balance (only exists when account balance changed)   |
| available   | string    | Available balance (only exists when available balance changed) |
| changeType  | string    | Change type, valid value: order-place,order-match,order-refund,order-cancel,order-fee-refund,other,deposit,withdraw |
| accountType | string    | account type, valid value: trade, frozen, loan, interest     |
| changeTime  | long      | Change time, unix time in millisecond                        |

Note:<br>

- A maker rebate would be paid in batch mode for multiple trades.<br>
