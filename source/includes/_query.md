# Query

## Record

### Asset

| Attribute | Description |
| --------- | ----------- |
| id | |
| name | |
| metadata | |
| fingerprint | |
| registrant | |
| status | `pending`, `confirmed` |
| block_number | |
| mined_at | |

### Bitmark

| Attribute | Description |
| --------- | ----------- |
| id | |
| asset | |
| latest_tx_id | |
| issuer | |
| owner | |
| offer | |
| status | `issuing`, `transferring`, `offering`, `settled` |
| created_at | |
| updated_at | |

### Tx

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
| mined_at | |

## List

Use builder to build query options.

```
sdk.$Record.List($RecordId, options)
```

## Get

```
sdk.$Record.Get($RecordId)
```
