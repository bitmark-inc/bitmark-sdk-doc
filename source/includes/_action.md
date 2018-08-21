# Register an asset

For every unregistered assets, one needs to register it before using it as a reference in an issue(next section).

```javascript
```

```swift
```

```java
```

```go
params := sdk.asset.NewRegistrationParams(
    "asset_name",
    map[string]string{"desc": "sdk example"},
)

file, _ := os.Open("path/file.ext")
params.SetFingerprint(file)

params.Sign(account)
params.JSON()

assetId, _ := sdk.asset.Register(params)
```

<aside class="notice">
An asset record will not be confirmed without an confirmed issue point to it. Unconfirmed asset records will be expired three days after they register into the blockchain.
</aside>

# Issue bitmarks

Issue is to register a digital asset into the blockchain. An issue needs to link to a specific asset. Before you issue a digital property, you need to create an asset. For each time you issue, it returns a list of bitmark objects.

Either `quantity` or `nonces` should be given for issue bitmarks.
If `nonces` is given, it will issue by using the nonces you provided and ignore the `quantity`.
If `quantity` is set, it generate random nonces matching the quantity and use those nonces for issue.

## create issuances with nonce

Nonce is a counter value that distinguishes between different issuances for the same asset. The nonce can be viewed as edition number. For example, to generate 100 editions for a digital card:

```javascript
```

```swift
```

```java
```

```go
params := sdk.bitmark.NewIssuanceParams(
    assetId,
    sdk.bitmark.QuantityOptions{
        Nonces: []int{1, 2, 3, ..., 100},
    })
params.Sign(issuer)

bitmarkIds, _ := sdk.bitmark.Issue(issuances)
```

Nonce should be unique. To issue more bitmarks, make sure that new nonces are set.

## create issuances without nonce

Issue as you go.

```javascript
```

```swift
```

```java
```

```go
params := sdk.bitmark.NewIssuanceParams(
    assetId,
    sdk.bitmark.QuantityOptions{
        Quantity: 100,
    }))
params.Sign(issuer)

bitmarkIds, _ := sdk.bitmark.Issue(params)
```

# Transfer a bitmark

Bitmark transfer, which is the process of transferring bitmark ownership from one Bitmark account to another. Once a transfer request is sumbitted, it returns you a transaction object which includes information, such as, transaction status and its half-signed record.

There are two ways to transfer bitmark:

- one-signature transfer
- two-signature transfer

What makes two-signature transfer different from one-signature transfer is that a transfer requires an acceptance from a receiptant.

## 1-sig transfer

```javascript
```

```swift
```

```java
```

```go
params := sdk.bitmark.NewTransferParams(receiverAccountNumber, false)
params.FromBitmark() // asynchrous, just to check the head_id
// params.FromTx() // or synchrous
params.Sign(sender)

txId, _ := sdk.bitmark.Transfer(params)
```

A user can submit a bitmark to another without any permission.

## 2-sig transfer

For some scenario, the developer want to get a permission from the receiver before we transfer a property to it. In the case, you will submit a two-signature transfer.

<aside class="notice">
You are not able to transfer a bitmark in the platform if there is an ongoing transfer for the bitmark.
</aside>

> Sender creates a transfer offer

```javascript
```

```swift
```

```java
```

```go
params := sdk.bitmark.NewOfferParams(receiverAccountNumber, true)
params.FromBitmark() // asynchrous, just to check the head_id
// params.FromTx() // or synchrous
params.Sign(sender)

sdk.bitmark.Offer(params)
```

> Receiver needs to query if there is any bitmark transder offer waiting for his signature.

```javascript
```

```swift
```

```java
```

```go
builder := sqlbuilder.NewListBuilder()
params := builder.
    Status("offering").
    OfferTo("e1pFRPqPhY2gpgJTpCiwXDnVeouY9EjHY6STtKwdN6Z4bp4sog").
    Build()
bitmarks, _ := sdk.bitmark.List(params)
```

> Receiver wants to accept the bitmark transfer offer 

```javascript
```

```swift
```

```java
```

```go
params := sdk.bitmark.NewTransferResponseParams(bitmark, sdk.bitmark.Accpet)
params.Sign(receiver)
txId, _ := sdk.bitmark.Respond(params)
```

> Receiver wants to reject the bitmark transfer offer 

```javascript
```

```swift
```

```java
```

```go
params := sdk.bitmark.NewTransferResponseParams(bitmark, sdk.bitmark.Reject)
params.Sign(receiver)
sdk.bitmark.Respond(params)
```

> Sender wants to cancel the bitmark transfer offer

```javascript
```

```swift
```

```java
```

```go
params := sdk.bitmark.NewTransferResponseParams(bitmark, sdk.bitmark.Cancel)
params.Sign(receiver)
sdk.bitmark.Respond(params)
```
