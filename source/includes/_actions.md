# Assets

```go
asset := sdk.NewAsset(fingerprint, name, metadata, accessibility)
asset.AddMetadata(map[string]string{
    "test": "test"
})
asset.Sign(account)
client.RegisterAsset(asset)
```

For every unregistered assets, one needs to register it before using it as a reference in an issue(next section).

### Asset parameter

Config    | Options       | Description
--------- | -----------    | -----------
fingerprint   | _string_     | Fingerprint of an asset
name   | _string_ | Name of the asset
metadata   | _object_ | Additional metadata for an asset
accessibility   | `public`, `private` | The accessibility of the asset

<aside class="notice">
An asset record will not be confirmed without an confirmed issue point to it. Unconfirmed asset records will be expired three days after they register into the blockchain.
</aside>

## Upload an asset

After you register an asset to the blockchain, you will receive an upload url for uploading your file. If it is a public asset, you need to make sure the fingerprint of the file match the record you just registered. For private asset, we will bypass the fingerprint check

```go
asset.Upload(fileBytes)
```

## Download an asset

You can use this function to download an asset file if the asset record has already confirmed in the blockchain.

```go
asset.Download()
```

# Issue

```go
issueRequest := sdk.NewIssue(asset.id, account, quantityOption)
issueRequest.Sign(account)
bitmarks := client.IssueBitmarks(issueRequest) // return []Bitmark
for _, bitmark := range bitmarks {
    bitmark.Status()
}
```

Issue is to register a digital asset into the blockchain. An issue needs to link to a specific asset. Before you issue a digital property, you need to create an asset. For each time you issue, it returns a list of bitmark objects.

### Issue parameter

Config    | Options   | Description
--------- | ----------- | -----------
assetId   | _string_  | an asset that this issue references to
account   | _account_ | owner of an issue
quantityOptions   | _object_ | options for an issue

### Quantity options

Either `quantity` or `nonces` should be given for issue bitmarks.
If `nonces` is given, it will issue by using the nonces you provided and ignore the `quantity`.
If `quantity` is set, it generate random nonces matching the quantity and use those nonces for issue.

```go
type IssueQuantityOptions struct {
    Quantity int
    Nonces []int
```

Config      | Options   | Description
---------  | ----------- | -----------
quantity   | _integer_  | numbers of bitmark to issue
nonces     | _array of integer_ | issue bitmark with specific nonces

## Batch Issues

```go
issue := sdk.NewIssue(asset.id, account, quantityOption)
issue.Sign(account)
// this step is a bit of redundant. but abstract more here,
// so that we can have further implementation in the future.
batchIssues := sdk.NewBatchIssues()
batchIssues.AddIssue(issue)

// for-loop api calls
bitmarks := client.BatchIssueBitmarks(batchIssues) // []Bitmarks
```

The quantity limitation of issuing is 100. If there are a large amounts of properties to be issue, you would like to use the bathc issue function.

## Giveaway Issue (issue then transfer)

To be defined

# Transfer

Bitmark transfer, which is the process of transferring bitmark ownership from one Bitmark account to another. Once a transfer request is sumbitted, it returns you a transaction object which includes information, such as, transaction status and its half-signed record.

There are two ways to transfer bitmark:

- one-signature transfer
- two-signature transfer

What makes two-signature transfer different from one-signature transfer is that a transfer requires an acceptance from a receiptant.

**Transfer parameter**

Config    | Options   | Description
--------- | ----------- | -----------
bitmark   | _bitmark_  | an asset that this issue references to
receiver   | _account_ | next owner of a bitmark
countersigned   | `true`, `false` | represnet whether this is a 1-signature or 2-signature

## Submit one-signature transfer

```go
transferRequest := sdk.NewTransfer(bitmark, receiver, false)
transferRequest.Sign(account)
client.TransferBitmark(transferRequest)
```

A user can submit a bitmark to another without any permission.


## Submit two-signature transfer

```go
transferRequest := sdk.NewTransfer(bitmark, receiver, true)
transferRequest.Sign(account)
tx := client.TransferBitmark(transferRequest)
```

For some scenario, the developer want to get a permission from the receiver before we transfer a property to it. In the case, you will submit a two-signature transfer.

<aside class="notice">
You are not able to transfer a bitmark in the platform if there is an ongoing transfer for the bitmark.
</aside>

### Cancel a two-signature transfer

```go
transferRequest := tx.Cancel(accountB)
tx := client.TransferBitmark(transferRequest)
```

The return of `TransferBitmark` is a transaction object. A transaction can be cancelled if something goes wrong.

## Take action to bitmarks

```go
q := sdk.NewQueryOption(BITMARK, QueryOptions)
q.Sign(Account)
bitmarks := sdk.Query(q) // []Bitmark
```

A bitmark object will include its latest transaction which means you can take action to it if it is in a `WAITING` status. Everyone can query its bitmark status.


### Cancel a transfer you submitted

```go
transferRequest := bitmarks.CancelTransfer()
tx := client.TransferBitmark(transferRequest)
```

A sender can cancel his pre-created transfers if it is not accpeted or rejected by the reciver.

### Take action to incoming two-signature tranfers

```go
transferRequest := bitmarks[0].AcceptTransfer()
tx := client.TransferBitmark(transferRequest)

> Another option is to reject a transfer

transferRequest := bitmarks[0].RejectTransfer(accountB)
tx := client.TransferBitmark(transferRequest)
```

A receiver can query his incoming transfers and take action on them. Each transfer can be either `accept` or `reject`.
