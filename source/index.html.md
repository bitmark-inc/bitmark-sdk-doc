---
title: Bitmark SDK Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - go--v1: go-v1
  - go--v2: go-v2
  - javascript
  - swift
  - java

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
  - objects
  - actions
  - query
  - examples


search: true
---

# Overview

Bitmark makes simple tools that empower anyone to assert ownership over their digital lives. Although digital assets increasingly constitute what we create and value most, there is no way for individuals to claim ownership of them. By building a universal digital property system, Bitmark envisions a world in which everyone owns a piece of the digital economy.The Bitmark SDK is a library that enables creation, transfer, and authentication of digital properties in the Bitmark property system. This SDK allows developers to easily build on the core Bitmark infrastructure by reading from and writing to the open-source Bitmark blockchain.

# Client

```go--v1
network := "mainnet"
client := sdk.NewClient("app-token", network)
```


```go--v2
network := "mainnet"
sdk.Init("app-token", network)
```

```javascript
var network = "mainnet"
var client = new sdk.Client("app-token", network)
```

Client is an object which performs requests to bitmark API server. Before you start using it, you need to register an account from Bitmark Inc to get an API token. Please contact our [support](mailto:support@bitmark.com).

<aside class="notice">
You need to replace <code>api-token</code> with your personal API token.
</aside>

### Client configurations

Config    | Options                      | Description
--------- | -----------                  | -----------
api-token   | _string_                       | Secret used by bitmark api for authorization
network   | `mainnet`, `testnet` | This will configure which network it is going to use in a session
