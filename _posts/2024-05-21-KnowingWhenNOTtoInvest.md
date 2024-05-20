---
title: "투자할 때 당신이 투자하지 말아야 하는 시점을 알아보세요"
description: ""
coverImage: "/assets/img/2024-05-21-KnowingWhenNOTtoInvest_0.png"
date: 2024-05-21 02:25
ogImage: 
  url: /assets/img/2024-05-21-KnowingWhenNOTtoInvest_0.png
tag: Tech
originalTitle: "Knowing When *NOT* to Invest"
link: "https://medium.com/@zimin.lu/knowing-when-not-to-invest-87e39ce2cea2"
---


만약 여러분의 투자 시계표가 25년 이상이라면, 이 질문은 그다지 중요하지 않을 수 있습니다. SPY는 2000년 이후 놀라운 173%의 총 수익률(배당금 + 가치 증가)을 기록했습니다. 유명한 매수보유 전략은 효과적으로 작동합니다. 물가 상승 경제에서는 시장이 확실하게 오르게 됩니다. 지금 투자해야 합니다. 최적의 진입 시점이 아닐 수 있지만, 시장이 가치를 인정받는 것을 기다릴 시간이 있습니다.

그러나 여러분의 투자 시계표가 5년이라면, 항해가 원활하지 않을 수 있습니다. 그림 1에서 주황색 선은 2000년 이후의 총 수익률 그래프입니다. 2003년, 2008년, 2020년 및 최근 2023년에는 빈번한 인도가 발생했는데, 그 비율은 각각 60%, 80%, 40%, 30%입니다. 이러한 상황은 상당한 재정 위기를 야기할 수 있습니다. 인도가 발생할 때 돈이 필요하다면, 심각한 재정 위기에 놓일 수 있습니다. 그러므로 아무 때나 발생할 수 있는 나쁜 상황을 피하기 위한 간단하면서도 효과적인 위험 관리 도구를 가지는 것이 현명합니다. 이 노트는 투자 차량으로서 SPY를 사용하여 모멘텀 및 변동성 필터를 사용하여 인도 위험을 어떻게 제어할 수 있는지 조사하는 것입니다.

그림 1에서 파란 선은 모멘텀 및 변동성 필터를 적용한 후의 총 수익률입니다. 이는 다음 코드로 정의됩니다:
```js
# 변동성 모델, 역사적 변동성 또는 VIX를 선택할 수 있음
df['vol']=df['return'].rolling(14).std()*15.87
df['vol_bb']=df['vol']

volmax=df['vol_bb'].quantile(0.97)

df['MAshort']=df[ticker].rolling(50).mean()
df['MAlong']=df[ticker].rolling(200).mean()
df['maCross']=df['MAshort']-df['MAlong']

모멘텀필터=(df['maCross']>0)  
변동성필터=(df['vol_bb']<volmax)

조건롱=  모멘텀필터 & 변동성필터
```

<div class="content-ad"></div>

![KnowingWhenNOTtoInvest_0.png](./assets/img/2024-05-21-KnowingWhenNOTtoInvest_0.png)

![KnowingWhenNOTtoInvest_1.png](./assets/img/2024-05-21-KnowingWhenNOTtoInvest_1.png)

![KnowingWhenNOTtoInvest_2.png](./assets/img/2024-05-21-KnowingWhenNOTtoInvest_2.png)

만약 conditionLong이 true인 경우에는 SPY에 투자하고, false인 경우에는 시장을 떠납니다. 다른 말로는 그림 1과 같이 d=1일 때에는 SPY에 투자하며, d=0일 때에는 투자하지 않습니다. 시장에 없는 시기를 선택함으로써, 80%의 최대 손실을 21%로 줄일 수 있습니다. 2000년부터 현재까지의 기간에 대해 리스크 피리오드 1%를 사용하여, Sharp 비율이 0.31에서 0.66으로 향상됩니다. 물론 총 수익은 173%에서 206%로 약간 상승하지만, 리스크 경제적인 전략에서의 Sharp 비율은 100% 증가했습니다.

<div class="content-ad"></div>

간단 정리: 우리는 모멘텀과 변동성 필터를 사용하여 SPY에 투자할 때 훨씬 더 편안하게 잠을 잘 수 있습니다.

제 글이 마음에 드시면 *좋아요* 버튼을 꾹 눌러주세요. 읽어 주셔서 감사합니다.

금융 자문 자격을 취득한 사람이 아니며, 이 글은 금융 조언이 아닙니다. 순전히 즐거움과 교육적 목적으로 작성되었습니다.