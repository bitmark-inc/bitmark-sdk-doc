# Query

Query is a function for get blockchain information for bitmarks, transactions, assets and blocks.

```go--v1
q := sdk.NewQueryOption(Query Type, QueryOptions)
q.Sign(Account)
t := client.Query(q)
```

## Bitmark

In this function, you can query bitmarks.

```go--v1
t := client.GetBimark(q)
t := client.GetBimarks(q)
```

```go--v2
sdk.bitmark.List(queryParameter)
sdk.bitmark.Get("id", queryParameter)
```

### Query Parameters

Config     | Options       | Description
--------- | -----------   | -----------
Type      | _string_      | `issue` or `transfer`
HalfSignedRecords   | _object_ | Object of half signed record


## Transaction

In this function, you can query transactions.

```go--v1
t := client.GetTransaction(q)
t := client.GetTransactions(q)
```

```go--v2
sdk.transaction.List(queryParameter)
sdk.transaction.Get("id", queryParameter)
```


### Query Parameters

Config     | Options       | Description
--------- | -----------   | -----------
Type      | _string_      | `issue` or `transfer`
HalfSignedRecords   | _object_ | Object of half signed record


## Asset

In this function, you can query assets.

```go--v1
t := client.GetAsset(q)
t := client.GetAssets(q)
```

```go--v2
sdk.asset.List(queryParameter)
sdk.asset.Get("id", queryParameter)
```

### Query Parameters

Config     | Options       | Description
--------- | -----------   | -----------
Type      | _string_      | `issue` or `transfer`
HalfSignedRecords   | _object_ | Object of half signed record

## Block

In this function, you can query blocks.

```go--v1
t := client.GetBlock(q)
t := client.GetBlocks(q)
```

```go--v2
sdk.blocks.List(queryParameter)
sdk.blocks.Get("id", queryParameter)
```

### Query Parameters

Config     | Options       | Description
--------- | -----------   | -----------
Type      | _string_      | `issue` or `transfer`
HalfSignedRecords   | _object_ | Object of half signed record

