---
title: "초기 이더리움 계약을 탐험하는 고고학 여행"
description: ""
coverImage: "/assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_0.png"
date: 2024-05-05 17:02
ogImage: 
  url: /assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_0.png
tag: Tech
originalTitle: "An Archeological Trip Across Early Ethereum Contracts"
link: "https://medium.com/etherscan-blog/an-archeological-trip-across-early-ethereum-contracts-232b0de33f8"
---


![An Archeological Trip Across Early Ethereum Contracts](/assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_0.png)

# 고대의 재미

인류 역사상 가장 이른 계약들은 수천 년 전 고대 메소포타미아에 있을지도 모릅니다. 아래는 익숙한 개념을 담고 있는 부동산 거래의 한 예시입니다:

Sini-Ishtar, Ilu-eribu의 아들, 그리고 형제인 Apil-Ili는 Sini-Ishtar의 집 옆에 위치한 Minani의 집 옆에 위치한 집과 함께 구축된 땅의 1/3을 구입했습니다. 그리고 Sini-Ishtar의 집 옆에 위치한 경작지의 1/3을 구입했습니다. 그러한 지역은 Sini-Ishtar의 집에 면한 거리에 있습니다. Migrat-Sin의 아들 Minani의 재산인 Minani로부터 구입했습니다. 그들은 합의된 가격으로 4.5 은로 지불했습니다. Minani의 집에 대한 추가적인 청구는 결코 없을 것입니다.



이러한 문서의 흥미로운 특징 중 하나는 평범한 것들을 되짚어주는 것입니다. 고대 이집트 기록들은 해독을 위한 큰 노력 끝에 가독성이 있게 되었는데, 이들은 독특하고 매혹적인 문화를 표현하지만 친숙한 형식으로 이루어져 있습니다: 계약서, 교육, 시, 개인 편지, 낙서 등이 그 예입니다.

![이미지](/assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_1.png)

현대 블록체인 시스템과 비교해보는 것은 흥미로운 점입니다. 분산 시스템의 코드는 미래에도 오랫동안 남아 있을 수 있으며, 그것은 원장에 남긴 문구처럼 우리의 인간적 성향을 비춥니다.

하지만 블록체인은 아직 20년도 안 되었습니다. 그럼에도 많은 블록체인 애호가들은 몇 년 전의 코드 샘플을 해독하고 조사함으로써 고대를 연상하는 "흥미"를 느낍니다. 이더리움에서는 지금 "역사적 NFT" 커뮤니티가 형성되어 이전 프로젝트를 면밀히 조사하며, 그것을 활성화하고 감상하며 투기하기 위해 노력합니다.



# 검증된 계약

작은 이야기를 살펴보는 한 가지 방법은 Etherscan의 초기 검증된 계약을 탐색하는 것입니다. 계약을 검증하면 사전 컴파일된 코드를 읽을 수 있습니다. 검증된 계약의 역사를 추적하면 새로운 미디엄이 사람들의 경험에 대한 익숙한 개념을 빠르게 표현하는 것을 보여줍니다. 이는 아마도 숨메르인과 이집트인들이 늘 고민했을, 인사와 메시지부터 부동산 시스템, 운명의 게임까지의 개념을 포함합니다.

하지만 새로운 미디엄은 새로운 메시지가 될 수 있습니다. 창의성은 독특한 문화에 대한 새로운 기회를 제공합니다. 블록체인에서 이 독특한 문화는 공개적 계산 시스템에서만 가능한 조합 가능한 기능을 구축하는 계약이 특징일 수 있습니다.

여기 이를 설명하는 초기 계약 배포들의 연속입니다.



## 2015년 8월 7일: "안녕 이더리움!"

Etherscan에서 확인된 가장 초기의 계약은 지갑 0x3d076에 의해 배포되었습니다. 이 간단한 계약은 "Hello Ethereum!"을 출력하는 go()라는 읽기 함수를 생성합니다.

![이미지](/assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_2.png)

우연히도, 이 지갑 0x3d076은 초기 이더리움 기여자인 "Linagee"로 알려져 있으며, 이제는 인간이 읽을 수 있는 문자열에 대한 조회 테이블을 구현하는 글로벌 레지스트리 계약으로 유명합니다. 이제 널리 사용되는 이더리움 네임 서비스와 마찬가지로, "takenstheorem"과 같은 훨씬 간단한 영숫자 시퀀스를 등록하고 사용할 수 있습니다. 이 계약은 이 문자열을 기억하기 어려운 주소를 가리킬 수 있습니다. 역사적인 NFT 애호가들은 최근 이전 계약 주변에 래퍼 함수를 구현했으며 레지스트리는 이제 OpenSea에서 널리 거래되고 있습니다. (이 글로벌 레지스트리 계약은 초기 이더리움 코드베이스의 일부로서 원래 Gavin Wood에 의해 작성되었을 수도 있음에 유의하십시오.)



## 2015년 8월 7일: Terra Nullius

![이미지](/assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_3.png)

Reddit에서 "테라 누리우스"는 사용자들에게 이더리움 역사에서 자신의 자리를 요구할 수 있는 기회로 소개되었습니다. 이 계약은 거래를 통해 체인상에 메시지를 "요구"할 수 있도록 허용합니다. 이러한 메시지들은 양도할 수 없기 때문에 요구자에게 "영혼에 묶인 토큰"과 같은 모습을 갖고 있습니다.

![이미지](/assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_4.png)



안녕하세요! 암호화폐 전문가 여러분. 지난 우리는 Linagee님께서 Terra Nullius에 작은 메시지를 남기셨습니다. NFT 커뮤니티에서는 2021년 9월에 Terra Nullius의 V2 버전을 만들었는데, 이 비교적 역사적이지 않은 재배치가 OpenSea에서 600 이상의 ETH 거래량을 기록했습니다.

![이미지 설명](/assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_5.png)

##2015년 8월 7일: 도박, 폰지 및 더 많은 것들

초기 확인된 계약들은 도박, 폰지, 그리고 관련 프로젝트로 가득 차 있습니다. "MySchema"라는 계약은 이와 같은 종류의 최초의 확인된 계약일 수 있습니다. 1이더 폰지 계획을 시행하는데, Reddit에서 2016년에 논의된 것입니다.



Provably fair, uncensorable, and permanent. The first financial contracts of their kind. Quite an achievement…for a Ponzi scheme. And they are still out there accumulating and distributing…

Interestingly, this old contract now holds over 9 ETH. This early engagement surely indicative of ETH at $3.

There are tons of these: SimpleLotto, LittleCactus, Goodfellas, ZeroPonzi, The10ETHPyramid, NoFeePonzi, CrazyEarning. It marks the association our industry has with speculation and financial play that is both tempting and unfortunate (games of fortune are likely older than recorded history).

[An Archeological Trip Across Early Ethereum Contracts](/assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_6.png)



## 2015년 10월 6일: 공공재!

지갑 0xd3cda는 "DateTime"이라는 계약을 생성했으며 유용한 타임스탬프 기능을 구현했습니다. 계약은 유닉스 타임스탬프(이 글을 쓰는 시간의 타임스탬프인 1669601568)를 인간이 읽을 수 있는 날짜로 변환하는 데 도움이 됩니다. 또한 읽기 쉬운 날짜에서 타임스탬프를 생성할 수도 있습니다. 이 계약을 생성하기 위해 배포자는 0.03 ETH의 가스를 소비했는데, 이는 배포 당시에 약 0.02 미국 달러와 같았습니다.

![AnArcheologicalTripAcrossEarlyEthereumContracts_7.png](/assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_7.png)

## 2015년 10월 9일: "Grove"



Grove by Piper Merriam was a project that provided openly stored and easily accessible organized data, making it a public good. While it was an interesting data experiment back in the days when gas prices were equivalent to $1 per ether, the project did not gain widespread adoption.

![AnArcheologicalTripAcrossEarlyEthereumContracts_8](/assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_8.png)

## October 19, 2015: A Virtual World

![AnArcheologicalTripAcrossEarlyEthereumContracts_9](/assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_9.png)



그 당시 복잡성의 이러한 벡터의 정점은 이더리아였습니다. 이더리아는 아마도 이더리움에서 최초의 진정한 NFT였으며, 높이 존중받는 ERC-721 표준 이전인 2015년 10월에 배포되었습니다. 래퍼(contract) 컨트랙트를 통해 이더리아는 OpenSea에서 거래될 수 있습니다. 이더리아의 원래 비전은 "플레이어가 타일을 소유하고 블록을 작물로 키우며 무언가를 짓는 가상 세계입니다. 전 세계의 상태는 유지되며 모든 플레이어 조작이 탈중앙화되고 신뢰할 수 있는 이더리움 블록체인을 통해 이루어집니다."

![이미지 10](/assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_10.png)

![이미지 11](/assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_11.png)

# 결론



작은 포스트를 시작하며 계약 역사가 일반적인 인간 경험을 반영하며 우리를 "고대인"에 연결한다고 설명했습니다. 이러한 사례는 이더리움 초기에 확인된 계약에서 많이 나타납니다. 예를 들어, 한 커플은 자녀의 이름을 선택하기 위해 온체인 투표 계약을 사용하기로 결정했습니다. 소프트웨어 개발자는 체인 상에 혼인 전 계약을 표현하기 위한 코드베이스를 작성했습니다. 심지어 유명한 DAO의 제안에도 인간의 호기심의 메아리가, 기본적인 지배 결정부터 체인상 장난스럽게 보이는 그래피티까지 있습니다.

![An Archeological Trip Across Early Ethereum Contracts](/assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_12.png)

이러한 관행은 때로는 농담의 대상이 되기도 합니다. "블록체인에 올려놓아"는 새로운 열성가들과 오리지널 그룹의 비판적인 한숨이기도 합니다. 그러나 이는 전혀 놀라운 것이 아닙니다. 우리는 이러한 것들을 인코딩하고 싶어합니다. 상형문자는 다양한 인간 메시지를 인코딩하는 데 상당한 시간과 노력을 투자하는 인간의 의지적인 예입니다. 

이더리움에서는 시간이 흐르면서 복합성 관행이 엄청난 복잡한 금융 시스템을 내놨습니다. 이는 DeFi 및 NFT에서도 마찬가지입니다. 예를 들어, 한 번 Avastars NFT가 그 시대를 대변할 수 있는지를 관찰한 적이 있었습니다:



Avastars are fascinating NFTs with intricate artwork, created on a blockchain network that used to have lower fees and Ethereum prices. They carry a sense of historical weight, like relics from the past, destined to impress anyone who delves into the contract's specifics.

It's a playful and somewhat nostalgic notion, appealing to our human curiosity.

![An Archeological Trip Across Early Ethereum Contracts](/assets/img/2024-05-05-AnArcheologicalTripAcrossEarlyEthereumContracts_13.png)

## Notes



- 약 30년 전 닉 살보(Nick Szabo)가 "스마트 계약"이라는 아이디어를 처음 제안했고, 비트코인 자체가 이제 여러 종류의 스마트 계약을 구현할 수 있습니다. 비트코인은 Script라는 제한된 스마트 계약 언어를 가지고 있습니다. 아마도 가장 유명한 비트코인 계약은 라이트닝 네트워크를 지원하는 것들입니다. 비트코인은 분명히 자체적인 흥미로운 역사를 제공합니다.
- "첫 번째 NFT"가 무엇인지에 대한 질문은 분명히 논란이 많은 가치 있는 질문입니다. 하지만 강요받는다면, 에테리아(Etheria)를 선택할 수 있을 것 같습니다. 왜냐하면 에테리아의 계약이 정말로 상당히 정교하며, NFT와 NFT 거래 도구로 생각하는 요소들을 많이 구현하고 있기 때문입니다.