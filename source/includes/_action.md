# Register an asset

```javascript
let params = Asset.newRegistrationParams(assetName, metadata);
params.setFingerprint(filePath);
params.sign(account);

let assetId = Asset.register(params);
```

```swift
var params = try Asset.newRegistrationParams(name: "asset_name",
                                            metadata: ["desc": "sdk example"])

let fileURL = Bundle.main.url(forResource: "file", withExtension: ".ext")!
try params.setFingerprint(fileurl: fileURL)
try params.sign(account)

let assetId = try Asset.register(params)
```

```java
Map<String, String> metadata = new HashMap<>(){{
	put("desc", "sdk_example");
}};
final RegistrationParams params = new RegistrationParams("asset_name", metadata);
final File file = new File("path/file.ext");
final Callback<String> callback = new Callback<>(){
	void onSuccess(String assetId){

	};

	void onError(Throwable Throwable){

	};
};
params.generateFingerprint(file, new Callback<String>(){
	void onCompleted(String fingerprint){
		params.setFingerprint(fingerprint);
		params.sign(account);
		BitmarkSDK.asset().register(params, callback);
	};
});
```

```go
params := sdk.Asset.NewRegistrationParams(
    "asset_name",
    map[string]string{"desc": "sdk example"},
)

file, _ := os.Open("path/file.ext")
params.SetFingerprint(file)

params.Sign(account)
params.JSON()

assetId, _ := sdk.Asset.Register(params)
```

For every unregistered assets, one needs to register it before using it as a reference in an issue(next section).

<aside class="notice">
An asset record will not be confirmed without an confirmed issue point to it. Unconfirmed asset records will be expired three days after they register into the blockchain.
</aside>

# Issue bitmarks

Issue is to register a digital asset into the blockchain. An issue needs to link to a specific asset. Before you issue a digital property, you need to create an asset. For each time you issue, it returns a list of bitmark objects.

<aside class="notice">
Either <code>quantity</code> or <code>nonces</code> should be given for issue bitmarks.
If <code>nonces</code> is given, it will issue by using the nonces you provided and ignore the <code>quantity</code>.
</aside>

## Create issuances with nonces

```javascript
let params = Bitmark.newIssuanceParams(assetId, nonces = [1, 2, ..., 100]);
params.sign(account);

let bitmarkIds = Bitmark.issue(params);
```

```swift
var params = Bitmark.newIssuanceParams(assetId: assetId,
                                       nonces: [1..100])
try params.sign(issuer)

let bitmarkIds = try Bitmark.issue(params)
```

```java
final IssuanceParams params = new IssuanceParams(assetId);
params.setNonce(1,2,3,4,5);
params.sign(issuer);
BitmarkSDK.bitmark().issue(params, new Callback<String>(){
	void onSuccess(String bitmarkId){
		// TODO stuff with bitmarkId
	};

	void onError(Throwable throwable){
		// TODO stuff with throwable
	};
});
```

```go
params := sdk.Bitmark.NewIssuanceParams(
    assetId,
    sdk.Bitmark.QuantityOptions{
        Nonces: []int{1, 2, 3, ..., 100},
    })
params.Sign(issuer)

bitmarkIds, _ := sdk.Bitmark.Issue(issuances)
```

Nonce is a counter value that distinguishes between different issuances for the same asset of the same owner. The nonce can be viewed as edition number. For example, to generate 100 editions for a digital card.

The combination of nonce, owner and asset should be unique over the blockchain. To issue more bitmarks, developers need to make sure there is no duplicated nonces for issuing within a same owner and asset pair.

## Create issuances without nonces

```javascript
let params = Bitmark.newIssuanceParams(assetId, quantity = 100);
params.sign(account);

let bitmarkIds = Bitmark.issue(params);
```

```swift
var params = Bitmark.newIssuanceParams(assetId: assetId,
                                       quantity: 100)
try params.sign(issuer)

let bitmarkIds = try Bitmark.issue(params)
```

```java
final IssuanceParams params = new IssuanceParams(assetId, 100);
params.sign(issuer);
BitmarkSDK.bitmark().issue(params, new Callback<String>(){
	void onSuccess(String bitmarkId){
		// TODO stuff with bitmarkId
	};

	void onError(Throwable throwable){
		// TODO stuff with throwable
	};
});
```

```go
params := sdk.Bitmark.NewIssuanceParams(
    assetId,
    sdk.Bitmark.QuantityOptions{
        Quantity: 100,
    }))
params.Sign(issuer)

bitmarkIds, _ := sdk.Bitmark.Issue(params)
```

In the case that the nonce does not meaningful in issues, developers can use set a `quantity` for issuing. In this way, the SDK will generate random nonces matching the quantity automatically and use those nonces for issuing.

# Transfer a bitmark

Bitmark transfer, which is the process of transferring bitmark ownership from one Bitmark account to another.

There are two ways to transfer a bitmark:

- one-signature transfer
- two-signature transfer

One-signature transfer is similar to sending emails, the sender does not get the consent from the receiver before sending a mail.

Two-signature transfer is similar to express delivery, the receiver has the right to accept or reject the delivery of a package.
The actual transfer won's take effect until the receiver explicitly provides the second signature, a.k.a. countersignature, as the consent.

<aside class="notice">
A bitmark can be transferred only when its status is settled.
</aside>

## 1-sig transfer

```javascript
let params = Bitmark.newTransferParams(receiverAccountNumber, requireCounterSign = false);
params.from(bitmarkId);
params.sign(account);

let txId = Bitmark.transfer(params);
```

```swift
var params = Bitmark.newTransferParams(receiver: receiverAccountNumber,
                                       requireCounterSign: false)
try params.from(bitmarkId: bitmarkId)
try params.sign(account)

let txId = try Bitmark.transfer(params)
```

```java
final TransferParams params = new TransferParams(receiverAccountNumber, false);
final Callback<String> callback = new Callback<>(){
	void onSuccess(String txId){
		// TODO stuff with txId
	};

	void onError(Throwable throwable){
		// TODO stuff with throwable
	};
};
// Asynchronous for get latest txId
params.getLatestTxId(bitmarkId, new Callback<String>(){
	void onSuccess(String latestTxId){
		params.setPrevTxId(latestTxId);
		params.sign(sender);
		BitmarkSDK.bitmark().transfer(params, callback);
	};

	void onError(Throwable throwable){
		// TODO stuff with throwable
	}
});

// Synchronous 
params.setPrevTxId(latestTxId);
params.sign(sender);
BitmarkSDK.bitmark().transfer(params, callback);

```

```go
params := sdk.Bitmark.NewTransferParams(receiverAccountNumber, false)
params.FromBitmark() // asynchrous, just to check the head_id
// params.FromTx() // or synchrous
params.Sign(sender)

txId, _ := sdk.Bitmark.Transfer(params)
```

The sender can transfer a bitmark to another account without additional consent.

## 2-sig transfer

For some scenario, the developer want to get a permission from the receiver before we transfer a property to it. In the case, you will submit a two-signature transfer.

![link to Google!](images/2sig_transfer_bitmark_status.png)

### Propose a bitmark transfer offer

```javascript
let params = Bitmark.newOfferParams(receiverAccountNumber, requireCounterSign = true);
params.from(bitmarkId);
params.sign(senderAccount);

Bitmark.offer(params);
```

```swift
var params := Bitmark.newOfferParams(receiver: receiverAccountNumber,
                                     requireCounterSign: true)
try params.from(bitmarkId: bitmarkId)
try params.sign(account)

try Bitmark.offer(params)
```

```java
final OfferParams params = new OfferParams(receiverAccountNumber, true);
final Callback<Void> callback = new Callback<>(){
	void onSuccess(Void aVoid){
		// TODO stuff after success
	};

	void onError(Throwable throwable){
		// TODO stuff with throwable
	};
};
// Asynchronous for get latest txId
params.getLatestTxId(bitmarkId, new Callback<String>(){
	void onSuccess(String latestTxId){
		params.setPrevTxId(latestTxId);
		params.sign(sender);
		BitmarkSDK.bitmark().offer(params, callback);
	};

	void onError(Throwable throwable){
		// TODO stuff with throwable
	}
});

// Synchronous
params.setPrevTxId(latestTxId);
params.sign(sender);
BitmarkSDK.bitmark().offer(params, callback);
```

```go
params := sdk.Bitmark.NewOfferParams(receiverAccountNumber, true)
params.FromBitmark() // asynchrous, just to check the head_id
// params.FromTx() // or synchrous
params.Sign(sender)

sdk.Bitmark.Offer(params)
```

The current owner of a bitmark can propose a transfer offer for another account if the status of the bitmark is `settled`,
i.e. either the issue or the transfer transaction of this bitmark is already confirmed on the blockchain.
The actual ownership transfer won't happen until the receiver accepts the offer.

### Query offering bitmarks

```javascript
let builder = QueryBuilder.newListBuilder();
let params = builder
    .status("offering")
    .offerTo("e1pFRPqPhY2gpgJTpCiwXDnVeouY9EjHY6STtKwdN6Z4bp4sog")
    .build();

let bitmarks = Bitmark.List(params);
```

```swift
var builder = QueryBuilder.newListBuilder()
let params = builder
    .status("offering")
    .offerTo("e1pFRPqPhY2gpgJTpCiwXDnVeouY9EjHY6STtKwdN6Z4bp4sog")
    .build()
let bitmarks = try Bitmark.List(params)
```

```java
QueryParams params = QueryParams.builder()
					.withStatus("offering")
					.offerTo("e1pFRPqPhY2gpgJTpCiwXDnVeouY9EjHY6STtKwdN6Z4bp4sog")
					.build();
BitmarkSDK.bitmark().list(params, new Callback<List<Bitmark>>(){
	void onSuccess(List<Bitmark> bitmarks){
		// TODO stuff with bitmark list
	};

	void onError(Throwable throwable){
		// TODO stuff with throwable
	}
});
```

```go
builder := sdk.NewBitmarkQueryBuilder()
params := builder.
    Status("offering").
    OfferTo("e1pFRPqPhY2gpgJTpCiwXDnVeouY9EjHY6STtKwdN6Z4bp4sog").
    Build()
bitmarks, _ := sdk.Bitmark.List(params)
```

The receiver needs to query if there is any bitmark transfer offer waiting for the countersignature.
For the details of query execution, please refer to [Query Bitmark](#bitmark).

### Accept the bitmark transfer offer

```javascript
let params = Bitmark.newTransferResponseParams(bitmark, response = RESPONSE_TYPE.ACCEPT);
params.sign(receiverAccount);

let txid = Bitmark.response(params);
```

```swift
var params = Bitmark.newTransferResponseParams(bitmark: bitmark, response: .accept)
try params.sign(receiver)
let txid = try Bitmark.response(params)
```

```java
final TransferResponseParams params = new TransferResponseParams(bitmark, "accept");
params.sign(receiver);
BitmarkSDK.bitmark().respond(params, new Callback<String>(){
	void onSuccess(String txId){
		// TODO stuff with txId
	};

	void onError(Throwable throwable){
		// TODO stuff with throwable
	};
});

```

If the receiver decides to accept the bitmark, the countersignature is generated and make the transfer action take effect.
The status of the bitmark will change from `offering` to `transferring`. The 

```go
params := sdk.Bitmark.NewTransferResponseParams(bitmark, sdk.Bitmark.Accpet)
params.Sign(receiver)
txId, _ := sdk.Bitmark.Respond(params)
```

### Reject the transfer offer

```javascript
let params = Bitmark.newTransferResponseParams(bitmark, response = RESPONSE_TYPE.REJECT);
params.sign(receiverAccount);

Bitmark.response(params);
```

```swift
var params = Bitmark.newTransferResponseParams(bitmark: bitmark, response: .reject)
try params.sign(receiver)
try Bitmark.response(params)
```

```java
final TransferResponseParams params = new TransferResponseParams(bitmark, "reject");
params.sign(receiver);
BitmarkSDK.bitmark().respond(params, new Callback<Void>(){
	void onSuccess(Void aVoid){
		// TODO stuff
	};

	void onError(Throwable throwable){
		// TODO stuff with throwable
	};
});
```

```go
params := sdk.Bitmark.NewTransferResponseParams(bitmark, sdk.Bitmark.Reject)
params.Sign(receiver)
sdk.Bitmark.Respond(params)
```

The receiver can also reject the bitmark transfer offer.
The status of the bitmark will reverted to `settled`, and the sender can create a new transfer offer.

### Cancel the transfer offer

```javascript
let params = Bitmark.newTransferResponseParams(bitmark, response = RESPONSE_TYPE.CANCEL);
params.sign(senderAccount);

Bitmark.response(params);
```

```swift
var params = Bitmark.newTransferResponseParams(bitmark: bitmark, response: .cancel)
try params.sign(receiver)
try Bitmark.response(params)
```

```java
final TransferResponseParams params = new TransferResponseParams(bitmark, "cancel");
params.sign(receiver);
BitmarkSDK.bitmark().respond(params, new Callback<Void>(){
	void onSuccess(Void aVoid){
		// TODO stuff
	};

	void onError(Throwable throwable){
		// TODO stuff with throwable
	};
});
```

```go
params := sdk.Bitmark.NewTransferResponseParams(bitmark, sdk.Bitmark.Cancel)
params.Sign(receiver)
sdk.Bitmark.Respond(params)
```

If the receiver hasn't responded to the bitmark transfer offer (neither accepted nor rejected), the sender can cancel the offer.
Similar to the case of the receiver rejectting the offer, the status of the bitmark will be set to `settled` again, and becomes available for the next transfer.
