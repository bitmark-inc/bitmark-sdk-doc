# Query

## Asset

| Attribute | Description |
| --------- | ----------- |
| id | The asset ID |
| name | The asset name |
| metadata | The asset metadata, described by key-value paris |
| fingerprint | The hash value of the asset content, which serves as the unique identifier for the asset record in the blockchain |
| registrant | The account registering the asset |
| status | Possible values: `pending`, `confirmed` |
| block_number | The block which the asset record is added to |
| created_at | When the asset status becomes `confirmed` |

### Query for a specific asset

```go
asset, err := asset.Get(assetId)
```

### Query for a set of assets

```go
params := asset.NewQueryParams().
    RegisteredBy("ec6yMcJATX6gjNwvqp8rbc4jNEasoUgbfBBGGyV5NvoJ54NXva").
    Limit(10)
    
it := bitmark.NewIterator(params)
for it.Prev() {
    for _, asset := range it.Values() {
        // read query results here
    }
}
if it.Err() != nil {
    // handler error here
}
```

## Bitmark

| Attribute | Description |
| --------- | ----------- |
| id | The bitmark ID |
| asset_id | The asset ID |
| asset | The asset record |
| latest_tx_id | The latest tx ID |
| issuer | The account issuing the bitmark |
| owner | The account currently owningthe bitmark |
| offer | See the offer attributes below. |
| status | Possible values: `issuing`, `transferring`, `offering`, `settled` |
| block_number | ??? |
| created_at | When the bitmark is issued |
| updated_at | The last time when the bitmark is transferred |

### Offer

| Attribute | Description |
| --------- | ----------- |
| id | The offer ID |
| from | Represents the account creating the offer |
| to | Represents the account which can accept/reject the bitmark |
| record | The half-signed transfer tx |
| extra_info | Attached JSON message for indicating the details of this offer |
| created_at | The create time of the offer |

### Query for a specific bitmark

```go
bitmark, err := bitmark.Get(bitmarkId, false)
```

### Query for a set of bitmarks

```go
params := NewQueryParams().
    IssuedBy("e1pFRPqPhY2gpgJTpCiwXDnVeouY9EjHY6STtKwdN6Z4bp4sog").
    OwnedBy("eZpG6Wi9SQvpDatEP7QGrx6nvzwd6s6R8DgMKgDbDY1R5bjzb9", true).
    OfferTo("dzJjGazcRuC7KhgU5o2Y2YV8wGXhBBabGRACa2Uyg4ZkVWwyNu").
    OfferFrom("eZpG6Wi9SQvpDatEP7QGrx6nvzwd6s6R8DgMKgDbDY1R5bjzb9").
    ReferencedAsset("1f21148a273b5e63773ceee976a84bcd014d88ac2c18a29cac4442120b430e158386b0ad90515c69e7d1fd6df8f3d523e3550741e88d0d04798627a57b0006c9").
    LoadAsset(true).
    Limit(10)

it := bitmark.NewIterator(params)
for it.Prev() {
    for _, bitmark := range it.Values() {
        // read query results here
    }
}
if it.Err() != nil {
    // handler error here
}
```

## Tx

| Attribute | Description |
| --------- | ----------- |
| id | The tx ID |
| bitmark_id | Links to the bitmark which this tx is applied to |
| asset_id | The asset ID |
| asset | The asset record |
| owner | The account owning the bitmark by this transaction |
| status | Possible values: `pending`, `confirmed` |
| block_number | The block which the tx record is added to |

A new tx record is generated accordingly when there is an update to the bitmark ownership.

### Query for a specific transaction

```go
tx, err := tx.Get(txId, true)
```

### Query for a set of transactions

```go
params := tx.NewQueryParams().
    OwnedBy("eZpG6Wi9SQvpDatEP7QGrx6nvzwd6s6R8DgMKgDbDY1R5bjzb9", true).
    ReferencedAsset("0e0b4e3bd771811d35a23707ba6197aa1dd5937439a221eaf8e7909309e7b31b6c0e06a1001c261a099abf04c560199db898bc154cf128aa9efa5efd36030c64").
    ReferencedBitmark("58737de5ad68a535da6277da62d11eb3ed76ff6dd7fc2adf3c42a4096d9a2518").
    LoadAsset(true).
    Limit(10)
    
it := tx.NewIterator(params)
for it.Prev() {
    for _, bitmark := range it.Values() {
        // read query results here
    }
}
if it.Err() != nil {
    // handler error here
}
```

### Query the provenance of a bitmark

```go
params := tx.NewQueryParams().
    ReferencedBitmark("58737de5ad68a535da6277da62d11eb3ed76ff6dd7fc2adf3c42a4096d9a2518")
```

### Query the transaction history of an account

```go
params := tx.NewQueryParams().
    OwnedBy("eZpG6Wi9SQvpDatEP7QGrx6nvzwd6s6R8DgMKgDbDY1R5bjzb9", true)
```