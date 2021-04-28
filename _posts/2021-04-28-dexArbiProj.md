---
layout: post
title: "DEX Arbitrage Project"
date: 2021-04-28 15:30:30
categories: PROJECT
permalink: /archivers/dexArbiProj
nocomments: false
use_math: true
---

# Requirements Specification

Crypto Currency의 CEX(Centralized Exchange) 및 DEX(Decentralized Exchange)간의 시세 차이를 이용한 Arbitrage Trading Program 및 그 보조 Program 개발

## Introduction

Crypto Currency의 최초 개발 철학은 불투명성, 검열성, 비효율성, 권위주의적 특징 등을 지닌 중앙화된 금융제도의 안티테제(Antithese)로서 '탈중앙화된 금융(Decentralized Finance)'이라 할 수 있다.

그러나, 특히 초기 Crypto Currency는 이러한 철학과 달리 대부분의 거래가 중앙화 된 거래소(CEX)에서 이루어 졌으며, 이는 최근들어 UniSwap, PancakeSwap 등의 파격적인 거래량 증가 와 같은 성장에도 불구하고 Crypto Currency가 CEX 위주의 trade volume을 지녔다는 사실은 아직 유효하다 할 수 있다.

2021년 4월 27일 기준 CoinGecko 통계에 따르면, 전체 Crypto 시장 거래량 최상위 거래소인 Binance의 24시간 거래량은 $35,722,509,284 이지만, CoinGecko에 등록된 모든 DEX의 24시간 거래량을 합한 값이 $9,812,715,640이다. (Binance 거래소만의 거래량이 전체 DEX 시장 거래량의 3.64배이다.) 물론 DEX의 성장세가 매우 가파른 것은 사실이나, Crypto Currency가 금융의 탈중앙화를 노리고 등장한 것을 고려하면 아직 거래 그 자체나 이용 방법에 있어서는 아직 상당히 거래의 집중화, 중앙화 된 면이 있고 탈중앙화 측면에서 미흡하다 할 수 있다.

한편, UniSwap으로 대표되는 DEX는 전통적인 Fiat Money(법정화폐)를 통한 거래가 아닌, token과 token으로 이루어진 쌍으로 하여금 pair를 만든다. 또, 이러한 pair에서 하나의 token을 다른 token으로 교환하는 식으로 거래가 이루어 진다. 이러한 거래를 위한 유동성 공급은 정부기관이나 은행같은 중앙화된 기관이 아니라 각 개인 누구나 유동성 공급자(Liquidity Provider, 이하 LP)가 될 수 있다. 이는 UniSwap의 algorithm을 통해 LP들에게 거래 수수료 등과 같은 유인을 제공함으로서 가능한 것이다.
또, 어느 한 token의 가격은 그 token이 속한 pool에서의 다른 token과의 비율(ratio)에 의해 결정된다. 이에 필연적으로 현실 세계 또는 CEX에서 Fiat Money를 통해 거래되는 선형적(linear) 가격 선정 방식과는 괴리가 발생한다. 그러한 괴리는 UniSwap의 백서(White Paper)에 따르면 자연스레 Arbitrageur(재정거래자)에 의해 이루어지는 것으로 여긴다. (경제학원론의 일물일가의 원칙을 생각해보라)

Crypto Currency Market은 상당한 Volatility(변동성)을 지니고 있는 시장이다. 이러한 변동성은 해당 token의 거래량이 많은 거래소에서 먼저 발생하고, 그 이후 trading volume이 적은 거래소들이 선행 거래소의 가격을 쫓는 경향이 있다. 이는 CEX와 DEX의 가격에서도 나타난다. 변동성이 강한 Crypto Currency Market의 특성 상, 예외는 존재하지만 대개 거래량이 많은 CEX에서의 강한 변동성 발생 이후 거래량이 상대적으로 적은 DEX가 가격을 쫓는 경향이 있다.

이에 우리는 변동성이 심한 Crypto Currency Market에서 CEX와 DEX의 순간적인 가격 괴리율을 포착하여 Arbitrage 거래를 수행하거나 그 거래를 돕는 Program을 개발하고자 한다.

## methodology

Fiat Money(e.g. KRW, USD, JPY..) 또는 그에 준하는 token(e.g. USDT, USDC..)을 margin(증거금)으로 하여금 Crypto Currency를 빌리거나(Borrowing), 선물(future)과 같은 leverage 거래 기능을 제공하는 거래소는 현재 국내에 존재하지 않는다. 그러한 거래 및 상품을 Derivative(파생상품)이라 한다. 한국의 거래소 코인원은 4배 고정의 leverage Margin 거래 기능을 제공하였으나, 2017년 당시 불법 도박장 운영 혐의로 경찰의 수사를 받게 되고 해당 Margin 거래 기능을 중단하였다. 이후 18년 6월 검찰로 사건이 송치된 뒤 다시 2년 10개월 만에 무혐의 처분을 받았다. 이는 법적, 제도적 가이드 라인 없는 죄형법정주의에 어긋난 수사의 결과로 밖에 보이지 않는다. 이에 대한 여파인지 우리나라 거래소들은 2021년 4월 현재, 파생 금융 기능을 제공하지 않는다.

반면, 이러한 파생 금융 기능을 제공하는 거래소는 홍콩 등에 소재하고 있는 해외의 Binance, Huobi, OKEX 등의 거래소가 있다. 이러한 거래소들의 거래량은 세계적으로 상위권에 랭크하고 있으며 이러한 상품으로 부터 막대한 수수료 수익을 얻고 있다.
이러한 거래소들은 각 거래소 마다 어느 정도의 차이는 있지만 대개 단순한 token 대출 기능에서부터 공매수(long)/공매도(short)와 같은 margin 거래, 고(高) leverage의 선물 거래와 같은 다양한 Derivatives를 제공한다.

이에 우리는 Derivative Market이 존재하는 거래소와 그렇지 않은 거래소를 구분하여 Arbitrage 거래를 실현하고자 한다.
Derivative Market이 존재하는 경우에는 Derivative Trading을 통해 약간의 수수료로 Crypto Currency의 전송 시간으로 인한 가격 변동 리스크를 상쇄(Hedging)할 수 있다.
Derivative Market이 존재하지 않는 CEX에서 거래할 때에도 Derivative Market이 존재하는 CEX를 활용할 수 있도록 파생적 접근을 해볼 수 있다.
이에 대한 자세한 방법론을 다음 단락에서 구분하여 예를 들어 설명한다.

### CEX without Derivative Market <=> DEX

파생상품이 없는 거래소의 예로는 한국의 Upbit를 기준으로 하고, DEX의 예로는 UniSwap으로 한다.

우선 Arbitrage Trading(재정거래)의 정의는 다음과 같다.
Arbitrage Trading(재정거래)란 두 개의 시장(우리 예에서의 Upbit와 UniSwap)에서 가치(가격)가 다른 물건(우리 예에서의 ETH 및 ERC20 Tokens)이 있을 때, 한 곳에서 사고 다른 곳에서 파는 방식으로 무위험 이익을 내는 것이다. 핵심은 일물일가의 법칙이 깨진 틈을 이용하여 무위험으로 초과수익을 내는 것이다. 이 점이 일정 리스크를 부담하는 투기, 혹은 투자와의 차이점이다.

앞선 정의와 같이, 서로 다른 두 개의 시장의 '가격'에 차이가 있을 때 Arbitrage Trading이 이루어 질 수 있다.
대개 실물경제에서는 물건의 운송에 드는 비용, 정보 획득에 들어가는 비용 등으로 인해 두 시장에 가격 차이가 발생한다.
이러한 실물경제와는 달리, CEX와 DEX간의 가격 차이 괴리율은 독특한 DEX의 가격 결정 algorithm에 기인하는 바가 크다.
다시 언급하자면, DEX의 거래 pair인 pool에서의 한 token의 가격은 다른 나머지 token과의 상대적인 비율(ratio)에 의해 결정된다. (x * y = k formula에 의함)
즉, pool에 존재하는 두 토큰의 amount(수량)의 비율이 바로 가격이다.
또, 앞서 언급하였듯이 시장의 변동성은 대개 CEX로 부터 먼저 기인한다. 따라서 어떤 한 token의 Upbit에서 가격 변동이 발생한다면,
그 이후에 후행적으로 Arbitrageur들이 해당 token이 존재하는 UniSwap pair에서 거래를 통해 가격을 일정수준으로 추종한다.

한편, Arbitrage Trading은 상승, 하락과 같은 가격의 방향성과는 상관없이 가격의 차이 및 변동성이 발생하였을 때 이루어진다.
따라서 Upbit에서의 상대적인 token 가격 급등 또는 급락이나 UniSwap에서의 상대적인 token 가격 급등 또는 급락 또한 모두 Arbitrage Trading 기회가 발생함을 기억해야한다.

또 한편, DEX는 주로 ETH(UniSwap에서는 WETH로 표기된다) 및 ERC20 token들의 거래 쌍을 구성한다.
(Binance에서 지원하는 BSC기반 체인으로 구성된 PancakeSwap 등의 DEX 또한 존재한다. 하지만 우리는 ETH / ERC20 pair로 구성된 기존 DEX를 주로 다루도록 한다.)
이에, 우리는 CEX(Upbit) 또는 DEX(UniSwap)에서, ETH에 비해 ERC20이 고평가 혹은 저평가 되었는지를 확인하여 그에 맞는 Arbitrage Trading을 수행한다.
이를 위해, 우리는 DEX에서의 token가격을 CEX에서의 Fiat Money로 환산된 가격으로 표기해야 가격차이인 premium을 계산 및 확인할 수 있다.

이를 아래와 같은 CASE로 구분을 통해 Arbitrage Trading을 구체화 한다.

#### A. Upbit에서의 token 또는 ETH의 가치가 UniSwap에서 보다 고평가 된 경우

##### 1) UniSwap에서 ETH => ERC20 Swap의 경우 (= Upbit에서 고평가 된 token이 ERC20 인 경우)

이 경우, Upbit의 특정 ERC20 token이 순간적인 가격 급등 등으로 인해 특정 UniSwap pair에 비해 높은 가격을 지니고 있는 경우다.
이 때에는 Upbit의 ETH를 구매하여 MetaMask와 같은 개인 이더리움 지갑으로 전송하고,
전송한 ETH를 UniSwap을 통해 해당 ERC20 token으로 Swap한다.
Swap된 ERC20은 Upbit로 전송하여 매도한다.

##### 2) UniSwap에서 ERC20 => ETH Swap의 경우 (= Upbit에서 고평가 된 token이 ETH 인 경우)

이 경우, Upbit의 ETH가 순간적인 가격 급등 등으로 인해 특정 UniSwap pair에 비해 높은 가격을 지니고 있는 경우다.
이 때에는 Upbit의 ERC20을 구매하여 MetaMask와 같은 개인 이더리움 지갑으로 전송하고,
전송한 ERC20을 UniSwap을 통해 ETH로 Swap한다.
Swap된 ETH는 Upbit로 전송하여 매도한다.

#### B. UniSwap에서의 token 또는 ETH의 가치가 Upbit에서 보다 고평가 된 경우
##### 1) UniSwap에서 ETH => ERC20 Swap의 경우 (= UniSwap에서 고평가 된 token이 ETH 인 경우)

##### 2) UniSwap에서 ERC20 => ETH Swap의 경우 (= UniSwap에서 고평가 된 token이 ERC20 인 경우)

B의 경우에는, 단순히 A의 경우의 반대의 경우라고 이해할 수 있다. UniSwap에서의 특정 ERC20 Token이 Upbit보다 고평가 된 경우를 생각해보자.

Upbit에서 1 ETH 당 가격이 100만원이고, ERC20 token A의 개당 가격이 20만원이라고 하면, Upbit에서는 아래의 식이 만족한다.

> 1 ETH = 5 ERC20 token = 1000000 KRW

그런데 가령, 해당 ERC20 token의 UniSwap pair에서의 교환비(ratio)가 다음과 같다고 하자.

> from 1 ETH to 4.5 ERC20 token

이는 1 ETH를 해당 ERC20 token으로 교환하고자 했을 때 4.5 ERC20 token으로 교환되는 것을 의미한다.
앞서 Upbit에서는 5 ERC20 token과 1ETH의 가치가 같았지만, UniSwap의 해당 pair의 ERC20 token이 0.5개 더 고평가 되어있다.
따라서, 아래와 같은 거래를 시도해볼 수 있다.

⚠️ cf. 이는 결국 B의 CASE 2이자 A의 CASE 2이다.
앞서 언급하였듯이 Arbitrage Trading은 가격 변동성이 발생하면 수행할 수 있고, 그러한 가격 변동성은 가격의 급등 또는 급락 양방향으로 급격한 가격 변화 발생시에 일어나는 것이며,
한 token의 상대적 고평가는 마찬가지로 다른 한 token의 상대적 저평가를 의미한다.

> from 4.5 ERC20 token to 1 ETH

이와 같이 Swap 방향을 반대로 뒤집는 방식으로, Upbit에서 해당 ERC20 token을 구매하여 UniSwap 으로 전송하여 ETH로 Swap한 후 그 ETH를 Upbit로 전송하여 매도하여 이익을 실현할 수 있을 것이다.
(4.5 ERC20 token을 구매할 때에는 100만원보다 적은 돈이 들 것이다.)

그러나, 한 가지더 고려해야 할 점이 있다.
실제 UniSwap Pool의 가격 산정 방식은 이러하지 못하다. 이는 앞서 언급하였듯이 UniSwap의 가격 선정 방식이 ratio에 의한 것이기 때문이다.
(x개의 A token을 y개의 B token으로 바꿀 수 있을 때, 역으로 y+α 개의 B token을 투입해야 x개의 A token의 output을 얻을 수도 있다)
따라서 실제 비율은 가령 다음과 같을 수 있다.

> from 4.8 ~ 5.2 ERC20 token to 1 ETH

1 ETH 의 output을 얻기 위해서는 오히려 4.5 이상의 ERC20을 지불해야 할 수 있으며, 특히 5를 넘어서는 ERC20을 지불해야 할 수도 있다.
이 경우 Ethereum Network를 이용하기 위해 필요한 Transaction Fee와 같은 수수료 지불을 차치하고도 손해가 이루어지는 가격이다.
따라서, 이러한 점을 고려하여 설계해야 한다.

아래는 실제 어느 한 시점에서 UniSwap Front-end에서 표기 되는 Swap Ratio이다.

<p align="center"><img src="/assets/posts/2021-04-28-dexArbiProj/fromethtodai.PNG"></p>

<p align="center"><img src="/assets/posts/2021-04-28-dexArbiProj/fromdaitoeth.PNG" height="645px" width="549px"></p>

위의 실례와 같이, ETH를 ERC20 token 중 하나인 DAI로 Swap 할 때에는 0.03개의 ETH 투입으로 약 79.291개의 DAI token을 받을 수 있으나,

반대로 0.03개의 ETH를 output으로 받기 위해서는 pool에 DAI token을 약 79.1874개를 투입해야 한다.

0.03 ETH를 판매하면 79.291의 DAI를 얻지만,

0.03 ETH를 구매하려면 79.1874의 DAI가 필요하다.

즉, 0.1306 DAI 만큼의 괴리가 있는 것이며, 이는 앞서 계속 언급한 UniSwap의 독특한 가격 산정 방식에 의한다.

또, 이러한 괴리는 Trading Volume이 크면 클 수록 더욱 클 것이다.

따라서 각 CASE 별로 정확한 가격 산정을 위해 별도의 설계가 필요할 것이다. (단순한 산수일 뿐이지만)

#### CEX without Derivative Market 에서 CEX with Derivative Market 활용하기

위의 예에서, 우리는 Derivative Market이 없는 Upbit에서 ERC20이 고평가 된 경우에 즉, Upbit의 ETH가 상대적으로 ERC20 보다 저평가 된 경우,
Upbit의 ETH를 구매한 후, ETH를 MetaMask 개인지갑으로 전송, 전송 된 ETH를 UniSwap에서 해당 ERC20으로 Swap 한 뒤에 Swap한 ERC20을 다시 Upbit로 전송하여 매도하는 기법을 사용하였다.

이는 총 3번의 Ethereum Transaction을 요구하게 되는데, 그 Transaction 내용은 다음과 같다.

1. Upbit => MetaMask (ETH 전송) : CEX의 전송 Risk 존재

2. UniSwap Swap Transaction (ETH => ERC20) : 경쟁 Arbitriguer 등의 Risk 존재

3. MetaMask => Upbit (ERC20 전송) : CEX의 전송 Risk 존재

Transaction이 발생할 때마다, Ethereum Transaction Fee가 발생함은 물론이고, CEX에서 요구하는 Block Confirmation의 수로 인한 입금 및 출금 지연 등의 Risk가 존재한다.

이를 Hedging하기 위해서, Derivative Market이 존재하는 CEX를 활용한다. 여기서는 그러한 CEX로 Binance를 예로한다.

적당한 수준의 USDT와 같은 Stable Coin을 Margin(증거금)으로 하여 ETH를 미리 차용(Borrow)해 두어 MetaMask에 전송해 둔다. (차용에 의한 수수료가 발생한다)
이후에 Upbit에서의 ERC20이 고평가 된 경우, 다음과 같은 방법으로 Arbitrage Trading을 수행한다.

1. UniSwap Swap Transaction (ETH => ERC20) [동시에 Binance에서 Swap한 ETH의 Volume만큼 차용한 Volume을 갚는다]

2. MetaMask => Upbit (ERC20 전송)

여기서 []안의 내용은 Ethereum Transaction이 아니라 CEX에서 즉시 이루어질 수 있는 거래이고, Ethereum Transaction과 동시에 이루어져야 한다.

1번의 경우에 두 가지 거래를 동시에 진행하는 것이다. 하나는 UniSwap에서의 Swap거래이고, 하나는 Binance에서의 ETH 차용에 대한 상환 거래이다.

이렇게 미리 ETH를 차용하여 MetaMask로 보내놓는다면, Ethereum Transaction의 과정을 미리 1회 수행 해 놓을 수 있다.

다만, 차용에 의한 수수료를 고려해야하며, Upbit와 Binance 간의 ETH 가격 괴리가 거의 없을 때 이 방법을 이용 할 수 있다.

다행히 차용에 의한 수수료 및 두 거래소 간 ETH 가격 괴리는 대개 bearable하다. (Crypto Currency 시장은 변동성 높은 시장이므로 항상 그러한 것은 아니다)

### CEX with Derivative Market <=> DEX

#### Binance에서 ERC20이 ETH 보다 상대적으로 고평가 된 경우

이 경우, 위에서의 예와 같이 Derivative Market이 존재하는 CEX의 예로는 Binance로, DEX로는 UniSwap을 예로 설명한다.

위에서 설명한 'CEX without Derivative Market 에서 CEX with Derivative Market 활용하기'에서의 원리를 확장하면 된다.

우선, 앞서 사례와 마찬가지로 ETH를 미리 차용하여 MetaMask로 전송한다.

이후 Binance에서 특정 ERC20 token이 고평가 된 순간,

1. UniSwap Swap Transaction (ETH => ERC20) [동시에 Binance에서 Swap한 ETH의 Volume만큼 차용한 Volume을 갚는다] [동시에 Binance에서 Swap의 output으로 얻은 ERC20의 volume만큼 차용하여 매도한다(short)]

2. MetaMask => Binance (ERC20 전송)

이후 Binance에서 ERC20을 차용한 후 매도하였으므로 전송된 ERC20으로 차용 분을 상환한다.

즉, 이 경우 사실상의 Arbitrage Trading의 수익 실현은 1번에서 끝난다. 즉, 수익 확정 그 자체에는 1번만의 Ethereum Transaction이 필요한 것이다.

(1번에서 모든 수익이 확정된다는 것이 한 번만에 잘 이해가 안 될 수 있을 것이다)

물론 여기서도 ETH 및 ERC20의 차용 수수료 등이 고려되어야 할 것이다. 그러나 마찬가지로 그러한 수수료는 bearable하며,
일반적으로 가격 변동성을 상쇄하는 hedging으로 수익을 확정 지을 수 있다는 점이 차용 수수료 보다 더 큰 효용을 제공한다.

#### 그렇다면 Binance에서 ETH가 고평가 된 경우에는?

이 경우에, Binance에서 특정 ERC20을 차용하여 미리 MetaMask에 전송 후, ETH가 고평가 된 시점이 오길 기다려야 한다.

그러한 시점이 온 경우에 그 즉시,

1. UniSwap Swap Transaction (ERC20 => ETH) [동시에 Binance에서 Swap한 ERC20의 Volume만큼 차용한 Volume을 갚는다] [동시에 Binance에서 Swap의 output으로 얻은 ETH의 volume만큼 차용하여 매도한다(short)]

2. MetaMask => Binance (ETH 전송)

과 같은 과정을 거쳐 수익을 실현할 수 있다.

그러나 수 많은 ERC20 token을 빌리기 위해 막대한 증거금이 소요되며, 어느 시점에 Binance의 ETH가 특정 ERC20 token 보다 고평가 될지 알 수 없기 때문에 이와 같은 기법은 잘 사용되지 않을 것이다. 따라서 이러한 상황에는 기존 'CEX without Derivative Market'과 같이 Risk는 존재하지만 3번의 Ethereum Transaction을 통해 거래를 시도하는 것이 좀 더 현실적이다.

## Specification

### Uniswap의 가격 산정 방식

#### Execution Price

#### Mid Price

### Ethereum Transaction Fee

#### Gas Limit

#### Gas Price

### Risk

#### CEX의 Transaction Risk

##### 입급 리스크

##### 출금 리스크

#### Infinite Pending

#### Tx pool

#### Front-Running

GraphQL