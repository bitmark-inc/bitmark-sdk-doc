# Account

Account is a fundenmantal unit in the bitmark blockchain system. There are multiple ways to create an account object.

## Create an account

```javascript
```

```swift
```

```java
```

```go
account := sdk.account.New()
```

## Export an account

We provide two formats for exporting an account: *seed* and *recovery phrase*, both of which store all the information needed to recover a Bitmark account.

The *seed* is designed for services which act as custodians of Bitmark accounts. By mapping user ID and bitmark account seed, you can manage bitmarks and assets on behalf of your application users. Make sure seeds are stored in a secure way.

The *recovery phrase*, which consists of 24 mnemonic words, is designed for individuals to backup their accounts. The recovery phrase should be handed over to the user after the account is created.

```javascript
```

```swift
```

```java
```

```go
// to seed
seed := account.Seed()
// to recovery phrase
phrase := account.RecoveryPhrase()
```

## Import an account

```javascript
```

```swift
```

```java
```

```go
// from seed
account := sdk.account.FromSeed("5XEECttvVsk5xPjZ1zrgtWoauw2xmPwTKCWEN5GF24UpaGZhAGS6tXd")

// from recovery phrase
account := sdk.account.FromSeed([]string{
    "acid", "maze", "movie", "turn", "stereo", "over", "legal", "unhappy",
    "deny", "early", "scout", "energy", "mule", "gap", "member", "vendor",
    "kangaroo", "toddler", "flower", "knee", "sick", "number", "acoustic", "you",
})
```

## Get the account number

The account number of an account serves as a pseudonymous identifier within the Bitmark blockchain.

```javascript
```

```swift
```

```java
```

```go
accountNumber := acct.AccountNumber()
// ffzcoJeg7p6kJrV6VNhS6juuceTCKMmek1WrXopvbzNTvYqANy
```

## Account number utility functions

```javascript
```

```swift
```

```java
```

```go
valid := sdk.account.IsValidAccountNumber(acctNo)
network, pubkey := sdk.account.parseAccountNumber(acctNo)
```

<aside class="warning">
THE FOLLOWING IS THE NOTES FOR SDK DESIGNERS
</aside>

## Account Method

```go--v1
type Account interface {
    Sign([]byte) []byte
    AccountNumber() string
    Encrypt([]byte) []byte
    Decrypt([]byte) []byte
    EncryptionPublicKey() []byte
}
```

```go--v2
type Account interface {
    Sign([]byte) []byte
    AccountNumber() string
    Encrypt([]byte) []byte
    Decrypt([]byte) []byte
    EncryptionPublicKey() []byte
}
```

For each account object, the following function should be implemented:

- `Sign([]byte) []byte`
- `AccountNumber() string`
- `RecoveryPhrase() string`
- `Seed() string`

## Account Static Function

- `FromRecoveryPhrase`
- `FromSeed`
- `FromShareKeys`


## Account Utility

- `IsValidAccountNumber`
- `GetNetwork`

## Local Account

In this SDK, we provide a built-in function for managing account locally. In this account, there are two keys included.
They are account key and encrpytion key. The public key of an encryption key will be published to everyone so that
people are able to exchange their data in a transfer.

```go--v1
account := client.CreateAccount()
```

```go--v2
account := sdk.account.Create()
```

```javascript
var account = new client.Account()
```

For a new account, since the registration of an encryption public key is required, you will create a new account by the client object.

<aside class="notice">
Or we can separate the registration part from account creation.
</aside>

### Back up and recover an account

For the local key, a user can backup and recover his accounts by the following three different kind of method:

- Seed bytes
- Mnemonic recovery phrase
- Shamir's Secret Sharing

### Seed bytes

```go--v1
// Back up the seed
seed := account.Seed()
// Recover from a seed
account := sdk.AccountFromSeed(seed)
```


```go--v2
// Back up the seed
seed := account.Seed()
// Recover from a seed
account := sdk.account.AccountFromSeed(seed)
```

```javascript
// Back up the seed
var seed = account.seed()
// Recover from a seed
var account = sdk.accountFromSeed(seed)
```

Back up an account from its seed is a straight forward way. It outputs bytes of an account seed. You can save it in your storage, such as, a database. And use it to recover an account for a later on operation.

### Mnemonic recovery phrase

```go--v1
// Back up a phrase
phrase := account.RecoveryPhrase()
// Recover from a phrase
account := sdk.AccountFromRecoveryPhrase(phrase)
```

```go--v2
// Back up a phrase
phrase := account.RecoveryPhrase()
// Recover from a phrase
account := sdk.account.AccountFromRecoveryPhrase(phrase)
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

## AccountNumber

> **Derived from an account object**

```go--v1
accountNumber := account.AccountNumber()
```

```go--v2
accountNumber := account.AccountNumber()
```

> **Recover from a WIF string**

```go--v1
sdk.AccountNumberFromString()
// sdk.AccountNumberFromBuffer()
```

Account number is a combination of arbitrary strings. In the real life, you deposit to and withdraw from a bank account. With the bitmark account number, people can check the ownership, transfer from and transfer to that account.

There are couples of ways to generate an account number. You can derive it by an account object or recover it from a KIF strings.
