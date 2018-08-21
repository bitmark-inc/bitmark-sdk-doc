### Shamir's Secret Sharing

```go--v1
// Back up share keys
shareKeys := account.ShamirSecretKeys()
// Recover from share keys
account := sdk.AccountFromShareKeys(shareKeys)
```

```go--v2
// Back up share keys
shareKeys := account.ShamirSecretKeys()
// Recover from share keys
account := sdk.account.AccountFromShareKeys(shareKeys)
```

```javascript
// Back up share keys
var shareKeys = account.ShamirSecretKeys()
// Recover from share keys
var account = sdk.AccountFromShareKeys(shareKeys)
```

Shamir's Secret Sharing is a form of secret sharing, where a secret is divided into parts, giving each participant its own unique part, where some of the parts or all of them are needed in order to reconstruct the secret.