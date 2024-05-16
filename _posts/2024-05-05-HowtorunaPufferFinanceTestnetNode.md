---
title: "퍼퍼 파이낸스 테스트넷 노드를 운영하는 방법퍼퍼 파이낸스 테스트넷 노드를 운영하려면 다음 단계를 따르세요1 퍼퍼 파이낸스 웹사이트에 접속하여 Testnet 섹션을 찾습니다2 해당 섹션에서 노드를 운영하기 위한 다운로드 링크를 찾아 설치 파일을 다운로드합니다3 설치 파일을 실행하여 노드를 설치한 후 설정을 구성합니다4 퍼퍼 파이낸스 테스트넷에 연결하여 노드가 정상적으로 작동하는지 확인합니다이와 같은 간단한 단계를 따라 퍼퍼 파이낸스 테스트넷 노드를 성공적으로 운영할 수 있습니다"
description: ""
coverImage: "/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_0.png"
date: 2024-05-05 17:09
ogImage: 
  url: /assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_0.png
tag: Tech
originalTitle: "How to run a Puffer Finance Testnet Node"
link: "https://medium.com/@stevenbravo_/how-to-run-a-puffer-finance-testnet-node-0b6616924265"
---


이 가이드에서는 Puffer Finance 노드를 Holesky Testnet Network에서 Security Signing 없이 실행하는 과정에 대해 안내해 드리겠습니다. Nimbus를 합의 클라이언트로 사용하고 Nethermind를 실행 클라이언트로 사용할 것입니다.

# 요구 사항:

운영 체제: Linux 64비트 (Ubuntu, Debian 등의 배포판 중 하나)

프로세서: 이 가이드에서는 AMD 또는 Intel 프로세서를 사용할 것이며, ARM 프로세서도 호환 가능하지만 여기에서 다루지 않는 가이드에 대한 몇 가지 변경이 필요합니다.



**시스템 요구 사항:**

- 메모리: 16GB 이상
- 디스크 공간: 1TB SSD (SATA 또는 NVMe) 이상
- 네트워크: 신뢰할 수 있는 고속 인터넷. 이 프로세스를 원할하게 진행하기 위해 데이터 용량 제한이 있는지 여부를 인터넷 제공업체나 서버 제공업체와 확인해보세요. 가능하면 제한 없이 사용하는 것이 좋습니다.

본 안내서에서는 특히 Ubuntu 22.04를 사용할 것입니다.



# 지정된 지갑 설정하기

먼저, Metamask를 통해 기존 지갑을 선택하거나 새 지갑을 생성해야 합니다.

- 이 과정을 진행하기 위해 이미 지정된 지갑을 보유하고 있다면 해당 지갑 주소와 개인 키를 손에 쥘 준비를 해주세요.

- 지갑이 없다면 Metamask를 사용하여 새로 만들고, 마찬가지로 해당 지갑 주소와 개인 키를 손에 쥘 준비를 해주세요.



**이 가이드에서는 안내를 위해 예시 지갑을 사용할 것입니다. 하지만 사용할 때는 여러분이 사용할 지갑 주소와 개인 키로 바꿔야 함을 명심해 주세요.**

예시 지갑 주소: `0xB0551d678a6746F2Cf300CF698d314EA1e74e3c7`

예시 지갑 개인 키: `9825b255c5a7bf027237140ebb86e11970027467d08f6a2ad84f02b6665b1f30`

# PC 또는 서버 설정하기



이 가이드에서는 aeza.net의 전용 서버를 사용할 것입니다. 구성은 다음과 같습니다:

![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_0.png)

해당 서버를 확인하고 싶다면, 등록 후에 이 링크를 사용하여 잔액을 입금하면 추가로 15%의 보너스를 받으실 수 있습니다. 이 보너스는 24시간 동안 유효합니다: [https://aeza.net/?ref=431568](https://aeza.net/?ref=431568)

저희는 이 테스트넷 노드를 적어도 2~3개월간 운영할 계획이며, 이 링크를 통해 다음 달 결제 시 15%를 절약할 수 있습니다. AEZA를 통해 여러 가지 암호화폐 또는 국제 신용 또는 체크 카드로 stripe를 통해 지불할 수 있습니다. 따라서 다양한 결제와 서버 옵션 중에서 선택하실 수 있습니다.



그러나이 프로세스를 수행하는 데 필요한 요구 사항을 충족하는 서비스 또는 하드웨어를 자유롭게 사용할 수 있습니다.

# 서버에 연결

서버에 액세스할 자격 증명을 획득하면 SSH 클라이언트가 필요합니다. Windows 용으로는 Putty 또는 Solar Putty와 같은 무료 소프트웨어를 사용하거나 MacOs 시스템의 터미널을 사용할 수 있습니다.

본 가이드에서는 Windows 기기의 Solar Putty를 통해 실행할 것이며, 무료이면서 동일한 창에서 여러 터미널을 실행할 수 있는 더 나은 접근성 옵션을 제공하므로 후속 프로세스에 도움이 되기 때문입니다. 그러나 걱정 마세요. 필요한 경우 MacOs에서 실행하는 방법도 안내해 드릴 겁니다.



링크:

Putty: [Putty 다운로드](https://www.putty.org/)

Solar Putty: [Solar Putty 다운로드](https://www.solarwinds.com/es/free-tools/solar-putty)

Solar Putty



Solar Putty를 올바르게 설치했으면, 이제 열어주세요. 서버 IP를 바에 붙여넣고 Enter 키를 누르세요.

![image1](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_1.png)

이렇게 하면 다음 화면이 표시됩니다:

![image2](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_2.png)



여기에 사용자 이름과 비밀번호를 입력하고 서버 제공 업체는 서버에 액세스하기 위해 필요한 모든 자격 증명을 제공할 것입니다. 자격 증명을 얻으려면 이메일을 확인하거나 공급 업체 웹 사이트를 방문하여 회원 영역에 액세스하십시오.

로그인을 정확하게 완료하면 터미널이 다음과 같이 보일 것입니다:

![terminal screenshot](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_3.png)

터미널이 그림과 같이 보인다면, 준비된 것입니다!



맥OS 터미널

맥OS 사용자는 터미널을 열고 다음을 입력한 후 엔터 키를 누르세요:

```js
ssh [서버 사용자명]@[서버 IP]
```

```js
예시:

ssh root@12.34.567.890
```



올바르게 하면 비밀번호를 입력하라는 메시지가 나타납니다. 비밀번호를 입력해주세요. MacOS에서는 보안 상의 이유로 비밀번호를 입력할 때 입력 내용이 표시되지 않지만 걱정하지 마세요. 천천히 한 글자씩 입력한 후 엔터를 누르면 됩니다.

올바르게 입력했다면 다음과 같이 표시될 것입니다:

![2024-05-05-HowtorunaPufferFinanceTestnetNode_4](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_4.png)

# 퍼퍼 코랄-Cli에 대한 종속성 설치



이번 단계에서는 몇 가지 종속성을 설치해야 합니다:

아래 코드 라인을 복사하여 터미널에 붙여넣기하세요. Windows를 사용 중이라면 터미널에서 마우스 오른쪽 클릭으로 붙여넣기하고, MacOs를 사용 중이라면 Command + V로 붙여넣을 수 있습니다.

```js
sudo apt update
sudo apt install build-essential curl
```

붙여넣기한 후 Enter 키를 누르면 어느 시간 후에 터미널에서 다음을 표시합니다:



![How to run a Puffer Finance Testnet Node Step 5](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_5.png)

"Y"를 입력하고 엔터 키를 누릅니다.

필요한 패키지를 다운로드하고 설치한 후, 다시 다음과 같은 터미널이 표시됩니다.

![How to run a Puffer Finance Testnet Node Step 6](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_6.png)



그 다음, 터미널에 이 코드를 붙여넣고 엔터를 누르세요:

```js
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

그러면 이렇게 나타날 거에요:

![이미지](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_7.png)



여기서는 그냥 엔터를 누르고 패키지를 다운로드하고 설치하도록 하세요.

이후에 터미널에 이 코드를 붙여넣고 엔터를 누르세요:

```js
source $HOME/.cargo/env
```

어떤 프롬프트도 표시되지 않겠지만 걱정하지 마세요, 무언가가 변경되었습니다.



이제 이 코드를 붙여넣고 Enter 키를 누르세요:

```bash
echo "export PATH=\"$HOME/.cargo/bin:$PATH\"" >> ~/.bashrc
source ~/.bashrc
```

터미널이 "source ~/.bashrc" 코드 라인을 표시하면 준비가 된 것입니다.

이 작업 후에 터미널에 다음 코드를 붙여넣고 Enter 키를 누르세요:



```js
rustc --version
```

터미널에서 다음과 같은 내용을 보여주어야 합니다:

![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_8.png)

만약 숫자 1.77.2가 달라도 걱정하지 마세요. 여러분의 rust 버전이 달라도 괜찮습니다. 이 과정에 특정 버전이 필요하지 않기 때문에 코드가 표시되면 진행할 수 있습니다!



다음으로, Puffer Coral-CLI를 실행하기 위해 더 많은 종속성이 필요합니다. 이제 터미널에 다음을 붙여넣고 Enter 키를 눌러주세요:

```js
sudo apt update
sudo apt install libssl-dev pkg-config
```

다운로드가 완료되면 서버에서 아마 이것을 보여줄 겁니다:

![Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_9.png)



텍스트 형식으로 바꾸면 다음과 같습니다:


![How To Run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_10.png)


탭을 눌러 "확인" 옵션으로 이동한 다음 Enter 키를 누릅니다. 이후 터미널로 돌아가서 다음 코드를 붙여 넣고 Enter 키를 누르면 됩니다. 응답을 요구하지 않지만 걱정하지 마세요, 뭔가가 변경되었습니다.






그런 다음이 코드로 동일한 작업을 수행합니다. 아무 응답이 표시되지는 않지만 필요한 변경 사항을 적용할 것입니다:


echo ‘export OPENSSL_DIR=/usr/include/openssl’ >> ~/.bashrc
source ~/.bashrc


다시 한 번 이 라인도 같은 방법으로 처리해주세요:




```sh
export OPENSSL_LIB_DIR=/usr/lib/x86_64-linux-gnu
export OPENSSL_INCLUDE_DIR=/usr/include/openssl
```

And now add the following lines to your `.bashrc` file:

```sh
echo 'export OPENSSL_LIB_DIR=/usr/lib/x86_64-linux-gnu' >> ~/.bashrc
echo 'export OPENSSL_INCLUDE_DIR=/usr/include/openssl' >> ~/.bashrc
source ~/.bashrc
```

Once you've completed these steps, let's verify that our installations were successful. Type the following code into the terminal and press Enter:
```



```js
pkg-config --version
```

그런 다음 아래 내용을 붙여넣고 엔터 키를 누르세요:

```js
pkg-config --libs openssl
```

터미널은 이렇게 보일 것입니다:



![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_11.png)

If it looks like this, we are good to go. As I previously mentioned, don't worry if your version numbers are different. As long as the terminal prompts these lines, it means we have installed our libraries correctly.

We are done here with dependencies, for now...

Setting up Puffer Coral-Cli



지금 파일들을 더 조직적으로 정리하기 위해 "puffer"라는 폴더를 생성해볼게요. 터미널에서 아래 명령어를 입력하여 폴더를 생성하세요:

```js
mkdir puffer
```

mkdir은 "Make directory"의 약자로, 새로운 디렉토리를 생성하는 명령어입니다.

이제 디렉토리를 만들었지만, 그 안으로 이동하려면 다른 명령어를 입력해야 해요.



`cd puffer`

"cd"란 “Change Directory”의 약자로 현재 작업 중인 디렉토리를 변경하는 명령어입니다.

위 명령을 실행하고 나면 터미널 화면은 다음과 같아야 합니다:

![](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_12.png)



이제 여기 있으면, 이 코드를 터미널에 붙여넣고 엔터를 눌러주세요:

```js
git clone https://github.com/PufferFinance/coral
```

터미널에서 위 명령을 입력하면 coral-cli 파일이 이 폴더에 복제됩니다.

이제 이것을 터미널에 입력하고 엔터를 눌러주세요:



```js
ls 
```

![How to Run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_13.png)

현재 위치한 디렉토리의 파일과 디렉토리를 보여주는 명령어입니다. 현재 이 폴더 안에는 "coral"이라는 유일한 디렉토리가 있습니다. 따라서 터미널에 이 명령어를 입력하고 Enter 키를 누르면 이 폴더로 이동할 수 있습니다.

```js
cd coral
```



한 번 여기에 들어오면 터미널에 이것을 입력하고 엔터를 누르세요:

```js
cargo build --release
```

![Puffer Coral-Cli](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_14.png)

이렇게 하면 Puffer Coral-Cli의 모든 파일을 빌드하는 작업이 시작됩니다. 하드웨어 스펙에 따라 시간이 다소 소요될 수 있습니다.




![How to run a Puffer Finance Testnet Node 15](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_15.png)

Once it's done building the files, your terminal should look like this:

![How to run a Puffer Finance Testnet Node 16](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_16.png)

You should see "Finished" as displayed on the last green line.




만약 다음을 입력하고 엔터 키를 누르면:

```js
ls
```

이제 puffer/coral/ 디렉토리는 다음과 같아야 합니다:

![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_17.png)



만약에 위와 같이 보인다면, 다음 단계로 준비가 된 것이며 검증자 키를 생성할 것입니다.

coral-cli를 빌드하는 동안 에러가 발생하는 경우

만약이 프로세스 중에 라이브러리나 의존성을 놓친 경우에 에러가 나타나면, 다시 의존성 섹션으로 가서 모든 것을 올바르게 설치했는지 확인한 후 이 디렉토리로 돌아와서 다시 빌드를 시도하기 전에 다음 명령을 입력하고 엔터를 눌러주세요:

```js
cargo clean
```



이전 빌드를 정리할 거에요. 빌드를 다시 하려면 "cargo build -- release" 명령어를 한 번 더 입력해서 보내주세요.

# 밸리데이터 키 만들기

지금 /puffer/coral/ 디렉토리에 있는 동안에 다음 명령어를 입력하고 엔터를 눌러봐요:

```js
nano
```



이것은 다음과 같이 텍스트 에디터가 열릴 것입니다.

![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_18.png)

여기서는 밸리데이터 키를 위한 비밀번호를 작성하려고 합니다. 마음에 드는 비밀번호를 생성해 주세요. 이 비밀번호는 아무에게도 공유하지 말아 주시기 바랍니다.

저는 이 비밀번호를 예시로 사용하겠습니다:




![2024-05-05-HowtorunaPufferFinanceTestnetNode_19](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_19.png)

After entering your password, press Ctrl+O. If you are using MacOS, remember to use the "control" key, not the "command" key. You will see the following at the bottom of the text editor:

![2024-05-05-HowtorunaPufferFinanceTestnetNode_20](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_20.png)

Type "password.txt" (without the quotation marks) and press Enter. This will display: 
```



![File](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_21.png)

지금 puffer/coral/ 디렉토리에 비밀번호가 적힌 파일을 만들었습니다. 이제 Ctrl+X를 누르면 터미널로 다시 돌아옵니다. 터미널에 들어가서 파일이 제대로 생성되었는지 확인해봅시다. 다음 명령어를 입력해 엔터를 눌러주세요:

```js
ls
```

아래에서 보듯이 비밀번호.txt 파일이 이제 디렉토리에 있습니다.



![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_22.png)

To generate our registration.json file, follow these steps:

1. Once we have completed the file generation process, visit https://launchpad.puffer.fi/Setup to obtain the command needed to create our validator key.

![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_23.png)



사이트가 Metamask에 연결을 요청할 것입니다. 연결하기 전에 특별히 이 목적으로 만든 지갑이나 계정에 있는지 확인해주세요. 만약 Metamask에 Holesky Testnet 네트워크를 추가하지 않았다면, 네트워크를 추가하라는 메시지가 표시될 것입니다. "네트워크 추가"를 클릭하면 괜찮습니다.

그렇지 않다면, 이미 추가했지만 다른 체인에 있었다면, 지갑을 Holesky Testnet으로 변경하라는 메시지가 나타날 것입니다. 변경하면 됩니다.

이 작업을 완료한 후, 페이지가 다음과 같이 보일 것입니다:

![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_24.png)



안녕하세요! 이 가이드에서는 Enclave를 사용하지 않을 것이므로 여기에서 끄는 것을 잊지 마세요. 한편, 이제 우리에게 명령이 생겼습니다. 이 명령을 복사하여 메모장, 워드, 페이지, 문서 또는 비주얼 스튜디오 코드와 같은 원하는 텍스트 편집기에 붙여넣어주세요. 거기에 Enclave를 사용하지 않는다는 걸 기억하세요.

이 명령어에서 변경해야 할 부분이 있습니다. 알아보기 쉽도록 이 두 부분을 수정해야 합니다:

`키스토어_비밀번호_파일_경로` `등록_JSON_파일_경로`



`PATH_TO_A_KEYSTORE_PASSWORD_FILE` → password.txt `PATH_TO_REGISTRATION_JSON` → registration.json

Remember, `password.txt` was the file we created a few steps ago, and `registration.json` is the file that will be generated in the `puffer/coral/` directory. We will need this file in a few steps ahead.

After making these modifications, copy the command and paste it into your terminal in the `puffer/coral/` directory. Then hit enter. The command will appear as follows:




![Step 1](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_25.png)


Once you've completed that, your terminal will resemble:


![Step 2](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_26.png)


If you see that, you're all set. We will revisit this directory later in the process.



이제 puffer coral-cli를 사용하여 검증자 키를 준비했으니, 합의 및 실행 클라이언트를 설치하고 구성해야 합니다.

# 합의 클라이언트 설정

이 가이드에서는 합의 클라이언트로 Nimbus를 사용할 것입니다. 따라서 터미널에서 다음 명령을 작성하고 실행하여 메인 디렉토리로 돌아가겠습니다:

```js
cd
```



새로운 dependencies와 라이브러리를 설치해야 하므로 터미널에 다음 명령어를 붙여넣고 엔터를 눌러주세요:

```js
sudo apt-get install build-essential git-lfs cmake
```

이렇게 보일 거에요:

![Command](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_27.png)



Y
![2024-05-05-HowtorunaPufferFinanceTestnetNode_28.png](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_28.png)

![2024-05-05-HowtorunaPufferFinanceTestnetNode_29.png](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_29.png)

좋아요. 이제 다시 메인 디렉토리로 이동할 것입니다. 여기서는 이 명령을 실행해야 합니다.



```shell
openssl rand -hex 32 | tr -d "\n" > "/tmp/jwtsecret"
```

터미널에 아무 프롬프트도 표시되지 않을 것입니다. 이 명령은 우리의 `/tmp/` 디렉토리에 "jwtsecret"이라는 파일을 생성합니다. 지금 접근해야 할 것이죠. 접근하기 위해 다음을 실행합니다:

```shell
cd /tmp/
```

이 명령은 우리를 `/tmp/` 디렉토리로 이동시켜 줍니다. 여기서 우리는 다음 명령을 실행하고 싶습니다.



```js
나노 jwtsecret
```

이렇게 보이게 될 거예요:

![이미지](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_30.png)

거기 보이는 숫자들은 비밀 키이지만, 기억해 주셔야 할 점은 우리가 지정한 지갑의 개인 키를 사용하려는 것이기 때문에, 여기서 해야 할 일은 지갑의 개인 키를 붙여 넣는 것입니다. 그러니 거기에 있는 숫자를 삭제하고, Windows에서는 오른쪽 클릭, MacOS에서는 Command+V를 통해 개인 키를 텍스트 편집기에 붙여 넣으세요. 이전에 보여 드린 것처럼, 저의 예시 개인 키는 "9825b255c5a7bf027237140ebb86e11970027467d08f6a2ad84f02b6665b1f30" 이므로, 그것을 거기에 붙여 넣을 겁니다.




Private Key를 올바르게 붙여넣은 후에 Ctrl+O를 누르고 Enter를 누르고 Ctrl+X를 눌러주세요. 이렇게 하면 파일에 적용한 변경 사항을 저장한 후 터미널로 돌아갈 수 있습니다.

이 작업을 완료한 후에 우리는 메인 디렉토리로 돌아가고 싶습니다. 이를 위해 터미널에 다음 명령을 입력하고 Enter를 누르세요:

```js
cd
```

메인 디렉토리에 도착하면 다음 명령을 실행해야 합니다.



```bash
git clone https://github.com/status-im/nimbus-eth
```

이 명령어를 사용하면 nimbus 저장소를 우리의 컴퓨터에 복제할 수 있습니다. 이 작업을 완료했으면 nimbus 디렉토리로 이동하기 위해 아래 명령어를 입력하세요:

```bash
cd nimbus-eth2
```

이 프로세스를 마치면 터미널이 이렇게 보여야 합니다:



![How to Run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_31.png)

Now, let's copy and paste this command into our terminal and hit Enter:

Replace the X with the amount of RAM you wish to allocate for this task. For systems with 4GB of memory or less, simply remove the “-jX” parameter.

For instance, if your machine has 128GB RAM, you can set X to 12. Similarly, if your machine has 16GB RAM, you can set X to 8. The process may take some time, so the suggested amount is 12, but the ideal value depends on your system. In my case, I will use 12.



```js
make -jX nimbus_beacon_node
```

명령을 실행하면 터미널이 다음과 같이 나타납니다:

![How to Run a PufferFinance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_32.png)

이전에 말씀드렸듯이 이 과정은 시간이 조금 걸릴 수 있으니 그냥 필요한 것을 모두 빌드할 수 있도록 하시면 됩니다.



한번 작업을 마치면, 아래와 같이 보일 것입니다:


![Image](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_33.png)


이 작업을 마치면, 합의 클라이언트를 동기화해야 합니다. 보통 걸리는 시간을 줄이기 위해 신뢰할 수 있는 노드에서 신뢰할 수 있는 동기화를 수행할 것입니다. 이를 위해 다음 명령어를 실행할 것입니다:

```js
build/nimbus_beacon_node trustedNodeSync \
  --network:holesky \
  --data-dir=build/data/shared_holesky_0 \
  --trusted-node-url=https://holesky-checkpoint-sync.stakely.io/
```



이제 당신의 터미널은 이렇게 보일 것입니다:

![Terminal Screenshot](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_34.png)

그리고 끝나면 이 메시지가 표시됩니다:

![Terminal Screenshot](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_35.png)



요청한 작업을 마치고 나서는 다음 몤령을 실행해야 합니다:

우선, 해당 명령어에서 "WALLET_ADDRESS"를 여러분의 지갑 주소로 바꿔야 합니다.

```js
./run-holesky-beacon-node.sh --web3-url=http://127.0.0.1:8551 --suggested-fee-recipient=WALLET_ADDRESS  --jwt-secret=/tmp/jwtsecret
```

이 가이드에서 사용 중인 예제 주소는 다음과 같습니다: 0xB0551d678a6746F2Cf300CF698d314EA1e74e3c7



그래서 나는 그것을이 주소로 대체하겠고 내 것은 이렇게 보일 거야 (하지만 당신 것은 대신 당신의 지갑 주소여야 함):

```js
./run-holesky-beacon-node.sh --web3-url=http://127.0.0.1:8551 --suggested-fee-recipient=0xB0551d678a6746F2Cf300CF698d314EA1e74e3c7  --jwt-secret=/tmp/jwtsecret
```

수정한 후에 이것을 복사하여 터미널에 붙여넣고 실행하면 다음과 같이 보일 것입니다:

![How to Run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_36.png)



이렇게 보이기를 기다려주세요:

![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_37.png)

노드 동기화를 일단 중지해주세요. 나중에 다시 완료할게요.

이제 퍼퍼 검증자 키 경로로 이동해야 해요. 여기서 바로 이동하려면 이 명령어를 사용하세요:



To get to the puffer validator keys path, type the following command in your terminal:

```bash
cd ~/puffer/coral/etc/keys/bls_keys
```

Then, inside this directory, run:

```bash
ls
```

You will see a file with a long string of numbers and letters. This file needs to be copied to 2 paths on our consensus client. Let's begin.



먼저 파일 이름을 복사하세요. 마우스를 사용하여 파일 이름을 선택한 후, Windows에서는 Ctrl+C를 사용하고, MacOs에서는 Command+C를 사용하세요. 복사한 후에는 현재 메모장이나 Notes와 같은 텍스트 편집기에 붙여넣기하세요. Windows에서는 터미널에 "^C"가 표시될 수 있지만 걱정하지 마세요. 아무 영향을 미치지 않습니다.

이 작업을 완료한 후에 이 명령을 실행하기 전에 명령을 수정해야 합니다.

"REPLACE"를 파일 이름으로 변경해야 합니다.

```js
cp -v ~/puffer/coral/etc/keys/bls_keys/REPLACE ~/nimbus-eth2/build/data/shared_holesky_0/validators/
```



예를 들어 파일 이름이 다음과 같다고 가정해봅시다: 7742nh471h20471h2409127943h021947h012947h0129834b8213g0d2jds0921dhn721908ed721dd021987h21dhd0

그렇다면 최종 명령어는 다음과 같이 보일 것입니다:

```js
cp -v ~/puffer/coral/etc/keys/bls_keys/7742nh471h20471h2409127943h021947h012947h0129834b8213g0d2jds0921dhn721908ed721dd021987h21dhd0 ~/nimbus-eth2/build/data/shared_holesky_0/validators/
```

수정이 완료되면 터미널에 붙여넣고 실행하세요. 이 명령어를 실행하면 파일이 첫 번째 디렉토리로 복사되며, 터미널은 디렉토리를 표시하는 프롬프트를 표시할 것입니다.



자 이제 해당 파일을 다음 디렉토리로 복사하기 위해 방금 사용한 명령어를 다음과 같이 수정해야 합니다:

```bash
mkdir ~/nimbus-eth2/validator_keys/ && cp -v ~/puffer/coral/etc/keys/bls_keys/7742nh471h20471h2409127943h021947h012947h0129834b8213g0d2jds0921dhn721908ed721dd021987h21dhd0 ~/nimbus-eth2/validator_keys/keystore.json
```

그런 다음, 저희는 puffer coral-cli를 통해 생성한 키를 사용하여 동기화 및 실행할 준비가 된 합의 클라이언트를 가지게 되었어요. 하지만 이 작업은 나중에 처리할 거예요. 지금은 실행 클라이언트를 설정해야 합니다. 실행 클라이언트는 합의 클라이언트와 함께 동기화되어 실행되어야 합니다.

이 작업을 마치면 다음 명령어를 실행하여 주요 디렉토리로 돌아갑니다:



```js
cd
```

# 실행 클라이언트 설정

이 가이드에서는 실행 클라이언트로 Nethermind를 사용할 것이므로 필요한 종속성을 설치하고 작업을 완료해보겠습니다.

먼저 다음 명령을 실행해야 합니다.



```js
wget https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

그런 다음 다음 명령어들을 실행하세요:

```js
sudo apt-get update
sudo apt-get install dotnet-sdk-8.0 dotnet-runtime-8.0
```

![이미지](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_38.png)




![그림 설명](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_39.png)

![그림 설명](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_40.png)

닷넷 설치가 제대로 되었는지 확인하기 위해 터미널에서 아래 명령어를 실행해봅시다:

```js
dotnet --list-sdks
dotnet --list-runtimes
```



자 그럼 다음 단계로 넘어가볼게요. 우리는 실행 클라이언트를 우리의 기계로 클론할 준비가 돼 있어요. 이를 위해 메인 디렉토리에서 다음 명령을 실행할 거에요:

```js
git clone https://github.com/NethermindEth/nethermind.git
```



클론이 완료될 때까지 기다렸다가 이 명령어를 사용하여 디렉토리를 nethermind의 src 폴더 내의 Nethermind 폴더로 변경합니다:

```js
cd nethermind/src/Nethermind/
```

여기에 들어가면 이 명령어를 실행해야 합니다:

```js
dotnet build Nethermind.sln -c Release
```



그리고 모든 필요한 파일을 만들면 다음과 같이 보일 것입니다:


![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_42.png)


작업이 완료되면 다음과 같이 보일 것입니다:


![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_43.png)




이제 실행 클라이언트를 실행할 준비가 되었습니다. 그러나 먼저 합의 및 실행 클라이언트를 실행하기 전에 몇 단계를 거쳐야 합니다.

# Hodlsky 테스트넷 ETH 수령하기

우리의 지정된 지갑을 테스트넷 ETH로 자금을 조달해야 합니다. 이를 위해 다음 POW Faucet을 사용할 것입니다: [Hodlsky Faucet](https://holesky-faucet.pk910.de/)



지갑 주소를 붙여넣기하고 채굴 시작을 클릭하면 다음과 같이 로딩됩니다:

![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_45.png)

여기서 2시간 정도 후에는 적어도 36 ~ 40 Holesky ETH를 얻게 됩니다. 이런 금액을 얻으면 “채굴 중지”를 클릭한 후 “보상 청구”를 클릭하면 시스템이 자동으로 귀하의 지갑으로 자금을 전송합니다. 이것은 테스트넷 토큰이므로 통화 가치는 없습니다.

![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_46.png)



헐스키 이더리움을 얻는 유일한 방법은 아닙니다. 다른 수도꼭지를 사용하거나 다른 지갑에 있는 헐스키 이더리움을 사용할 수도 있습니다. https://www.holeskyeth.org/buy-holesky-eth 같은 유료 서비스도 있습니다. 약 13달러에 50개의 헐스키 이더리움을 얻을 수 있어요. 따라서 전체 과정을 가속화할 시간이나 돈을 사용할 의향이 있는지에 따라 다릅니다. 돈을 보내기 전에 꼭 직접 조사를 해야 하며, 여기서는 단지 헐스키 이더리움을 얻는 다양한 방법과 예시를 제공할 뿐이니, 혹시 발생하는 모든 일은 본인의 책임 하에 이루어져야 합니다.

# 헐스키 런치패드 등록

지갑에 최소 36개에서 40개의 헐스키 이더리움이 있다면, 우리는 Holesky 런치패드에 가서 발행자를 등록해야 합니다.

[Holesky 런치패드](https://holesky.launchpad.ethereum.org/en/)로 이동하세요.



![2024-05-05-HowtorunaPufferFinanceTestnetNode_47](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_47.png)

![2024-05-05-HowtorunaPufferFinanceTestnetNode_48](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_48.png)

![2024-05-05-HowtorunaPufferFinanceTestnetNode_49](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_49.png)

![2024-05-05-HowtorunaPufferFinanceTestnetNode_50](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_50.png)



![screenshot](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_51.png)  
이제 스테이킹 예금 CLI를 설치하여 예금 데이터를 생성해야 합니다. 이를 위해 메인 디렉토리로 돌아가는 다음 명령어를 실행해주세요:

```js
cd
```

그런 다음 다음 명령어를 실행하여 새 디렉토리를 생성해보세요.



```sh
mkdir stakingdepositcli
cd stakingdepositcli
```

새 디렉토리를 만들고 현재 디렉토리를 해당 디렉토리로 변경합니다. 이제 다음 명령어를 사용해야 합니다:

```sh
wget -O sdc.tar.gz https://github.com/ethereum/staking-deposit-cli/releases/download/v2.7.0/staking_deposit-cli-fdab65d-linux-amd64.tar.gz
```

참고: 이 패키지는 인텔 또는 AMD x64 프로세서를 사용하는 Linux 기기에서만 작동합니다. ARM 프로세서 또는 다른 유형의 하드웨어를 사용 중이라면 시스템에 필요한 패키지를 확인하려면 https://github.com/ethereum/staking-deposit-cli/releases/를 먼저 확인하세요.



다운로드한 파일을 해제하려면 이 명령어를 실행해야 합니다:

```js
tar -xzvf sdc.tar.gz
```



![image](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_53.png)

이제 새로 추출한 디렉토리로 이동해야 합니다. 다음 명령어를 사용하여 실행하세요:

```js
cd staking_deposit-cli-fdab65d-linux-amd64
```

참고: 우리는 amd64 아키텍처를 사용하기 때문에 이것이 디렉토리 이름입니다. ARM 또는 다른 유형의 OS를 사용하는 경우 디렉토리 이름이 다를 수 있습니다. 먼저 "ls" 명령어를 사용하여 파일 이름을 확인하고 해당 디렉토리로 이동하십시오.



이제 이 작업을 완료했으니 Holesky Launchpad로 돌아가 페이지 하단으로 이동해보세요. 거기에 이 내용을 찾을 수 있을 거에요:

![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_54.png)

Holesky Launchpad에서 해당 명령어를 복사하고 터미널에 붙여넣은 후 실행해보세요. 정상적으로 작동할 거에요:

![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_55.png)




![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_56.png)

![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_57.png)

![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_58.png)

![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_59.png)




![이미지](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_60.png)

![이미지](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_61.png)

이제 이 작업을 마무리했으니, 입금 데이터가 있는 디렉토리로 이동해 봅시다. 다음 명령어를 사용하세요:

```js
cd validator_keys
```



여기서는 이 디렉토리의 파일을 확인하는 명령을 사용하십시오:

```js
ls
```

![이미지](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_62.png)

지금 해야 할 일은 입금 데이터 파일을 Holesky Launchpad를 열어둔 주 pc로 전송하는 것입니다. 이를 복사하여 붙여넣을 수 없으며, 이 파일을 깨끗하게 전송하기 위해 특정 명령을 사용해야 합니다. 그렇지 않으면 Holesky Launchpad에 업로드할 때 실패할 것입니다.



먼저 파일의 이름을 복사하세요. 파일 이름을 드래그한 후 Windows에서는 Ctrl+C를 사용하세요. Windows에서는 Ctrl+C를 누른 후에 터미널 라인이 나타납니다. 걱정마세요, 이건 정상입니다. 파일 이름을 원하는 텍스트 편집기에 붙여넣으세요.

그런 다음 Windows에서 명령 프롬프트를 열어야 합니다.

- Windows 키 + R을 누릅니다.
- cmd를 입력한 후 Enter를 누르세요. 이렇게 하면 터미널이 열립니다.



MACOS에서

- Command+Space를 눌러주세요.
- "Terminal"을 입력하면 터미널 앱이 나타날 것입니다. 그것을 엽니다.

이후에 다음 단계를 따라주세요:

아래 명령어를 실행해야 합니다: 



```js
scp username@remote_host:~/stakingdepositcli/staking_deposit-cli-fdab65d-linux-amd64/validator_keys/deposit_data-1713341827.json C:\Users\PC\Desktop

"username"은 서버의 사용자 이름으로 변경해주세요. 저의 경우엔 "root"입니다.
"remote_host"는 서버의 IP 주소로 대체해주세요. 예시로 0.0.0.0을 사용하겠습니다.

그래서 윈도우에서 실행하는 명령어는 다음과 같을 것입니다:
scp root@0.0.0.0:~/stakingdepositcli/staking_deposit-cli-fdab65d-linux-amd64/validator_keys/deposit_data-1713341827.json C:\Users\PC\Desktop

맥OS에서 실행하는 명령어는 다음과 같을 것입니다:
scp root@0.0.0.0:~/stakingdepositcli/staking_deposit-cli-fdab65d-linux-amd64/validator_keys/deposit_data-1713341827.json ~/Desktop
```

![How to Run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_63.png)

![How to Run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_64.png)

여기까지 오셨으면, puffer coral cli 디렉토리에서 registration.json 파일을 다운로드해봅시다. 곧 필요해질 것이기 때문에 미리 다운로드 받아두는 것이 좋습니다. 다음 명령어를 실행하시면 됩니다:```



```js
scp username@remote_host:~/puffer/coral/registration.json C:\Users\PC\Desktop

"username"을 서버의 사용자 이름으로 변경하세요. 예를 들어 제 경우에는 "root"입니다.
"remote_host"를 서버의 IP 주소로 변경하세요. 예시로 0.0.0.0을 사용하겠습니다.

윈도우에서는 다음과 같이 명령어를 입력합니다:
scp root@0.0.0.0:~/puffer/coral/registration.json C:\Users\PC\Desktop

맥OS에서는 다음과 같이 입력할 수 있습니다:
scp root@0.0.0.0:~/puffer/coral/registration.json ~/Desktop
```

이제 "Yes"를 쓰는 첫 번째 프롬프트를 더 이상 묻지 않습니다. 이제 서버 비밀번호를 입력하라는 메시지만 표시됩니다. 비밀번호를 입력하고 Enter 키를 누르세요. 파일이 Windows 또는 맥OS의 데스크탑으로 다운로드됩니다.

이제 시스템에 입금 데이터 및 Registration .json 파일이 올바르게 저장되었습니다. 그러나 먼저 Holesky Launchpad에서 등록을 마무리해야 합니다. 지금까지 있던 페이지의 아래부분에 있는 "Continue"를 클릭하면 아래 페이지로 이동합니다:

![How to Run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_65.png)```



여기서 방금 다운로드한 "입금 데이터" 파일을 업로드하려고 합니다. 파일을 업로드해주세요.

![Deposit Data Upload](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_66.png)

만약 파일이 서버에서 PC로 제대로 다운로드되었다면 확인 표시가 나타날 것입니다. "계속"을 클릭해주세요.

![Continue After Successful Download](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_67.png)




![How to run a Puffer Finance Testnet Node 68](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_68.png)

![How to run a Puffer Finance Testnet Node 69](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_69.png)

![How to run a Puffer Finance Testnet Node 70](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_70.png)

![How to run a Puffer Finance Testnet Node 71](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_71.png)





![image](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_72.png)

![image](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_73.png)

![image](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_74.png)

![image](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_75.png)




![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_76.png)

우리가 이것을 한 이제, 우리는 합의 및 실행 클라이언트의 동기화를 계속할 수 있습니다. 서버로 돌아가서 다음 명령어를 사용하여 메인 디렉토리로 돌아갑시다:

```js
cd
```

# 합의 및 실행 클라이언트 실행하기



이를 위해 서버에 Screen을 설치해야 합니다. Screen을 설치하면 터미널을 종료해도 클라이언트가 계속 실행될 수 있습니다. 다음 명령어를 실행하세요:

```js
sudo apt update
sudo apt install screen
```

설치되었는지 확인하려면 이 명령어를 실행하세요:

```js
screen --version
```



이제 우리는 우리의 합의 클라이언트를 위한 화면을 생성해야 합니다. 이를 위해 다음 명령을 사용하세요:

```js
screen -S consensus
```

이 명령을 실행하면 다음과 같이 일반 명령 줄이 나타나는 화면이 보입니다:



![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_78.png)

It may appear as if we haven't done anything, but we have. We've successfully created a session named "consensus" to ensure that our consensus client continues to run even after we've closed our terminal or SSH client.

To run the consensus client, simply enter the following command:



```js
cd ~/nimbus-eth2
```

해당 디렉토리 안으로 들어가시면 다음 명령어를 실행해주세요:

```js
./run-holesky-beacon-node.sh --web3-url=http://127.0.0.1:8551 --suggested-fee-recipient=WALLET --jwt-secret=/tmp/jwtsecret
```

"WALLET"을(를) 지정한 지갑 주소로 바꿔주세요.

예시:
제 경우에는 다음과 같을 것입니다:
```js
./run-holesky-beacon-node.sh --web3-url=http://127.0.0.1:8551 --suggested-fee-recipient=0xB0551d678a6746F2Cf300CF698d314EA1e74e3c7 --jwt-secret=/tmp/jwtsecret
```

![이미지](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_79.png)




![How to Run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_80.png)

For now, we can leave this here, depending on your network, this can take from 2 to 4 hours or more.

### Running the Execution Client

Now, let's open a new SSH client. You can open a new window of PuTTY, a new tab on Solar Putty, or open a new terminal on MacOS. Don't worry; you can run multiple clients on the same server.



위 단계를 완료하셨다면, 다음 명령어를 사용하여 "execution"이라는 새로운 스크린 세션을 생성해 보세요:

```bash
screen -S execution
```

[2024-05-05-HowtorunaPufferFinanceTestnetNode_81.png](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_81.png)

이 명령을 실행하면 다음과 같은 일반적인 명령줄 화면으로 이동됩니다:



![Crypto Image](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_82.png)

Hey there! So, it might not look like much, but trust me, we just accomplished something pretty cool. We've set up a session called "execution" that keeps our execution client up and running even if we close our terminal or ssh client.

Now, let's head over to our Nethermind directory to start our execution client by using this command:

```js
cd ~/nethermind/src/Nethermind/Nethermind.Runner
```



여기서 이 명령을 실행하십시오:

```js
dotnet run -c Release --   --config=holesky   --datadir="../../../../nethermind-datadir"   --JsonRpc.Host=0.0.0.0   --JsonRpc.JwtSecretFile=/tmp/jwtsecret
```

그러면 다음과 같이 합의 클라이언트와 동기화를 시작합니다:

![How to Run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_83.png)




![How to run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_84.png)


그래서 이제 우리가 Consensus 클라이언트 동기화하고, 실행 클라이언트를 연결했으니, 그냥 완료될 때까지 기다리면서 우리의 Puffer 대시보드로 돌아가서 등록을 완료하면 됩니다.

참고: jwtsecret를 생성한 후에 시스템을 다시 부팅하면, /tmp/ "임시" 디렉토리에 저장되었으므로 삭제됩니다. 하지만 걱정하지 마세요. Consensus 또는 Execution 클라이언트를 실행하기 전에 과정을 반복할 수 있습니다. 아무것도 변경되지 않습니다.

# Puffer 대시보드에서 등록 완료하기



다시 한번 가보세요: [Puffer Finance Launchpad](https://launchpad.puffer.fi/Setup)

![Step 1](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_85.png)

![Step 2](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_86.png)

![Step 3](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_87.png)



![Image](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_88.png)

If you'd like to add the pufETH and Validator Ticket contracts to your MetaMask, here they are:

Holesky pufETH Contract: 0x98408eadD0C7cC9AebbFB2AD2787c7473Db7A1fa

Holesky Puffer Validator Ticket Contract: 0xA143c6bFAff0B25B485454a9a8DB94dC469F8c3b




![Image](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_89.png)

![Image](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_90.png)

Remember the file called “registration.json” we downloaded earlier? This is where we need it

![Image](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_91.png)




파일을 업로드하고 파일을 확인하면, 모두 정상이면 Metamask에서 2개의 거래에 서명하고 1개의 거래를 확인하라는 메시지가 표시됩니다. 이를 수행한 후에 다음 링크로 이동하세요: [https://launchpad.puffer.fi/Dashboard](https://launchpad.puffer.fi/Dashboard)

![image1](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_92.png)

![image2](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_93.png)

![image3](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_94.png)




![How to Run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_95.png)

이렇게 하면 콘센서스와 실행 클라이언트가 활성화 대기열(Beacon Chain)이 종료되기 전에 완전히 동기화되었는지 확인하기만 하면 됩니다.

클라이언트들이 서로 및 블록체인과 동기화되었으면, 활성화 대기열이 종료될 때까지 기다리기만 하면 됩니다. 그럼 beaconcha.in 페이지가 다음과 같이 표시될 것입니다:

![How to Run a Puffer Finance Testnet Node](/assets/img/2024-05-05-HowtorunaPufferFinanceTestnetNode_96.png)




이제 holesky 테스트넷에서 Puffer NoOp Validator를 실행하는 전체 과정을 알았습니다. 궁금한 점이 있으시면 Puffer Finance Discord의 노드 운영자 섹션에 방문하여 질문하거나 도움을 받기 위해 티켓을 여는 것을 망설이지 마세요.

이 안내서가 많은 도움이 되기를 바라며, 건배해요!

Steven Bravo

Twitter: [https://twitter.com/stevenbravo_](https://twitter.com/stevenbravo_)



Puffer Finance Discord: [https://discord.gg/pufferfi](https://discord.gg/pufferfi)