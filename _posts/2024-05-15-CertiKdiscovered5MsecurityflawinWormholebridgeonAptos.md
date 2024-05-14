---
title: "삼성전자의 새로운 스마트폰 모델이 최근 공개되었습니다 이 모델은 이전 모델보다 더 빠르고 효율적인 프로세서를 장착했으며, 놀라운 그래픽 성능을 자랑합니다 새로운 스마트폰은 독특한 디자인과 고급 소재로 만들어져 있어 눈길을 끌고 있습니다 사용자들은 또한 좋은 카메라 성능과 긴 배터리 수명에도 흥미를 느끼고 있습니다 새로운 삼성전자 스마트폰을 기대해봅시다"
description: ""
coverImage: "/assets/img/2024-05-15-CertiKdiscovered5MsecurityflawinWormholebridgeonAptos_thumbnail.png"
date: 2024-05-15 02:09
ogImage: 
  url: /assets/img/2024-05-15-CertiKdiscovered5MsecurityflawinWormholebridgeonAptos_thumbnail.png
tag: Tech
originalTitle: "CertiK discovered $5M security flaw in Wormhole bridge on Aptos"
link: "https://cointelegraph.com/news/certik-discovered-5-million-security-flaw-wormhole-bridge-aptos"
---


![Wormhole Bridge Security Flaw](/assets/img/2024-05-15-CertiKdiscovered5MsecurityflawinWormholebridgeonAptos_thumbnail.png)

Aptos 네트워크의 Wormhole 다리에서의 보안 결함으로 인해 500만 달러의 손실이 발생할 수 있었으나, 블록체인 보안 플랫폼 CertiK의 소셜미디어 게시물에 따르면 이는 발견되어 조치되었습니다. 해당 플랫폼은 이 버그를 발견하고 Wormhole 팀에 보고하여 문제가 발생하기 전에 조치했다고 밝혔습니다. 이 결함은 수정되었으며, 해당 다리는 더 이상 취약하지 않습니다.

![Wormhole Bridge Security Flaw](/assets/img/2024-05-15-CertiKdiscovered5MsecurityflawinWormholebridgeonAptos_0.png)



Aptos는 MOVE 프로그래밍 언어를 사용하는 블록체인 네트워크입니다. 이 언어는 원래 Facebook이 Libra 프로젝트를 위해 개발한 것입니다. MOVE의 지지자들은 Ethereum의 Solidity나 다른 대안들과 비교했을 때 스마트 계약을 작성하는 데 보다 안전한 언어임을 주장합니다.

CertiK 보고서는 비디오 형식으로 게시되었습니다. 보고서는 "MOVE 프로그래밍 언어 내 'public(friend)' 및 'entry' 수정자의 잘못된 구현에서 발생했다"고 주장했습니다. 'public(friend)' 수정자는 같은 모듈 내의 다른 함수나 "친구 목록"에 지정된 외부 계정들에 의해 호출될 수 있지만 다른 호출자에 의해 호출될 수는 없습니다. 반면 'entry' 수정자는 함수가 외부 계정에 의해 호출될 수 있음을 지정합니다.

다리에는 'publish_event'라는 함수가 포함되어 있었는데, 이 함수는 토큰 전송과 같은 이벤트를 알리는 데 사용되었습니다. 이 함수는 동일한 모듈 내의 다른 함수에서 또는 특정 "지정된 외부 엔티티"에서만 호출될 수 있는 것으로 예상되었습니다. 그러나 CertiK가 연구한 버전의 다리에서는 이 함수가 'public(friend)'와 'entry'로 수정되었습니다. 이로써 'publish_event'를 승인받은 호출자가 아니더라도 누구나 호출할 수 있게 되었습니다.

이 결함으로 공격자는 실제 토큰을 이동시키지 않고도 계정 간에 토큰을 이동한 것처럼 가짜 트랜잭션을 생성할 수 있었습니다. 이러한 "이벤트"들로 인해 Ethereum 버전의 다리가 Aptos 측에 실질적인 예금이 없이도 토큰을 주조하거나 잠금해제할 수 있었습니다. 결과적으로 CertiK에 따르면 공격자는 최대 $5백만 가치의 자금을 다리에서 빼낼 수 있었을 것입니다.



CertiK에서 2023년 12월 5일에 Wormhole 팀 멤버들에게 결함에 대해 알렸습니다. 보고서를 조사한 후 팀은 보안 구멍을 메우기 위한 패치를 개발하고 시험했으며 프로토콜 수호자들에게 문제를 알렸습니다. 다중 서명 투표를 통해 수호자들은 패치를 시행할 것을 승인했고 프로토콜의 Aptos 계약은 새 코드를 시행하기 위해 업그레이드되었습니다. 결함이 보고된 후 이를 해결하는 과정은 약 3시간이 걸렸으며 브릿지의 새 버전은 이제 더 이상 이러한 악용에 취약하지 않습니다.

![Link to image report](/assets/img/2024-05-15-CertiKdiscovered5MsecurityflawinWormholebridgeonAptos_1.png)

'publish_event' 함수에서 'entry' 키워드를 제거하는 것 외에도 새 패치는 Aptos의 "지배자 요금 제한" 값을 $5백만에서 $1백만으로 제한하여 하루에 $1백만을 초과하는 금액을 Aptos에서 인출하는 것을 효과적으로 방지했습니다. 이는 잠재적인 악용의 경우 손실을 제한하기 위해 수행되었습니다. 현재 사용량은 하루 평균 $1백만 미만으로, CertiK는 대부분의 사용자에게는 이 요율 제한이 영향을 미치지 않을 것이라고 주장했습니다.

또한 Wormhole은 문제로 인해 사용자 자금이 영향을 받았는지 여부를 결정하기 위한 "회고적 분석"을 수행했습니다. 그들은 어떠한 자금도 불법적으로 이체되지 않았고 사용자의 잔액이 안전한 것으로 결론을 내렸습니다.



Wormhole은 항상 보안 취약점을 악용하기 전에 제대로 처리하지 못한 적이 있었습니다. 2022년에는 브릿지의 Solana 부분에 버그가 발생하여 공격자가 미지원 토큰을 만들어 약 3억 2천1백만 달러 이상을 소실했습니다. 그러나 팀은 나중에 이 버그를 수정하고 사용자들에게 보상했습니다. 1월에는 Wormhole이 그 사건 이후 처음으로 총 잠긴 가치 10억 달러를 회수했으며, 이를 통해 일부 사용자들이 보안 관행이 개선되었다고 느끼고 있음을 보여주었습니다.

관련 기사: 이익 네트워크 포크의 버그로 매매 시마다 900% 수익 창출 가능: 보고서