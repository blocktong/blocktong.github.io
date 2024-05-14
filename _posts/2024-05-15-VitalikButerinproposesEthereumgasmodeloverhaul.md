---
title: "비탈릭 부테린이 이더리움 가스 모델 개편을 제안합니다"
description: ""
coverImage: "/assets/img/2024-05-15-VitalikButerinproposesEthereumgasmodeloverhaul_thumbnail.png"
date: 2024-05-15 02:08
ogImage: 
  url: /assets/img/2024-05-15-VitalikButerinproposesEthereumgasmodeloverhaul_thumbnail.png
tag: Tech
originalTitle: "Vitalik Buterin proposes Ethereum gas model overhaul"
link: "https://cointelegraph.com/news/ethereum-vitalik-buterin-gas-model-overhaul"
---


![2024-05-15-VitalikButerinproposesEthereumgasmodeloverhaul_thumbnail](/assets/img/2024-05-15-VitalikButerinproposesEthereumgasmodeloverhaul_thumbnail.png)

## 현재 이더리움 기반 거래에는 두 가지 가스 수수료가 있습니다: 거래 실행을 위한 것과 데이터 저장을 위한 것이 따로 있어요.

이더리움의 공동 창시자인 Vitalik Buterin은 새로운 이더리움 개선 프로토콜인 EIP-7706을 제안했는데, 이는 거래 호출 데이터를 위한 새로운 가스 모델에 초점을 맞추고 있어요.

현재 이더리움 기반 거래에는 거래 실행을 위한 가스 수수료와 거래를 위한 계산적 노력을 지불하는 데 필요한 가스와 데이터 저장을 위한 가스 수수료인 '데이터 덩어리' 저장 비용이 따로 있습니다.



Buterin의 EIP-7706은 이더리움 거래에서 중요 데이터가 전달되는 부분인 콜 데이터 전용으로 새로운 가스 형태를 제안합니다.

이것은 이더리움 블록체인이 거래 중 전송된 데이터에 대해 별도로 비용을 할당할 것을 의미합니다. 이는 계약 코드 실행 비용이나 데이터 저장 비용과는 별도로 처리될 것입니다.

새로운 계산 모델은 실행 가스, 덩어리 가스, 그리고 콜 데이터 가스에 대한 값 제공을 위해 max_basefee와 priority_fee를 벡터로 제공하는 거래 유형을 추가할 것입니다.

현재 기준 비용 조정은 거래 실행 비용과 덩어리 형태의 데이터 저장을 위해 별도 메커니즘을 사용하고 있습니다.



하지만 이미지가 추가되면 텍스트만 으로 작성하는 것이 좋습니다.



Buterin은 호출 데이터를 위해 별도의 가스 수수료를 도입함으로써 "블록의 이론상 최대 호출 데이터 크기가 크게 축소될 것이며, 기본적인 경제 분석은 평균적으로 호출 데이터가 상당히 싸게 되는 것을 시사한다"고 제안했다.

이더리움 네트워크는 속도 및 비용을 더 낮추기 위한 핵심 동기가 될 증명 과제와 증명 지분 간의 이동에 대한 가스 수수료 문제로 수 년간 고전해 왔다.

그러나 약속된 대로 네트워크의 확장성을 개선하지 못했으며, 후속 EIP가 네트워크를 돕고 있다.

잡지: 중요한 질문: 사토시 나카모토가 ZK-프루프에 대해 어떻게 생각했을까요?