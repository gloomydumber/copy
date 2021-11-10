---
layout: post
title: "Crypto Exchange APIs"
date: 2021-11-10 15:40:00
categories: PROJECT
permalink: /archivers/cryptoAPIs
nocomments: false
use_math: true
---

## Crypto Exchange APIs

Crypto API lists

### Upbit

[API Limit](https://docs.upbit.com/docs/user-request-guide#exchange-api-%EC%9E%94%EC%97%AC-%EC%9A%94%EC%B2%AD-%EC%88%98-%ED%99%95%EC%9D%B8-%EB%B0%A9%EB%B2%95){: target="\_blank"} https://docs.upbit.com/docs/user-request-guide#exchange-api-%EC%9E%94%EC%97%AC-%EC%9A%94%EC%B2%AD-%EC%88%98-%ED%99%95%EC%9D%B8-%EB%B0%A9%EB%B2%95

Exchange API - 주문 초당 8회, 분당 200회 / 주문 외 초당 30회, 분당 900회 Quatation API - 분당 600회, 초당 10회 (종목, 캔들, 체결, 티커, 호가별)

(Remaining-Req Header를 통해 알 수 있음)

[API 아님, 입출금현황](https://www.upbit.com/service_center/wallet_status){: target="\_blank"} https://www.upbit.com/service_center/wallet_status

[전체 MARKET, 한글/영어 이름](https://api.upbit.com/v1/market/all?isDetails=false){: target="\_blank"} https://api.upbit.com/v1/market/all?isDetails=false

[Market Depth 예시 15단 호가(현재 가격(Open Price)은 알 수 없음것으로 보임)](https://api.upbit.com/v1/orderbook?markets=KRW-XRP){: target="\_blank"} https://api.upbit.com/v1/orderbook?markets=KRW-XRP

[API 이용 예제](https://note.heyo.me/%EC%97%85%EB%B9%84%ED%8A%B8api-%ED%8A%B8%EB%A0%88%EC%9D%B4%EB%94%A9-1-%EC%A4%80%EB%B9%84-%EB%B0%8F-%ED%85%8C%EC%8A%A4%ED%8A%B8/){: target="\_blank"}

### Bithumb

https://api.bithumb.com/public/ticker/ALL_KRW

### Binance

[API Limit](https://binance-docs.github.io/apidocs/spot/en/#limits){: target="\_blank"} https://binance-docs.github.io/apidocs/spot/en/#limits

https://api.binance.com/api/v3/exchangeInfo 를 통해 현재 Limit 확인 가능

[API Limit 2 한글](https://www.binance.com/kr/support/faq/360004492232){: target="\_blank"} https://www.binance.com/kr/support/faq/360004492232

[전체 MARKET, 마진가능여부](https://api.binance.com/api/v3/exchangeInfo){: target="\_blank"} https://api.binance.com/api/v3/exchangeInfo

[POST요청, 유형별입출금가능상태, freeze, trading가능여부](https://api.binance.com/sapi/v1/capital/config/getall){: target="\_blank"} https://api.binance.com/sapi/v1/capital/config/getall

[API 아님, New Listing 공지](https://www.binance.com/en/support/announcement/c-48?navId=48){: target="\_blank"} https://www.binance.com/en/support/announcement/c-48?navId=48

[Market Depth](https://binance-docs.github.io/apidocs/spot/en/#order-book){: target="\_blank"} https://binance-docs.github.io/apidocs/spot/en/#order-book

[Market Depth 예시](https://www.binance.com/api/v3/depth?symbol=BTCUSDT){: target="\_blank"} https://www.binance.com/api/v3/depth?symbol=BTCUSDT

[Weight이 무엇인가](https://dev.binance.vision/t/what-are-the-ip-weights/280/2){: target="\_blank"} https://dev.binance.vision/t/what-are-the-ip-weights/280/2

말그대로 5000이면 ask 5000개 호가 bid 5000개 호가 총 1만개 보여줌.

[BINANCE API 에러코드](https://systemtraders.tistory.com/1001){: target="\_blank"} https://systemtraders.tistory.com/1001

[BINANCE MARGIN LEVEL](https://www.binance.com/en/support/articles/360030493931){: target="\_blank"} https://www.binance.com/en/support/articles/360030493931

MARGIN LV. > 2 여야 Transfer 가능

가격조회 https://github.com/zoeyg/binance/issues/48
(symbol query를 단수로 받고 ,등의 특문 받지 않으므로 여러 가격 조회 불가능)

### Huobi

[API Limit](https://huobiapi.github.io/docs/spot/v1/en/#new-version-rate-limit-rule){: target="\_blank"} https://huobiapi.github.io/docs/spot/v1/en/#new-version-rate-limit-rule

1초에 주문 및 조회 (ApiKey, IP당) 10회 제한으로 보임

[코인 및 코인 유형별 입출금 상태](https://api.huobi.pro/v2/reference/currencies){: target="\_blank"} https://api.huobi.pro/v2/reference/currencies

[전체 MARKET(quote = USDT면 USDT마켓)](https://api.huobi.pro/v1/common/symbols){: target="\_blank"} https://api.huobi.pro/v1/common/symbols

### OKEX

[전체 MARKET 및 가격, best bid/ask](https://aws.okex.com/api/spot/v3/instruments/ticker){: target="\_blank"} https://aws.okex.com/api/spot/v3/instruments/ticker

### FTX

### Gate.io

### MXC

### KuCoin

### US Dollar / KRW

[간헐적 CORS 주의, 달러말고도 추가가능](https://quotation-api-cdn.dunamu.com/v1/forex/recent?codes=FRX.KRWUSD){: target="\_blank"} https://quotation-api-cdn.dunamu.com/v1/forex/recent?codes=FRX.KRWUSD

## Crypto Arbitrage Check Link list

[USDTKRW Calculator](https://gloomydumber.github.io/USDTKRWCalculator/){: target="\_blank"} https://gloomydumber.github.io/USDTKRWCalculator/

### Upbit

[websocket ex1](https://niceman.tistory.com/109){: target="\_blank"} https://niceman.tistory.com/109

[websocket ex2](https://coinbugs.net/board/3432){: target="\_blank"} https://coinbugs.net/board/3432

[websocket ex3](http://webschool.kr/page.php?bbs=dev_javascript&bbs_idx=32){: target="\_blank"} http://webschool.kr/page.php?bbs=dev_javascript&bbs_idx=32

### Bithumb

### Binance

### Huobi

[주문내역, AVG Price 참고](https://www.huobi.com/en-us/transac/?tab=1&type=0){: target="\_blank"} https://www.huobi.com/en-us/transac/?tab=1&type=0

### OKEX

### FTX

### Gate.io

### MXC

### KuCoin

## Coingekco

https://api.coingecko.com/api/v3/coins/list

http://api.coingecko.com/api/v3/coins/{id}

코인 ID 추출 후, 상장리스트 바로 파악
