---
title: "Bugs in the Gains Network fork allowed traders to make a profit of 900 on each trade, according to a report"
description: ""
coverImage: "/assets/img/2024-05-15-BugsinGainsNetworkforklettradersprofit900oneverytradeReport_thumbnail.png"
date: 2024-05-15 02:19
ogImage: 
  url: /assets/img/2024-05-15-BugsinGainsNetworkforklettradersprofit900oneverytradeReport_thumbnail.png
tag: Tech
originalTitle: "Bugs in Gains Network fork let traders profit 900% on every trade: Report"
link: "https://cointelegraph.com/news/bugs-in-unnamed-gains-network-fork-let-traders-profit-900-on-every-trade"
---


![Gains Network report](/assets/img/2024-05-15-BugsinGainsNetworkforklettradersprofit900oneverytradeReport_thumbnail.png)

## Zellic 보안 플랫폼에 따르면, Gains Network 레버리지 트레이딩 프로토콜의 포크에서 발견된 두 가지 버그로 인해 토큰의 가격과 무관하게 매매마다 900%의 이윤을 얻을 수 있었습니다. 이 보고서는 4월 19일 발표되었습니다. 

버그 중 하나는 이전 버전의 Gains에서 발견되었지만 나중에 수정되었습니다. 다른 하나는 프로토콜의 포크에서만 발견되었습니다. 

Zellic에 따르면, 해당 기업은 Gains의 판매자인 Gambit Trade, Holdstation Exchange 및 Krav Trade의 개발자에게 이 취약점에 대해 알렸으며, 이들 개발 팀은 이러한 두 가지 결함을 포함하지 않도록 보장했습니다. 그러나 Zellic은 다른 Gains 포크는 여전히 취약할 수 있다고 경고했습니다.



공식 웹사이트에 따르면, Gains Network은 Polygon 및 Arbitrum에서 운영되는 탈중앙화 금융(DeFi) 제품의 생태계입니다. 그들의 레버리지 트레이딩 앱의 공식 명칭은 “gTrade”입니다. 2023년 5월 시작 이후 디파이 알라마 블록체인 분석 플랫폼에 따르면, 총 250억 달러 이상의 파생 상품 거래량을 보유하고 있습니다.

![이미지](/assets/img/2024-05-15-BugsinGainsNetworkforklettradersprofit900oneverytradeReport_0.png)

Zellic은 몇몇 인기 DeFi 트레이딩 앱이 Gains Network의 베이스 코드에서 파생되었다고 주장했습니다. 이 중 Gambit Trade와 Holdstation 이외에도 여러 프로토콜이 포함됩니다. 그들은 특정 포크를 연구하는 동안 해당 취약점을 발견했지만 어느 것에서 발견했는지는 공개하지 않았습니다.

보고서에 따르면, Gains Network 계약을 통해 사용자는 시장, 반전 또는 모멘텀 거래 주문을 개설할 수 있습니다. 시장 주문은 가격에 관계없이 즉시 자산을 매수 또는 매도할 수 있습니다.



사용자가 모멘텀 또는 반전 거래를 요청하면 스마트 계약은 거래할 가격에 대한 데이터가 포함된 "주문"을 기록합니다. 이 가격에 도달하면 어떤 사용자라도 executeLimitOrder 함수를 호출하여 주문을 처리할 수 있습니다. 실행을 호출하는 사용자는 주문을 넣은 사용자와 동일할 필요가 없습니다. 실행을 호출하는 사용자는 이 역할을 수행하고 소액의 "실행 수수료"를 받게 됩니다.

이를 통해 사용자들은 센트럴라이즈된 거래소에서 할 수 있는 것과 유사한 방식으로 한정 주문(모멘텀) 및 스탑-한정 주문(반전)을 할 수 있지만, 중앙화된 엔티티가 주문을 채우기 위해 필요하지 않습니다.

사용자가 주문을 넣을 때, 수익실현 가격, 손절 가격 또는 둘 다를 설정할 수 있습니다. 이 설계의 의도는 거래자들이 수익실현 지점에서 수익을 자동으로 포착하거나 손해를 손절 지점에서 자동으로 판매할 수 있도록 하는 것입니다.

## Gains 포크에서 발생한 버그는 매수 주문에서 900%의 수익을 가능하게 했습니다.



When analyzing the Gains fork, Zellic discovered a significant vulnerability. They found that the stop-loss price was mistakenly stored in the “currentPrice” variable when an order was opened, impacting profit and loss calculations. This flaw enabled users to exploit trades by setting stop-loss above the open price, ensuring automatic profits.

Consider a situation where Bitcoin (BTC) was priced at $63,000, and a user set the open price at $62,000 with a stop-loss at $64,000. If the price dropped to $62,000, the order would fill, but the price would trigger an automatic exit just below the set stop-loss.

Moreover, setting stop-loss recorded the user's preferred price as the current price, allowing unrealized profits. For instance, in the scenario above, the user could profit $2,000 instead of the correct value of $0. This flaw permitted attackers to exploit trades systematically, leading to significant losses within the protocol.

To prevent such exploits, the protocol contained a safeguard that generated a “wrong_sl” error if a user attempted to set their stop-loss above the open price on a buy order.



```markdown
![Screenshot](/assets/img/2024-05-15-BugsinGainsNetworkforklettradersprofit900oneverytradeReport_1.png)

However, the investigators found that under certain conditions, the system's checks could be bypassed.

When a user initiated an order, they would specify the opening trade price, stored in the "openPrice" variable. The system checked this value at the start. Yet, the order execution function modified this variable to match "a.Price," the current price with the order's impact included.

This loophole allowed an executor to circumvent the validation by executing the order even if the user had set an unreasonably high opening price. Additionally, the executor could fulfill the order at a price lower than the user's original setting.
```



예를 들어, Zellic은 1조 달러에 해당하는 10의 100000배인 $100000e10에서 토큰을 구매하는 주문을 넣고, 이 주문에 대한 스탑-로스를 해당 주문 가격보다 $1 낮은 $999.999999999999 십조로 설정하는 공격자 아이디어를 고려했습니다. 주문이 완료되면, 공격자는 자신의 주문을 실행하여 거래의 가격 영향이 고려된 현재 가격으로 openPrice를 변경합니다.

거래가 실행되어 오픈되고, 최종 오프닝 가격이 초기에 설정된 스탑-로스보다 낮으면 이제 해당 거래를 스탑-로스를 실행하여 종료할 수 있습니다. 공격자가 자신의 스탑-로스를 실행하면, 종가와 스탑-로스 가격 간의 차익을 얻게 됩니다.

Zellic은 이 거래가 공격자에게 900%의 이윤을 줄 것이라고 주장했습니다.

![2024-05-15-BugsinGainsNetworkforklettradersprofit900oneverytradeReport_2.png](/assets/img/2024-05-15-BugsinGainsNetworkforklettradersprofit900oneverytradeReport_2.png)



Gains Network에서 이 결함은 Zellic 팀이 발견될 당시 존재하지 않았습니다. 이 문제는 조사 중이었던 포크 버전에만 존재했습니다. 그러나 이 문제를 연구하는 과정에서 Gains의 이전 버전에서 발견된 두 번째 결함을 발견했습니다.

## 두 번째 버그로 900%의 이익 얻을 수 있었던 사례

두 번째 버그로 인해 거래자들이 가격 변동과 관계없이 판매 주문으로 900%의 이익을 얻을 수 있었습니다.

Gains 포크에서 거래가 종료될 때, 사용자의 손절가나 익절가 포인트를 "int"라는 변수로 변환하고, 그것을 사용하여 이익을 백분율로 계산했습니다. 그러나 사용자가 2^256-1과 정확히 일치하는 손절가나 익절가 값을 입력한 경우, 그 결과 계산은 "int"가 음수가 되도록 만들었을 것입니다.



그 이유는 Ethereum의 양수의 최대값인 2^256-1 때문에 발생했습니다. 이 값 이상의 수치는 "오버플로우"가 발생하여 0부터 다시 시작되고 계산에 시작 가격이 추가되었기 때문입니다. Solidity 프로그래밍 언어에서 2^256-1은 "type(uint256).max"로도 알려져 있습니다.

Zellic에 따르면, 공격자가 9배 이상의 레버리지를 사용했다면 이 취약성을 통해 900%의 이익을 얻을 수 있었다고 합니다:

**"판매 주문을 고려해보겠습니다. currentPrice를 type(uint256).max로 설정한다면 diff의 결과 값은 openPrice + 1이 됩니다(int(type(uint256).max) = -1). 그리고 이로 인해 이익률은 거의 100 * 레버리지와 같아집니다. 따라서, 레버리지가 9보다 크면 이 함수는 이익을 900%로 반환할 것입니다."**

계약에는 2^256-1을 이익 실현 지점으로 입력하지 못하도록 하는 체크가 있었지만, 이 체크는 주문을 처음 열 때에만 수행되었습니다. 사용자가 주문을 열고 난 후에 이익 실현 지점을 변경한다면, 해당 체크를 우회하여 2^256-1을 이익 실현 지점으로 입력할 수 있었으며, 매 거래마다 900%의 이익을 얻을 수 있었습니다.



두 번째 결함은 이전 버전의 Gains에 존재했지만 나중에 수정되었습니다. 현재 버전에는 해당 결함이 포함되어 있지 않으며, 이익 실현 및 손실 제한이 업데이트될 때와 처음 설정될 때 모두 확인이 수행됩니다.

Zellic은 이러한 두 가지 보안 결함에 대해 상기된 모든 포크(하위 암호화폐)들에게 알리고, 해당 결함에 영향을 받을 수 있는 기타 프로토콜을 찾으려는 시도로 Crypto Security Alliance에 연락했습니다. 그러나 일부 Gains 포크들이 여전히 이러한 버그를 포함할 수 있으며, 이는 사용자 자금을 훔침할 위험을 초래할 수 있습니다.

Cointelegraph는 Gains Network, Gambit Trade, Holdstation Exchange 및 Krav Trade에게 의견을 얻기 위해 연락했지만 게시 시점까지 응답을 받지 못했습니다.

Gains Network은 해당 자산의 "실제 장부 가격"을 제공하며, 영구 계약에 기반한 덜 정확한 가격들 대비 우수한 외환 거래를 제공한다고 주장합니다.



관련 기사: 리브라 관련 수이 블록체인, '수십억 달러'를 위험에 빠뜨렸던 중대한 버그 수정