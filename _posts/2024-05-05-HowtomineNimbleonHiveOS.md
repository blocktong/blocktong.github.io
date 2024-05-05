---
title: "하이브 OS에서 Nimble을 채굴하는 방법하이브 OS에서 Nimble을 채굴하는 방법을 알고 싶으시죠 이를 위해서는 먼저 Hive OS 웹 대시보드에 로그인해야 합니다 그런 다음, 원하는 채굴 기기를 선택하고 Nimble을 채굴할 풀pool을 설정해야 합니다 그 후 채굴 소프트웨어 설정을 편집하여 Nimble의 채굴을 시작할 수 있습니다 이제 Nimble을 채굴하는 방법을 알았으니, Hive OS를 통해 쉽게 수익을 창출할 수 있을 것입니다"
description: ""
coverImage: "/assets/img/2024-05-05-HowtomineNimbleonHiveOS_0.png"
date: 2024-05-05 16:23
ogImage: 
  url: /assets/img/2024-05-05-HowtomineNimbleonHiveOS_0.png
tag: Tech
originalTitle: "How to mine Nimble on Hive OS"
link: "https://medium.com/@owlyeagle/how-to-mine-nimble-on-hive-os-b15980ce48d3"
---


중요: 귀하의 특정 리그가 충분한 저장 용량을 갖고 있는지 확인하세요. 현재로서는 최소 요구 사항이 128GB이며, 권장 공간은 256GB입니다.

환경 설정 준비:

- 🚀 아이콘으로 이동하고 “unset current FS”를 선택하세요.

![이미지](/assets/img/2024-05-05-HowtomineNimbleonHiveOS_0.png)



- 터미널 아이콘을 클릭하여 "하이브 쉘 시작"을 선택하세요.

![이미지](/assets/img/2024-05-05-HowtomineNimbleonHiveOS_1.png)

- 우분투 터미널에 접속하려면 "`하이브 쉘`"을 클릭하세요.

![이미지](/assets/img/2024-05-05-HowtomineNimbleonHiveOS_2.png)  



```markdown
![Image](/assets/img/2024-05-05-HowtomineNimbleonHiveOS_3.png)

To set up Python3 venv:

Make sure you have Python 3.9 or a newer version along with pip3 installed.

For Linux:
```



![이미지](/assets/img/2024-05-05-HowtomineNimbleonHiveOS_4.png)

```bash
sudo apt update
```

```bash
sudo apt install python3-venv
```

Nimble Miner Repository를 복제하세요:



- Nimble Miner를 위한 디렉토리를 생성하세요:

```shell
mkdir $HOME/nimble
```

```shell
cd $HOME/nimble
```

- Nimble Miner 저장소를 복제하세요:



```sh
git clone https://github.com/nimble-technology/nimble-miner-public.git
```

```sh
cd nimble-miner-public
```

업데이트 요구 사항:

- 기존 요구 사항 파일 삭제하기:



```bash
sudo rm requirements.txt
```

- HiveOS와 호환되는 수정된 버전으로 새로운 requirements 파일을 생성해보세요:

```bash
echo '
requests==2.31.0
torch==1.9.1
accelerate==0.27.0
transformers==4.38.1
datasets==2.17.1
numpy==1.24
gitpython==3.1.42' > requirements.txt
```

Nimble Miner을 설치해보세요:



- 수정된 요구 사항을 갖춘 Nimble Miner를 설치하세요:

```js
make install
```

Nimble Miner를 활성화하세요:

- 마이너를 활성화하세요:



```js
source ./nimenv_localminers/bin/activate
```

마이너 실행:

- Nimble 지갑 주소를 사용하여 채굴을 시작하세요:

```js
make run addr=<여기에 "nimblexxx" 지갑 주소를 붙여넣으세요>
```



`nimblexxx` 월렛 주소를 여러분의 실제 Nimble 월렛 주소로 교체해 주세요. 어떤 단계든 추가 설명이 필요하다면 알려주세요!

즐거운 채굴되세요!