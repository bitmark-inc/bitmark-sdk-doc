# Account

Account is a fundenmantal unit in the bitmark blockchain system. There are multiple ways to create an account object.

## Create an account

```javascript
```

```swift
let account = try Account()
```

```java
```

```go
account := sdk.account.New()
```

## Export an account

We provide two formats for exporting an account: *seed* and *recovery phrase*, both of which store all the information needed to recover a Bitmark account.

The *seed* is designed for services which act as custodians of Bitmark accounts. By mapping user ID and bitmark account seed, you can manage bitmarks and assets on behalf of your application users. Make sure seeds are stored in a secure way.

Back up an account from its seed is a straight forward way. It outputs bytes of an account seed. You can save it in your storage, such as, a database. And use it to recover an account for a later on operation.

The *recovery phrase*, which consists of 24 mnemonic words, is designed for individuals to backup their accounts. The recovery phrase should be handed over to the user after the account is created.

The concept of this recovery phrase comes from [BIP39 - Mnemonic code for generating deterministic keys](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki). With these phrase, people can easily written down the human readable words instead of random strings which is difficult to understand.

```javascript
```

```swift
// to seed
let seed = try account.seed()

// to recovery phrase
let phrase = try account.recoveryPhrase()
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
// from seed
account = Account(fromSeed: "5XEECttvVsk5xPjZ1zrgtWoauw2xmPwTKCWEN5GF24UpaGZhAGS6tXd")

// from recovery phrase
let account = try Account(fromRecoveryPhrase: [
    "acid", "maze", "movie", "turn", "stereo", "over", "legal", "unhappy",
    "deny", "early", "scout", "energy", "mule", "gap", "member", "vendor",
    "kangaroo", "toddler", "flower", "knee", "sick", "number", "acoustic", "you"]
)
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

Account number is a combination of arbitrary strings. In the real life, you deposit to and withdraw from a bank account. With the bitmark account number, people can check the ownership, transfer from and transfer to that account.

```javascript
```

```swift
let accountNumber = account.accountNumber()
// ffzcoJeg7p6kJrV6VNhS6juuceTCKMmek1WrXopvbzNTvYqANy
```

```java
```

```go
accountNumber := acct.AccountNumber()
// ffzcoJeg7p6kJrV6VNhS6juuceTCKMmek1WrXopvbzNTvYqANy
```

## Account utility functions

In the account library, there are some helper functions.

### Validate account number

```javascript
```

```swift
let isValid = Account.isValidAccountNumber(accountNumber)
```

```java
```

```go
valid := sdk.account.IsValidAccountNumber(accountNumber)
```

The function chech whether a given account number is valid in currently runtime environment. It returns `true` / `false`.

### Parse information from an account number

```javascript
```

```swift
let (network, pubkey) = try Account.parseAccountNumber(accountNumber)
```

```java
```

```go
network, pubkey := sdk.account.parseAccountNumber(accountNumber)
```

The function parses an account number and returns its network and public key.
