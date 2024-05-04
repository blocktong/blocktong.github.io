---
title: "비탈릭 부테린은 제로지식 증명 속도를 높이는 Binius를 소개합니다"
description: ""
coverImage: "/assets/img/2024-05-01-VitalikButerinbreaksdownBiniusasawaytospeedupzero-knowledgeproofs_thumbnail.png"
date: 2024-05-01 18:17
ogImage: 
  url: /assets/img/2024-05-01-VitalikButerinbreaksdownBiniusasawaytospeedupzero-knowledgeproofs_thumbnail.png
tag: Tech
originalTitle: "Vitalik Buterin breaks down ‘Binius’ as a way to speed up zero-knowledge proofs"
link: "https://cointelegraph.com/news/vitalik-buterin-explains-binius-improve-zero-knowledge-proofs"
---


![이미지](/assets/img/2024-05-01-VitalikButerinbreaksdownBiniusasawaytospeedupzero-knowledgeproofs_thumbnail.png)

## 이더리움 공동 창업자 Vitalik Buterin은 이진 필드 기반 증명 기술을 통해 "훨씬 더 많은 성능을 기대"하고 있다고 전했습니다.

이더리움 공동 창업자인 Vitalik Buterin이 지난 4월 29일 블로그 게시물에서, "Binius"라는 이진 필드 기반 고효율 암호화 증명 시스템에 대해 설명했습니다. 이 시스템은 기존의 zk-SNARK와 같은 전통적인 증명 시스템보다 큰 성능 향상을 실현할 것을 목표로 합니다.



Binius aims to achieve greater efficiency by performing computations directly over individual binary bits - zeros and ones - instead of larger numbers.

The motivation behind the system stems from the traditional cryptographic proof systems like SNARKs (Succinct Non-Interactive Argument of Knowledge) and STARKs (Scalable Transparent Argument of Knowledge) working with larger numbers such as 64-bit or 256-bit integers.

Underlying data being processed often consists of small values like counters, indices, and boolean flags. By operating on bits directly, Binius can process this data more efficiently, as stated by Buterin.

![Vitalik Buterin breaks down Binius as a way to speed up zero-knowledge proofs](/assets/img/2024-05-01-VitalikButerinbreaksdownBiniusasawaytospeedupzero-knowledgeproofs_0.png)



버터린에 따르면, 새로운 입증 시스템은 데이터를 비트의 다차원 "초정방체"로 표현하고 이진 "유한 체"를 사용하여 비트와 비트 시퀀스에 대한 효율적인 산술 연산을 허용하는 등의 개선을 제공합니다.

또한 바이너리를 사용하는 동안 비트 수준의 데이터를 "다항식" 처리와 Merkle 증명에 적합한 형태로 변환하는 특수 인코딩 및 디코딩 프로세스를 사용하며, 동시에 바이너리에서 작업하는 효율성 이점을 유지합니다.

바이너리 시스템은 암호학적 증명 시스템의 핵심 산술에 대한 중요한 개선을 제공하여 복잡한 암호 응용 프로그램을 더 효율적이고 확장 가능하게 만듭니다.

다항식은 종종 zk-증명에서 사용되며, 데이터와 계산을 인코딩하여 기본 정보를 공개하지 않고 증명을 확인할 수 있는 방식으로 구성되어 있어 "제로 지식"이라는 용어가 사용됩니다.



Buterin은 Binius 프로토콜을 복잡한 수학적 계산을 통해 보여줌으로써 데이터를 인코딩하고 증명을 생성하며 검증자가 이러한 증명을 효율적으로 확인할 수 있는 방법을 보여주었습니다.

관련: ZK-프로프는 개발자들에게 보안적인 도전을 제시합니다.

이 개념은 암호학자인 Benjamin E. Diamond과 Jim Posen이 "이진 필드의 타워를 통한 간결한 주장"이라는 2023년 백서에서 처음으로 제안되었습니다.

전반적으로, Binius는 작은 값과 비트 수준 연산을 포함하는 계산에 대해 더 전통적인 증명 시스템보다 상당한 성능 향상을 이루고자 합니다.



"Buterin concluded by stating his expectations for many more advancements in binary-field-based proving techniques in the upcoming months.

Magazine: Big Questions: What were Satoshi Nakamoto's thoughts on ZK-proofs?"