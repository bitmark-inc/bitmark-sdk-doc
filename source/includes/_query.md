# Query

## Asset

| Attribute | Description |
| --------- | ----------- |
| id | |
| name | |
| metadata | |
| fingerprint | |
| registrant | |
| status | `pending`, `confirmed` |
| block_number | |
| created_at | time |

### Query for a specific asset

```go
asset, err := asset.Get(assetId)
```

<br />

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
| id | |
| asset_id | |
| asset | |
| latest_tx_id | |
| issuer | |
| owner | |
| offer | See the offer attributes below. |
| status | Possible values: `issuing`, `transferring`, `offering`, `settled` |
| block_number | |
| created_at | |
| updated_at | |

### Offer

| Attribute | Description |
| --------- | ----------- |
| id | |
| from | |
| to | |
| message | |
| extra_info | |

### Query for a specific bitmark

```go
bitmark, err := bitmark.Get(bitmarkId)
```

<br />

### Query for a set of bitmarks

```go
params := NewQueryParams().
    IssuedBy("e1pFRPqPhY2gpgJTpCiwXDnVeouY9EjHY6STtKwdN6Z4bp4sog").
    OwnedBy("eZpG6Wi9SQvpDatEP7QGrx6nvzwd6s6R8DgMKgDbDY1R5bjzb9", true).
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
| id | |
| previous_id | |
| bitmark_id | |
| asset_id | |
| type | `1-sig`, `2-sig` |
| status | |
| owner | |
| status | `pending`, `confirmed` |
| block_number | |
| created_at | |
