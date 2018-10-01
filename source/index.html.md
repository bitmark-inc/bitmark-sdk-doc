---
title: Bitmark SDK Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript
  - swift
  - java
  - go

toc_footers:
  - <a href='https://bitmark.com'>Website</a>
  - <a href='https://blog.bitmark.com'>Blog</a>
  - <a href='https://bitmark.com/assets/bitmark_technical-white-paper.pdf'>White Paper</a>
  - <a href='https://github.com/bitmark-inc/bitmark-sdk'>Javascript SDK</a>
  - <a href='https://github.com/bitmark-inc/bitmark-sdk-go'>Go SDK</a>
  - <a href='https://github.com/bitmark-inc/bitmark-sdk-swift'>Swift SDK</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  # - errors
  - account
  - action
  - query

search: true
---

# Overview

The Bitmark property system is an universal property system for conferring the same property rights to digital assets that have long existed for physical assets. This system records ownership claims for digital assets as digital property titles known as bitmarks on the Bitmark public blockchain. A digital asset can be any digital object, including files, applications, code, and data. In the digital world:

**digital property = digital asset + bitmark**

The Bitmark SDK enables creation, transfer, and authentication of digital properties in the Bitmark property system. The SDK's simplified interface allows developers to easily build on the core Bitmark infrastructure by reading from and writing to the Bitmark blockchain.

# Getting Started

## Installation

```javascript
npm install bitmark-sdk
```

```swift
pod 'BitmarkSDK'
```

```java
// From your build.gradle
repositories {
    jcenter()
}

dependencies {
    implementation 'com.bitmark.sdk:java-sdk:1.0'
}

```

```go
go get github.com/aws/bitmark-sdk-go
```

## Get your API token

The API token is required to authenticate your requests to the Bitmark API.

Please contact our [support](mailto:support@bitmark.com) to create your developer account and get the API token.

## Initialize

### Configuration

```javascript
const sdk = require('bitmark-sdk');

const config = {
  API_token: "api-token",
  network: "testnet"
};

sdk.init(config);

```

```swift
import BitmarkSDK

BitmarkSDK.initialize(config: SDKConfig(apiToken: "api-token",
                                        network: .testnet,
                                        urlSession: URLSession.shared))
```

```java
final GlobalConfiguration.Builder builder = GlobalConfiguration.builder().withApiToken("api-token").withNetwork(Network.LIVE_NET);
BitmarkSDK.init(builder);

```

```java
BitmarkSDK.init("api-token");
```

```go
import sdk "github.com/bitmark/bitmark-inc/bitmark-sdk-go"

func main() {
  // Go to https://github.com/bitmark-inc/bitmark-sdk-go/config.go for details.
  httpClient := &http.Client{
    Timeout: 10 * time.Second,
  }
  config := &sdk.Config{
    APIToken: "api-token",
    Network: "testnet",
    HttpClient: httpClient,
  }
  sdk.Init(config)
}
```

<aside class="notice">
You need to replace <code>api-token</code> with your personal API token.
</aside>
