---
title: "루스트로 블록체인 구현하기"
description: ""
coverImage: "/assets/img/2024-05-05-ImplementingaBlockchaininRust_0.png"
date: 2024-05-05 17:33
ogImage: 
  url: /assets/img/2024-05-05-ImplementingaBlockchaininRust_0.png
tag: Tech
originalTitle: "Implementing a Blockchain in Rust"
link: "https://medium.com/dev-genius/implementing-a-blockchain-in-rust-28cce9a3c968"
---


![Implementing a Blockchain in Rust](/assets/img/2024-05-05-ImplementingaBlockchaininRust_0.png)

안녕하세요! 이 기사는 Rust에서 기본적인 블록체인을 구현하는 단계별 방법을 제공합니다. 블록 구조의 초기 설정부터 시작하여 고유 식별자와 암호 해시를 포함하고, 블록 생성, 채굴, 그리고 유효성 검사로 그 기초를 마련합니다. 각 함수의 밑바탕과 그 이유를 설명하여 작업 증명, 논스 계산, 블록체인의 무결성과 연속성을 유지하는 메커니즘에 대한 통찰을 제공합니다.

참고: 이는 학습 목적을 위한 베어본과 기본 블록체인 구현이며 프로덕션 환경에는 사용하지 않는 것이 좋습니다! :)

자, 함께 Rustaceans 여러분, 바로 들어가 봅시다!



# 기본 사항

블록체인의 P2P 네트워크 상호작용 처리에서 핵심 개념과 주요 요소는 다음과 같습니다:

## 노드와 피어 찾기

- 노드 식별: 블록체인 네트워크의 각 노드는 암호화 키 쌍에서 파생된 고유 식별자를 갖습니다.
- 피어 찾기: 노드는 네트워크를 형성하기 위해 서로를 발견해야 합니다. 미리 정의된 피어(정적 구성), DNS 기반 발견, 또는 로컬 네트워크 발견을 위해 mDNS와 같은 프로토콜을 사용하는 등, 다양한 방법으로 이를 달성할 수 있습니다.
- 부트스트랩 노드: 새로운 노드는 종종 신뢰할 수 있는 알려진 노드(부트스트랩 노드)에 연결하여 빠르게 네트워크에 통합됩니다.



## 네트워크 프로토콜

- 프로토콜 스택: 블록체인 P2P 네트워크는 특정 프로토콜 스택을 사용하여 통신합니다. 주로 사용되는 프로토콜로는 기본 전송을 위한 TCP/IP와 안전한 통신을 위한 암호 프로토콜(예: TLS 또는 Noise)이 있습니다.
- 메시징 프로토콜: Floodsub이나 Gossipsub과 같은 프로토콜은 메시지 방송 및 네트워크 전파를위해 사용됩니다.

## 데이터 전파 및 동기화

- 방송: 노드는 트랜잭션 및 새롭게 채굴된 블록을 네트워크로 방송하여 모든 참가자가 최신 데이터를 수신하도록합니다.
- 체인 동기화: 노드는 블록체인 복사본을 가장 긴 체인(일반적으로 유효한 것으로 인정됨)에 동기화하여 네트워크의 일관성을 유지합니다.
- 합의 메커니즘: 합의 알고리즘인 작업 증명(PoW) 또는 지분 증명(PoS) 등이 새로운 블록을 검증하고 추가하기위해 특히 블록체인의 상태에 대한 합의에 사용됩니다.



이러한 핵심 개념을 이해했다면, 이제 코드로 넘어가 봅시다!

# cargo.toml — 다음 종속성을 추가하세요

```toml
[dependencies]
chrono = "0.4"
sha2 = "0.9.8"
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"
libp2p = { version = "0.40.0", features = ["tcp-tokio", "mdns"] }
tokio = { version = "1.0", features = ["io-util", "io-std", "macros", "rt", "rt-multi-thread", "sync", "time"] }
hex = "0.4"
once_cell = "1.5"
log = "0.4"
pretty_env_logger = "0.4"
```

우리는 많은 시간을 잡아먹지 않도록 모든 라이브러리를 자세히 다루지 않겠습니다. 하지만 P2P 구현에 중요한 라이브러리로써 libp2p를 언급해 봅니다.



저는 libp2p에 대해 자세한 기사를 작성했습니다. 여기서 이 풍부한 Rust 크레이트에 익숙해질 수 있습니다.

## blockchain.rs — 이벤트 처리 및 P2P 통신

우리가 이벤트 처리 및 p2p 방법을 구현할 'blockchain.rs'라는 새 파일을 만들어봅시다.

### 기본 설정



- KEYS, PEER_ID, CHAIN_TOPIC, BLOCK_TOPIC: 이러한 정적 변수는 신원을 위한 암호화 키, 네트워크 노드를 위한 피어 ID, 그리고 Floodsub 프로토콜을 사용하여 체인 및 블록 관련 메시지를 처리하기 위한 주제를 초기화합니다.

```rust
use super::{App, Block};
use libp2p::{
    floodsub::{Floodsub, FloodsubEvent, Topic},
    identity,
    mdns::{Mdns, MdnsEvent},
    swarm::{NetworkBehaviourEventProcess, Swarm},
    NetworkBehaviour, PeerId,
};
use log::{error, info};
use once_cell::sync::Lazy;
use serde::{Deserialize, Serialize};
use std::collections::HashSet;
use tokio::sync::mpsc;

pub static KEYS: Lazy<identity::Keypair> = Lazy::new(identity::Keypair::generate_ed25519);
pub static PEER_ID: Lazy<PeerId> = Lazy::new(|| PeerId::from(KEYS.public()));
pub static CHAIN_TOPIC: Lazy<Topic> = Lazy::new(|| Topic::new("chains"));
pub static BLOCK_TOPIC: Lazy<Topic> = Lazy::new(|| Topic::new("blocks"));
```

# 이벤트 유형

- ChainResponse, LocalChainRequest, EventType: 이러한 데이터 구조는 노드가 보내고 받을 수 있는 이벤트 및 메시지 유형을 정의합니다. ChainResponse 및 LocalChainRequest는 체인 요청에 응답하고 로컬 체인 상태를 요청하는 데 사용됩니다.



```rust
#[derive(Debug, Serialize, Deserialize)]
pub struct ChainResponse {
    pub blocks: Vec<Block>,
    pub receiver: String,
}

#[derive(Debug, Serialize, Deserialize)]
pub struct LocalChainRequest {
    pub from_peer_id: String,
}

pub enum EventType {
    LocalChainResponse(ChainResponse),
    Input(String),
    Init,
}
```

## AppBehaviour

- NetworkBehaviour Implementation (AppBehaviour): This struct implements the NetworkBehaviour trait, combining different behaviours like Floodsub (for pub/sub messaging) and mDNS (for local network peer discovery). It also holds channels for sending responses and initializing events and an instance of the App struct which contains the blockchain logic.

```rust
#[derive(NetworkBehaviour)]
#[behaviour(out_event="Event")]
pub struct AppBehaviour {
    pub floodsub: Floodsub,
    pub mdns: Mdns,
    #[behaviour(ignore)]
    pub response_sender: mpsc::UnboundedSender<ChainResponse>,
    #[behaviour(ignore)]
    pub init_sender: mpsc::UnboundedSender<bool>,
    #[behaviour(ignore)]
    pub app: App,
}

#[derive(Debug)]
pub enum Event {
    ChainResponse(ChainResponse),
    Floodsub(FloodsubEvent),
    Mdns(MdnsEvent),
    Input(String),
    Init,
}

impl From<FloodsubEvent> for Event {
    fn from(event: FloodsubEvent) -> Self {
        Self::Floodsub(event)
    }
}

impl From<MdnsEvent> for Event {
    fn from(event: MdnsEvent) -> Self {
        Self::Mdns(event)
    }
}

impl AppBehaviour {
    pub async fn new(
        app: App,
        response_sender: mpsc::UnboundedSender<ChainResponse>,
        init_sender: mpsc::UnboundedSender<bool>,
    ) -> Self {
        let mut behaviour = Self {
            app,
            floodsub: Floodsub::new(*PEER_ID),
            mdns: Mdns::new(Default::default())
                .await
                .expect("can create mdns"),
            response_sender,
            init_sender,
        };

        behaviour.floodsub.subscribe(CHAIN_TOPIC.clone());
        behaviour.floodsub.subscribe(BLOCK_TOPIC.clone());

        behaviour
    }
}
```



# 이벤트 처리

- FloodsubEvent 및 MdnsEvent를 위한 NetworkBehaviourEventProcess: 이러한 구현은 응용 프로그램이 다양한 네트워크 이벤트에 반응하는 방식을 정의합니다.
- Floodsub 이벤트: 새 블록, 체인 응답 또는 로컬 체인 요청과 같은 블록체인 관련 메시지를 처리합니다. 예를 들어, 새 블록이 수신되면 try_add_block을 통해 블록체인에 추가됩니다.
- mDNS 이벤트: 로컬 네트워크에서 새 피어의 발견 또는 기존 피어의 손실을 처리합니다. 이는 Floodsub 프로토콜에서 피어 목록을 업데이트합니다.

```rust
// 수신 이벤트 핸들러
impl NetworkBehaviourEventProcess<FloodsubEvent> for AppBehaviour {
    fn inject_event(&mut self, event: FloodsubEvent) {
        if let FloodsubEvent::Message(msg) = event {
            if let Ok(resp) = serde_json::from_slice::<ChainResponse>(&msg.data) {
                if resp.receiver == PEER_ID.to_string() {
                    info!("{}로부터 응답:", msg.source);
                    resp.blocks.iter().for_each(|r| info!("{:?}", r));

                    self.app.blocks = self.app.choose_chain(self.app.blocks.clone(), resp.blocks);
                }
            } else if let Ok(resp) = serde_json::from_slice::<LocalChainRequest>(&msg.data) {
                info!("{}로부터 로컬 체인 전송", msg.source.to_string());
                let peer_id = resp.from_peer_id;
                if PEER_ID.to_string() == peer_id {
                    if let Err(e) = self.response_sender.send(ChainResponse {
                        blocks: self.app.blocks.clone(),
                        receiver: msg.source.to_string(),
                    }) {
                        error!("채널을 통해 응답 전송 중 오류 발생, {}", e);
                    }
                }
            } else if let Ok(block) = serde_json::from_slice::<Block>(&msg.data) {
                info!("{}로부터 새 블록 수신", msg.source.to_string());
                self.app.try_add_block(block);
            }
        }
    }
}

impl NetworkBehaviourEventProcess<MdnsEvent> for AppBehaviour {
    fn inject_event(&mut self, event: MdnsEvent) {
        match event {
            MdnsEvent::Discovered(discovered_list) => {
                for (peer, _addr) in discovered_list {
                    self.floodsub.add_node_to_partial_view(peer);
                }
            }
            MdnsEvent::Expired(expired_list) => {
                for (peer, _addr) in expired_list {
                    if !self.mdns.has_node(&peer) {
                        self.floodsub.remove_node_from_partial_view(&peer);
                    }
                }
            }
        }
    }
}
```

# 유틸리티 함수



- get_list_peers: 네트워크에서 발견된 피어 목록을 반환합니다.
- handle_print_peers: 콘솔에 피어 목록을 로깅합니다.
- handle_print_chain: 로컬 블록체인 상태를 기록하며, 블록체인의 시각적 표현을 제공합니다.
- handle_create_block: 사용자 입력을 처리하여 새 블록을 생성합니다. 제공된 데이터로 새 블록을 생성하고, 로컬 블록체인을 업데이트하고, Floodsub을 사용하여 새 블록을 피어에 브로드캐스트합니다.

```rust
pub fn get_list_peers(swarm: &Swarm<AppBehaviour>) -> Vec<String> {
    info!("발견된 피어:");
    let nodes = swarm.behaviour().mdns.discovered_nodes();
    let mut unique_peers = HashSet::new();
    for peer in nodes {
        unique_peers.insert(peer);
    }
    unique_peers.iter().map(|p| p.to_string()).collect()
}

pub fn handle_print_peers(swarm: &Swarm<AppBehaviour>) {
    let peers = get_list_peers(swarm);
    peers.iter().for_each(|p| info!("{}", p));
}

pub fn handle_print_chain(swarm: &Swarm<AppBehaviour>) {
    info!("로컬 블록체인:");
    let pretty_json =
        serde_json::to_string_pretty(&swarm.behaviour().app.blocks).expect("can jsonify blocks");
    info!("{}", pretty_json);
}

pub fn handle_create_block(cmd: &str, swarm: &mut Swarm<AppBehaviour>) {
    if let Some(data) = cmd.strip_prefix("create b") {
        let behaviour = swarm.behaviour_mut();
        let latest_block = behaviour
            .app
            .blocks
            .last()
            .expect("적어도 하나의 블록이 있어야 함");
        let block = Block::new(
            latest_block.id + 1,
            latest_block.hash.clone(),
            data.to_owned(),
        );
        let json = serde_json::to_string(&block).expect("can jsonify request");
        behaviour.app.blocks.push(block);
        info!("새 블록을 브로드캐스팅 중");
        behaviour
            .floodsub
            .publish(BLOCK_TOPIC.clone(), json.as_bytes());
    }
}
```

# main.rs — Main Loop 및 블록 채굴

이제 'main.rs'라는 새 파일을 생성하여 메인 루프와 블록 채굴을 담당할 것입니다. 코어 로직은 아래와 같습니다:



# 블록 구조

블록 구조체는 블록체인 내의 블록 구조를 정의합니다. 이 구조에는 다음이 포함됩니다:

- id: 블록의 고유 식별자입니다.
- hash: 블록의 해시입니다.
- previous_hash: 체인 내 이전 블록의 해시입니다.
- timestamp: 블록의 생성 시간입니다.
- data: 블록의 데이터 또는 페이로드입니다.
- nonce: 채굴 과정 중 사용되는 값입니다.

```rust
use std::io::{Read, Write};
use chrono::prelude::*;
use libp2p::{
    core::upgrade,
    futures::StreamExt,
    mplex,
    noise::{Keypair, NoiseConfig, X25519Spec},
    swarm::{Swarm, SwarmBuilder},
    tcp::TokioTcpConfig,
    Transport,
};
use log::{error, info, warn};
use serde::{Deserialize, Serialize};
use sha2::{Digest, Sha256};
use std::time::Duration;
use tokio::{
    io::{stdin, AsyncBufReadExt, BufReader},
    select, spawn,
    sync::mpsc,
    time::sleep,
};

const DIFFICULTY_PREFIX: &str = "00";

mod blockchain;
pub struct App {
    pub blocks: Vec<Block>,
}

#[derive(Serialize, Deserialize, Debug, Clone)]
pub struct Block {
    pub id: u64,
    pub hash: String,
    pub previous_hash: String,
    pub timestamp: i64,
    pub data: String,
    pub nonce: u64,
}
```



# 블록 생성 (새로운 기능)

블록 구현 블록(DOM)에 있는 새로운 기능은 새 블록을 생성합니다. 이 기능은 mine_block 함수를 호출하여 블록의 해시와 논스를 생성합니다. 다른 속성들은 입력 값과 현재 시간을 기준으로 설정됩니다.

```rust
impl Block {
    pub fn new(id: u64, previous_hash: String, data: String) -> Self {
        let now = Utc::now();
        let (nonce, hash) = mine_block(id, now.timestamp(), &previous_hash, &data);
        Self {
            id,
            hash,
            timestamp: now.timestamp(),
            previous_hash,
            data,
            nonce,
        }
    }
}
```

# 해시 계산 (calculate_hash 함수)



`calculate_hash` 함수는 블록의 해시를 생성합니다. 이 함수는 블록의 속성을 JSON 객체로 만들고 이 데이터를 해시하기 위해 SHA-256 알고리즘을 사용합니다. 이 함수는 블록체인의 무결성을 보장하는 데 중요합니다.

```rust
fn calculate_hash(id: u64, timestamp: i64, previous_hash: &str, data: &str, nonce: u64) -> Vec<u8> {
    let data = serde_json::json!({
        "id": id,
        "previous_hash": previous_hash,
        "data": data,
        "timestamp": timestamp,
        "nonce": nonce
    });
    let mut hasher = Sha256::new();
    hasher.update(data.to_string().as_bytes());
    hasher.finalize().as_slice().to_owned()
}
```

## 채굴 (mine_block 함수)

`mine_block`은 작업 증명 알고리즘이 구현된 곳입니다. 이 함수는 nonce 값을 반복하여 특정 접두사(난이도 접두사로 정의됨)로 시작하는 해시를 찾으려고 시도합니다. 유효한 해시가 발견되면 함수는 nonce와 해당 해시를 반환합니다.



```rust
fn mine_block(id: u64, timestamp: i64, previous_hash: &str, data: &str) -> (u64, String) {
    info!("블록 채굴 중...");
    let mut nonce = 0;

    loop {
        if nonce % 100000 == 0 {
            info!("논스: {}", nonce);
        }
        let hash = calculate_hash(id, timestamp, previous_hash, data, nonce);
        let binary_hash = hash_to_binary_representation(&hash);
        if binary_hash.starts_with(DIFFICULTY_PREFIX) {
            info!(
                "채굴 성공! 논스: {}, 해시: {}, 이진 해시: {}",
                nonce,
                hex::encode(&hash),
                binary_hash
            );
            return (nonce, hex::encode(hash));
        }
        nonce += 1;
    }
}
```

# 이진 해시 표현 (hash_to_binary_representation 함수)

해시를 해당 이진 표현으로 변환하는 함수입니다. 이는 채굴 과정에서 해시가 난이도 조건을 충족하는지 확인하는 데 사용됩니다.

```rust
fn hash_to_binary_representation(hash: &[u8]) -> String {
    let mut res: String = String::default();
    for c in hash {
        res.push_str(&format!("{:b}", c));
    }
    res
}
```



# 제네시스 블록 (genesis 함수)

이 함수는 블록체인에서 첫 번째 블록을 생성하는 함수로, 제네시스 블록이라고 알려져 있습니다. 이 블록은 수동으로 블록체인에 추가됩니다.

```rust
impl App {
    fn new() -> Self {
        Self { blocks: vec![] }
    }

    fn genesis(&mut self) {
        let genesis_block = Block {
            id: 0,
            timestamp: Utc::now().timestamp(),
            previous_hash: String::from("genesis"),
            data: String::from("genesis!"),
            nonce: 2836,
            hash: "0000f816a87f806bb0073dcf026a64fb40c946b5abee2573702828694d5b4c43".to_string(),
        };
        self.blocks.push(genesis_block);
    }
```

# 블록 추가하기 (try_add_block 함수)



`try_add_block` 함수는 블록체인에 새로운 블록을 추가하려고 시도합니다. 먼저, 새로운 블록을 최신 블록과 검증합니다. 새로운 블록이 유효하면 체인에 추가됩니다.

```rust
fn try_add_block(&mut self, block: Block) {
    let latest_block = self.blocks.last().expect("there is at least one block");
    if self.is_block_valid(&block, latest_block) {
        self.blocks.push(block);
    } else {
        error!("could not add block - invalid");
    }
}
```

## 블록 유효성 검사 (is_block_valid 함수)

이 함수는 이전 블록에 대해 블록을 유효성 검사합니다. 다음을 확인합니다:



- 이전 해시 필드는 이전 블록의 해시와 일치합니다.
- 해시 난이도 수준이 올바릅니다.
- 블록 ID가 이전 블록의 ID를 따릅니다.
- 블록 해시가 유효합니다.

```rust
fn is_block_valid(&self, block: &Block, previous_block: &Block) -> bool {
    if block.previous_hash != previous_block.hash {
        warn!("블록 ID가 {}인 블록은 잘못된 이전 해시를 가지고 있습니다.", block.id);
        return false;
    } else if !hash_to_binary_representation(
        &hex::decode(&block.hash).expect("16진수로 디코딩할 수 있어야 합니다."),
    )
        .starts_with(DIFFICULTY_PREFIX)
    {
        warn!("블록 ID가 {}인 블록은 유효하지 않은 난이도를 가지고 있습니다.", block.id);
        return false;
    } else if block.id != previous_block.id + 1 {
        warn!(
            "블록 ID가 {}인 블록은 최신 블록 이후의 다음 블록이 아닙니다: {}",
            block.id, previous_block.id
        );
        return false;
    } else if hex::encode(calculate_hash(
        block.id,
        block.timestamp,
        &block.previous_hash,
        &block.data,
        block.nonce,
    )) != block.hash
    {
        warn!("블록 ID가 {}인 블록은 유효하지 않은 해시를 가지고 있습니다.", block.id);
        return false;
    }
    true
}
```

# 체인 유효성 검사 (is_chain_valid 함수)

is_chain_valid는 전체 블록 체인의 유효성을 검사합니다. 블록체인이 일관적이고 유효한 상태를 유지하는 데 사용됩니다.



```rust
fn is_chain_valid(&self, chain: &[Block]) -> bool {
    for i in 0..chain.len() {
        if i == 0 {
            continue;
        }
        let first = chain.get(i - 1).expect("has to exist");
        let second = chain.get(i).expect("has to exist");
        if !self.is_block_valid(second, first) {
            return false;
        }
    }
    true
}
```

# 체인 선택 (choose_chain 함수)

이 함수는 블록체인의 여러 버전이 충돌하는 경우 해결하는 데 사용됩니다. 항상 가장 긴 유효한 체인을 선택합니다.

```rust
// 항상 가장 긴 유효한 체인을 선택합니다
fn choose_chain(&mut self, local: Vec<Block>, remote: Vec<Block>) -> Vec<Block> {
    let is_local_valid = self.is_chain_valid(&local);
    let is_remote_valid = self.is_chain_valid(&remote);

    if is_local_valid && is_remote_valid {
        if local.len() >= remote.len() {
            local
        } else {
            remote
        }
    } else if is_remote_valid && !is_local_valid {
        remote
    } else if !is_remote_valid && is_local_valid {
        local
    } else {
        panic!("로컬 및 원격 체인이 모두 유효하지 않습니다");
    }
}
```



# 메인 기능 및 블록체인 네트워킹

주요 기능은 블록체인 애플리케이션의 네트워킹 및 이벤트 처리 부분을 설정합니다. 토키오(tokio)와 립투피(libp2p) 라이브러리를 사용하여 피어 간 상호 작용을 관리하며, 체인 요청 및 사용자 입력 처리와 같은 이벤트에 응답합니다.

```rust
#[tokio::main]
async fn main() {
    pretty_env_logger::init();

    info!("Peer Id: {}", blockchain::PEER_ID.clone());
    let (response_sender, mut response_rcv) = mpsc::unbounded_channel();
    let (init_sender, mut init_rcv) = mpsc::unbounded_channel();

    let auth_keys = Keypair::<X25519Spec>::new()
        .into_authentic(&blockchain::KEYS)
        .expect("can create auth keys");

    let transp = TokioTcpConfig::new()
        .upgrade(upgrade::Version::V1)
        .authenticate(NoiseConfig::xx(auth_keys).into_authenticated())
        .multiplex(mplex::MplexConfig::new())
        .boxed();

    let behaviour = blockchain::AppBehaviour::new(App::new(), response_sender, init_sender.clone()).await;

    let mut swarm = SwarmBuilder::new(transp, behaviour, *blockchain::PEER_ID)
        .executor(Box::new(|fut| {
            spawn(fut);
        }))
        .build();

    let mut stdin = BufReader::new(stdin()).lines();

    Swarm::listen_on(
        &mut swarm,
        "/ip4/0.0.0.0/tcp/0"
            .parse()
            .expect("can get a local socket"),
    )
        .expect("swarm can be started");

    spawn(async move {
        sleep(Duration::from_secs(1)).await;
        info!("sending init event");
        init_sender.send(true).expect("can send init event");
    });

    loop {
        let evt = {
            select! {
                line = stdin.next_line() => Some(blockchain::EventType::Input(line.expect("can get line").expect("can read line from stdin"))),
                response = response_rcv.recv() => {
                    Some(blockchain::EventType::LocalChainResponse(response.expect("response exists")))
                },
                _init = init_rcv.recv() => {
                    Some(blockchain::EventType::Init)
                }
                event = swarm.select_next_some() => {
                    info!("Unhandled Swarm Event: {:?}", event);
                    None
                },
            }
        };

        if let Some(event) = evt {
            match event {
                blockchain::EventType::Init => {
                    let peers = blockchain::get_list_peers(&swarm);
                    swarm.behaviour_mut().app.genesis();

                    info!("connected nodes: {}", peers.len());
                    if !peers.is_empty() {
                        let req = blockchain::LocalChainRequest {
                            from_peer_id: peers
                                .iter()
                                .last()
                                .expect("at least one peer")
                                .to_string(),
                        };

                        let json = serde_json::to_string(&req).expect("can jsonify request");
                        swarm
                            .behaviour_mut()
                            .floodsub
                            .publish(blockchain::CHAIN_TOPIC.clone(), json.as_bytes());
                    }
                }
                blockchain::EventType::LocalChainResponse(resp) => {
                    let json = serde_json::to_string(&resp).expect("can jsonify response");
                    swarm
                        .behaviour_mut()
                        .floodsub
                        .publish(blockchain::CHAIN_TOPIC.clone(), json.as_bytes());
                }
                blockchain::EventType::Input(line) => match line.as_str() {
                    "ls p" => blockchain::handle_print_peers(&swarm),
                    cmd if cmd.starts_with("ls c") => blockchain::handle_print_chain(&swarm),
                    cmd if cmd.starts_with("create b") => blockchain::handle_create_block(cmd, &mut swarm),
                    _ => error!("unknown command"),
                },
            }
        }
    }
}
```

와우, 이것 참 많이네요!



내 GitHub 저장소에서 완전한 구현을 찾을 수 있어요: [루스티체인](https://github.com/luishsr/rustychain).

## 블록체인 테스트

Rust로 제공된 블록체인 구현을 사용하고 테스트하려면 환경 설정, 노드 시작 및 상호 작용하는 일련의 단계를 따라야 해요. 시작하는 데 도움이 되는 빠른 가이드를 제공할게요:

## 단일 노드 실행



- **컴파일 및 실행**: Rust 블록체인 코드가 있는 디렉토리로 이동한 후 cargo build를 사용하여 프로젝트를 컴파일합니다. 성공적인 컴파일이 완료되면 cargo run을 사용하여 노드를 실행합니다.
- **초기 테스트**: 처음에는 제네시스 블록이 올바르게 생성되고 노드가 제대로 시작되는지 확인하기 위해 단일 노드로 테스트합니다. 만약 구현되어 있다면 현재 체인이나 노드 상태를 표시하는 명령을 사용할 수 있습니다.

# 여러 노드 실행하기

실제 블록체인 네트워크를 시뮬레이션하려면 여러 노드를 동시에 실행해야 합니다.

- **여러 터미널 열기**: 여러 터미널 창 또는 탭을열어주세요. 각각은 네트워크에서 별개의 노드를 나타냅니다.
- **독립적으로 노드 실행**: 각 터미널에서 프로젝트 디렉토리로 이동하고 cargo run을 실행합니다. 각 인스턴스는 블록체인 네트워크의 별개 노드로 동작하게 됩니다.



# 노드와 상호작용하기

블록체인의 기능을 테스트하려면 노드와 상호작용해야 합니다.

- 새 블록 생성: 구현된 명령어(예: `data`를 이용한 `create b`)를 사용하여 새 블록을 생성하세요. 이는 블록체인에 거래나 데이터 추가를 시뮬레이션할 것입니다.
- 블록 전파: 한 노드에서 새 블록이 생성되면 이 블록을 다른 노드에 전파해야 합니다. 다른 노드가 이 새 블록을 수신하고 유효성을 검사하여 자신의 블록체인 버전에 추가하는지 확인하세요.
- 블록체인 상태 보기: 정기적으로 각 노드의 블록체인 현재 상태를 출력하는 명령어를 사용하세요. 노드 간에 일관성이 있어야하며 최신 유효 블록을 반영해야 합니다.
- 체인 충돌 테스트: 서로 다른 노드에서 동시에 다른 블록을 생성하여 체인 충돌을 시뮬레이트하세요. 구현이 이러한 충돌을 해결하는 방식(일반적으로 가장 긴 유효 체인을 선택함)을 관찰하세요.

# 🚀 루이스 소아레스에 의한 소프트웨어 개발 및 기타 다양한 리소스 탐색하기



📚 학습 플랫폼: 러스트, 소프트웨어 개발, 클라우드 컴퓨팅, 사이버 보안, 블록체인, 리눅스 등 다양한 기술 분야에서 지식을 넓히세요. 저의 포괄적인 자료 모음을 통해:

- GitHub 리포지토리와 함께 하는 실습 튜토리얼: 전용 GitHub 리포지토리를 통해 단계별 실습을 통해 다양한 기술을 실용적으로 익힐 수 있습니다. [튜토리얼 접근하기](링크)
- 심층 안내서 및 기사: 러스트, 소프트웨어 개발, 클라우드 컴퓨팅 등의 핵심 개념을 자세히 다룬 안내서와 실제 예제가 풍부한 기사로 깊게 파보세요. [더 읽기](링크)
- 전자책 모음집: "러스트 소유권 마스터하기" 및 "애플리케이션 보안 안내서"와 같은 제목을 포함한 무료 전자책 시리즈로 다양한 기술 분야의 이해를 향상시키세요. [전자책 다운로드](링크)
- 프로젝트 쇼케이스: API 게이트웨이, 블록체인 네트워크, 사이버 보안 도구, 클라우드 서비스 등과 같이 다양한 분야의 완전 기능 프로젝트를 발견하세요. [프로젝트 보기](링크)
- LinkedIn 뉴스레터: LinkedIn에서 제 뉴스레터를 구독하여 러스트, 소프트웨어 개발 및 신흥 기술에 관한 규칙적인 업데이트와 통찰력을 유지하세요. [여기서 구독하기](링크)

🔗 저와 연결해보세요:

- Medium: 미디엄에서 제 글을 읽고 유용하다고 생각하면 박수를 보내주세요. 이는 저에게 글쓰기와 러스트 콘텐츠 공유를 계속 이끌어주는 원동력이 됩니다. [Medium 팔로우하기](링크)
- 개인 블로그: 제 개인 블로그에서 더 많은 러스트 관련 콘텐츠를 확인하세요. [블로그 방문하기](링크)
- LinkedIn: 더 많은 통찰력 있는 토론과 업데이트를 위해 제 전문 네트워크에 참여하세요. [LinkedIn 연결하기](링크)
- 트위터: 빠른 업데이트와 러스트 프로그래밍에 대한 생각을 보려면 트위터에서 저를 팔로우하세요. [트위터에서 팔로우하기](링크)



Wassup, folks? Feel free to leave a comment or shoot me a message!

Cheers,

Luis Soares
[luis.soares@linux.com](mailto:luis.soares@linux.com)

Senior Software Engineer | Cloud Engineer | SRE | Tech Lead | Rust | Golang | Java | ML AI & Statistics | Web3 & Blockchain