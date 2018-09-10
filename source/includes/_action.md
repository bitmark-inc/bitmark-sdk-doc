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
params, _ := asset.NewRegistrationParams(
	"name", // asset name
	map[string]string{"k1": "v1", "k2": "v2"}, // asset metadata
)

dat, _ := ioutil.ReadFile("/tmp/dat")
params.SetFingerprint(dat) // calculate the fingerprint

params.Sign(account)
params.JSON()

assetId, _ := sdk.Asset.Register(params)
```

The first step to create a digital property is to register assets.
An asset can any digital object, including files, applications, code, and data.

Each asset is described by name and metadata (optional), and can be uniquely identified by its fingerprint.

If an asset record with the same fingerprint value already exists in the blockchain, the new asset record is rejected from incorporation in the blockchain.

<aside class="notice">

An asset record won't be added to the blockchain without accompanying bitmark issuances (refer to the next section for more info). The "orphaned" asset records will be vanished after 3 days.

</aside>

# Issue bitmarks

This system records ownership claims for digital assets as digital property titles known as bitmarks.

After the asset is registered, you can issue bitmarks with a permananent reference to the corresponding asset record. 

There can be multiple issues for the same asset, each defined by a different nonce, and each representing a different instance of the property.

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
params := bitmark.NewIssuanceParams(
	assetId,
	bitmark.QuantityOptions{
		Nonces: []uint64{uint64(1), uint64(2), uint64(3)},
	},
)
params.Sign(sender)
bitmarkIds, err := bitmark.Issue(params) // returns three bitmark IDs
```

Nonce is a counter value that distinguishes different issuances for the same asset of the same owner. Typically, these represent “limited editions” of a digital asset, and the nonce can be viewed as edition number.

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
params := bitmark.NewIssuanceParams(
	assetId,
	bitmark.QuantityOptions{
		Quantity: 3,
	},
)
params.Sign(sender)
bitmarkIds, _ := bitmark.Issue(params) // returns three bitmark IDs
```

If you simply want to generate authorized copies of digital data on demand, you can set `quantity` instead. The SDK will generate random nonces automatically for issuing.

<aside class="notice">
Either <code>quantity</code> or <code>nonces</code> should be specified when issuing bitmarks. The setting of <code>nonces</code> takes precendence over <code>quantity</code>.
</aside>

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
params := bitmark.NewTransferParams("eZpG6Wi9SQvpDatEP7QGrx6nvzwd6s6R8DgMKgDbDY1R5bjzb9") // set receiver's account number
params.FromBitmark("71131367bc56628bb2eee15da274e466f2ae1533c192d60c9eeef6484b1117e3") // specify which bitmark to be transferred

// In addition to specifying bitmark directly, you can also specify the latest tx ID of this bitmark
// params.FromLatestTx("0374d3cd9a901d0c3d084c0e1d57b2c29331eafbbd183fa4fabb40eae331a3d7")

params.Sign(sender)
txId, _ := bitmark.Transfer(params)
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
