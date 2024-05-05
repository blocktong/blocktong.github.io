---
title: "비트스마일리 포인트 시스템인 비트포인트를 소개합니다"
description: ""
coverImage: "/assets/img/2024-05-05-IntroducingbitPointthebitSmileyPointSystem_0.png"
date: 2024-05-05 17:31
ogImage: 
  url: /assets/img/2024-05-05-IntroducingbitPointthebitSmileyPointSystem_0.png
tag: Tech
originalTitle: "Introducing bitPoint — the bitSmiley Point System"
link: "https://medium.com/@bitsmiley/introducing-bitpoint-the-bitsmiley-point-system-1595af1ea22a"
---


**bitPoint**은 프로토콜 내 참여를 통해 사용자에게 제공되는 토큰 에어드랍 보상입니다. 사용자들은 다양한 활동을 통해 포인트를 획득할 수 있으며, 각 시즌의 끝에 포인트가 집계되어 자동으로 보상으로 전환되며, 그 후 초기화됩니다.

프리시즌은 2024년 5월 1일에 시작되어 2024년 5월 11일에 마감될 예정입니다. 이 기간 동안 사용자들은 bitlayer에서 bitUSD를 생성하거나 bitCow에서 지정된 거래 페어에 대한 유동성을 제공함으로써 포인트를 획들할 수 있습니다(예: bitUSD-USDT/bitUSD-WBTC). 프리시즌 종료시, 총 bitSmiley 토큰 공급의 1%가 사용자들 사이에서 포인트에 기반하여 분배될 것입니다.

다음은 프리시즌에 대한 구체적인 규정입니다:

포인트 획득 방법 및 계산 공식:



(a) bitUSD 발행하기

- Mint 포인트 = 발행된 bitUSD * 3

(b) bitCow에서 유동성 제공하기

- Add 포인트 = bitUSD로 제공된 유동성 * 4



Team Bonus: 사용자들은 초대 코드를 사용하여 팀을 만들거나 가입할 수 있으며, 팀 포인트에 따라 순위가 매겨집니다. 전체 포인트가 높은 팀에 가입하면 개인 포인트가 크게 증가하며, 아래 표에 나와 있습니다:

![Team Bonus Table](/assets/img/2024-05-05-IntroducingbitPointthebitSmileyPointSystem_0.png)

* 그룹에 가입하기 전에 획득한 모든 팀 보너스는 개인 포인트 보너스에 기여하지 않습니다.

bit-Disc 스테이킹 보너스: bitSmiley 웹사이트에서 M-bitDisc를 스테이킹하고 EVM 월렛 주소를 연결한 사용자들은 아래에 설명된 대로 추가 보상을 받게 됩니다.



![Introducing BitPoint: the BitSmiley Point System](/assets/img/2024-05-05-IntroducingbitPointthebitSmileyPointSystem_1.png)

To sum it up, individual BitDisc points bonuses will be revised at the end of the pre-season based on user activities from previous days to determine points.

For AA wallet users, we strongly suggest using your EVM wallet to engage with the BitPoint system during this pre-season. If you have restaked M-bitDisc-Black with your AA wallet, please link your AA wallet with your EVM wallet before the pre-season concludes to receive your restaking bonus points.

The formula for calculating total points is as follows:



포인트 Per day는 (포인트 Mint + 포인트 Add) * (1 + 개별 bitDisc 포인트 보너스) * (1 + 개별 팀 포인트 보너스)와 같이 계산됩니다.

예를 들어, 사용자가 10,000 bitUSD를 Mint하고 5,000 bitUSD를 유동성 추가하며, 3 개의 bitDisc 토큰을 스테이킹하고, 가입한 팀이 총 포인트 기준 상위 5%에 속한다면, 그들의 하루 총 포인트는 다음과 같이 계산됩니다:

포인트 총합 = (10,000 * 3 + 5,000 * 4) * (1 + 5%) * (1 + 10%)

만약 사용자의 팀 순위가 다음 날 8%로 떨어지고, 사용자가 추가로 2개의 bitDisc-Black을 스테이킹한다면, 사용자의 총 포인트는 다음과 같을 것입니다:



To calculate this formula, we first determine the value of the expressions within parentheses:

1. \( 10,000 \times 3 + 5,000 \times 4 = 30,000 + 20,000 = 50,000 \)
2. \( 10,000 \times 3 + 5,000 \times 4 + 5,000 \times 1.5 = 30,000 + 20,000 + 7,500 = 57,500 \)

Next, we apply the percentages:

1. For the first expression: \( 50,000 \times 1.05 \times 1.10 = 58,575 \)
2. For the second expression: \( 57,500 \times 1.08 \times 1.08 = 69,954 \)

Finally, we sum the two results:

\( 58,575 + 69,954 = 128,529 \)

Therefore, the total point value is 128,529.