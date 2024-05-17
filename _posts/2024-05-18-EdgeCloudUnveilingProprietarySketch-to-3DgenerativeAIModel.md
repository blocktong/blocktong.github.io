---
title: "엣지클라우드 독자적인 스케치로 3D 모델 생성하는 AI 모델 공개"
description: ""
coverImage: "/assets/img/2024-05-18-EdgeCloudUnveilingProprietarySketch-to-3DgenerativeAIModel_0.png"
date: 2024-05-18 00:40
ogImage: 
  url: /assets/img/2024-05-18-EdgeCloudUnveilingProprietarySketch-to-3DgenerativeAIModel_0.png
tag: Tech
originalTitle: "EdgeCloud: Unveiling Proprietary Sketch-to-3D generative AI Model"
link: "https://medium.com/theta-network/edgecloud-unveiling-proprietary-sketch-to-3d-generative-ai-model-5f31c6efea8c"
---


![EdgeCloud Unveiling Sketch-to-3D Generative AI Model](/assets/img/2024-05-18-EdgeCloudUnveilingProprietarySketch-to-3DgenerativeAIModel_0.png)

안녕하세요! Theta 팀에서는 2024년 5월 15일 AMA 중에 CTO Jieyi가 데모를 선보인 Sketch-to-3D generative AI 모델에 대한 자세한 내용을 곧 공유할 예정입니다. 녹화 영상은 여기서 확인하실 수 있어요. 이 모델의 AI 기술은 정말 혁신적이며 놀랍게 복잡합니다. 이 사유적인 AI 모형 파이프라인은 지난 해 대부분 Theta에서 개발되었습니다. 저희는 기쁘게 알려드립니다. EdgeCloud의 프로덕션 환경에 배포되었다는 사실을:

[Theta EdgeCloud AI Service Model Explorer](https://www.ThetaEdgeCloud.com/dashboard/ai/service/model-explorer)

# 도전적인 generative AI 문제

<div class="content-ad"></div>

스케치를 3D로 변환하는 작업은 선과 모양을 해석하여 손으로 그린 스케치를 정확하게 3D 디지털 객체로 변환하는 매우 어려운 생성적 AI 작업입니다. 이 기술은 영화 애니메이션, 게임부터 건축 및 산업 디자인까지 다양한 분야에서 활용될 수 있으며, 예술가와 디자이너들이 아이디어를 빠르게 프로토타입화하고 개념을 완전히 상호작용 3D 공간에서 시각화하는 데 도움이 됩니다.

간단하고 종종 거친 2D 스케치를 상세하고 정확한 3D 모델로 변환하는 작업은 예술가가 2D 스케치에서 가상으로 그리는 섬세한 세부 사항과 모양을 보존한 고 공간 해상도의 3D 모델을 생성하는 것이 어려운 점이 많습니다.

우리는 이 작업을 두 단계로 나눠 처리합니다. 먼저 스케치를 "2.5D" 이미지로 변환하고, 이는 3D 개체를 2D 평면에 투영한 것입니다. 그리고 이러한 투영 이미지를 3D 모델로 변환합니다. 이 두 단계 접근법은 작년 GoogleCloud Next 컨퍼런스에서 Theta Labs와 GoogleCloud가 공동으로 발표한 "모델-파이프라인" 개념의 또 다른 예시입니다.

# 단계 1 — 스케치를 3D 모델로 변환하는 파이프라인 디자인

<div class="content-ad"></div>

![EdgeCloudUnveilingProprietarySketch-to-3DgenerativeAIModel_1.png](/assets/img/2024-05-18-EdgeCloudUnveilingProprietarySketch-to-3DgenerativeAIModel_1.png)

우리의 첫 번째 단계는 스케치로부터 2.5차원 이미지를 생성하는 것인데, 이는 StableDiffusion과 ControlNet의 강력한 조합을 사용합니다. 특히, 우리는 고급 신경망 구조인 ControlNet을 통합했습니다. ControlNet은 확산 모델을 사용한 이미지 합성의 맥락에서 모델 생성 과정 중에 추가적인 제어와 정밀도를 제공하도록 설계된 신경망입니다. ControlNet은 이러한 모델들의 기능을 향상시킴으로써 추가 조건이나 제약 조건을 통합하여 출력물을 보다 효과적으로 가이드할 수 있습니다. 이는 스케치를 2.5차원 이미지로 변환하는 응용 프로그램과 같이 원본 스케치의 무결성과 세부 정보를 유지하는 데 중요합니다. 우리는 ReV Animated 모델을 재교육하여 상세한 2.5차원 이미지를 생성하는 능력 때문에 Stable Diffusion을 선택했습니다. 이 접근 방식은 3D 모델링 작업에 견고한 기반을 확립했습니다.

![EdgeCloudUnveilingProprietarySketch-to-3DgenerativeAIModel_2.png](/assets/img/2024-05-18-EdgeCloudUnveilingProprietarySketch-to-3DgenerativeAIModel_2.png)

# Stage 2 — 스케치에서 3D 모델로의 파이프라인 디자인

<div class="content-ad"></div>

![EdgeCloudUnveilingProprietarySketch-to-3DgenerativeAIModel_3](/assets/img/2024-05-18-EdgeCloudUnveilingProprietarySketch-to-3DgenerativeAIModel_3.png)

이제 두 번째 단계인 2.5D 이미지를 3D 모델로 변환하는 과정이 시작됩니다. 이 프로세스는 고급 객체 추출 알고리즘을 사용하여 2.5D 이미지에서 주요 객체를 추출하여 배경 소음과 간섭을 효과적으로 줄이는 것으로 시작됩니다. 이 단계는 주요 객체가 정확하게 분리되도록 정밀하고 세심한 주의가 필요합니다. 이후 TripoSR을 활용하여 3D 모델을 구성했습니다. TripoSR은 Stability.ai에서 개발된 모델로, 자세하고 정확한 3D 표현에 뛰어납니다. 이 첨단 기술의 조합은 간단한 스케치를 효율적이고 정확하게 세밀한 3D 모델로 변환하는 복잡하면서도 매우 효과적인 워크플로를 만들어 냅니다.

# 대규모 제품 배포 모델

Theta의 독점적인 Sketch-to-3D 모델은 이제 Theta EdgeCloud 대시보드에서 사용 가능합니다:

<div class="content-ad"></div>

https://www.ThetaEdgeCloud.com/dashboard/ai/service/model-explorer

![EdgeCloud Unveiling Sketch-to-3D generative AI Model](/assets/img/2024-05-18-EdgeCloudUnveilingProprietarySketch-to-3DgenerativeAIModel_4.png)

여기서 몇 번의 클릭만으로 모델을 실행할 수 있습니다. 모델이 EdgeCloud에 성공적으로 배포되면 https://sketchtoxxxxx.tec-s1.onthetaedgecloud.com/와 같은 추론 엔드포인트를 얻을 수 있습니다. 해당 엔드포인트를 클릭하면 WebUI가 표시되고, 여기서는 스케치 이미지를 업로드하거나 직접 그림판에 스케치할 수 있습니다. 스케치를 업로드한 후, 생성하고자 하는 3D 개체를 설명하는 텍스트 프롬프트를 제공할 수 있습니다. 그런 다음 "2D 생성" 버튼을 클릭하세요. 만족스러운 2.5D 이미지가 생성될 때까지 몇 번 시도할 수 있습니다. 그런 다음 "3D 생성" 버튼을 클릭하여 3D 모델로 변환하세요! 생성된 3D 모델은 WebUI에서 확대 및 회전할 수 있습니다. 또한 모델을 다운로드하여 다른 3D 모델링 도구로 가져올 수 있습니다. 첫 번째 생성에는 모델 매개변수를로드해야 하므로 약 2-3분이 소요될 수 있지만 두 번째 생성부터는 더 빨라질 것입니다.

![EdgeCloud Unveiling Sketch-to-3D generative AI Model](/assets/img/2024-05-18-EdgeCloudUnveilingProprietarySketch-to-3DgenerativeAIModel_5.png)

<div class="content-ad"></div>

![Gradio WebUI and API endpoint documentation](/assets/img/2024-05-18-EdgeCloudUnveilingProprietarySketch-to-3DgenerativeAIModel_6.png)

**그림 5. Theta EdgeCloud에서 실행 중인 Sketch-to-3D 모델의 Gradio WebUI 및 API 엔드포인트 문서**

이 모델은 개발자들이 프로그래밍 방식으로 스케치에서 3D 생성을 수행하거나 이 기능을 사용자 정의 애플리케이션에 통합할 수 있도록 하는 API 세트를 제공합니다. API 엔드포인트 및 문서를 보려면 WebUI 하단의 "API를 통해 사용" 버튼을 클릭하거나 추론 엔드포인트에 `?view=api`를 추가하십시오. 예를 들어, https://sketchtoxxxxx.tec-s1.onthetaedgecloud.com/?view=api와 같습니다.

우리는 곧 AI 쇼케이스에 스케치에서 3D 생성을 추가할 예정이니 기대해 주세요. 한편, Theta의 독점 3D GenAI를 즐기세요! 🌟