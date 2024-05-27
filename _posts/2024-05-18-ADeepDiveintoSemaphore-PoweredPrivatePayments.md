---
title: "세마포어를 활용한 은행설명서 포인트에 대해 자세히 알아보기"
description: ""
coverImage: "/assets/img/2024-05-18-ADeepDiveintoSemaphore-PoweredPrivatePayments_0.png"
date: 2024-05-18 00:41
ogImage: 
  url: /assets/img/2024-05-18-ADeepDiveintoSemaphore-PoweredPrivatePayments_0.png
tag: Tech
originalTitle: "A Deep Dive into Semaphore-Powered Private Payments"
link: "https://medium.com/@matan_79468/a-deep-dive-into-semaphore-powered-private-payments-3cc1393de862"
---


![Screenshot](/assets/img/2024-05-18-ADeepDiveintoSemaphore-PoweredPrivatePayments_0.png)

## Prerequisites:

To get the most out of this article, it's essential to have a good understanding of Semaphore. You can find the documentation [here](https://docs.semaphore.pse.dev/).

# Introduction 

<div class="content-ad"></div>

암호화폐 세계에서는 개인 정보 보호가 매우 중요합니다. 블록체인 기술은 투명성을 제공하지만 종종 민감한 금융 정보를 노출시킬 수 있습니다. 이에 Tornado Cash와 같은 해결책이 등장했는데, 이러한 해결책은 거래에서 익명성을 제공하려는 목표를 가지고 있습니다. 그러나 Tornado Cash는 남용 가능성으로 인해 조명을 받았습니다.

여기에 강력한 프로토콜인 Semaphore이 등장했습니다. Semaphore은 거래 세부 정보를 숨기기 위해 제로-지식 증명 (ZKP)을 활용합니다. 이는 높은 수준의 제어와 사용자 정의 기능을 제공하여 개인 정보 보호 애플리케이션을 구축하는 데 이상적입니다.

이 기사에서는 Semaphore의 새로운 응용 프로그램인 Paybox를 탐색할 것입니다. 이 분산 시스템은 자금을 안전하고 비공개적으로 관리하기 위해 Semaphore 프로토콜의 힘을 활용합니다.

## Paybox

<div class="content-ad"></div>

Paybox는 사용자가 자신의 기금을 예금할 수 있는 개인 “paybox”를 만들 수 있도록 합니다.

다음은 작동 방식입니다:

- Paybox 생성: Paybox를 생성하여 상자의 “소유자”가 됩니다.
- 자금 예금: 그 후 다른 사람들은 당신의 Paybox로 돈을 송금할 수 있습니다. 그들은 당신의 개인 정보를 알 필요가 없으며 Paybox ID만 필요합니다.
- 자금 액세스: 소유자로서 원할 때마다 당신의 Paybox의 돈을 인출할 수 있습니다.

이것을 디지턈 기부 상자처럼 생각해보세요:

<div class="content-ad"></div>

- 특정 페이박스로 돈을 보내는 방식으로 사람들은 목적에 기부할 수 있습니다.
- 목적을 시작한 사람(페이박스 소유자)은 그 후에 rekte된 자금을 인출할 수 있습니다.

페이박스는 가장 간단한 형태로 익명 및 안전하게 돈을 모으는 방법이며 전통적인 은행 계좌나 결제 처리기가 필요하지 않습니다.

## 페이박스를 위한 세마포어 사용법

세마포어 프로토콜은 제로-지식-증명(ZKP)을 페이박스 프로젝트에 도입하는 혁신적인 방법입니다. 세마포어는 우리의 주요 페이박스 계약 내에서 많은 머클 트리를 빠르고 쉽게 만들 수 있는 자원을 제공합니다. 각각의 머클 트리는 사용자가 예금하길 원하는 다른 금액에 대해 사용될 것입니다.

<div class="content-ad"></div>

# 스마트 계약

우리는 생성자에서 이미 1~100까지의 그룹을 만들 것입니다. 이러한 그룹들은 자신의 paybox로 자금을 입금하는 과정에서 믹서로 사용될 것입니다. _createGroup 함수는 그룹 ID와 관리자를 입력받습니다. 믹서 그룹의 경우 누구도 관리자가 되면 안 되기 때문에 스마트 계약 자체가 그 역할을 수행할 것입니다.

```js
constructor(ISemaphoreVerifier _verifier) Semaphore(_verifier) {
      for (uint i=1; i<=100; i++) {
          _createGroup(i, address(this));
      }
  }
```

## 상자 열기

<div class="content-ad"></div>

상자를 열려면 상자 매핑에서 현재 그룹 ID와 함께 메시지 보내는 사람을 소유자로 설정합니다. 이 변수는 101부터 시작하여 새 상자마다 증가합니다.

```js
uint currentGroupId = 101;    
function createBox() public {
      boxes[currentGroupId].owner = msg.sender;
      currentGroupId++;
  }
```

## 입금

상자에 자금을 입금하려면 세마포어 ID를 생성해야 합니다. 이 ID에는 후에 증명을 생성하는 데 사용할 커밋먼트와 시크릿, 널리파이어가 포함됩니다. Identity를 생성하기 위해 Semaphore에는 매우 쉽게 사용할 수있는 JS 패키지가 있습니다.

<div class="content-ad"></div>

```javascript
const { Identity } = require("@semaphore-protocol/identity")
const identity = new Identity()
console.log(identity)
```

이 신원 정보에는 후에 사용할 개인 키도 함께 포함되어 있습니다. 현재는 단순히 커밋먼트만 필요합니다.

커밋먼트와 함께 자금을 예금할 때 사용자는 삽입할 금액을 입력해야 합니다. 이 금액은 사용자의 커밋먼트가 100개 그룹 중 어느 그룹에 배치될지 결정하는 데 사용됩니다.

```javascript
function deposit(uint amount, uint256 commitment) public payable {
    require(1 <= amount && amount <= 100, "Amount is not valid");
    require(amount <= msg.value - 1);
    this.addMember(amount, commitment);
}
```

<div class="content-ad"></div>

## 상자에 자금 추가하기

이제 사용자가 특정 금액에 해당하는 그룹 내의 약속을 가지고 있으므로 addAmountToBox 함수를 호출할 수 있습니다. 이 호출은 유효한 증명만 있으면 어떤 지갑에서든 수행할 수 있습니다. 이 단계에서 사용자는 상자와 구분됩니다.

사용자는 자신의 식별정보로 생성된 증명, 약속이 있는 그룹의 groupId(금액에 해당) 및 자금을 제공하려는 상자의 boxId와 함께 이 함수를 호출할 것입니다. 증명이 유효성 검사를 통과하면 nullifierHash가 그룹에 저장되고 groupId(금액)이 boxId의 boxFund에 추가됩니다.

```js
function addAmountToBox(SemaphoreProof calldata proof, uint groupId, uint boxId)public {
    require(scope == proof.scope, "유효 범위가 맞지 않음");
    if (groups[groupId].nullifiers[proof.nullifier]) {
        revert Semaphore__YouAreUsingTheSameNullifierTwice();
    }
    // 증명이 성공적으로 검증되지 않으면 함수가 되돌아갑니다.
    if (!verifyProof(groupId, proof)) {
        revert Semaphore__InvalidProof();
    }
    groups[groupId].nullifiers[proof.nullifier] = true;
    boxes[boxId].boxFund += groupId;
}
```

<div class="content-ad"></div>

프로프를 생성하고 인출 함수를 호출하려면 Semaphore 패키지를 사용할 수 있습니다.

```js
const {SemaphoreEthers} = require("@semaphore-protocol/data")
const { Group } =require ("@semaphore-protocol/group")
const { generateProof, verifyProof} =require ("@semaphore-protocol/proof")

const semaphoreEthers = new SemaphoreEthers("http://3.22.38.202:8545", {
    address: "0xf7C339C5520f6800266227878A6A44Db4eEc35C4"
})
function main(){
  const payboxContract = new ethers.Contract("0xf7C339C5520f6800266227878A6A44Db4eEc35C4", payboxTCAbi, wallet)
  const identity = new Identity("e6d8b796a4d35d6b7c3461048ec2e7a0f9d48a61d9d19063f95bcbf384d21330")
  const members = await semaphoreEthers.getGroupMembers("75")
  const group = new Group(members)
  const proof = await generateProof(identity, group, 0, publicNullifier)
  const tx = await payboxContract.addAmountToBox(proof, 75, 102)
  console.log("자금을 인출 중입니다...")
  await tx.wait()
}
```

## 출금

출금 함수는 매우 간단합니다. 상자 ID 소유자에 해당하는 지갑은 출금하려는 금액과 함께 함수를 호출해야 합니다. 이 금액은 그런 다음 계약으로부터 지갑으로 이체되며 상자 자금은 재설정됩니다.

<div class="content-ad"></div>

```js
function withdraw(uint boxId, uint amountToWithdraw)public{
    require(boxes[boxId].owner == msg.sender, "ur not the owner of the box");
    uint amount;
    if(amountToWithdraw > boxes[boxId].boxFund){    // if amount req is bigger than fund give everything in box
        payable(msg.sender).transfer(boxes[boxId].boxFund);
        boxes[boxId].boxFund = 0;
    }
    else{   // if amount is smaller than fund give amount
        payable(msg.sender).transfer(amount);
        boxes[boxId].boxFund -= amount;
    }
}
```

# 결론

세마포어 프로토콜을 기반으로 한 Paybox 시스템은 암호화폐 분야에서 더 안전하고 비공개적인 거래를 위한 중요한 발전을 나타냅니다. 토네이도 캐시의 익명성 원칙을 Semaphore의 향상된 제어 및 사용자 정의 기능과 결합하여 자금을 관리하기 위한 독특하고 강력한 시스템을 만들었습니다.

주요 포인트:

<div class="content-ad"></div>

- Paybox은 결제 시 개인 정보 보호와 보안을 위해 Semaphore 프로토콜을 활용하고 있습니다.
- 개별 Paybox를 나타내기 위해 Semaphore 그룹을 사용합니다.
- 사용자는 익명으로 자금을 입금할 수 있으며, Paybox 관리자는 언제든지 인출할 수 있습니다.
- 이 시스템은 강화된 개인 정보 보호와 안전을 제공하면서 자금을 유연하고 강력하게 관리할 수 있는 방법을 제시합니다.
- Paybox 계약은 암호화폐나 ERC20과 함께 사용되며, 달러와 연동할 수 있는 USDC와도 잠재적으로 사용될 수 있습니다.

앞으로 이 기술을 더욱 탐구하고 발전시키면, 개인 결제, 금융 포용성, 탈중앙화 시스템에 대한 더욱 강력하고 견고한 응용 프로그램이 탄생할 수 있습니다.

문의나 의견이 있으시면 LinkedIn에서 접할 수 있습니다: [마탄 프리드만 LinkedIn](https://www.linkedin.com/in/matan-friedman-898358249/)

Semaphore 문서: [Semaphore docs](https://github.com/semaphore-protocol/semaphore)