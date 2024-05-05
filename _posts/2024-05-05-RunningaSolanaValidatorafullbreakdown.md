---
title: "솔라나 밸리데이터 운영 완벽 분석"
description: ""
coverImage: "/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_0.png"
date: 2024-05-05 16:06
ogImage: 
  url: /assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_0.png
tag: Tech
originalTitle: "Running a Solana Validator: a full breakdown"
link: "https://medium.com/@apfikunmi/running-a-solana-validator-a95cdfd6488a"
---


"내가 배우고 있는 내용을 기반으로 하면, 모든 것을 제대로 한다면 새로운 검증자는 재담투자 없이도 약 10~20 에포크 내에 수익을 내게 될 것입니다." — SolDev

솔라나의 탈중앙화는 논란이 많은 주제입니다.

노드를 운영하는 데 필요한 하드웨어 및 대역폭 요구 사항은 종종 솔라나를 허가된 것으로 묘사하거나 중앙 집중화된 것처럼 비친되기도 합니다.

또한, 검증자를 운영하고 얼마나 수익성이 있는지는 모호한 부분이 있습니다.



이 기사는 솔라나( Solana) 검증자( validator)를 운영하는 복잡한 점들을 밝혀 명확성을 제공하기 위한 것입니다.

이 기사를 마칠 때, 당신은:

- 검증자가 무엇이며 어떤 역할을 하는지 이해하게 될 것입니다.
- 자체 검증자를 운영하는 것과 관련된 경제적 측면, 혜택 및 도전과제를 이해하게 될 것입니다.
- 검증자를 운영하는 것의 짧은, 중기 및 장기적 현실을 이해하게 될 것입니다.

검증자를 온라인으로 운영하는 방법에 대한 프로세스에 대해서는 상세히 다루지는 않겠지만, 관심 있는 분들을 위한 자료 링크를 제공할 것입니다.



이 기사는 블록체인 개념, 특히 합의 및 Proof-of-Stake에 대한 중간 수준의 이해를 전제로 합니다. 해당 개념에 대한 충분한 이해를 시작하고 싶다면 여기를 참고해주세요.

## 개요

검증자란 무엇이며 무엇을 하는가?

검증자 운영



Validator Economics

효율적인 검증기 운영하기

결론

# 용어들



시작하기 전 몇 가지 용어가 주기적으로 등장할 것이기 때문에 미리 알아두는 것이 좋습니다:

- 노드 (Node): 블록체인 프로그램을 실행 중인 컴퓨터.
- 포크 (Fork): 아직 주요 체인이 아닌 블록체인의 버전.
- 슬롯 (Slot): 하나의 블록이 생성되는 시간 단위로, 블록 시간(Block Time)이라고도 불립니다.
- 에포크 (Epoch): 432,000 연속 슬롯으로 이루어진 블록. 단순히 432,000 슬롯이 아니라 구분된 블록들로 이루어진 것을 의미합니다.
- 에포크-년 (Epoch-year): 365일에 대한 에포크의 수(400ms 슬롯 시간으로 계산). 정확히 182.5
- 리더 (Leader): 그 슬롯에 대한 블록을 생성하는 검증자(Validator).
- 클라이언트 (Client): 클라이언트는 블록체인 프로그램의 버전을 가리킵니다. 기본 클라이언트는 원본 프로그램입니다. 다른 클라이언트는 동일한 기능을 다른 언어로 구현하거나 일부 기능을 변경하여 동일한 결과를 얻을 수 있습니다.

그럼 이제 시작해볼까요?

# 검증자(Validator)란 무엇이며 무슨 일을 하는 걸까요?



**Validator** is a term in PoS that refers to a node taking part in blockchain consensus. 

In PoS, voting nodes stake tokens as collateral, which can be confiscated or lost if they act maliciously. 

Voting nodes have the following responsibilities:

- Generating blocks when they are the leader.
- Validating and voting for or against blocks when they are not the leader.



Validators는 효과적으로 무엇이 진실인지 결정하는 네트워크에서 중요합니다. Validators의 수가 많을수록 네트워크는 보안 및 분산화가 증가합니다. Stake 분포도 매우 중요합니다. 하지만 아마 당신에게는 이 모든 것이 새로운 정보가 아닐 것입니다.

이제 왜 우리가 여기 있는지 살펴봅시다.

# Validator 운영

더 나아가기 전에, Solana에서의 validators와 RPC 노드 간의 차이점을 명확히 해야 합니다.



이상적인 세상에서는 블록체인을 사용하는 모든 사람이 자체 노드를 운영할 것입니다. 이러한 노드들은 다른 노드에 연결되어 사용자의 거래를 전파하고 블록체인이 진실임을 검증할 것입니다.

하지만 실제로는 기술적으로나 재정적으로 노드를 운영하는 것이 어렵기 때문에 이러한 일이 발생하지 않습니다. 대부분의 사람들은 RPC 노드를 통해 블록체인과 상호작용합니다.

RPC 노드는 두 가지 기능을 합니다:
- 투표된 포크가 진실인지 확인합니다
- RPC 노드는 다른 블록체인 노드(검증자도 포함)에 연결되어 거래를 처리하고 블록체인의 상태에 대한 데이터를 제공할 수 있습니다. 블록체인을 사용할 때마다 RPC 노드를 사용한 적이 있을 것입니다.



이 기사는 주로 검증자에 초점을 맞추고 있습니다. RPC 노드를 운영하는 경제는 불명확합니다.

dApps(지갑과 같은)은 자체 RPC 노드를 운영하거나 Helius와 같은 엔티티에게 RPC 노드 사용료를 지불합니다. 그러나 이러한 약정은 오프체인 상에서 이루어집니다.

Helius의 가격을 확인할 수는 있지만 실제 운영 비용이나 수요에 대해 거의 알 수가 없습니다. 그래서 RPC 제공업체들이 얼마나 많은 이익(또는 손실)을 얻는지 말할 수가 없습니다.

이에 따라 앞으로는 검증자에만 주로 초점을 맞출 것입니다.



암호화폐에 흥미를 느끼는 분들을 위해, 위 글의 끝에 있는 Solana Tech 디스코드에는 ann RPC 노드 운영자 채널이 있습니다. 거기서 더 많은 질문을 할 수 있습니다.

# 그래서, 발리데이터를 운영하기 위해 어떤 것이 필요한가요?

발리데이터 운영자의 주요 업무(셋업을 완료한 후)는 자신의 노드를 모니터링하고 성능을 향상시키는 것입니다.

발리데이터가 필요로 하는 주요 두 가지가 있습니다:



- 기술 전문 지식,
- 자금.

그 이외에는 모두 보조적인 것입니다.

기술 전문 지식은 매우 중요합니다. 곧 알게 되겠지만, 발리데이터를 운영하는 것은 기술적으로 많이 요구됩니다. 성공적으로 진행하려면 하드웨어, 데브옵스, 블록체인 아키텍처 및 백엔드 개발을 이해해야 합니다.

이러한 것들을 이해하지 않고도 노드를 실행할 수는 있지만, 심지어 가장 "기본적인" 것들에도 어려움을 겪을 것입니다.



요즘 많은 사람들이 하드웨어 비용을 두 배 이상 지불하면서도 노드를 설정할 때 문제에 직면하는 것을 경험하고 있어요. 그때 그 이유가 설정 자체가 제대로 이뤄지지 않았기 때문이었어요.

![Solana Validator Full Breakdown](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_0.png)

저희가 앞으로 계속 이야기할 때 이와 유사한 예시들이 많이 나오게 됩니다. 반드시 명심해 두세요, 노드를 운영하는 것은 기술적으로 많은 요구가 필요하다는 거.

기술적 전문 지식 뿐만 아니라, 배리데이터를 운영하는 것은 비용이 많이 들 수 있다는 점도 명심해야 합니다.



자세히 살펴보겠습니다.

## 노드 요구 사항

밸리데이터를 실행하려면 하드웨어, 대역폭, 스테이크 및 투표 SOL이 필요합니다.

## 하드웨어 및 대역폭



솔라나 랩스 공식 문서에서 추천하는 사양입니다:

![Solana Validator](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_1.png)

대역폭: 1 GBit/s 대칭, 상업용. 10 GBit/s 권장 사양.

실제로 대부분의 검증자는 약간 더 강력한 기기를 사용하지만, 지나치게 과도한 사양은 혜택이 없습니다.



# 스테이킹

솔라나에서는 최소 스테이크가 없으며 네이티브 델리게이션을 지원하여 발령자가 보유한 SOL이 모두 필요하지는 않습니다.

하지만 많이 스테이크할수록 리더로 선출될 가능성이 높아집니다. 목표로 설정해야 하는 구체적인 금액은 경제학 섹션에서 논의될 예정입니다.

# 투표 SOL



On Solana, votes count as transactions, and a validator is expected to vote on every block. This translates to approximately 1 SOL per day, but we'll delve into all the intricacies in the economics section.

With these four components, anyone can operate their own validator.

Now, onto the burning question: How lucrative is it?

To lay the foundation for validator economics, let's take a brief glance at a simple summary of Solana's tokenomics.



# Solana의 토큰 업데이트

Solana의 토큰 업데이트는 이더리움과 비슷합니다; 새 토큰을 만들기 위한 인플레이션이 있고, 그것들을 파괴하기 위한 연소가 있습니다. 세부 사항은 조금 다릅니다.

# Solana의 인플레이션

블록이 생성될 때, SOL이라는 일정량의 토큰이 블록 보상으로 생성됩니다.



현재 인플레이션율에 따라서 정확한 금액이 달라집니다.

인플레이션은 8%에서 시작하여 매 에포크 년도마다 15%씩 감소하도록 설계되었습니다. 매 에포크마다 조정되며 터미널 인플레이션 비율인 1.5%에 도달할 때까지 계속 감소할 것입니다.

![이미지](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_2.png)

이번 에포크의 인플레이션은 5.466%입니다 (2024년 2월 20일).



Solana CLI에서 solana inflation 명령을 실행하면 현재 숫자를 얻을 수 있습니다.

![Running a Solana Validator: a full breakdown](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_3.png)

이 인플레이션은 현재 새로운 SOL 토큰을 만드는 유일한 메커니즘입니다.

사이드바: 인플레이션이 혼란스러울 수 있습니다. 토큰 보유자가 희석된다고 생각되지만, 사실이 아닙니다. 인플레이션은 비스테이커들이 네트워크를 안정화하기 위해 스테이커들에게 지불하는 가격으로 생각할 수 있습니다. 스테이커들이 희석되는 것은 아니고, 비스테이커들만 희석됩니다. 이것은 단순히 탈중앙화의 비용일 뿐입니다. 게다가 이더리움도 유사한 시스템을 운영하고 있습니다.



# Solana의 소각

Solana의 현재 소각 메커니즘은 매우 단순합니다: 수수료의 50%가 소각됩니다. SIMD-0096은 우선 순위 수수료 소각을 중지시킬 것이지만 현재는 수수료의 50%가 소각됩니다.

나머지 50%는 해당 슬롯의 리더에게 보내집니다.

여기까지입니다.



이제 Solana의 경제를 분석했으니, 다음으로 넘어가서 여기 있는 이유를 살펴보죠: 검증자 경제학.

## 검증자 경제학

이것 또한 두 부분으로 나눠질 것입니다: 비용과 수익

## 비용



# 하드웨어 비용

만약 컴퓨터 하드웨어에 익숙하다면, 이미 추천된 하드웨어의 값어치에 대해 대략적인 감을 가지고 있을 것입니다.

검증 노드는 항상 활성화되어 있어야 하기 때문에 실제로 더 비싸게 나옵니다.

컴퓨터를 모니터링하는 전용 전원 및 냉각 시스템, 백업 및 교체 구성 요소, 그리고 상시 근무하는 스태프가 필요하기 때문입니다.



모든 이러한 이유로 집에서 자신의 밸리데이터를 운영하는 것은 비현실적입니다.

이 문제를 해결하기 위해 대부분의 밸리데이터 운영자들은 노드에 적합한 메탈 서버를 Tier 3 데이터 센터로부터 사용합니다. 위치에 따라 가격이 크게 다를 수 있지만, Latitude는 현재 요금이 월 373달러부터 608달러 사이인 머신을 제공합니다.

![Bandwidth Costs](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_4.png)

# 대역폭 비용



대역폭 소비량을 추정하기 어려운 이유는 지분 크기에 따라 다르기 때문이에요.

만약 자주 리더가 되는 경우에는, 리더가 적은 경우보다 더 많은 데이터를 보내야 할 거에요. 하지만 평균적인 검증자는 보통 매달 60-100TB의 데이터를 처리하게 됩니다.

이것이 데이터 센터를 사용하는 또 다른 이유에요. 만약 집에서 이러한 대역폭을 원한다면, 아마 ISP로부터 전용 라인을 이용해야 할 것 같아요.

집에서 시도한 누군가가 문제를 겪고 있다는 이유가 여기 있어요:



![Solana Validator](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_5.png)

데이터 센터 비용은 제공업체 및 위치에 따라 다릅니다.

Latitude는 지역에 따라 egress당 0.64~3.60 미국 달러의 요금을 부과합니다. Ingress는 무료입니다.

한 달에 60~100 TB를 예상하면 총 비용은 월 38.4~360 미국 달러가 됩니다.



# 투표 비용

솔라나(Solana)에서는 투표가 거래로 계산됩니다.

각 밸리데이터는 한 슬롯 당 한 번 투표할 것으로 예상됩니다.

각 투표 비용은 0.000005 SOL입니다. 한 시대(epoch)에는 2.16 SOL이 됩니다.



일년에 그러면 394.2 SOL이 되고 하루에 약 1.08 SOL 정도인거죠.

실제로는 시간 슬롯이 정확히 400 ms가 아니기 때문에 이 숫자는 약간 더 낮습니다. 그러나 현재로서는 하루에 투표하는 데 약 1 SOL이 소요됩니다.

투표 비용은 검증자를 운영하는 데 가장 중요한 비용으로, 이를 피할 방법은 없습니다.

매 올바른 투표는 검증자에게 신용을 얻게 됩니다. 각 시대의 끝에서, 시대를 위한 인플레이션 보상은 참여하는 노드들 사이에서 분할되는데, 이 기준은 다음과 같습니다:



- 그들이 보유한 총 크레딧 수,
- 그리고 총 스테이크에 대한 그들의 지분 가중치입니다.

따라서 노드가 투표를 하지 않으면 에포크 끝에 보상을 놓치게 될 거에요.

노드들에게 투표를 장려하는 것은 중요한데, 그렇지 않으면 합의가 멈춰버릴지도 몰라요.

Toly는 투표 트랜잭션이 실제 SOL을 필요로 하는 이유에 대해, 이것은 일종의 최소 스테이크 요구 사항이라고 말합니다.



![image](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_6.png)

커뮤니티에서는 이 비용이 중앙 집중화 벡터를 만든다는 것에 합의하고, 그 효과를 줄일 제안된 변화가 있지만, 우리는 이후에 이에 대해 논의할 것입니다.

# 기회 비용

스테이킹 토큰은 그들의 기능성을 상당부분 가져가고, 앞에서 언급했듯이, 발레이더를 유지하는 것은 풀타임으로 하는 일입니다.



LSTs, 스테이크 풀 및 Sanctum과 같은 프로토콜이 금융 기회 비용을 줄이도록 노력하고 있습니다. 하지만 앞으로 발전할 여러분, 특히 시간이란 소중한 자원을 투자하게 될 것을 인지해야 합니다.

그것들이 바로 Solana 검증자를 운영하는 데 따르는 비용입니다.

이제 좋은 내용으로 넘어가보겠습니다:

# 소득



모든 검증자는 두 가지 주요 수익원과 두 가지 (선택적) 수익원을 가지고 있습니다. 이는 검증자가 운영 중인 클라이언트에 따라 달라집니다.

# 수수료

솔라나는 네이티브 위임형 POS를 운영하므로 검증자는 일반적으로 자신의 모든 지분을 소유하지 않습니다. 그러나 인플레이션 보상은 지분 소유자에게 속하므로 검증자는 대부분의 인플레이션 보상을 소유하지 않는 경우가 많습니다.

수수료는 검증자가 자신의 작업에 대해 위임자들에게 청구하는 금액을 의미합니다.



이 수수료는 현재 소규모 검증자들의 주요 수익원입니다.

## 거래 수수료

이미 언급했듯이, 블록에서 발생한 모든 거래 수수료의 절반은 소각되며, 나머지 절반은 해당 슬롯의 리더에게 속합니다.



![Solana Validator](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_7.png)

# MEV Bribes (Jito-Client)

솔라나의 네이티브 블록 빌더인 스케줄러는 부분적으로 무작위입니다. 먼저 들어온 순서(First-In, First-Out, FIFO)나 우선순위 수수료에 기반한 것이 아닙니다.

우선순위 수수료는 거래 포함의 가능성을 높일 수 있지만 순서를 제어하지는 않습니다. 다시 말해, 가장 높은 우선순위 수수료를 지불한다고 해서 거래가 가장 먼저 처리된다는 것을 의미하지는 않습니다.



Solana에서 트랜잭션 처리에 있어 스팸 공격은 특히 MEV 캡처와 같이 시간적 제약이 있는 트랜잭션에 더 효과적인 것으로 나타났습니다. 이에 더해 수수료가 매우 저렴하다는 점이 그 효과를 더 높이는데 일조했습니다.

이 문제의 심각성을 짚어보면, Jito는 무효화된 어빠레이지 트랜잭션으로 블록 공간의 58%가 채워졌다고 추정했습니다.

JitoLabs은 이를 해결하기 위한 솔루션을 제안했습니다. 어떻게 작동하는지는 여기에서 확인할 수 있습니다.



JITO 클라이언트를 실행하면 특정 거래를 처리하기 위해 뇌물을 받을 수 있습니다.

JITO 뇌물은 오프체인에서 이루어지기 때문에 일반 수수료로 간주되지 않으며, MEV 뇌물에서 모든 수수료는 검증자에게 속합니다.

![image description](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_9.png)

이것들이 검증자의 수입원입니다.



밸리데이터는 스스로에게 MEV를 빼앗을 수도 있지만, 그렇게 한다면 발견되면 블랙리스트에 올라갈 가능성이 높습니다.

이 모든 것이 수익성에 어떤 영향을 미칠까요?

## 그것은 달려있습니다

밸리데이터 경제를 이해하는 가장 큰 문제는 다양한 변수의 수입니다.



만약 수익에 관한 방정식을 작성해야 한다면, 다음과 같을 것입니다:

![equation](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_10.png)

위 방정식에서 유일한 상수는 하드웨어 및 대역폭 비용이며, 심지어 이러한 비용들도 제공업체 및 위치에 따라 다양합니다.

다른 구성 요소들은 예측하기 어렵습니다.



- 효과적인 프로그램 실행과 관련된 노드 성능에 따라 인플레이션 보상이 적립됩니다.
- 트랜잭션 수수료는 네트워크 사용량과 노드의 성능에 따라 달라집니다.
- 투표 비용은 현재 하루에 약 $110이 소요되지만, 정확히 1년 전에는 하루에 $26이었습니다.

결국 수익을 확신하기 어려운 점입니다. 따라서 이 수치들을 신중하게 고려해야 합니다.

현재 시장 상황에서, 100% 수수료 프라이빗 발리데이터는 평균 성능으로 5100 SOL 스테이킹이 필요합니다.

Jito 클라이언트를 실행한다면 약 5,000 SOL이 필요합니다.



물론, 이는 연간 시간, 토큰을 잠그는 기회 비용 또는 변수 변화와 같은 무형적인 요소를 고려하지 않았습니다.

평균적인 검증자가 오늘날 이득을 내기 위해서는 5% 수수료로 80k SOL을 위임해야 한다고 추정됩니다.

![이미지](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_11.png)

다양한 변형을 시도하여 다른 조건하에서 어느 정도의 지분이 필요한지 알아낼 수 있는 Cogent의 이윤 계산기를 사용해보세요. 정말 멋진 도구입니다.



가장 중요한 점은 충분한 스테이크를 올릴 수 있다면 수익을 올릴 수 있다는 것입니다. 

하지만 출연 스테이크를 늘리고 수익성 있는 검증자를 운영하기 위해서는 어떻게 해야 할까요?

# 수익성 있는 검증자 운영

지금쯤이면 아시다시피, 수익성 있는 검증자를 운영하는 것은 어려운 과제이며 복잡한 작업입니다. 이탈로의 말이 이를 가장 잘 설명해주는 것 같아요.



![Running a Solana Validator a full breakdown](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_12.png)

Setting up the validator is one thing, but the key question is: How exactly do you increase your stake?

## Increasing Stake

Apart from self-staking, there are four ways to increase stake on Solana: organic growth, stake pools, the Solana Foundation delegation program, and the Jito delegation program.



- 유기적 스테이킹

유기적 스테이킹은 솔라나 사용자를 직접 위임받는 것을 말합니다. 큰 검증자들이 더 높은 APY를 제공하고 신뢰성이 높기 때문에 처음 시작하기는 매우 어렵지만, 모든 수준의 검증자에게 절대적으로 필요합니다.

2. 스테이크 풀

Marinade 및 Blaze와 같은 프로토콜은 스테이크 풀을 운영합니다. 스테이크 풀은 토큰을 수집하고 알고리즘을 사용하여 네트워크의 분산화와 사용자를 위한 APY를 극대화하는 방식으로 토큰을 위임합니다. 대부분의 검증자들이 시작하는 방법입니다.



**3. Solana Foundation Delegation Program:** The Solana Foundation supports new validators through its delegation program where it delegates some tokens to them. If your validator fulfills the requirements, you could receive over 100,000 SOL delegated to you. Moreover, the foundation temporarily covers the voting costs for eligible validators. The only drawback is the time it takes to join the program and complete the KYC process.

**4. Jito Delegation Program:** Jito also offers a similar delegation program. To participate, you need to operate the JITO client and distribute your MEV bribes with delegators. Essentially, it mirrors the Solana Foundation's program but without the KYC requirements.

These are the main avenues for acquiring stake on Solana. However, simply knowing where to obtain stake is not sufficient; understanding how to optimize your delegation is crucial.

Ultimately, two key factors come into play: uptime and decentralization.



## 가동 시간

가동 시간은 검증자에 대해 가장 중요한 고려 사항입니다. 직접적으로 얼마나 많은 신용을 통해 벌어들일지 영향을 미치는 것뿐만 아니라, 주식을 유치할 가능성에도 영향을 미칩니다.

가동 시간은 수익과 APY에 영향을 미치므로 중요합니다. 낮은 APY는 유기적 주식을 얻기 어렵게 만들 수 있습니다. 다른 주식 출처도 가동 시간 기준을 갖추고 있으며, 이를 충족하지 못하면 어떠한 위임도 받을 수 없게 될 수 있습니다.

검증자의 가동 시간이 평균 이하일 경우 다른 모든 것은 중요하지 않습니다.



# 분산화

제2로 중요한 요소는 분산화입니다.

유기적 지분은 분산화에 영향을 받지 않지만, 다른 모든 지분 소스에서 사용되는 주요 지표입니다.

EU 어딘가에 이미 4개의 다른 유효성 검사기를 호스팅하는 데이터 센터를 사용하면 위임이 크게 줄어들지 않을 겁니다. 따라서 당신이 귀하의 검증자를 설정하기로 결정할 때 사용할 데이터 센터와 그 위치를 고려해야 합니다.



서로 다른 위임 프로그램에는 섬세한 뉘앙스가 있지만, 이 두 가지 지표를 최적화할 수 있다면 재단으로부터 어떠한 지분도 필요하지 않고 2~3개월 안에 비용을 회수할 수 있습니다.

![Running a Solana Validator](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_13.png)

지금쯤이면 당신은 발리데이터를 운영하는 것이 (단기적으로) 얼마나 유리한지 그리고 그것을 극대화하기 위해 무엇을 할 수 있는지에 대해 좋은 아이디어를 갖고 있어야 합니다.

중요한 후속 질문은: 장기적으로 합리적인가요?



그 질문에 대한 대답은 다시 한 번 어렵게 말할 수밖에 없어요.

장기적인 지속 가능성은 더 복잡한 주제인데요, 그 이유는 시간이 지남에 따라 변수들이 크게 달라질 수 있기 때문이에요.

따라서 SOL의 가격과 블록 공간 수요가 어떻게 변할지 예측하려고 하지 않고 다룰 수 있는 몇 가지 발전 사항에 초점을 맞출 거에요. 이미 논의한 내용과 함께 이 정보들이 밸리데이터를 운영하는 장기적인 전망의 기초가 되어야 해요.



## SIMD-0096

SIMD-0096은 모든 우선 수수료를 리더에게 보내도록 제안합니다. 승인되었으며, 이것이 구현되면 검증자의 경제 구조가 크게 변할 것입니다.

1월에 우선 수수료는 비투표 수수료의 92%를 차지했습니다.

![Running a Solana Validator a full breakdown](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_14.png)



만약 우선순위 수수료의 전부가 슬롯 리더에게 전달된다면 수익이 증가하고 검증자들이 인플레이션에 대한 의존을 줄일 수 있을 것입니다.

다시 말해, 검증자들은 더 적은 지분으로 더 많은 일을 할 수 있게 될 것입니다.

## 개선된 거래 수수료 모델

현재 솔라나의 거래 구조는 순진합니다.



거래 수수료는 수행하는 계산의 양이 아니라 필요한 서명의 수에 따라 부과됩니다.

이 구조는 여러 가지 문제를 일으키는데, 이에 대해 논의하는 것이 더 나을 것입니다. 중요한 점은 이러한 발전이 완료되면 투표 비용이 대부분의 다른 거래보다 훨씬 저렴해질 것이라는 것입니다.

또한 투표를 위한 블록 공간을 확보하려는 노력이 있습니다.

투표에 전용 블록 공간이 할당되면 사용자 거래에 의해 투표가 소모되는 위험이 줄어들어 수수료가 줄어들고, 결과적으로 투표 비용이 낮아질 것입니다.



## 스케줄러 개선

현재 스케줄러가 우선 수수료의 효율성을 높이기 위해 개선 작업이 진행 중에 있습니다.

이 변경 사항이 적용되면 MEV 캡처가 더 쉬워지며 온체인 스팸이 크게 줄어들고 우선 수수료가 높아질 것으로 예상됩니다.

## 리퀴드 스테이킹



Dune에 따르면 ETH의 26%가 스테이킹되어 있습니다. 이 중 약 39%는 유동성 스테이킹을 통해 진행됩니다.

Solana에서는 67.3%가 스테이킹되어 있으며 그 중 67%의 5.1%만이 유동성 스테이킹을 통해 이루어집니다.

DeFi 성숙도가 가장 많이 언급되는 이유로 이에는 여러 가능한 이유들이 있습니다.

관련성 있는 이유는 Solana에서 DeFi가 성숙해지면 네이티브 스테이킹에서 유동성 스테이킹으로의 이동이 있을 것이라는 점입니다.



이것은 좋은 소식입니다! 세 가지 가장 큰 유동 스테이킹 프로토콜 중 두 가지 (Marinade 및 SolBlaze)는 **분산화를 우선**시합니다.

다시 말해, 생태계가 성숙해짐에 따라 스테이크 분산이 개선되고, **초보 유효성 검사자**를 위한 초기 마찰이 줄어들 것으로 예상됩니다 (단, 우수한 운영 시간과 분산화가 제공되는 경우).

![RunningaSolanaValidatorafullbreakdown](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_15.png)

## SIMD-0033



SIMD-0033은 늦은 투표를 처벌할 것으로 승인된 제안입니다. 일부 유효성 검사자들이 일치에 도달한 후에만 투표를 지연하는 현상이 있었기 때문에 이 제안이 제기되었습니다. 이는 그들이 옳바르게 투표하지 않았기 때문에 투표 크레딧을 최대화할 수 있었지만, 일치 속도가 늦어지게 만들었습니다.

SIMD-0033을 통해 시간성이 크레딧 획득에 영향을 미칠 것입니다. 

이것을 두 가지 이유로 언급하고 있습니다:

- 노드의 성능이 이제 이전보다 중요하다는 것을 상기시키기 위해서입니다.
- 또한 유효성 검사자가 농담이 아님을 경고하기 위해서이며, 톨리의 말을 차용하자면, 그것은 사나운 싸움이라고 할 수 있습니다.



![RunningaSolanaValidatorafullbreakdown_16.png](/assets/img/2024-05-05-RunningaSolanaValidatorafullbreakdown_16.png)

## Firedancer

파이어댄서 클라이언트 출시의 효과는 높은 논란이 되고 있습니다.

직접적으로 블록 공간 가치를 높일 수는 없으며 이론적으로는 낮출 수도 있습니다. 그러나 만약 솔라나의 처리량을 10배로 증가시킨다면 주목할 가치가 있을 겁니다.



## 슬래싱

Solana는 현재 프로그램에 의한 슬래싱 기능이 없습니다.

노드가 악의적으로 작동하는 것이 발견되면, 검증자들은 그것을 슬래시 할지 결정할 수 있지만 현재는 확립된 메커니즘이 없습니다. 구현된 후에는 검증자의 행위가 적응되도록 변할 것입니다. 검증자 경제에 어떤 영향을 줄지 예측하기 어렵지만, 이것을 염두에 둘 것이 중요합니다.

그렇다면 지속가능성에 대해 어떤 의미를 가지고 있을까요?



모두가 만족할만한 보상 체계가 점점 더 개선되고 공정해지고 있습니다. 물론 경쟁은 갈수록 치열해지고 있지만, 고가용성을 유지하며 탈중앙화를 실현하는 우수한 검증자는 가까운 미래나 먼 미래에 수익을 올릴 수 없는 이유가 없습니다.

# 결론

Solana 검증자로 운영하는 것은 비용이 많이 들고 시간도 많이 소요됩니다. 그러나 네트워크는 우수한 성과를 내고 네트워크 탈중앙화를 촉진하는 검증자들에게 보상을 제공합니다.

만약 그러한 분야에 속한다면, 빠르게 스테이크를 모으고 순환이 가능할 것입니다.



비록 그렇지 않아도, Solana 검증자 운영은 돈이 아득한 구멍으로 사라지는 것이 아닙니다. 제대로 한다면, 재빨리 손실을 메울 수 있고, 재단으로부터 어떠한 스테이크도 필요하지 않습니다. 장기적 전망도 밝으며, 초기 마찰은 이후로만 감소할 것입니다.

요약하자면, 검증자 운영은 수익성 높은 시도일수도 있으며, 처음에는 사양할 수 없을 것 같지만 실제로 그렇지 않을 수도 있습니다.

모든 궁금증이 해결되었기를 바랍니다.

관심 있는 분들을 위해 검증자 디스코드, Solana 문서 및 기타 유용한 자료에 대한 링크는 다음 섹션에 있습니다.



# 자료

자체 노드를 구동하길 원하는 분들을 위한 자료:

- Solana Tech Discord

- Validator Documentation



**발리데이터로서 시작하는 워크숍**

저희와 함께 발리데이터로서의 첫 걸음을 떼어보세요!

# 더 많은 정보

Solana의 FTM을 향상시키기 위한 제안들



SIMD-033은 여기서 간단히 논의되었습니다.

Toly의 SVM Execution Economics

# 참고문헌 및 감사의 글

- [Solana 공식 문서 - 인플레이션 스케줄](https://solana.com/docs/economics/inflation/inflation_schedule)
- [Umbraresearch - Solana의 MEV](https://www.umbraresearch.xyz/writings/mev-on-solana)
- [Helius Blog - Solana 검증자 경제 구조](https://www.helius.dev/blog/solana-validator-economics-a-primer)
- [X.com - Ethereum 에서의 MEV](https://x.com/smyyguy/status/1753415253726015922?s=20)
- [Dune - 이더리움 (ETH)](https://dune.com/cz_5052/etherum-eth)
- [Dune - Solana 스테이킹](https://dune.com/ilemi/solana-staking)



Cogent, Ferric, Trent.sol, Montchik, Dan Smith, Ryan Chern, Zantetsu (Shinobi Systems), 특히 Italo Casas에게 이 작업을 수행하면서 모든 도움에 감사드립니다.

Toly를 인정하는 것은 건장할까요?

Italo는 SolDev Validator를 운영하며 Validator의 수익은 새로운 개발자를 Solana로 영입하는 데 사용됩니다. 직접 지원하셔도 됩니다. 그는 생태계를 위한 훌륭한 일을 하고 있습니다.

Cogent도 놀라운 Validator를 운영하고 연구 자금을 지원하고 있습니다. 여기에서 그들을 찾아볼 수 있습니다.