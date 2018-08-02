# Account

Account is a fundenmantal unit in the bitmark blockchain system. There are multiple ways to create an account object.

## New Account

```go
account := client.CreateAccount()
```

```javascript
var account = new client.Account()
```

For a new account, since a public key registration is required, you will create a new account by the client object.

## Back up and recover an account

To make a user be able backup and recover his accounts eailsy, this SDK offers three different backing up mechanism.

- Seed bytes
- Mnemonic recovery phrase
- Shamir's Secret Sharing

### Seed bytes

```go
// Back up the seed
seed := account.Seed()
// Recover from a seed
account := sdk.AccountFromSeed(seed)
```

```javascript
// Back up the seed
var seed = account.seed()
// Recover from a seed
var account = sdk.accountFromSeed(seed)
```

Back up an account from its seed is a straight forward way. It outputs bytes of an account seed. You can save it in your storage, such as, a database. And use it to recover an account for a later on operation.

### Mnemonic recovery phrase

```go
// Back up a phrase
phrase := account.RecoveryPhrase()
// Recover from a phrase
account := sdk.AccountFromRecoveryPhrase(phrase)
```

```javascript
// Back up a phrase
var phrase = account.recoveryPhrase()
// Recover from a phrase
var account = sdk.accountFromRecoveryPhrase(phrase)
```

The concept of this recovery phrase comes from [BIP39 - Mnemonic code for generating deterministic keys](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki). With these phrase, people can easily written down the human readable words instead of random strings which is difficult to understand.

In this SDK, we turn an account seed into 24 word for backing up.

### Shamir's Secret Sharing


```go
// Back up share keys
shareKeys := account.ShamirSecretKeys()
// Recover from share keys
account := sdk.AccountFromShareKeys(shareKeys)
```

```javascript
// Back up share keys
var shareKeys = account.ShamirSecretKeys()
// Recover from share keys
var account = sdk.AccountFromShareKeys(shareKeys)
```

Shamir's Secret Sharing is a form of secret sharing, where a secret is divided into parts, giving each participant its own unique part, where some of the parts or all of them are needed in order to reconstruct the secret.

## AccountNumber

> **Derived from an account object**

```go
accountNumber := sdk.NewAccountNumber()
```

> **Recover from a WIF string**

```go
sdk.AccountNumberFromString()
// sdk.AccountNumberFromBuffer()
```

Account number is a combination of arbitrary strings. In the real life, you deposit to and withdraw from a bank account. With the bitmark account number, people can check the ownership, transfer from and transfer to that account.

There are couples of ways to generate an account number. You can derive it by an account object or recover it from a KIF strings.
