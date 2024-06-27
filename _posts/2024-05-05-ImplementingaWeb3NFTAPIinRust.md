---
title: "루스트로 웹 3 NFT API를 구현하기"
description: ""
coverImage: "/assets/img/2024-05-05-ImplementingaWeb3NFTAPIinRust_0.png"
date: 2024-05-05 17:27
ogImage: 
  url: /assets/img/2024-05-05-ImplementingaWeb3NFTAPIinRust_0.png
tag: Tech
originalTitle: "Implementing a Web3 NFT API in Rust"
link: "https://medium.com/dev-genius/implementing-a-web3-nft-api-in-rust-ed3a0c7577e6"
---


![ImplementingaWeb3NFTAPIinRust](/assets/img/2024-05-05-ImplementingaWeb3NFTAPIinRust_0.png)

안녕하세요 러스테이션 분들! 🦀

오늘의 글에서는 이더리움 블록체인을 활용하여 NFT를 발행하는 Rust API를 구현해 보겠습니다. 또한 IPFS와의 분산 파일 저장소 통합, 그리고 솔리디티를 사용한 스마트 계약 구현도 다룰 예정이에요.

이 글을 마치면 swagger-ui를 사용하여 API와 상호 작용할 수 있게 되며, Web3, RESTful Rust API, 이더리움 블록체인, 그리고 솔리디티를 사용한 스마트 계약을 어떻게 통합하는지에 대한 기본 지식을 습득할 수 있을 거예요.



기대하시는 대로, 러스트 NFT API에 대한 이 깊은 탐험은 정보가 풍부하고 흥미로울 것입니다. 다만 일반적으로 쓰는 글보다 조금 더 길어져서 죄송합니다. 보다 실용적인 접근을 선호하시거나 코드를 더 탐구하고 싶어하는 분들을 위해 좋은 소식이 있어요!

🚀 프로젝트를 실행하고 자세히 살펴볼 수 있는 전체 코드베이스와 단계별 지침을 https://github.com/luishsr/rust-nft-api GitHub 저장소에 깔끔하게 정리해 놨습니다. 언제든지 참여하셔서 즐거운 코딩하세요!

바로 시작해봅시다!

# 프로젝트 구조 개요



프로젝트는 다음과 같이 구성되어 있습니다:


rust-nft-api/
├── contract/
│   └── MyNFT.sol
├── nft-images/
│   └── token.jpg
├── src/
│   ├── main.rs
│   ├── error.rs
│   ├── ipfs.rs
│   ├── model.rs
│   ├── utils.rs
│   └── web3client.rs
├── static/
│   └── swagger-ui/
├── .env
└── Cargo.toml


- contract/: NFT에 대한 Solidity 스마트 계약(MyNFT.sol)을 포함하며, NFT의 발행 및 전송 규칙을 정의합니다.
- nft-images/: 각 NFT와 관련된 이미지 또는 자산을 저장하며, NFT 메타데이터에서 참조됩니다.
- src/: 러스트 파일이 있는 소스 디렉토리이며, API 기능에서 특정 목적을 제공합니다:
  - main.rs: API의 진입점으로 서버 및 라우트 설정을 수행합니다.
  - error.rs: API에 대한 사용자 지정 오류 처리를 정의합니다.
  - ipfs.rs: IPFS와 상호 작용하여 오프체인 메타데이터를 저장합니다.
  - model.rs: API에서 사용되는 데이터 모델을 정의하며, NFT 및 메타데이터 구조를 포함합니다.
  - utils.rs: 프로젝트 전체에서 사용되는 유틸리티 함수를 포함합니다.
  - web3client.rs: Web3를 사용하여 이더리움 블록체인과의 통신을 관리합니다.
- static/: API 문서화를 위한 Swagger UI와 같은 정적 파일을 포함합니다.
- .env: API 키 및 블록체인 노드 URL과 같은 환경 변수를 관리하는 dotenv 파일입니다.
- Cargo.toml: 의존성 및 프로젝트 정보를 나열하는 러스트 패키지 매니페스트 파일입니다.

# 주요 구성 요소 및 기능



# 스마트 계약 (MyNFT.sol)

이 스마트 계약은 Solidity로 작성되어 이더리움 블록체인에 배포되었습니다. ERC-721 표준에 따라 NFT의 발행, 이전 및 관리 규칙을 정의합니다. 이는 이더리움에서 NFT에 널리 사용되는 표준입니다.

# IPFS 통합 (ipfs.rs)

IPFS 또는 InterPlanetary File System은 NFT의 오프체인 메타데이터를 저장하는 데 사용됩니다. 이를 통해 이미지와 설명 정보를 포함한 메타데이터가 분산화되고 위변조되지 않음을 보장합니다. ipfs.rs 모듈은 IPFS로부터 메타데이터를 업로드하고 검색하는 작업을 처리합니다.



# 웹3 클라이언트 (web3client.rs)

이 모듈은 Web3 라이브러리를 사용하여 이더리움 블록체인에 연결을 설정합니다. 블록체인과 상호 작용하도록 API를 활성화하여 NFT 발행, NFT 세부 정보 검색 및 블록체인 이벤트 수신과 같은 작업을 수행할 수 있습니다.

# API 엔드포인트 (main.rs)

main.rs 파일은 RESTful API 서버를 설정하고 NFT 생성, 토큰 ID로 NFT 세부 정보 가져오기 및 모든 NFT 나열과 같은 다양한 엔드포인트에 대한 경로를 정의합니다. Actix-web 프레임워크를 사용하여 HTTP 요청과 응답을 처리합니다.



# 에러 처리 및 유틸리티 (error.rs, utils.rs)

강력한 API를 위해서 적절한 에러 처리가 중요합니다. error.rs 모듈은 사용자 정의 에러 유형과 처리 메커니즘을 정의하여 클라이언트에게 명확하고 유용한 오류 메시지가 반환되도록 보장합니다. utils.rs 모듈에는 데이터 유효성 검증 및 형식 지원과 같은 API 내에서 다양한 작업을 지원하는 유틸리티 함수가 포함되어 있습니다.

# 단계 1. 스마트 계약 구현

Solidity로 개발된 MyNFT 계약은 안전한 블록체인 개발을 위한 표준 라이브러리인 OpenZeppelin의 ERC721URIStorage 계약을 확장합니다. 이는 NFT 소유권을 표현하는 인기있는 표준인 ERC721 프로토콜을 활용하며 NFT에 URI 기반 메타데이터를 연결하는 기능을 추가합니다.



## 주요 구성 요소

- 토큰 카운터: OpenZeppelin의 Counters 유틸리티를 활용하여 발행된 각 NFT에 대한 고유 식별자를 유지합니다.
- 토큰 세부 정보 구조: 각 NFT에 대한 중요 정보인 ID, 이름, 소유자 및 관련 URI를 보유하는 TokenDetails 구조체를 정의합니다.
- 매핑: 세 가지 주요 매핑이 사용되어 NFT 소유권 및 세부 정보를 추적합니다:
  - _tokenDetails는 각 토큰 ID를 해당 TokenDetails로 매핑합니다.
  - _ownedTokens는 소유자 주소를 소유한 토큰 ID 목록으로 매핑합니다.
  - _ownedTokensIndex는 토큰 ID를 소유자의 토큰 목록에서의 위치로 매핑합니다.

```javascript
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/utils/Counters.sol";

contract MyNFT is ERC721URIStorage {
    using Counters for Counters.Counter;
    Counters.Counter private _tokenIds;

    struct TokenDetails {
        uint256 tokenId;
        string tokenName;
        address tokenOwner;
        string tokenURI;
    }

    mapping(uint256 => TokenDetails) private _tokenDetails;
    mapping(address => uint256[]) private _ownedTokens;
    mapping(uint256 => uint256) private _ownedTokensIndex;  // 토큰 ID를 소유자의 토큰 목록에서의 위치로 매핑합니다

    constructor() ERC721("MyNFT", "MNFT") {}

    function mintNFT(address recipient, string memory tokenName, string memory tokenURI) public returns (uint256) {
        _tokenIds.increment();
        uint256 newItemId = _tokenIds.current();
        _mint(recipient, newItemId);
        _setTokenURI(newItemId, tokenURI);

        _tokenDetails[newItemId] = TokenDetails({
            tokenId: newItemId,
            tokenName: tokenName,
            tokenOwner: recipient,
            tokenURI: tokenURI
        });

        _addTokenToOwnerEnumeration(recipient, newItemId);

        return newItemId;
    }

    function _addTokenToOwnerEnumeration(address to, uint256 tokenId) private {
        _ownedTokens[to].push(tokenId);
        _ownedTokensIndex[tokenId] = _ownedTokens[to].length - 1;
    }

    function getAllTokensByOwner(address owner) public view returns (uint256[] memory) {
        if (owner == address(0)) {
            uint256 totalTokens = _tokenIds.current();
            uint256[] memory allTokenIds = new uint256[](totalTokens);
            for (uint256 i = 0; i < totalTokens; i++) {
                allTokenIds[i] = i + 1;  // 토큰 ID는 발행 방식에 따라 1부터 시작합니다
            }
            return allTokenIds;
        } else {
            return _ownedTokens[owner];
        }
    }

    function getTokenDetails(uint256 tokenId) public view returns (uint256, string memory, address, string memory) {
        require(_ownerOf(tokenId) != address(0), "ERC721: 존재하지 않는 토큰에 대한 조회");

        TokenDetails memory tokenDetail = _tokenDetails[tokenId];
        return (tokenDetail.tokenId, tokenDetail.tokenName, tokenDetail.tokenOwner, tokenDetail.tokenURI);
    }
}
```

# 단계 2. Web3Client 작성하기



The `web3client.rs` file includes the implementation of the `Web3Client` struct, responsible for enabling interactions with smart contracts on the Ethereum network through the Rust programming language. Let's explore the main features of this implementation.

## Web3Client Structure

The `Web3Client` struct comprises two primary fields:

- `web3`: An object of the `Web3` type, establishing a link to an Ethereum node.
- `contract`: A `Contract` instance, representing the smart contract on the Ethereum blockchain that the API will engage with.



## 구현 세부 사항

- 새로운 함수: 이것은 Web3Client 구조체를 위한 생성자입니다. 제공된 스마트 계약 주소로 새 Web3 인스턴스 및 새 Contract 인스턴스를 초기화합니다.
- 이더리움 노드 연결: ETH_NODE_URL 환경 변수로 지정된 이더리움 노드에 HTTP 연결을 설정합니다. 이 연결은 이더리움 블록체인으로 트랜잭션을 보내거나 호출하는 데 필수적입니다.
- 스마트 계약 ABI: Rust 애플리케이션이 계약과 상호 작용하는 방법을 이해하는 데 스마트 계약의 ABI(응용 프로그램 이진 인터페이스)가 필요합니다. ABI는 CONTRACT_ABI_PATH 환경 변수로 지정된 파일에서 로드됩니다. 해당 ABI 파일은 일반적으로 스마트 계약이 컴파일될 때 Solidity 컴파일러에 의해 생성됩니다.
- 계약 초기화: ABI 및 스마트 계약 주소를 사용하여 새 Contract 인스턴스가 생성됩니다. 이 인스턴스를 사용하면 Rust 애플리케이션이 스마트 계약의 함수를 호출하거나 해당에서 발생한 이벤트를 수신하고 상태를 쿼리할 수 있습니다.

```rust
use std::env;
use std::error::Error;
use web3::contract::Contract;
use web3::transports::Http;
use web3::{ethabi, Web3};

pub struct Web3Client {
    pub web3: Web3<Http>,
    pub contract: Contract<Http>,
}

impl Web3Client {
    pub fn new(contract_address: &str) -> Result<Self, Box<dyn Error>> {
        let http = Http::new(&env::var("ETH_NODE_URL")?)?;
        let web3 = Web3::new(http);

        let contract_abi_path = env::var("CONTRACT_ABI_PATH")?;
        let contract_abi_file = std::fs::File::open(contract_abi_path)?;
        let contract_abi: ethabi::Contract = serde_json::from_reader(contract_abi_file)?;

        let contract = Contract::new(web3.eth(), contract_address.parse()?, contract_abi);

        Ok(Web3Client { web3, contract })
    }
}
```

# 단계 3. 데이터 구조



안녕하세요! 오늘은 Rust NFT API 프로젝트 내 모델.rs 파일에 대해 이야기해보려고 해요.

해당 파일은 Rust의 강력한 타입 시스템을 이용하여 주요 데이터 구조를 정의하며, serde를 통한 직렬화 기능과 utoipa의 API 문서화 기능을 결합하고 있어요.

```rust
use serde::{Deserialize, Serialize};
use utoipa::Component;

#[derive(Serialize, Deserialize, Component)]
pub struct MintNftRequest {
    pub(crate) owner_address: String,
    pub(crate) token_name: String,
    pub(crate) token_uri: String,
    pub(crate) file_path: String,
}

#[derive(Serialize, Deserialize, Component)]
pub struct TokenFileForm {
    file: Vec<u8>,
}

#[derive(Serialize, Deserialize, Component)]
pub struct ApiResponse {
    pub(crate) success: bool,
    pub(crate) message: String,
    pub(crate) token_uri: Option<String>,
}

#[derive(Serialize, Deserialize, Component)]
pub struct NftMetadata {
    pub(crate) token_id: String,
    pub(crate) owner_address: String,
    pub(crate) token_name: String,
    pub(crate) token_uri: String,
}

#[derive(Serialize, Deserialize)]
pub struct UploadResponse {
    token_uri: String,
}
```

## MintNftRequest

이 구조는 새로운 NFT를 발행하기 위한 요청 본문을 나타냅니다. 소유자 주소, 토큰 이름, 토큰 URI(해당 NFT와 관련된 메타데이터 또는 자산을 가리킴), 그리고 NFT와 연결할 자산의 파일 경로에 대한 필드를 포함하고 있어요. pub (crate)의 사용은 이들 필드가 크레이트 내에서 접근 가능하게 만듭니다.

더 궁금한 점이 있으면 언제든지 물어보세요! 😊



# TokenFileForm

TokenFileForm은 파일 업로드 양식의 데이터 구조를 정의합니다. 특히 NFT와 관련된 파일을 업로드하기 위한 것입니다. 파일 필드는 업로드되는 파일의 바이너리 내용을 나타내는 'Vec u8'의 바이트 벡터입니다.

## ApiResponse

다양한 API 작업의 결과를 전달하는 데 사용할 수 있는 일반적인 API 응답 구조입니다. 작업이 성공했는지를 나타내는 성공 플래그, 추가 정보 또는 오류 세부 정보를 제공하는 메시지, 그리고 NFT가 관련된 작업에서 특히 중요한 옵션인 token_uri가 포함되어 있습니다. 여기서는 NFT의 메타데이터 또는 자산을 가리키는 URI가 반환될 수 있습니다.



## NftMetadata

NFT와 관련된 메타데이터를 나타냅니다. 토큰 ID, 소유자 주소, 토큰 이름 및 토큰 URI가 포함되어 있습니다. 이 모델은 NFT 세부 정보를 검색하거나 표시하는 작업에 중요합니다.

## UploadResponse

파일 업로드 작업에 특별히 맞춘 이 모델은 업로드 작업의 응답을 캡처하며 주로 업로드된 파일의 토큰 URI를 포함합니다. 이 URI는 그 후 민팅 프로세스나 업로드된 자산을 연결하는 데 필요한 다른 목적에 사용할 수 있습니다.



# 단계 4. IPFS와 인터페이스

러스트 NFT API 프로젝트 내 ipfs.rs 모듈은 InterPlanetary File System (IPFS)와의 상호 작용을 처리하는 데 전념합니다. IPFS는 분산형 저장 솔루션으로, 오프 체인 NFT 메타데이터나 자산을 저장하는 데 중요한 역할을 합니다.

```rust
use crate::model::ApiResponse;
use axum::Json;
use reqwest::Client;
use serde_json::Value;
use std::convert::Infallible;
use std::env;
use tokio::fs::File;
use tokio::io::AsyncReadExt;

pub async fn file_upload(file_name: String) -> Result<Json<ApiResponse>, Infallible> {
    let client = Client::new();
    let ipfs_api_endpoint = "http://127.0.0.1:5001/api/v0/add";

    // 현재 디렉토리 가져오기
    let mut path = env::current_dir().expect("Failed to get current directory");
    // 경로에 'nft-images' 하위 디렉토리 추가
    path.push("nft-images");
    // 파일 이름을 경로에 추가
    path.push(file_name);

    //println!("Full path: {}", path.display());

    // 파일을 비동기적으로 열기
    let mut file = File::open(path.clone()).await.expect("Failed to open file");

    // 파일 바이트 읽기
    let mut file_bytes = Vec::new();
    file.read_to_end(&mut file_bytes)
        .await
        .expect("Failed to read file bytes");

    // 경로에서 파일 이름 추출
    let file_name = path
        .file_name()
        .unwrap()
        .to_str()
        .unwrap_or_default()
        .to_string();

    let form = reqwest::multipart::Form::new().part(
        "file",
        reqwest::multipart::Part::stream(file_bytes).file_name(file_name),
    );

    let response = client
        .post(ipfs_api_endpoint)
        .multipart(form)
        .send()
        .await
        .expect("Failed to send file to IPFS");

    if response.status().is_success() {
        let response_body = response
            .text()
            .await
            .expect("Failed to read response body as text");

        let ipfs_response: Value =
            serde_json::from_str(&response_body).expect("Failed to parse IPFS response");
        let ipfs_hash = format!(
            "https://ipfs.io/ipfs/{}",
            ipfs_response["Hash"].as_str().unwrap_or_default()
        );

        Ok(Json(ApiResponse {
            success: true,
            message: "File uploaded to IPFS successfully.".to_string(),
            token_uri: Some(ipfs_hash),
        }))
    } else {
        Ok(Json(ApiResponse {
            success: false,
            message: "IPFS upload failed.".to_string(),
            token_uri: None,
        }))
    }
}
```

처리 흐름



### 초기 설정:
Reqwest로 HTTP 요청을 보내기 위해 Client 인스턴스가 생성됩니다.

### 파일 경로 구성:
해당 함수는 현재 작업 디렉토리, nft-images 하위 디렉토리 및 제공된 파일 이름을 결합하여 파일 경로를 구성합니다.

### 파일 읽기:
지정된 파일을 비동기적으로 열고 읽어서 해당 바이트를 벡터로 수집합니다.

### 폼 준비:
파일 바이트를 포함하는 멀티파트 폼을 준비하고, 파일 이름을 폼 데이터의 일부로 사용합니다.



**IPFS API 요청:** IPFS 노드의 add 엔드포인트 (/api/v0/add)로 멀티파트 폼을 전송하는 POST 요청을 보냅니다.

**응답 처리:**

- 성공 시, IPFS 응답을 구문 분석하여 파일의 IPFS 해시를 추출하고, 해당 파일을 IPFS 게이트웨이를 통해 액세스할 수 있는 URL을 구성한 후, 이 URL이 포함된 성공적인 ApiResponse를 생성합니다.
- 실패 시, 업로드 실패를 나타내는 ApiResponse를 생성합니다.

**단계 5. 에러 처리**



에러.rs 파일은 응용 프로그램 작동 중 발생할 수 있는 다양한 에러 유형을 정의하고 관리하는 데 전념되어 있습니다. 이 모듈은 사용자 정의 에러 유형을 정의하는 데 thiserror 크레이트를 사용하고 이러한 에러를 적절한 HTTP 응답으로 매핑하는 axum 프레임워크를 사용합니다. 이 파일 내에서 에러 처리가 어떻게 구조화되어 있는지 살펴보겠습니다:

```rust
use axum::{
    http::StatusCode,
    response::{IntoResponse, Response},
    Json,
};
use serde_json::json;
use thiserror::Error;

// `thiserror`를 사용하여 사용자 정의 응용 프로그램 에러 유형 정의
#[derive(Error, Debug)]
pub enum AppError {
    #[error("잘못된 요청: {0}")]
    BadRequest(String),

    #[error("내부 서버 에러: {0}")]
    InternalServerError(String),

    #[error("Web3 에러: {0}")]
    Web3Error(#[from] web3::Error),

    #[error("직렬화 에러: {0}")]
    SerdeError(#[from] serde_json::Error),

    #[error("내부 에러: {0}")]
    GenericError(String),

    #[error("스마트 컨트랙트 에러: {0}")]
    NotFound(String),
}

impl From<Box<dyn std::error::Error>> for AppError {
    fn from(err: Box<dyn std::error::Error>) -> Self {
        AppError::GenericError(format!("오류가 발생했습니다: {}", err))
    }
}

// `AppError`를 `IntoResponse`에 구현하여 HTTP 응답으로 변환
impl IntoResponse for AppError {
    fn into_response(self) -> Response {
        let (status, error_message) = match &self {
            AppError::BadRequest(message) => (StatusCode::BAD_REQUEST, message.clone()),
            AppError::InternalServerError(message) => {
                (StatusCode::INTERNAL_SERVER_ERROR, message.clone())
            }
            AppError::Web3Error(message) => {
                (StatusCode::INTERNAL_SERVER_ERROR, message.to_string())
            }
            AppError::SerdeError(message) => {
                (StatusCode::INTERNAL_SERVER_ERROR, message.to_string())
            }
            AppError::GenericError(message) => (StatusCode::INTERNAL_SERVER_ERROR, message.clone()),
            AppError::NotFound(message) => (StatusCode::INTERNAL_SERVER_ERROR, message.clone()),
        };

        let body = Json(json!({ "error": error_message })).into_response();
        (status, body).into_response()
    }
}

// 파일 업로드 에러를 처리하기 위한 사용자 정의 UploadError 유형
#[derive(Error, Debug)]
pub enum UploadError {
    #[error("IO 에러: {0}")]
    IoError(#[from] std::io::Error),
}

#[derive(Error, Debug)]
pub enum SignatureError {
    #[error("Hex 디코딩 에러: {0}")]
    HexDecodeError(#[from] hex::FromHexError),
}

impl From<SignatureError> for AppError {
    fn from(err: SignatureError) -> AppError {
        match err {
            SignatureError::HexDecodeError(_) => {
                AppError::BadRequest("잘못된 hex 형식".to_string())
            }
        }
    }
}

// `UploadError`를 `IntoResponse`에 구현하여 HTTP 응답으로 변환
impl IntoResponse for UploadError {
    fn into_response(self) -> Response {
        let (status, error_message) = match self {
            UploadError::IoError(_) => (
                StatusCode::INTERNAL_SERVER_ERROR,
                "내부 서버 에러".to_string(),
            ),
        };

        let body = Json(json!({ "error": error_message })).into_response();
        (status, body).into_response()
    }
}
```

## 사용자 정의 응용 프로그램 에러 유형

- AppError: 응용 프로그램의 주요 에러 유형으로, 잘못된 요청, 내부 서버 에러 및 Web3 상호 작용, 직렬화 문제 및 일반적인 에러와 관련된 특정 에러와 같은 다양한 에러 시나리오를 포괄합니다. AppError의 각 변형은 설명적인 에러 메시지와 함께 에러 응답의 디버깅 및 사용자 친화성을 향상시킵니다.
- UploadError 및 SignatureError: 이들은 각각 파일 업로드 에러 및 서명 관련 에러를 처리하기 위한 특수화된 에러 유형입니다. AppError와 마찬가지로 다양한 실패 시나리오에 대해 특정 에러 메시지를 제공합니다.



## 오류 변환

- From 트레이트는 보다 넓은 오류 유형(std::io::Error 및 hex::FromHexError와 같은)에서 구체적인 응용 프로그램 오류(UploadError 및 SignatureError)로의 변환을 허용하기 위해 구현됩니다. 이를 통해 다양한 오류 원천을 잘 정의된 범주로 캡슐화하여 응용 프로그램의 다양한 부분에서 원활한 오류 처리가 가능합니다.

## 오류 응답

- AppError, UploadError 및 SignatureError에 대한 IntoResponse 트레이트 구현은 이러한 오류를 HTTP 응답으로 변환합니다. 오류 유형 및 해당 메시지에 따라 적합한 HTTP 상태 코드(예: StatusCode::BAD_REQUEST 또는 StatusCode::INTERNAL_SERVER_ERROR)가 선택됩니다. 그런 다음 오류 메시지가 JSON 객체로 직렬화되어 API 소비자를 위한 일관된 및 정보 제공이 되는 오류 응답 형식을 제공합니다.



# Step 6. 유틸리티 함수

utils.rs 모듈은 암호화 방식으로 개인 키를 사용하여 데이터에 서명하는 방법을 보여줍니다. 이 함수는 블록체인 트랜잭션이나 안전한 데이터 교환과 같이 데이터의 신뢰성과 무결성을 확인해야 하는 시나리오에서 특히 유용합니다.

```rust
use secp256k1::{Message, Secp256k1, SecretKey};
use sha3::{Digest, Keccak256};
use std::error::Error;

pub fn mock_sign_data(data: &[u8], private_key_hex: &str) -> Result<String, Box<dyn Error>> {
    // 16진수 개인 키를 디코드합니다
    let private_key = SecretKey::from_slice(&hex::decode(private_key_hex)?)?;

    // 새로운 Secp256k1 컨텍스트를 생성합니다
    let secp = Secp256k1::new();

    // Keccak256를 사용하여 데이터 해싱
    let data_hash = Keccak256::digest(data);

    // 해시에 서명합니다
    let message = Message::from_digest_slice(&data_hash)?;
    let signature = secp.sign_ecdsa(&message, &private_key);

    // 서명을 16진수로 인코딩합니다
    Ok(hex::encode(signature.serialize_compact()))
}
```

프로세스:



### Hex Decoding: 
The process kicks off with the hex::decode function, which decodes the hexadecimal private key into bytes.

### Private Key Preparation: 
Next up, the decoded bytes are transformed into a SecretKey instance that aligns with the secp256k1 cryptographic library.

### Hashing: 
The data undergoes hashing through the Keccak256 algorithm, a variation of SHA-3 extensively utilized in Ethereum for hashing operations.

### Signing: 
Subsequently, the hash gets enveloped in a Message type, and the secp256k1 library steps in to sign this message with the designated private key.



16진수 인코딩: 마지막으로, 서명은 간결한 형식으로 직렬화되어 쉬운 전송과 저장을 위해 16진수 문자열로 인코딩됩니다.

## 암호 라이브러리

- 이 기능은 secp256k1 라이브러리를 활용하여 타원 곡선 암호화를 수행합니다. 이 라이브러리는 이더리움과 비트코인에서 서명 생성 및 확인에 사용되는 secp256k1 곡선을 특별히 지원합니다.
- sha3 크레이트는 Keccak256 해시 알고리즘의 구현을 제공하여, 데이터 서명 프로세스가 블록체인 기술에서 흔히 사용되는 암호화 관행과 일치함을 보장합니다.

# 단계 7. 핵심 API 구현



`main.rs` 파일은 Rust NFT API의 진입점 역할을 하며, 다양한 구성 요소를 조정하여 NFT 관리를 위한 포괄적인 백엔드를 제공합니다. 각 코드 섹션별로 설명과 함께 아래와 같이 코드를 살펴보겠습니다:

## OpenAPI 스키마 생성

```rust
#[derive(utoipa::OpenApi)]
#[openapi(
    handlers(process_mint_nft, get_nft_metadata, list_tokens),
    components(MintNftRequest, NftMetadata)
)]
struct ApiDoc;

// OpenAPI 스키마의 JSON 버전 반환
#[utoipa::path(
    get,
    path = "/api/openapi.json",
    responses(
        (status = 200, description = "JSON 파일", body = Json)
    )
)]
async fn openapi() -> Json<utoipa::openapi::OpenApi> {
    Json(ApiDoc::openapi())
}
```

이 함수는 JSON 형식으로 OpenAPI 스키마를 생성하며, 개발자들에게 API 엔드포인트, 요청 본문 및 응답의 명확한 명세를 제공합니다. 이는 API 발견 및 상호 작용을 돕는 OpenAPI 문서화를 위해 utoipa 크레이트를 활용합니다.



## NFT Minting Endpoint

```js
async fn process_mint_nft(
    Extension(web3_client): Extension<Arc<Web3Client>>,
    Json(payload): Json<MintNftRequest>,
) -> Result<Json<NftMetadata>, AppError> {
#[utoipa::path(
post,
    path = "/mint",
    request_body = MintNftRequest,
responses(
    (status = 200, description = "NFT minted successfully", body = NftMetadata),
    (status = 400, description = "Bad Request"),
    (status = 500, description = "Internal Server Error")
)
)]
async fn process_mint_nft(
    Extension(web3_client): Extension<Arc<Web3Client>>,
    Json(payload): Json<MintNftRequest>,
) -> Result<Json<NftMetadata>, AppError> {
    let owner_address = payload
        .owner_address
        .parse::<Address>()
        .map_err(|_| AppError::BadRequest("Invalid owner address".into()))?;

    // Retrieve the mock private key from environment variables
    let mock_private_key = env::var("MOCK_PRIVATE_KEY").expect("MOCK_PRIVATE_KEY must be set");

    // Simulate data to be signed
    let data_to_sign = format!("{}:{}", payload.owner_address, payload.token_name).into_bytes();

    // Perform mock signature
    let _mock_signature = mock_sign_data(&data_to_sign, &mock_private_key)?;

    let upload_response = match ipfs::file_upload(payload.file_path.clone()).await {
        Ok(response) => response,
        Err(_) => unreachable!(), // Since Err is Infallible, this branch will never be executed
    };

    let uploaded_token_uri = upload_response.token_uri.clone().unwrap();

    // Call mint_nft using the file_url as the token_uri
    let token_id = mint_nft(
        &web3_client.web3,
        &web3_client.contract,
        owner_address,
        uploaded_token_uri.clone(),
        payload.token_name.clone(),
    )
    .await
    .map_err(|e| AppError::InternalServerError(format!("Failed to mint NFT: {}", e)))?;

    Ok(Json(NftMetadata {
        token_id: token_id.to_string(),
        owner_address: payload.owner_address,
        token_name: payload.token_name,
        token_uri: uploaded_token_uri.clone(),
    }))
}    


}
```

위 코드는 NFT Minting Endpoint에 대한 내용입니다. 해당 엔드포인트는 새로운 NFT를 발행하는 요청을 처리합니다. MintNftRequest 페이로드를 통해 NFT 소유자의 주소, 토큰 이름 및 파일 경로를 입력받습니다. 이 함수는 모의 서명 작업을 수행하고, 연관된 파일을 IPFS에 업로드하며, 스마트 계약과 상호작용하여 NFT를 발행하고 성공 시 NFT 메타데이터를 반환합니다.

## NFT Metadata Retrieval Endpoint



```rust
#[utoipa::path(
get,
    path = "/nft/{token_id}",
params(
    ("token_id" = String, )),
responses(
    (status = 200, description = "NFT 메타데이터가 성공적으로 검색되었습니다", body = NftMetadata),
    (status = 400, description = "잘못된 요청"),
    (status = 500, description = "내부 서버 오류")
)
)]
async fn get_nft_metadata(
    Extension(web3_client): Extension<Arc<Web3Client>>,
    Path(token_id): Path<String>,
) -> Result<Json<NftMetadata>, AppError> {
    let parsed_token_id = token_id
        .parse::<U256>()
        .map_err(|_| AppError::BadRequest("잘못된 토큰 ID".into()))?;

    match get_nft_details(&web3_client.contract, parsed_token_id.to_string()).await {
        Ok((_, token_name, token_owner, token_uri)) => {
            // 토큰을 위한 NftMetadata 구성
            let nft_metadata = NftMetadata {
                token_id: parsed_token_id.to_string(),
                owner_address: format!("{:?}", token_owner),
                token_name,
                token_uri,
            };

            Ok(Json(nft_metadata))
        }
        Err(AppError::NotFound(msg)) => Err(AppError::NotFound(msg)),
        Err(_) => Err(AppError::InternalServerError(
            "NFT 세부 정보를 검색하는 데 실패했습니다".into(),
        )),
    }
}
```

이 엔드포인트는 토큰 ID를 제공받아 해당 NFT의 메타데이터를 검색합니다. 토큰의 이름 및 URI와 같은 세부 정보를 스마트 계약에서 쿼리하여 NFT의 메타데이터를 제공하는 구조화된 응답을 제공합니다.

## NFT 리스트 엔드포인트

```rust
#[utoipa::path(
get,
    path = "/tokens/{owner_address}",
params(
    ("owner_address" = Option<String>, description = "토큰을 소유자별로 필터링할 소유자 주소. 모든 토큰을 나열하려면 유형 0을 입력하십시오.")
),
responses(
    (status = 200, description = "토큰 목록이 성공적으로 검색되었습니다", body = [NftMetadata]),
    (status = 400, description = "잘못된 요청"),
    (status = 500, description = "내부 서버 오류")
)
)]

async fn list_tokens(
    Extension(web3_client): Extension<Arc<Web3Client>>,
    token_owner: Option<Path<String>>,
) -> Result<Json<Vec<NftMetadata>>, StatusCode> {
    let owner_address = match token_owner {
        Some(ref owner) if owner.0 != "0" => match owner.0.parse::<Address>() {
            // 소유자가 "0"이 아닌지 확인
            Ok(addr) => addr,
            Err(_) => return Err(StatusCode::BAD_REQUEST),
        },
        _ => Address::default(), // "0" 또는 없음을 모든 토큰을 나열하는 표시로 취급
    };

    let token_ids =
        match get_all_owned_tokens(&web3_client.web3, &web3_client.contract, owner_address).await {
            Ok(ids) => ids,
            Err(_) => return Err(StatusCode::INTERNAL_SERVER_ERROR),
        };

    let mut nft_metadata_list = Vec::new();
    for token_id in token_ids {
        match get_nft_details(&web3_client.contract, token_id.to_string()).await {
            Ok((_, token_name, _onwer, token_uri)) => {
                let nft_metadata = NftMetadata {
                    token_id: token_id.to_string(),
                    owner_address: _onwer.to_string(),
                    token_name,
                    token_uri,
                };
                nft_metadata_list.push(nft_metadata);
            }
            Err(e) => eprintln!("토큰 {}에 대한 메타데이터 검색 실패: {:?}", token_id, e), // 필요에 따라 로그 또는 오류 처리
        }
    }

    Ok(Json(nft_metadata_list))
}
```



**list_tokens** 엔드포인트는 특정 주소가 소유한 모든 NFT를 나열하거나 특별한 매개변수가 제공된 경우 모두 발행된 NFT를 나열합니다. 이는 스마트 계약으로 소유한 토큰 ID를 조회하고 각각의 메타데이터를 가져오는 과정을 포함합니다.

## NFT 생성 유틸리티 함수

```rust
#[utoipa::path(
post,
    path = "/mint",
    request_body = MintNftRequest,
responses(
    (status = 200, description = "NFT가 성공적으로 생성됨", body = NftMetadata),
    (status = 400, description = "잘못된 요청"),
    (status = 500, description = "내부 서버 오류")
)
)]
async fn process_mint_nft(
    Extension(web3_client): Extension<Arc<Web3Client>>,
    Json(payload): Json<MintNftRequest>,
) -> Result<Json<NftMetadata>, AppError> {
    let owner_address = payload
        .owner_address
        .parse::<Address>()
        .map_err(|_| AppError::BadRequest("유효하지 않은 소유자 주소".into()))?;

    // 환경 변수에서 모의 개인 키 가져오기
    let mock_private_key = env::var("MOCK_PRIVATE_KEY").expect("MOCK_PRIVATE_KEY가 설정되어 있어야 함");

    // 서명될 데이터 시뮬레이션
    let data_to_sign = format!("{}:{}", payload.owner_address, payload.token_name).into_bytes();

    // 모의 서명 수행
    let _mock_signature = mock_sign_data(&data_to_sign, &mock_private_key)?;

    let upload_response = match ipfs::file_upload(payload.file_path.clone()).await {
        Ok(response) => response,
        Err(_) => unreachable!(), // Err은 Infallible이므로 이 부분은 실행되지 않음
    };

    let uploaded_token_uri = upload_response.token_uri.clone().unwrap();

    // 파일 URL을 토큰 URI로 사용하여 mint_nft 호출
    let token_id = mint_nft(
        &web3_client.web3,
        &web3_client.contract,
        owner_address,
        uploaded_token_uri.clone(),
        payload.token_name.clone(),
    )
    .await
    .map_err(|e| AppError::InternalServerError(format!("NFT 생성 실패: {}", e)))?;

    Ok(Json(NftMetadata {
        token_id: token_id.to_string(),
        owner_address: payload.owner_address,
        token_name: payload.token_name,
        token_uri: uploaded_token_uri.clone(),
    }))
}
```

이 유틸리티 함수는 스마트 계약과 상호작용하여 새 NFT를 생성하며, 소유자, 토큰 URI 및 토큰 이름을 지정합니다. 블록체인으로 거래를 구성하고 보내는 세부사항을 캡슐화합니다.



## 소유한 토큰 검색 유틸리티 함수

```rust
async fn get_all_owned_tokens<T: Transport>(
    _web3: &Web3<T>,
    contract: &Contract<T>,
    owner: Address,
) -> Result<Vec<u64>, Box<dyn Error>> {
    let options = Options::with(|opt| {
        opt.gas = Some(1_000_000.into());
    });

    let result: Vec<u64> = contract
        .query("getAllTokensByOwner", owner, owner, options, None)
        .await?;

    Ok(result)
}
```

이 함수는 지정된 주소가 소유한 토큰 ID 목록을 가져와 사용자가 소유한 NFT 목록을 나열하는 데 도움이 됩니다. 이 함수는 스마트 계약을 쿼리하고 응답을 쉽게 소비할 수 있도록 포맷합니다.

## 서버 초기화 및 라우트 정의



## 주소 지정 및 들어오는 요청 수신 시작

메인 함수는 Axum 서버를 초기화하며, 정의된 엔드포인트에 대한 경로를 설정하고 CORS와 같은 미들웨어를 구성합니다. 서버를 지정된 주소에 바인딩하고 들어오는 요청을 수신하기 시작합니다.

## 정적 파일 제공 오류 처리

```rust
async fn handle_serve_dir_error(error: io::Error) -> (StatusCode, String) {
    (
        StatusCode::INTERNAL_SERVER_ERROR,
        format!("정적 파일 제공에 실패했습니다: {}", error),
    )
}
```



이 기능은 정적 파일을 제공하는 중에 발생하는 문제를 명확히 보고하는 오류 처리를 제공하여 static/swagger-ui 디렉토리에서 파일을 제공하는 동안 발생하는 문제를 처리합니다.

main.rs 파일의 각 섹션은 Rust NFT API의 전체 기능에 기여합니다. 엔드포인트 정의 및 요청 처리부터 이더리움 및 IPFS와 상호 작용하여 NFT 응용 프로그램에 강력한 백엔드를 제공합니다.

GitHub 레포를 확인해보세요! 🚀

Rust NFT API에 대한 이 깊은 탐구가 유익하고 흥미로웠기를 바랍니다. 우리 일반적인 글보다 조금 더 길어진 것 같아도 감사합니다.



그 결과에 만족하시는 분들이나 코드를 더 탐구하고 싶은 분들께 좋은 소식이 있어요!

전체 코드베이스 및 프로젝트를 설정하는 단계별 지침을 찾을 수 있는 GitHub 저장소를 소개합니다: [https://github.com/luishsr/rust-nft-api](https://github.com/luishsr/rust-nft-api). 언제든지 참여해보세요. 즐거운 코딩 되세요!

# 🚀 Luis Soares의 더 많은 정보 확인하기

📚 학습 허브: 러스트, 소프트웨어 개발, 클라우드 컴퓨팅, 사이버 보안, 블록체인, 리눅스 등 다양한 기술 분야에서 지식을 확장할 수 있는 광범위한 자료 모음으로 지식을 향상해보세요:



- GitHub Repos와 함께한 실전 튜토리얼: 단계별 튜토리얼로 다양한 기술에 대한 실용적인 기술을 습득하고 전용 GitHub 리포지토리를 이용하세요. [튜토리얼 확인하기](link)
- 깊이 있는 안내서 및 기사: Rust, 소프트웨어 개발, 클라우드 컴퓨팅 등 핵심 개념을 자세한 안내서와 실용적인 예시가 가득한 기사로 깊게 들어가 보세요. [더 읽기](link)
- E-Book 컬렉션: "Mastering Rust Ownership"와 "Application Security Guide"와 같은 제목의 무료 E-Book 시리즈로 여러 기술 분야에 대한 이해를 높이세요. [E-Book 다운로드](link)
- 프로젝트 쇼케이스: API 게이트웨이, 블록체인 네트워크, 사이버 보안 도구, 클라우드 서비스 등 다양한 도메인에서 완전히 기능하는 프로젝트를 발견하세요. [프로젝트 보기](link)
- LinkedIn 뉴스레터: LinkedIn에서 구독하면 Rust, 소프트웨어 개발 및 신생 기술에 대한 정기 업데이트와 통찰을 얻을 수 있습니다. [여기서 구독하기](link)

🔗 나와 소통하기:

- Medium: Medium에서 제가 쓴 기사를 읽고 도움이 됐다면 claps를 주세요. 제게 글쓰고 Rust 콘텐츠를 계속 공유하게 하는 원동력이 됩니다. [Medium 팔로우하기](link)
- 개인 블로그: 제 개인 블로그에서 더 많은 Rust 관련 콘텐츠를 찾아보세요. [블로그 방문하기](link)
- LinkedIn: 더 많은 통찰을 얻기 위해 전문 네트워크에 참여하세요. [LinkedIn에서 접속하기](link)
- Twitter: Rust 프로그래밍에 대한 빠른 업데이트와 생각을 보려면 Twitter로 팔로우하세요. [Twitter 팔로우하기](link)

대화하고 싶으세요? 댓글을 남기거나 메시지를 보내주세요!



모두 좋은 일 되길 바라며,

Luis Soares  
luis.soares@linux.com  

시니어 소프트웨어 엔지니어 | 클라우드 엔지니어 | SRE | 기술 리드 | Rust | Golang | Java | ML AI 및 통계 | Web3 & 블록체인