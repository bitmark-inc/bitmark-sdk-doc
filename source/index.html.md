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
  - examples


search: true
---

# Overview

Bitmark makes simple tools that empower anyone to assert ownership over their digital lives. Although digital assets increasingly constitute what we create and value most, there is no way for individuals to claim ownership of them. By building a universal digital property system, Bitmark envisions a world in which everyone owns a piece of the digital economy.The Bitmark SDK is a library that enables creation, transfer, and authentication of digital properties in the Bitmark property system. This SDK allows developers to easily build on the core Bitmark infrastructure by reading from and writing to the open-source Bitmark blockchain.

# Getting Started

## Installation

```javascript
```

```swift
pod 'BitmarkSDK'
```

```java
```

```go
go get github.com/aws/bitmark-sdk-go
```

Bitmark packages the SDK library for varies of languages. To install it, you can run the command.

```javascript
```

```swift
import BitmarkSDK
```

```java
```

```go
import "github.com/bitmark-inc/bitmark-sdk-go"
```

After install the sdk, you can import the library in your code like this.

## Get your API token

Before you start using it, you need to register an account from Bitmark Inc to get an API token. Please contact our [support](mailto:support@bitmark.com).

## Initialize

### Configuration

```javascript
```

```swift
import BitmarkSDK
```

```java
```

```go
import "github.com/bitmark/bitmark-inc/bitmark-sdk-go"

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
