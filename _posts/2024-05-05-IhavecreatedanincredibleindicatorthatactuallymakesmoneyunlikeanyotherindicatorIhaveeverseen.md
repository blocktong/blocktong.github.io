---
title: "제가 본 중에서는 본 적이 없는 놀라운 지표를 만들었어요 다른 지표들과는 달리 실제로 수익을 올리는 거죠"
description: ""
coverImage: "/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_0.png"
date: 2024-05-05 15:44
ogImage: 
  url: /assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_0.png
tag: Tech
originalTitle: "I have created an incredible indicator that actually makes money, unlike any other indicator I have ever seen"
link: "https://medium.com/@paullenosky/i-have-created-an-indicator-that-actually-makes-money-unlike-any-other-indicator-i-have-ever-seen-fd7b36aba975"
---


![Image 1](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_0.png)

# 인디케이터 작동 원리 간략히 설명

3 단계로

![Image 2](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_1.png)



## 1. 터치 감지

이 지표는 가격이 여러 번 닿은 강력한 지지 및 저항 수준을 자동으로 식별합니다. 차트에 나타나는 것은 가격이 최소 몇 번이라도 닿은 수준들뿐입니다. 지표 설정에서 최소 터치 횟수를 선택합니다. 더 능동적인 거래에는 차트에 수준을 표시하기 위해 최소 2번의 터치를 사용하고, 덜 활발한 거래에는 최소 3번의 터치를 사용합니다.

![이미지](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_2.png)

## 2. 터치 감지 영역



터치 검색 영역의 너비는 변동성 분석을 통해 자동으로 결정됩니다. 이것은 가장 어려운 작업 중 하나였습니다. 기존의 구현이 맞지 않아서 제 자신의 변동성 분석을 만들어야 했습니다.

![image](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_3.png)

## 3. 알림 전송

차트에서 새로운 지지 또는 저항 수준이 감지되는 즉시, 지표가 차트에 표시되고 알림이 전송됩니다.



# 왜 이것이 작동하는 이유

![이미지](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_4.png)

▸ 수십 년 동안 사람들은 매매의 방향에 따라 가격의 고점과 저점에 손실을 제한해야 한다는 것을 배워왔습니다. 이것은 20년 전에도 적용되었고, 오늘에도 작동하며 미래에도 작동할 것입니다. 여러분이 손실을 제한하는 방식도 이와 같다고 가정할 수 있겠습니다.

▸ 가격이 어떤 수준을 여러 번 건들였을수록, 해당 수준을 보는 참여자들이 더 많이 나타나게 되며, 동시에 예시와 같이 손실을 제한하는 가격최소치에 포지션을 진입합니다.



▸ 가격이 다시 해당 수준에 접근할 때, 대규모 거래자나 수많은 소규모 거래자들에 의해 돌파됩니다. 이 시점에서, 가격 최저점에 걸어둔 모든 손절 주문이 대규모로 활성화됩니다. 가격이 강력한 움직임을 시작하면 가능한 멀리 놓인 나머지 손절 주문들도 활성화됩니다. 이로써 손절 주문이 대폭 증가하고 가격은 우주로 치솟습니다.

# 지표를 활용한 거래 방법 중 하나.

3단계에서

## 1. 주문 상태 설정



![이미지](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_5.png)

지표가 차트에 새로운 수준을 그린 즉시 또는 해당 수준 내부에 표시된 가격에 대한 알림을 받으면 거래소에 Stop Market Buy 주문을 놓으십시오. 

이 예시에서 최대 가격은 2.7704이므로, Stop Market Buy 주문은 2.7705에 놓아야 합니다.
손실 제한은 수준 내부에 기재된 최소값에 놓아야 합니다.

## 2. 대기



![IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_6](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_6.png)

한 번 주문이 실행되고 알림을 받으면 서둘러너무 빨리 닫지 마세요. 그렇지 않으면 더 많은 수익을 얻지 못할 수 있습니다.

(거래 화면의 스크린샷은 트레이딩뷰에서 제공되는 것이 아니기 때문에 여기에는 이 인디케이터가 표시되어 있지 않습니다)

## 3. 많이 가져가세요



![Link to image](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_7.png)

Trading is all about understanding the concept of mathematical expectation. In the market, not every trade will be profitable, and that's just the nature of trading.

For instance, imagine you hit 10 Stop Losses in a row, losing $100 in each trade, totaling up to $1,000 in losses. However, on the 11th trade, if you let it run, it could bring you a profit of $6,000.

In the end, you've made a profit of $5,000. This is the essence of mathematical expectation in trading - understanding that individual losses are part of the journey towards overall profitability.



한 번의 좋은 거래가 신화적인 10번의 나쁜 거래로 인한 손실을 상쇄할 수 있음을 기억하세요.

# 어떻게 이 지표 "지원 및 저항선 (Support and resistance with touches [Paul Lenosky])"가 탄생했을까요?

저는 교육을 받은 프로그래머입니다. 하지만 7년 이상 동안은 오로지 거래로만 생활했습니다. 이 지표를 처음 만들 때는 엄마를 위해 만들었습니다. 엄마는 헤어 드레서로 일생을 보내고 은퇴 후에 방황하고 쓰임 없다고 느꼈을 때, 거래를 가르쳐드리기로 결심했습니다. 필요했던 것은 거래를 돕는 간단한 지표였죠.

TradingView의 지표 중에서 뭔가 유효한 것을 찾을 수 있을 거라 생각했지만, 4000개의 지표를 몇 일 동안 확인해본 결과 (제가 직접 코드를 쓰기 귀찮아해서요) 아무것도 작동하지 않는다는 사실을 깨달았습니다. 완벽한 설정으로는 과거에서는 작동할지 모르나 주식이나 암호화폐를 바꿔보면 작동하지 않는다는 것 알죠. 아마 인터넷에 유료 지표 중에서 좋은 것이 있을 지도 모르겠다고 생각했지만, 거액 사기거나 작동하지 않는 지표 외에 아무것도 찾지 못했습니다.



아침마다 레벨을 찾아 시작하는 것은 그녀에게 부담이 될 수 있기 때문에 거래 시스템을 가르치지 않는 것이 좋은 아이디어입니다. 집안일을 처리해야 할 일이 많습니다. 그래서 최소한의 거래 시간을 투자할 수 있도록 나만의 지표를 만들었습니다. 이제는 차트에서 강력한 지지선과 저항선을 찾기 위해 아침마다 시작할 필요가 없습니다. 물론, 어머니를 위해서도 그렇습니다. 원래 그녀를 위해 지표를 만들었거든요. 그녀는 아침에 주문을 넣고, 수익에 만족하면 저녁 잠들기 전에 거래를 마감합니다.

![I have created an incredible indicator that actually makes money unlike any other indicator I have ever seen](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_8.png)

# 몇 가지 질문을 예상해 봅니다:

## 지표가 작동하려면 무얼 설정해야합니까?



일반적인 지표는 전혀 사용자 정의 없이 즉시 작동해야 한다고 생각해요. 실행하면 즉시 작동해야 하죠. 이 지표는 모든 것을 자체적으로 분석하고 시장에 맞게 설정을 조정해야만 의미가 있는 거죠. 이 지표에서 실현한 것이 정확히 그 자동화입니다. 하지만 모든 것을 직접 사용자화하고 조정하는 것을 좋아하는 사람들을 위해 유연한 맞춤 설정 옵션을 구현했습니다.

## 이 지표는 어떤 시간프레임에서 작동하나요?

이 지표는 모든 시간프레임에서 작동합니다. 1분마다 레벨 브레이크아웃은 1시간 시간프레임보다 훨씬 작을 것이라는 것을 인식하는 것이 중요합니다. 최소 다음날까지는 주문을 두고 사용하고 그 뒤에 따라가지 않기를 선호하는 사람들을 위해 최소 1시간 시간프레임을 추천하고 있어요.

## 암호화폐만을 위한 것인가요?



세계 기업 주식 차트에서 이 지표를 실행했는데, 손절가 수준이 작용한다는 사실을 발견했습니다. 즉, 이 지표는 그런 시장에서도 작동합니다. 개인적으로는 암호화폐를 거래하는 것을 선호합니다.

**저는 이 지표에 뭔가를 추가하거나 변경하고 싶습니다. 가능한 경우 추가할 수 있을까요?**

네, 물론이죠. 모든 제안을 고려하고 가능할 때 추가할 것입니다.

**저는 훌륭한 아이디어가 있고 누군가에게 구현해 달라고 하고 싶습니다. 저에게 상담을 구할 수 있을까요?**



물론, 언제든지 연락 주시기 바랍니다. 코드 작성을 좋아하고 TradingView를 위한 지표를 만드는 데 사용되는 Pine 스크립트 프로그래밍 언어를 배우는 데 많은 시간을 투자했기 때문에 이러한 기술을 낭비하고 싶지 않아요. 그러니 망설이지 마시고 연락 주세요.

# 지표에 어떻게 접근할 수 있을까요?

텔레그램에서 문자 보내주세요: [https://t.me/PaulLenosky](https://t.me/PaulLenosky) 또는 [twitter.com/Paullenosky](https://twitter.com/Paullenosky)
 
업데이트: 현재 개인 메시지에 대답할 시간이 매우 적기 때문에, 제 텔레그램 채널에서 최신 뉴스를 확인해주세요: [https://t.me/lenoskypaul](https://t.me/lenoskypaul)



# 몇 가지 스크린샷

![image 1](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_9.png)

![image 2](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_10.png)

![image 3](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_11.png)



```markdown
![Image 1](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_12.png)

![Image 2](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_13.png)

![Image 3](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_14.png)

![Image 4](/assets/img/2024-05-05-IhavecreatedanincredibleindicatorthatactuallymakesmoneyunlikeanyotherindicatorIhaveeverseen_15.png)
```



## 만약 이 지표에 대해 토론하거나 구매를 원하신다면, 텔레그램(t.me/PaulLenosky)이나 트위터(twitter.com/Paullenosky)에서 저에게 연락주세요.

## 업데이트: 지금은 개인 메시지에 회신할 시간이 많이 없어서, 최신 소식은 제 텔레그램 채널에서 확인해주세요: [lenoskypaul](https://t.me/lenoskypaul)

이 전략이 작동하는지에 대한 의심이 여전히 남아 있다면, 이 상승 전략 거래에 대한 최소 500개의 비디오를 제공할 수 있습니다.

적어도 조언으로 이 지표를 함께 향상시킬 커뮤니티를 만들고 싶습니다.



이 긴 기사를 읽어 주셔서 감사합니다. 만약 지표를 개선할 아이디어가 있다면 연락해 주세요. 완벽한 것은 없습니다.

제 Medium을 구독하여 다음 업데이트에 대해 읽어보세요. 또한 좋아요도 부탁드립니다.