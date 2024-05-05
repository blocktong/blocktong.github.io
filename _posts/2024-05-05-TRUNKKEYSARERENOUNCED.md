---
title: "프론트 도어의 열쇠를 작별합니다"
description: ""
coverImage: "/assets/img/2024-05-05-TRUNKKEYSARERENOUNCED_0.png"
date: 2024-05-05 15:40
ogImage: 
  url: /assets/img/2024-05-05-TRUNKKEYSARERENOUNCED_0.png
tag: Tech
originalTitle: "TRUNK KEYS ARE RENOUNCED"
link: "https://medium.com/elephant-money/trunk-keys-are-renounced-a66ca60d61e9"
---


```markdown
![TRUNK Token’s Administrative Key Renouncement](/assets/img/2024-05-05-TRUNKKEYSARERENOUNCED_0.png)

# TRUNK 토큰의 관리 키 포기

2024년 4월 30일 - 획기적인 탈중앙화 금융(DeFi) 플랫폼인 Elephant Money가 자체 토큰 TRUNK에 대한 관리 키를 즉시 포기했다고 기쁜 소식을 전합니다. 이를 통해 보안, 투명성 및 커뮤니티 자율성을 높이는 새로운 시대가 열립니다.

# 이 소식이 중요한 이유
```



강화된 보안:

- 관리 키의 철회는 개인이나 단일 단체가 TRUNK에 대한 통제를 보유하지 않음을 의미합니다. 이러한 탈중앙화는 러그 풀 또는 무단 발행과 같은 악의적인 행동의 위험을 크게 줄입니다.
- 이제 키가 버려졌기 때문에 TRUNK의 운명은 그 커뮤니티 손안에 있습니다. 이는 성공에 헌신한 집단적인 힘입니다.

투명성과 신뢰:

- 투명성은 DeFi의 기초입니다. 키들을 철회함으로써 Elephant Money는 개방성에 대한 확고한 헌신을 나타냅니다.
- 투자자와 사용자들은 블록체인 상에서 그 철회를 검증할 수 있어 숨겨진 특혜가 없음을 보장받습니다.



커뮤니티 자치성:

- TRUNK 홀더들은 토큰의 실제 관리자들이에요. 그들의 영향력은 단순 소유 이상으로 확장되며, TRUNK의 운명을 적극적으로 결정합니다.
- 업그레이드, 제안 및 거버넌스와 관련된 결정은 커뮤니티 주도로 이루어지며, 소유와 책임감을 촉진합니다.

# TRUNK에 대한 이것의 의미는 무엇인가요?

가격 독립성:



- TRUNK은 더 이상 가격 제한에 구속되어 있지 않습니다. 시장 역동에 반응하여 자유롭게 거래됩니다.
- 발행 제거로 인해 토큰 공급이 고정되어 있어, 노면화된 성격을 유지합니다.

단순화된 생태계:

- STAMPEDE와 FARMS는 중단되었으며, 생태계가 단순화되었습니다. 사용자는 STAMPEDE 잔액을 TRUMPET으로 이전하고 FARMS에서 인출할 수 있습니다.
- TRUNK의 유일한 수익 메커니즘인 TRUMPET은 매력적인 보상을 제공합니다.

CEX 상장 및 노출:



- TRUNK은 중앙화 거래소(CEXs)로의 여정을 시작합니다. 더 넓은 시장과 증가한 유동성이 기다리고 있어요.
- Elephant.Money 생태계 전체에 대한 광고로, TRUNK는 ELEPHANT 토큰의 성장을 위해 길을 열어주고 있어요.

# 스탬피드에서 트럼펫으로 이주하는 중앙요소들

스탬피드에서 트럼펫으로 지체되는 사용자들을 이주시키기 위한 자동화된 이주 도구가 구축되었습니다. 글 쓰는 시점에서 스탬피드가 6개월 이상 폐지되었습니다. 사용자가 할 수 있는 유일한 작업은 스탬피드 v5에서 v6로 업그레이드한 다음 TRUMPET으로 수동으로 이주하는 것이었습니다. 이 도구는 실행되어 5220명의 사용자를 이주시켰으며 11.36 TRUNK을 생산했습니다. TRUNK의 총 공급량은 111백만에서 124백만으로 급증했습니다.

다음은 이주 중에 실행된 소스와 거래의 스탬피드 이주 컨트랙트에 대한 링크입니다:



https://bscscan.com/address/0x49241ea8ec03ffa5521ad8717579e396ceb02400

# TRUNK 소유권 포기

지금 기준으로 마지막 48시간 동안 TRUNK를 포기하기 위해 실행된 실제 계약 및 거래 내역이 여기에 나와 있습니다. 포기의 첫 단계는 TRUNK를 만들기 위한 화이트리스트에 어떤 스마트 계약이 있는지 심사하는 것이었습니다. 맞춤형 도구가 개발된 후 계약의 상태를 확인하고 제거했습니다.

마지막으로, TRUNK 소유권을 완전히 포기하기 위해 Elephant Money Deployer(0x16E76819aC1f0dfBECc48dFE93B198830e0C85EB)로부터 ZERO 주소(0x0000000000000000000000000000000000000000)로 소유자를 바꿨습니다. 이 과정은 "renounceOwnership" 방법을 통해 이루어졌습니다.



잠금/잠금해제 기능 버그로 인해 lock 메서드를 호출한 contract의 마지막 소유자가 이전Owner 개인 변수를 통해 contract를 잠금 해제하고 소유권을 회복할 수 있습니다. Contract가 한 번도 잠겨지지 않은 경우, 포기하는 것은 문제가 되지 않습니다. 이 문제가 없음을 증명하기 위해 일련의 거래 내역을 제공했습니다:

- "OwnershipTransferred" 이벤트가 기록된 최초의 Contract 생성 — [https://bscscan.com/tx/0x32ca0be261688690c5a36355e22600172e1b5aeff48be9f8c8b6ffda89f13090#eventlog](https://bscscan.com/tx/0x32ca0be261688690c5a36355e22600172e1b5aeff48be9f8c8b6ffda89f13090#eventlog)
- "renounceOwnership" 메서드를 호출한 후 "OwnershipTransferred" 이벤트가 마지막으로 기록된 경우 — [https://bscscan.com/tx/0x486b87e5d8245dd3a102fe89d2b631daa1b408fea6a86cf31f8eb1cfce07bc12#eventlog](https://bscscan.com/tx/0x486b87e5d8245dd3a102fe89d2b631daa1b408fea6a86cf31f8eb1cfce07bc12#eventlog)
- 소유권의 유일한 두 번의 이전이 설명되었으므로 lock이 호출되지 않았으며 _previousOwner이 설정되지 않았습니다.

![Transaction Records](/assets/img/2024-05-05-TRUNKKEYSARERENOUNCED_1.png)

# 참여해보세요!



홀드 중인 경우:

- TRUNK를 이미 보유하고 계신다면, 할 일이 없습니다. 이를 사용하여 TRUMPET을 생성하거나 다른 토큰과 마찬가지로 거래할 수 있습니다.
- TRUNK의 희귀성과 잠재력으로 인해 흥미진진한 투자 기회가 될 수 있습니다.

그라운드 플로어 기회:

- TRUNK의 우수한 홀더 분포와 유동성은 성공을 위한 좋은 위치에 있습니다.
- CEX에 상장되기 전에 이 여정의 한 부분이 되어보세요.



**더 알아보기:**

- Elephant Money 생태계를 탐험하고 TRUNK의 힘을 발견하세요.
- 자세한 통찰력을 얻기 위해 공식 TRUNK 문서를 방문해보세요.

**Elephant Money 소개:** Elephant Money는 혁신, 투명성 및 커뮤니티 중심의 성장에 헌신한 DeFi 플랫폼입니다. 저희의 미션은 금융 주권을 통해 사용자들을 성과적으로 함께 할 수 있도록 하는 것입니다.

미디어 문의: 언론 관계 이메일: press@elephant.money



**면책 조항:** 본 프레스 릴리스에는 예상과는 다를 수 있는 내용이 포함되어 있습니다. 독자는 자체 연구와 신중한 조사를 진행할 것을 권고합니다.

**코인뉴스**  
**Elephant Money: 금융 자유를 부여받다**

1. TRUNK 토큰 개요 - Medium  
2. YouTube: 선물 V10 업데이트에 따른 TRUNK 토큰 공급 충격?  
3. Elephant Money의 큰 변화: TRUNK 및 생태계의 변화 이해하기