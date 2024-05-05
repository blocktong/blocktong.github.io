---
title: "곡선 모양의 잔디밭에서 독사를 발견"
description: ""
coverImage: "/assets/img/2024-05-05-Findingaviperinthecurvedlawn_0.png"
date: 2024-05-05 17:44
ogImage: 
  url: /assets/img/2024-05-05-Findingaviperinthecurvedlawn_0.png
tag: Tech
originalTitle: "Finding a viper in the curved lawn"
link: "https://medium.com/@kupiasec/finding-a-viper-in-the-curved-lawn-e43401997cce"
---


```markdown
![Finding a viper in the curved lawn](/assets/img/2024-05-05-Findingaviperinthecurvedlawn_0.png)

복잡성이 있는 곳에는 종종 버그가 숨어 있습니다.

2023년 11월 말에, ETH 메인넷에서 버그 사냥을 여러 차례 시도했지만 성공하지 못한 후 결정을 내렸습니다. Avalanche의 Multichain Router V6에서의 공격은 Ethereum이 철저한 검토를 받고 다수의 MEV 봇을 유치하는 반면, 다른 L1 체인들이 상대적으로 적은 주목을 받는다는 점을 상기시켜 주었습니다. 이는 해당 공격의 단순성과 2년 가까이 감지되지 않은 사실에서 명백합니다. Avalanche도 복잡한데, X, P 및 C 체인으로 구성되어 있습니다. 복잡성이 오류와 간과의 가능성을 높인다는 점이 명확히 드러납니다.

![Finding a viper in the curved lawn](/assets/img/2024-05-05-Findingaviperinthecurvedlawn_1.png)
```



Avalanche 프로젝트 목록에서 ImmuneFi를 거친 결과, Geode Finance에 주목했습니다. Geode Finance는 Curve의 Solidity 이식 버전으로, Solidly/Nerve Finance와 유사합니다.

Curve는 DeFi의 선두주자 중 하나이며 여러 Solidity 버전의 포크된 Curve가 있었습니다. Ellipsis, Swerve, LeetSwap 등이 그 예시입니다. 이러한 필드 테스트를 통해 Curve 포크들은 꽤 탄탄해졌는데, Geode Finance도 마찬가지였습니다.

제 평가 과정에서 Geode Finance에서 취약점을 발견하지 못했습니다. 그러나 Geode Finance에는 ERC1155 표준을 기반으로 한 gAVAX라는 고유 토큰이 있었으며, 풀 코드에는 onERC1155Received 함수가 포함되어 있었습니다.

이것이 AVAX 또는 ERC1155 토큰 전송 중에 재진입 공격이 가능하다는 생각을 하게 만들었습니다.



Curve의 Vyper 코드를 이미 알고 있었기 때문에 빠르게 ETH 수영장을 열었습니다.

Curve 수영장은 ERC1155 토큰을 보유하지 않지만 원시 ETH를 처리하기 때문에 재진입이 가능합니다.

일반적인 시나리오는 토큰을 직접 수영장으로 이체하여 토큰 계정을 깨는 것이었습니다. 만약 어떤 계산에서 token.balanceOf를 사용했다면, 증가한 잔고를 이용해 조작이 가능했습니다.

다행히 코드는 모두 self.balances 상태 변수에서 작동했기 때문에 재진입이 그것들에 영향을 미치지 못했습니다. 또한 중요한 메소드에 재진입 잠금도 있었습니다.



token.balanceOf을 사용하는 몇 가지 경우가 있었습니다. 채무자가 충분한 잔액이 있는지 확인하거나 balanceOf를 사용하여 실제 전송된 토큰 금액을 얻는 데 사용되었습니다. 하나를 제외하고 withdrawAdminFees()에서 사용되었습니다.

![image](/assets/img/2024-05-05-Findingaviperinthecurvedlawn_2.png)

withdrawAdminFees()는 가능한 공격자를 위한 완벽한 포인트였습니다. 재진입 잠금이 없었고 token.balanceOf를 사용했습니다.

하지만 곧 해당 함수가 특권이 있는 것을 알게 되었습니다. 누구나 액세스할 수 있는 일반 함수였다면, 재진입 공격을 유발할 가능성이 있었으며 풀 상태와 실제 토큰 잔액 사이의 불일치를 초래할 수 있었습니다. 이는 Curve의 다양한 풀 유형들을 생각나게 했고, 공개 withdrawAdminFees() 함수가 어딘가에 숨어 있을 수 있을지 궁금해졌습니다.



CodeIsLaw를 사용하여 계약을 검색했습니다.

![image](https://example.com/assets/img/2024-05-05-Findingaviperinthecurvedlawn_3.png)

https://etherscan.io/address/0x94b17476a93b3262d87b9a326965d1e91f9c13e7#code - 해당 풀이 조건을 충족했습니다.

그 다음으로, 외부 호출이 잔고 업데이트 전에 발생하는 재진입 지점을 찾아야 했는데, remove_liquidity_imbalance() 메서드가 해당되었습니다!



깊게 숨을 들이마시고 컵 하나의 커피와 함께 PoC를 작성하기 시작했다.

해당 취약점은 remove_liquidity_imbalance의 fallback 내에서 withdraw_admin_fees를 호출함으로써 일시적으로 일부 풀이 강제로 파괴될 수 있다는 것이었다.

![Image](/assets/img/2024-05-05-Findingaviperinthecurvedlawn_4.png)

자세한 절차:



- 토큰을 준비합니다.
- add_liquidity를 통해 토큰을 사용하여 단방향 유동성을 추가합니다.
- remove_liquidity_imbalance를 통해 유동성을 제거하고 ETH 인출 금액으로 1 wei를 지정하여 폴백이 호출되도록 하고, 토큰 금액은 10⁹ wei와 같이 약간의 숫자로 감소시켜 기능이 되돌지 않도록 합니다.
- 폴백 함수(receive) 내에서 withdraw_admin_fees를 호출합니다.

이러한 과정을 거치면 풀의 토큰 잔액이 어느 정도 감소하면서 호출자 측에서는 스왑 수수료 및 추가/제거 유동성 반올림에 손실이 발생합니다.

![Findingaviperinthecurvedlawn_5](/assets/img/2024-05-05-Findingaviperinthecurvedlawn_5.png)

잔액이 balance 상태 변수보다 낮아진 것을 확인할 수 있습니다.



이 초기 보고서 제출 이후에는, 이어지는 후속 커뮤니케이션을 통해 추가 세부 정보와 더 나아가 그에 따른 영향도 제공했습니다.

몇 시간 후, 회신을 받았습니다. 우리는 계속해서 소통을 나누었습니다. 플래시 대출을 사용하여 영향을 확대시킬 수 있습니다. OETH 보류이 있기 때문에 공격자가 이를 사용하여 ETH를 대출 받아 OETH를 발행하고 풀을 강제로 대부분의 토큰을 수수료 수집자에게 전송하도록 할 수 있습니다.

나는 항상 Klein 병풍 팀을 좋아했습니다. 그들은 최초의 혁신자 중 하나였으며 그들의 코드는 간결하고 수학적으로 깔끔했습니다. 예상대로, 논의가 매우 원활했고 팀은 최대 현상금인 25만 달러를 수여하기로 결정했습니다. 설립자인 Michael Egorov가 직접 제출을 처리해준 것에 놀랐는데, 그는 만나 뵙는 경험이었습니다.

긴급 조치가 즉시 시행되었으며 그 후 지속적인 수정안이 거버넌스 투표를 위해 제안되었습니다. 이제 수수료 수취자들은 ETH 수령 시 재진입을 확인하므로, withdraw_admin_fees()가 재진입할 경우 되돌아가게 됩니다.



Curve 포크를 확인한 결과, 대부분의 Solidity 기반 포크는 withdrawAdminFees에 특권이 주어지거나 removeLiquidity 프로세스에서 raw ETH 대신 WETH를 사용합니다. 이에 따라 이러한 구현은 취약성으로부터 안전하다고 여겨집니다.

2023년의 마지막 날들은 상대적으로 조용했으며 몇 가지 보안 사고가 있었습니다. 그러나 개인적으로는 흥미로운 경험으로 가득한 기간이었습니다.

--- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- 

타임라인



11월 29일 13:10 보고서
11월 29일 18:47 답변, 인지
11월 30일 내부 보안 검토
12월 2일 긴급 수정 제안 후 투표
12월 3일 긴급 수정 적용, 수수료 변환 비활성화
12월 6일 수정 제안 후 투표
12월 12일 수정 적용, 바운티 지급

DeFi 프로토콜과 비교해 진지한 보안 연구자 수가 비교적 적다는 점을 여전히 느낍니다.

발견될 기회가 많은 버그들이 많지만, 기회는 준비된 마음에 옵니다.

나는 KupiaSec의 리드 보안 연구원 Marco Croc입니다.



KupiaSec은 블록체인 보안 기관으로, 최첨단 보안 감사 프로세스를 활용하여 최고 품질의 보안 검토를 달성하는 것을 목표로 합니다.