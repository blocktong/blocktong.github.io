---
title: "제타체인 20 최초의 유니버설 블록체인"
description: ""
coverImage: "/assets/img/2024-05-16-ZetaChain20TheFirstUniversalBlockchain_0.png"
date: 2024-05-16 15:47
ogImage: 
  url: /assets/img/2024-05-16-ZetaChain20TheFirstUniversalBlockchain_0.png
tag: Tech
originalTitle: "ZetaChain 2.0: The First Universal Blockchain"
link: "https://medium.com/@zetablockchain/zetachain-2-0-the-first-universal-blockchain-8709116a411d"
---


# ZetaChain 2.0: The First Universal Blockchain

**TL;DR:** ZetaChain 2.0 is introducing potential upgrades to create the first Universal EVM for chain abstraction. Features include Universal Proof of Stake and Universal Apps, enabling seamless cross-chain interactions.

🚀 Today, we are thrilled to unveil ZetaChain 2.0 and the groundbreaking Universal EVM for chain abstraction. Developed by the decentralized ZetaChain community, these upgrades focus on the Chain Abstraction Framework (CAF). The goal is to open up new possibilities, such as Universal Proof of Stake and Universal Apps, streamlining interactions and asset management across different chains for users.

🔧 These proposed upgrades are currently in development within the ZetaChain community's public node repository. To be implemented, they must first be approved by community vote through on-chain governance. This post provides insight into the current status of the ZetaChain protocol, outlining the vision and the steps needed to realize ZetaChain 2.0 — the revolutionary Universal EVM for chain abstraction.

<div class="content-ad"></div>

# ZetaChain 1.0 개요

2024년 2월 1일부터 메인넷에서 운영 중인 ZetaChain 1.0입니다. 이 네트워크는 Bitcoin, Ethereum 및 BNB Chain을 연결하는 완전한 기능을 제공합니다. 개발자들은 ZetaChain의 EVM에 Omnichain 스마트 계약을 배포할 수 있으며 ZRC-20 프리미티브를 사용하여 토큰을 래핑하거나 잠그지 않고 모든 연결된 체인을 통해 가치와 데이터를 조정할 수 있습니다.

ZetaChain 네트워크는 시작된 지 세 달 만에 전체 활성 사용자 수로 상위 다섯 대의 블록체인 중 하나로 랭크되었으며 1억 건 이상의 거래가 생성되었습니다 (출처: DappRadar). 이 생태계는 인프라, DeFi, SocialFi, Gaming 등 다양한 분야에서 활동하는 250명 이상의 개발자와 파트너들로 구성되어 있으며, 5,500개 이상의 dApp 계약이 배포되었습니다.

지난달에 Ecosystem Growth Program을 출시하여, 공유되는 체인 추상화 및 네트워크 개발을 진화시키는 혁신적인 프로젝트를 수행하는 개발자들을 위한 지원금을 제공하기로 약정했습니다. 전체 ZETA 공급의 5%를 할당하였으며, 이 중 1% (21백만 ZETA)는 Bitcoin 네트워크를 잠금 해제하기 위해 ZetaChain을 사용하는 중요한 프로젝트에 할당됩니다.

<div class="content-ad"></div>

XP 프로그램은 생태계가 옴니체인 사용을 이해하고 지속 가능한 성장을 위한 보상을 삼각측량하는 데 도움이 되도록 구축되었습니다. 5월 1일은 XP의 첫 번째 단계 완료를 기념하는 날이었습니다. 생태계 성장 프로그램(총 공급의 5%)과 사용자 성장 풀(총 공급의 6%)은 XP 사용자 충성 프로그램 데이터를 활용하여 보조금 및 할당을 결정합니다. 새로운 개발, 보상 메커니즘, 그리고 보조금 할당이 진행 중이며 곧 업데이트를 통해 공개될 예정입니다.

# 유니버설 EVM 스택 소개

2.5년 전 ZetaChain은 첫 번째 유니버설 EVM 스택을 제안하여 원자 수준의 옴니체인 상호작용을 가능하게 했습니다. 유니버설 EVM를 통해 체인, 자산 및 데이터가 추상화되어 조율되고 통합되어 보다 이상적인 암호 사용자 경험을 제공합니다. ZetaChain 스택은 다중체인 생태계를 보는 새로운 프레임을 개척합니다. 이것이 바로 유니버설 EVM입니다.

![2024-05-16-ZetaChain20TheFirstUniversalBlockchain_1.png](/assets/img/2024-05-16-ZetaChain20TheFirstUniversalBlockchain_1.png)

<div class="content-ad"></div>

## 유니버설 앱

유니버설 EVM 상의 앱들은 사용자가 네트워크를 전환할 필요 없이 모든 연결된 체인에 네이티브하게 접근하고 접근 받을 수 있습니다. 완벽한 EVM 호환성을 갖추어 개발자들은 기존의 견고한 EVM 에코시스템 도구를 활용하고 기존의 Solidity 계약을 배포하여 유니버설 앱을 구현할 수 있습니다. 이는 모든 연결된 체인과 호환되며, 몇 줄의 코드로 작성된 Universal Apps를 구현할 수 있습니다.

## 체인 추상화 프레임워크 (CAF)

체인 추상화 프레임워크 (CAF)는 ZetaClient와 그의 Observer-Signer 노드로 이루어져 있으며, 네트워크의 옴니체인 연결성을 어떠한 블록체인에도 제공합니다. 이 프레임워크는 Universal EVM을 통해 접근할 수 있으며, 이는 어떤 체인에서도 호출될 수 있는 동기 환경으로 구성되어 있습니다. 이는 어느 체인에서든 네이티브 자산을 관리하고 다른 체인의 계약을 호출하기 위해 비동기 임의 메시징에 접근할 수 있습니다. CAF를 사용하여 개발하면 견고한 상태 관리 및 다중-레그, 다중-체인 앱의 요구 사항을 결합한 개발 환경을 구축할 수 있으며, 이는 친숙한 EVM 개발 환경에서 이루어집니다. 그 결과로 사용자 경험은 무제한으로, 거의 모든 앱이 어떤 단일 네트워크에서도 완전히 사용될 수 있으면서 앱 로직의 나머지 부분은 공정하고 안전하며 높은 성능을 갖춰 추상화될 수 있습니다.

<div class="content-ad"></div>

## 유니버셜 EVM (zEVM)

제타체인의 유니버셜 EVM 스택은 EVM 생태계가 지속적인 혁신을 경험하더라도( L2 스케일링 기술의 급부상, 리스테이킹, 새로운 EIP 등) 체인 단편화 딜레마를 충분히 해결하지 못한다는 개념에 바탕을 두고 있습니다. 제타체인의 유니버셜 EVM는 개발자들이 한 번 앱을 배포하고 모든 체인에 액세스할 수 있는 일반 목적의 플랫폼을 제공합니다. 유니버셜 EVM은 모든 종류의 체인(모듈식, 단일체, L2, 앱 특정) 사이의 연결 조직적인 구조를 제공합니다. 사용자들이 시작점과 여정을 어디서 시작하고 어디에서 끝내더라도 모두에게 암호화폐에 액세스할 수 있도록 합니다.

## 유니버셜 PoS / 합의

유니버셜 스테이크(Proof-of-Stake, PoS)는 제타체인 블록체인 네트워크를 지탱하고 시스템을 보호하며 거래를 확인합니다. 제타체인은 네이티브 코인 ZETA 외에도 BTC, ETH, BNB와 같은 다른 블록체인의 주요 암호화폐를 PoS를 통해 보호할 것입니다. 이러한 추가 네트워크와 자산은 제타체인의 보안 계층을 강화하며 네트워크 보호를 위한 블록 생성, 체인 간 거래 수수료 및 zEVM 가스 수수료를 통해 보상을 제공합니다.

<div class="content-ad"></div>

## 보편적인 체인 추상화 달성하기

유니버셜 EVM 스택의 이러한 구성 요소들은 비트코인, EVM, 이질적인 L2들, 또는 다른 곳에 있는 사용자라도 암호화폐 전체에서 추상화된 체인 UX를 지원하기 위해 함께 노력합니다. 체인 추상화 접근은 전문화된 레이어 및 모듈을 위한 모듈식 블록체인 논문을 더 확장하는 것을 목표로 합니다.

블록체인은 모듈식으로 구성될 수 있을 뿐만 아니라 모든 블록체인의 주요 가치 제안을 활용하는 유니버셜 앱 아래로 통합될 수도 있습니다. 더 지속적이고 유용한 플랫폼을 달성하기 위해 설계된 여러 하부 구성 요소를 갖춘 모듈식 체인과 유사하게, ZetaChain의 유니버셜 앱은 여러 체인의 주요 또는 최상의 구성 요소로 이루어질 수 있습니다.

미래로 나아가면 유니버셜 앱은 계속하여 모든 새로운 체인 연결 업그레이드와 내재적으로 호환성을 유지합니다. 유니버셜 체인 추상화는 ZetaChain 네트워크의 핵심 목표로, 블록체인 기술을 사용자 경험으로부터 추상화하는 컴포저블하고 지속 가능한 방법을 제공합니다.

<div class="content-ad"></div>

# ZetaChain 2.0

ZetaChain 2.0은 Universal EVM Stack의 현재 기능을 확장하는 제안된 업그레이드 시리즈입니다.

## 비기술 개요:

- ZetaChain의 메시징 및 자산 관리 기능이 합성 가능해져 더 복잡한 다단계 거래도 최종 사용자를 위해 추상화될 수 있습니다.
- ZetaChain의 Universal 앱은 사용자를 대신하여 여러 체인에서 자산을 합성할 수 있어, 최종 사용자는 다양한 체인에서 다양한 앱을 이용할 수 있으면서도 ZetaChain의 단 하나의 앱과만 상호 작용하면 됩니다.
- Universal Proof-of-Stake는 BTC, ETH, BNB 등 네이티브 자산을 ZetaChain의 PoS에 스테이킹할 수 있는 기능을 제공하여 네트워크의 보안을 더욱 강화하고 그에 대한 보상을 받을 수 있습니다.
- 비트코인 호환성, IBC, 그리고 추가 체인 통합을 통해 더 많은 사용자와 지갑이 Universal 앱에 접속할 수 있습니다.

<div class="content-ad"></div>

이러한 주요 기능 제안에 대한 자세한 개요는 오픈 소스 저장소에서도 확인할 수 있습니다.

## 크로스체인 메시징

![2024-05-16-ZetaChain20TheFirstUniversalBlockchain_2.png](/assets/img/2024-05-16-ZetaChain20TheFirstUniversalBlockchain_2.png)

이 기능 제안은 ZetaChain을 통해 다중 레그 트랜잭션을 조립 가능하게 하고, ZRC-20 기본 요소를 메시징 기능과 병합합니다. 메시징 능력을 Omnichain Smart Contracts와 조합함으로써 애플리케이션은 네이티브 자산 이동 및 상호 작용과 함께 다중 레그 크로스체인 트랜잭션을 추상화할 수 있습니다. 예를 들어, ZetaChain의 계약은 Bitcoin에서 사용자에 의해 호출될 수도 있고, 동시에 Ethereum 및 BNB Chain에서 외부 계약 호출을 실행하고, 그 외의 레그까지 — 모두 사용자에게 단 한 단계로 이루어질 수 있습니다.

<div class="content-ad"></div>

이 기능 개발은 완료되어 현재 심사를 받고 있으며, 다음 업그레이드(v16)를 통해 거버넌스 제안을 거쳐 거버넌스의 일부가 될 예정입니다.

## 옴니체인 계정

![이미지](/assets/img/2024-05-16-ZetaChain20TheFirstUniversalBlockchain_3.png)

본 기능은 ZetaChain의 계약이 원천적으로 연결된 다른 체인의 계약과 상호작용하고, 사용자 계정을 대신하여 자산을 원활하게 이동할 수 있는 기능을 제공합니다. 이는 옴니체인 스마트 계약의 기능을 확장하여, ZRC-20의 출금 가능력에 withdrawAndCall을 추가하여 달성됩니다.

<div class="content-ad"></div>

Omnichain Accounts와 Native Asset Composability를 활용하면 사용자를 대신하여 모든 연결된 체인에서 기회를 집곅하고 최적화할 수 있는 응용 프로그램을 개발할 수 있습니다. ZetaChain의 dApp과 단일 상호 작용만으로도 이 기능을 활용할 수 있습니다. ZRC-20 기본 기능을 확장한 앱은 사용자가 네트워크를 전환하거나 추가 트랜잭션을 서명할 필요가 없는 경험을 구축할 수 있습니다.

이 기능 제안은 ZetaChain이 제공하는 일반적인 omnichain composability를 확장하여 수익 집곅기, 포트폴리오 관리 도구, 멀티체인 자동화 등을 더욱 간단히 구축할 수 있게 합니다. Universal EVM이 이를 가능하게 하여 여러 체인 자동화 규칙을 사용한 탈중앙화 앱을 구축할 수 있습니다.

이 기능 제안에 대해 더 자세히 알아보세요.

## Universal PoS, enabling Bitcoin, Ethereum, BNB Staking

<div class="content-ad"></div>

![ZetaChain](/assets/img/2024-05-16-ZetaChain20TheFirstUniversalBlockchain_4.png)

Hey there! ZetaChain is rocking a Proof-of-Stake consensus mechanism secured by ZETA. And get this, the latest feature proposal suggests diving into staking ZRC-20 assets like ETH, BNB, Etc., creating a hybrid staking bonanza at the heart of ZetaChain's core consensus layer. By introducing this new mechanism, the network's security and utility can skyrocket, especially with the integration of more assets whitelisted via ZRC-20.

Even if it's just Bitcoin in the mix for now, ZetaChain's network can tap into the vast pool of over a million unique Bitcoin miners, fortifying the Universal EVM. This cool proof-of-stake model goes all out to consolidate the network's security with fresh native assets on ZetaChain.

Looking further ahead, this feature proposal also hints at the potential for liquid staking and restaking foreign assets like BTC, an innovative move to beef up security for additional protocols. It's all about pairing liquidity directly with network security. Want to dive deeper into how ZetaChain is shaping up? Learn how it's paving the way for crafting protocol mechanisms akin to EigenLayer and Babylon Chain through Omnichain Smart Contracts. For more juicy details, you can check out the proposal [here](link).

## Bitcoin Compatibility

<div class="content-ad"></div>

비트코인은 ZetaChain의 주요 연결고리입니다. ZetaChain의 Universal EVM과의 더 나은 체인 추상화 및 지갑 호환성을 허용하기 위해 네트워크는 다음 Bitcoin 스크립트 유형을 지원해야 합니다: Pay-to-Witness-Public-Key-Hash (P2WPKH), Pay-to-Taproot (P2TR), Pay-to-Public-Key-Hash (P2PKH), Pay-to-Script-Hash (P2SH), Pay-to-Witness-Script-Hash (P2WSH). 이를 통해 Taproot, Segwit 및 Legacy 주소를 지원함으로써 Bitcoin 사용자와 지갑에 더 많은 커버리지를 제공할 수 있습니다.

이 기능 개발은 완료되었으며 현재 감사를 받은 후 다가오는 업그레이드(v16)를 통해 제안을 위해 거버넌스를 통해 움직이고 있습니다.

## 추가 확장 및 개발

노드 저장소는 매우 활발하게 개발 중입니다. 수십 명의 기여자들이 있고 계속 증가 중이며, 이미 개발된 키 컴포넌트나 개발 중인 다른 몇 가지 중요한 구성 요소가 있습니다:

<div class="content-ad"></div>

- 이더민트 EVM 모듈의 포킹 및 확장을 통해, zEVM 계약을 호출하거나 상태를 변경하는 교차 체인 거래의 RPC 호환성을 개선합니다.
- 다른 Cosmos SDK 모듈과 Ethermint/EVM의 통합을 높이기 위해 Cosmos SDK 모듈의 데이터 및 기능을 EVM 계약 인터페이스에 노출시킵니다.
- ZRC-20 또는 ZETA 토큰의 이상적이지 않은 흐름을 방지하는 가변 쓰로틀 메커니즘을 허용하는 속도 제한기를 도입합니다.
- 이더리움을 통한 ZETA 공급 상한선 조정을 통해 멀티체인 ZETA 토큰의 전반적인 보안 프로필을 개선합니다.
- 코드 커버리지 및 최적화/리팩터링을 통해 신규 기능의 개발 및 테스트를 모듈화하고 개선하여 모든 기여자가 더욱 원활하게 기여할 수 있도록 합니다.

협력을 위한 더 많은 문제와 아이디어가 있습니다. 관심 있는 개발자라면 어떤 기여라도 가능하며, 업그레이드를 위한 투표를 통해 승인될 수 있습니다. 노드 저장소는 여기에서 확인할 수 있습니다.

# Chain Support

![ZetaChain 20 - the First Universal Blockchain](/assets/img/2024-05-16-ZetaChain20TheFirstUniversalBlockchain_5.png)

<div class="content-ad"></div>

ZetaChain의 아키텍처는 Chain Abstraction Framework에 새로운 체인 통합을 추가할 수 있도록 합니다. 체인 통합은 거버넌스에서 제안되고 투표를 통해 소프트웨어 업그레이드를 통해 이루어집니다. 현재 기여자들은 아래 사항을 조사하고 있습니다:

- Base — [링크](https://github.com/zeta-chain/node/issues/2161)
- Polygon PoS/ZK L2 — [링크](https://github.com/zeta-chain/node/issues/2162)
- Solana — [링크](https://github.com/zeta-chain/node/issues/2165)
- IBC (자세한 내용은 아래 참조) — IBC omnichain 통합 · Issue #2108 · zeta-chain/node (github.com)

주의: ZetaChain의 Chain Abstraction Framework에서 구축된 dApp은 자동으로 새로운 체인 통합과 호환되게 됩니다. 새로운 체인 통합을 위한 모든 제안을 환영합니다. ZetaChain 생태계에 미치는 새로운 체인의 영향을 조사하고 분석하기 위해 블록체인 전문가가 되어야 할 필요는 없습니다. 누구나 Github에 토론을 위한 이슈를 게시하여 참여할 수 있습니다.

## IBC를 전송 계층으로 추가하기

<div class="content-ad"></div>

IBC support를 지원하는 것은 IBC 호환 체인 우주로부터 데이터를 운송하는 새로운 방식을 소개하며, ZetaChain의 Chain Abstraction Framework에 새로운 잠재력 있는 유틸리티를 제공합니다. IBC를 통합함으로써 외부 자산을 위한 ZRC-20 원시물은 ibc-transfer와 통합되며, 크로스체인 트랜잭션은 ZetaChain 개발자들에게 간단한 인터페이스로 유지되면서 통합된 IBC 체인으로 확장될 수 있습니다.

이동 레이어로 IBC를 추가하면 IBC 체인에서 옴니체인 스마트 계약 호출이 가능해질 것입니다. 이 기능 제안에 대해 더 알아보세요.

주의: BTC, ETH 및 BNB와 같은 ZETA 자산을 IBC 체인으로 출금하려면 별도의 기능이 필요합니다. 이 제안된 기능은 현재 ZetaChain 사용자 커뮤니티에 의해 조사 중입니다.

# ZetaChain 2.0 요약하기

<div class="content-ad"></div>

### ZetaChain 네트워크의 핵심 목표는 Universal Chain Abstraction입니다. ZetaChain 2.0에서는 Chain Abstraction Framework (CAF)에 중요한 업그레이드를 제안하여 Universal POS Staking 및 유저를 위해 외부 체인의 앱을 관리하고 한 번의 클릭으로 다중 스텝 크로스체인 트랜잭션을 시작할 수 있는 Universal Apps와 같은 혁신적인 기능을 활성화할 계획입니다. 이러한 발전들은 모든 사용자가 어디에서 시작하든 자신의 여정을 보낼 수 있도록 암호화폐를 모두 접근 가능하게 만들기 위한 것입니다. 이러한 제안에 대한 커뮤니티의 투표에 대한 참여를 기대합니다.

## BTC 생태계 성장 프로그램 신청서

BTC 생태계 성장 프로그램은 현재 접수 중입니다. 프로젝트는 ZetaChain 개발 그랜트를 방문하여 그랜트 신청서를 완료할 수 있습니다.

# ZetaChain 소개

<div class="content-ad"></div>

ZetaChain은 간단하고 빠르며 안전한 옴니체인 블록체인입니다. 체인 추상화 개념을 구현하는 선구자로서, ZetaChain은 탈중앙화된 인터넷의 기초층 역할을 합니다. ZetaChain의 옴니체인 스마트 계약을 통해 개발자들은 이더리움부터 비트코인을 포함한 기존 블록체인을 횡단하는 진정한 상호 운용성 dApp을 구축할 수 있습니다. 미래로 확장되는 새로운 블록체인 혁신을 포함하여 모든 체인에서 암호화폐에 액세스할 수 있습니다. ZetaChain의 미션은 전 세계적인 액세스, 간편성 및 유틸리티를 제공하는 플랫폼을 구축하는 것입니다.

Twitter에서 ZetaChain을 팔로우해보세요: @zetablockchain, Discord와 Telegram에서 대화에 참여하세요. ZetaChain 상에서 구축 중이라면 partnerships@zetachain.com으로 연락해보세요.

어떤 프로젝트든 제3자의 것이며, ZetaChain의 것이 아닙니다.

면책사항: 위 내용은 ZetaChain과 ZETA에 관한 현재의 생각과 모델입니다. ZetaChain 메인넷 베타 및 네트워크의 모든 업그레이드는 성공적인 거버넌스 투표를 통해만 활성화될 수 있습니다.