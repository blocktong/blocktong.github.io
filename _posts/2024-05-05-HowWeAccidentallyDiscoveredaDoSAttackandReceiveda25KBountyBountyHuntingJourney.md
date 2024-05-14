---
title: "어떻게 우리가 우연히 DoS 공격을 발견하고 2만 5천 달러의 현상수배를 받게 되었는지  현상수배 여정"
description: ""
coverImage: "/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_0.png"
date: 2024-05-05 17:48
ogImage: 
  url: /assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_0.png
tag: Tech
originalTitle: "How We Accidentally Discovered a DoS Attack and Received a $25K Bounty | Bounty Hunting Journey"
link: "https://medium.com/0-d/how-we-found-a-25k-bug-in-a-blockchain-project-by-mistake-bounty-hunting-journey-21c0c20440f6"
---


## 아이콘 취약성 발견 및 보상 수령

이번에 우리는 아이콘(Icon)에서 버그 바운티 프로그램의 범위를 벗어나는 RCE 취약성을 발견했고, 이어서 블록체인의 트랜잭션 처리와 새 블록 생성을 방해할 수 있는 DoS 공격을 발견하여 25,000달러의 보상을 받게 되었습니다.

# 배경

dWallet Labs에서의 블록체인 보안에 대한 우리의 비전에 따르면, 블록체인 네트워크는 모든 레이어와 측면에서 안전해야 합니다. 많은 노력이 스마트 계약의 보안에 투자되었지만, 이번 연구에서는 덜 이야기된 또 다른 영역인 블록체인 네트워크의 코어 네이티브 코드의 보안에 집중하기로 했습니다. 연구 과정에서 블록체인 네트워크의 코어 네이티브 코드에서 작은 코딩 실수가 얼마나 중대하고 피해를 입힐 수 있는지에 대한 흥미로운 발견을 했습니다.

# 첫 번째 단계 - 공격 대상 선정



우리는 상위 10개 블록체인 프로젝트 중 하나가 아니지만 수억 달러의 시가총액을 가진 블록체인 네트워크를 찾고 있습니다. Imunnifi 플랫폼의 현상금 목록을 필터링하면 ICON 프로젝트(시가총액 약 25억 달러)를 선택했습니다.

ICON 네트워크(https://icon.community/)는 여러 블록체인 네트워크를 연결하여 ICON Republic라고 불리는 탈중앙화 네트워크를 통해 서로 상호 작용할 수 있도록 하는 블록체인 프로젝트 및 암호화폐입니다. ICON 블록체인 또는 ICON 네트워크로 불리는 이 네트워크는 ICON 재단이 개발한 것으로, 이 재단은 한국의 블록체인 기술 기업입니다.

# 준비 및 설정

첫 번째 단계는 ICON 웹사이트의 공식 도커를 사용하여 로컬 테스트넷을 배포하는 것이었습니다. 도커는 “goloop”이라는 깃허브 저장소에서 발행자 코드를 실행합니다. 우리는 이 매뉴얼을 따라 로컬 네트워크를 초기화했습니다. 테스트를 위해 제네시스 파일을 사용하여 지갑을 생성했습니다.



로컬에서 도커를 실행한 후 깃허브에서 코드를 복제하고 GoLand로 열고 로컬 ICON 도커와의 원격 디버그 연결을 설정했습니다. 이를 통해 트랜잭션 파싱 프로세스를 단계별로 따라가며 중단점을 원하는 때에 설정하여 중지할 수 있었습니다. (더 많은 정보는 이 안내서를 참조하세요).

# 사냥을 떠나자

블록체인 네트워크의 코드는 꽤 복잡할 수 있으며 많은 공격 가능한 표면이 있습니다. 우리는 새로운 트랜잭션을 처리하는 과정에 주목하기로 결정했습니다. 이 과정은 복잡한 파싱 로직을 포함하고 있으며 사용자 입력과 직접적으로 관련이 있으며 RPC 인터페이스와 P2P 인터페이스를 통해 활성화될 수 있다는 점에서 선택했습니다. 게다가, 우리는 이 프로세스를 통해 검증자를 성공적으로 악용한다면 모든 검증자를 곧장 공격할 수 있을 것으로 추정했습니다.

새로운 트랜잭션 처리 흐름을 파헤치기로 결정하자 우리는 특정한 트랜잭션 유형에 초점을 맞추기로 했습니다: 새로운 스마트 계약을 배포하는 것.



ICON 네트워크는 2 종류의 스마트 계약을 지원합니다. ICON 생태계에서는 이를 "SCORE"라고 합니다 ("Smart Contract on Reliable Environment"). 두 종류는 Java 기반과 Python 기반입니다. 먼저 Python SCORE를 배포하는 과정을 시작했습니다.

사용자가 새로운 Python 계약을 배포하는 트랜잭션을 보내면, Python 계약이 zip 파일로 압축됩니다.

검증자가 일부 검사를 완료한 후 (서명 확인, 가스 등), 계약을 로컬에 저장합니다. 이 저장은 "goloop\service\contract\contractmanager.go" 파일의 "storePython" 함수를 통해 이뤄집니다 (아래 코드 참조).

해당 함수는 zip 파일을 검증자의 로컬 파일 시스템의 임시 폴더로 추출합니다. 함수는 모든 파일의 경로에서 공유 접두사를 제거하지만 공유되지 않는 경로 부분은 유지합니다.



예를 들어, 만약 zip 파일에 다음과 같은 파일들이 포함되어 있다면:

- hello_world/package.json
- hello_world/internal/init.py

함수는 "hello_world" 접두어를 제거하지만 두 번째 파일에서 "internal" 접두어는 유지할 것입니다. 그런 다음 함수는 "tmpPath"에 "non-shared" 경로 부분을 추가하여 연결하고(이는 이 SCORE용으로 생성된 임시 폴더입니다), 트리에 모든 폴더를 생성한 뒤 해당 폴더에 파일을 생성하고 그 내용을 쓸 것입니다.

이 코드에서 이 흐름을 보겠습니다:



```markdown
![image 0](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_0.png)

![image 1](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_1.png)

![image 2](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_2.png)

Now, the function will go over all the files in the zip and check (line 398) if the file path starts with the package name extracted from the path of the “package.json” file (pkgBase, which is “hello_world” in our example).
```



만약 경로가 패키지 이름으로 시작한다면, 해당 함수는 해당 부분을 경로에서 제거할 것입니다(line 401).

흥미로운 점은 420번째 줄에서 발생합니다 - 몇 가지 확인 작업을 수행한 후, 함수는 zip 파일의 파일 이름으로부터 temp 폴더 경로까지의 경로를 연결할 것입니다. 따라서, 만약 zip 파일 내의 경로가 "hello_world/internal/init.py"인 경우, 접두사를 제거하고 경로를 연결한 후에 생성되는 파일은 "tempDir/internal/init.py"가 될 것입니다.

이게 흥미로운 점인 이유는, 함수가 "non-shared" 경로 접두어에 "../"가 포함되어 있지 않음을 확인하지 않는다는 것입니다. 따라서, 이 경로가 tmpPath와 연결된다면, 공격자는 작성된 파일의 위치를 자유롭게 변경할 수 있습니다.

게다가, 파일은 755 권한으로 생성되어 실행할 수 있는 상태가 될 것입니다.



# RCE(원격 코드 실행)를 위해 이를 악용하세요

우리는 언제든지 실행 권한이 있는 내용을 원하는대로 파일에 쓸 수 있는 옵션이 있다. RCE로 이를 악용하는 다양한 방법이 있지만, 우리는 ICON 공식 도커 이미지에서 검색하기로 했습니다. 빠른 검색으로 "cronjob"을 사용하여 주기적으로 "/etc/periodic/15min/" 디렉토리에 있는 모든 파일을 실행하도록 도커가 구성되어 있다는 것을 발견했습니다.

![이미지](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_3.png)

예를 들어, 공격자는 zip 파일의 경로를 다음과 같이 설정할 수 있습니다:



### Image Insertion Vulnerability Found

We identified an image insertion vulnerability that could potentially compromise the security of your website. This vulnerability could allow an attacker to bypass security measures and execute malicious code on your website.

To mitigate this vulnerability, we recommend implementing input validation and encoding user-supplied content before displaying it on your website. Additionally, consider implementing Content Security Policy (CSP) headers to prevent unauthorized code execution.

If you need any assistance in securing your website or have any questions about this vulnerability, feel free to reach out. Your website's security is our top priority!



# 기타 악성 작업

이를 통해 "validator" 도커에서 " /etc/periodic/15min/evil" 경로에 악성 파일이 생성됩니다. 15분 이내에 bash 스크립트가 실행되어 RCE를 얻을 수 있습니다.

또한, 함수가 이미 존재하는 경우 해당 파일을 덮어쓸 것에 유의해야 합니다. 이는 취약점을 악용하는 또 다른 방법으로 악성 스크립트를 사용하여 다른 컨트랙트 코드를 덮어씌우고 해당 코드를 호출하여 모든 자금을 인출할 수도 있음을 의미합니다.

악의적인 zip 파일 생성은 다음 단계를 따릅니다.



- “zip -r” 명령을 사용하여 정기적인 zip 파일 생성
- zip 파일을 텍스트 편집기에서 열고 파일 이름을 "traversed-path" 파일 이름으로 변경합니다 (예제 이미지 참조).
- 악의적인 zip 파일을 계약 배포 방법으로 전송하기:

```bash
./goloop/bin/goloop rpc sendtx deploy /mnt/c/repos/exploit_python_contract/hello.tar — uri http://localhost:9080/api/v3/icon — key_store wallet.json — key_password gochain — nid 0x9383b0 — step_limit 10000000000 — content_type application/zip — param name=Alice
```

우리는 악의적인 행위를 방지하고, 이를 악용하는 악성 행위자를 피하기 위해 공개 테스트넷에서 노출하거나 시험하지 않았습니다. 또한, 한 번 블록체인에 악용이 올라가면 누구나 그것을 확인할 수 있기 때문입니다. 대안으로, 우리는 최신 버전의 코드(v1.3.3–1-g733a19d9)을 실행한 로컬 테스트넷에서 성공적으로 악용을 테스트하였습니다.

전체 PoC는 매우 간단합니다:



![HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_4.png](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_4.png)

![HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_5.png](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_5.png)

# ICON에 보고

블록체인 네트워크에서 잠재적인 RCE를 발견했어요. 네트워크에 대한 재앙이 될 수 있는 상황이죠. 왜냐하면 우리가 모든 검증자를 제어할 수 있을 정도로 엄청난 영향력을 행사할 수 있기 때문이에요. ICON 네트워크의 버그 바운티 프로그램 하에 Immunefi 보고 시스템에 새로운 보고를 했어요. 몇 시간의 긴장 속에서 기다린 끝에 화가 나게도 귀찮은 답변을 받았습니다.



![How We Accidentally Discovered a DoS Attack and Received a 25KB Bounty - Bounty Hunting Journey](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_6.png)

요지는 로컬 테스트넷에서는 이 취약점이 작동했지만, 메인넷에서는 작동하지 않을 것이라는 것이었습니다. 이것이 스코프에서 벗어났다는 이유였습니다.

# 계속 노력해보세요

첫 번째 보고서는 거부당했지만, ICON 네트워크와 코드 구조에 대해 많은 것을 배웠습니다. 거부 사유가 파이썬 컨트랙트가 스코프를 벗어난다는 내용이었으므로, 다음 단계는 Java SCORE 배포 프로세스에 집중하는 것이었습니다.



새로운 Java SCORE를 작성하고 Java bytecode로 코드를 컴파일한 후에는 사용자가 ICON Foundation이 제공하는 "jar-optimizer"를 실행해야 합니다.

이 "optimizer"는 다음과 같은 작업을 수행합니다:

- 악의적인 클래스(파일 또는 프로세스 조작과 같은) 사용을 방지하기 위해 Java 코드에 허용된 Java 클래스만 사용되었는지 확인합니다. 이러한 보안 검사는 유효성 검사자도 수행하므로 클라이언트 측에서 이러한 검사를 우회해도 효과가 없습니다.
- Java 바이트 코드를 최적화하여 파일 크기를 줄입니다.
- SCORE의 내보낸 함수, 해당 매개변수 및 함수 및 매개변수의 유형을 설명하는 API 파일을 생성합니다.

우리는 클래스 이름 필터링을 우회하기 위해 여러 테스트를 수행했지만 성공하지 못했습니다. 그래서 우리는 다른 방법을 시도했습니다. 프로세스를 시작하는 데 사용할 수 있는 ProcessBuilder 클래스와 같은 화이트리스트되지 않은 클래스의 객체를 만드는 대신, 이 객체를 계약이 내보낸 함수 중 하나의 매개변수로 전달해보려 합니다.



하지만 문제는 악용된 함수가 수신할 수 있는 매개변수 유형을 검증하는 것이었습니다. 우리는 유효한 매개변수를 수신하는 적절한 함수를 생성하여 이 검증을 우회하려고 노력했고, "jar-optimizer"를 실행한 후에 바이트코드 내 객체의 유형을 수동으로 변경하였습니다.

또한 ICON에서 사용하는 Java VM이 계약 내 함수를 호출할 때 사용하는 API 파일의 함수 시그니처를 변경해야 했습니다.

이 API 파일은 MessagePack 인코딩을 사용하여 인코딩되어 있으며, 이는 효율적인 이진 직렬화 형식입니다. 따라서이를 변경하기 위해 멋지고 쉬운 MsgPack 편집기를 찾았습니다. 좋아 보이는 하나를 발견하여 원하는 변경 사항을 적용하고 파일을 저장한 후 JAR 파일을 다시 패킹했습니다.

![How We Accidentally Discovered a DoS Attack and Received a 25KBounty_Bounty Hunting Journey](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_7.png)



거래를 보내어 이 계약을 배포한 후에 거래 ID를 받았습니다. 거래 상태를 확인하기 위해 (icx_getTransactionResult를 사용) 확인했더니 거래가 “진행 중”인 것을 알았습니다. 잠시 기다렸다 다시 확인했는데도 여전히 “진행 중”이었습니다. 그래서 다른 거래를 보내어 상태를 확인해 보았지만 “대기 중”이라는 응답을 받았습니다. 그 순간에 우리는 여기에 흥미로운 점이 있다는 것을 깨달았습니다.

![이미지1](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_8.png)

![이미지2](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_9.png)

![이미지3](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_10.png)



ICON 도커로 이동하여 예외가 발생했음을 확인했습니다:

![Image](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_11.png)

# 그래서 무슨 일이 있었나요?

MsgPack 편집기를 사용하면 API 파일이 손상되었음을 확인했습니다. 궁금한 점은, 왜 이 예외가 정상적으로 처리되지 않았을까요?



이 질문에 대한 대답은 API 파일의 구문 분석 코드에 있습니다. API 파일을 검증하는 "Validate" 함수를 검토해 봅시다:

![image](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_12.png)

해당 함수가 MethodUnpacker.readFrom 함수를 사용하고 가능한 IOException을 처리하고 있다는 것을 알 수 있습니다. MethodUnpacker.readFrom 함수는 JAVA MsgPack 라이브러리의 unpackArrayHeader라는 함수를 호출하고 있습니다:

![image](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_13.png)



The `unpackArrayHeader` function, as defined, can also throw an IOException. However, upon closer inspection, we find that this function may also raise another type of exception when calling the `unexpected` function.

![Link to image](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_14.png)

In fact, this function actually returns an object of type `MessagePackException`, which is a subclass of `RuntimeException` and not `IOException`.

![Link to another image](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_15.png)



![Image](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_16.png)

앞으로는 이미지 파일을 Markdown 형식으로 변경합니다.

API 파일이 잘못됨으로 인해 처리되지 않은 예외가 발생했습니다. 어떻게든 이로 인해 유효성 검사기 코드에 무한 루프가 생성되어 다른 트랜잭션을 처리하거나 새 블록을 생성하는 것을 방해했습니다. 게다가 이 트랜잭션을 검증기에서 다른 모든 검증기로 전파했기 때문에 전체 네트워크가 새로운 트랜잭션을 처리하지 못하도록 멈춘 상태가 되었습니다.

# 보고서 작성과 협상

저희는 Immunefi 대시보드에 또 다른 보고서를 작성했습니다. 이 번 보고서는 더 짧았지만, 가장 중요한 부분인 작동하는 POC를 포함했습니다. ICON 팀에게 이 POC가 실제로 DOS 공격임을 설득하고 이 문제의 심각성을 협상했습니다. 이 문제가 "네트워크가 새로운 트랜잭션을 확인할 수 없는" 중대한 문제로 분류될 수 있다고 믿었지만, 우리는 심각성을 높이는 데 합의했습니다 (“블록 생성 불가능 (합의 작동하지 않음)”). 마침내 우리는 바운티를 받을 수 있었습니다!



```markdown
![Screenshot](/assets/img/2024-05-05-HowWeAccidentallyDiscoveredaDoSAttackandReceiveda25KBountyBountyHuntingJourney_17.png)

# 결론

블록체인 네트워크는 복잡한 로직을 수행하는 방대한 코드베이스로 이루어져 있어 다양한 잠재적 문제점을 가지고 있습니다. 보상 사냥꾼들에게는 이 도메인이 매우 매력적하며, 저에게는 스마트 계약보다 훨씬 더 흥미로운 곳입니다. 기능적 디버깅 환경은 버그 사냥을 현저하게 가속화하고 간소화해줍니다. 게다가 운은 모든 보상 사냥꾼의 필수 도구로서 핵심적인 역할을 합니다.
```