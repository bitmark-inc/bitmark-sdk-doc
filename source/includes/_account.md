# Account

Account is a fundenmantal unit in the bitmark blockchain system. There are multiple ways to create an account object.

## Create an account

```javascript
let account = sdk.Account();
```

```swift
let account = try Account()
```

```java
```

```go
account := sdk.Account.New()
```

## Export an account

We provide two formats for exporting an account: *seed* and *recovery phrase*, both of which store all the information needed to recover a Bitmark account.


```javascript
let seed = account.getSeed();
// 5XEECttvVsk5xPjZ1zrgtWoauw2xmPwTKCWEN5GF24UpaGZhAGS6tXd
```

```swift
let seed = try account.seed()
// 5XEECttvVsk5xPjZ1zrgtWoauw2xmPwTKCWEN5GF24UpaGZhAGS6tXd
```

```java
```

```go
seed := account.Seed()
// 5XEECttvVsk5xPjZ1zrgtWoauw2xmPwTKCWEN5GF24UpaGZhAGS6tXd
```

The *seed* is designed for services which act as custodians of Bitmark accounts. By mapping user ID and bitmark account seed, you can manage bitmarks and assets on behalf of your application users. Make sure seeds are stored in a secure way.

Back up an account from its seed is a straight forward way. It outputs bytes of an account seed. You can save it in your storage, such as, a database. And use it to recover an account for a later on operation.

```javascript
let recoveryPhrase = account.getRecoveryPhrase();
// ["acid", "maze", "movie", "turn", "stereo", "over",
//  "legal", "unhappy", "deny", "early", "scout", "energy",
//  "mule", "gap", "member", "vendor", "kangaroo", "toddler",
//  "flower", "knee", "sick", "number", "acoustic", "you"]
```

```swift
let phrase = try account.recoveryPhrase()
// ["acid", "maze", "movie", "turn", "stereo", "over",
//  "legal", "unhappy", "deny", "early", "scout", "energy",
//  "mule", "gap", "member", "vendor", "kangaroo", "toddler",
//  "flower", "knee", "sick", "number", "acoustic", "you"]
```

```java
```

```go
phrase := account.RecoveryPhrase()
// ["acid", "maze", "movie", "turn", "stereo", "over",
//  "legal", "unhappy", "deny", "early", "scout", "energy",
//  "mule", "gap", "member", "vendor", "kangaroo", "toddler",
//  "flower", "knee", "sick", "number", "acoustic", "you"]
```

The *recovery phrase*, which consists of 24 mnemonic words, is designed for individuals to backup their accounts. The recovery phrase should be handed over to the user after the account is created.

The concept of this recovery phrase comes from [BIP39 - Mnemonic code for generating deterministic keys](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki). With these phrase, people can easily written down the human readable words instead of random strings which is difficult to understand.

## Import an account

On the contrast, there are functions for you to recover the accounts.

```javascript
let account = Account.fromSeed("5XEECttvVsk5xPjZ1zrgtWoauw2xmPwTKCWEN5GF24UpaGZhAGS6tXd");
```

```swift
account = Account(fromSeed: "5XEECttvVsk5xPjZ1zrgtWoauw2xmPwTKCWEN5GF24UpaGZhAGS6tXd")
```

```java
```

```go
account := sdk.Account.FromSeed("5XEECttvVsk5xPjZ1zrgtWoauw2xmPwTKCWEN5GF24UpaGZhAGS6tXd")
```

### Recover from seed

With a seed, you can recover an account.

### Recover from phrase


```javascript
let account = Account.fromRecoveryPhrase([
    "acid", "maze", "movie", "turn", "stereo", "over", "legal", "unhappy",
    "deny", "early", "scout", "energy", "mule", "gap", "member", "vendor",
    "kangaroo", "toddler", "flower", "knee", "sick", "number", "acoustic", "you"]
);
```

```swift
let account = try Account(fromRecoveryPhrase: [
    "acid", "maze", "movie", "turn", "stereo", "over", "legal", "unhappy",
    "deny", "early", "scout", "energy", "mule", "gap", "member", "vendor",
    "kangaroo", "toddler", "flower", "knee", "sick", "number", "acoustic", "you"]
)
```

```java
```

```go
account := sdk.Account.FromRecoveryPhrase([]string{
    "acid", "maze", "movie", "turn", "stereo", "over", "legal", "unhappy",
    "deny", "early", "scout", "energy", "mule", "gap", "member", "vendor",
    "kangaroo", "toddler", "flower", "knee", "sick", "number", "acoustic", "you",
})
```

With a phrase, you can recover an account.

## Get the account number

```javascript
let accountNumber = account.getAccountNumber();
// ffzcoJeg7p6kJrV6VNhS6juuceTCKMmek1WrXopvbzNTvYqANy
```

```swift
let accountNumber = account.accountNumber()
// ffzcoJeg7p6kJrV6VNhS6juuceTCKMmek1WrXopvbzNTvYqANy
```

```java
```

```go
accountNumber := account.AccountNumber()
// ffzcoJeg7p6kJrV6VNhS6juuceTCKMmek1WrXopvbzNTvYqANy
```

The account number of an account serves as a pseudonymous identifier within the Bitmark blockchain.

Account number is a combination of arbitrary strings. In the real life, you deposit to and withdraw from a bank account. With the bitmark account number, people can check the ownership, transfer from and transfer to that account.

## Account utility functions

In the account library, there are some helper functions.

### Validate account number

```javascript
let isValid = Account.isValidAccountNumber(accountNumber);
```

```swift
let isValid = Account.isValidAccountNumber(accountNumber)
```

```java
```

```go
valid := sdk.Account.IsValidAccountNumber(accountNumber)
```

The function chech whether a given account number is valid in currently runtime environment. It returns `true` / `false`.

### Parse information from an account number

```javascript
let accountInfo = Account.parseAccountNumber(accountNumber);
//{
//  network: "livenet",
//  pubKey: "6kJrV6VNhS6juuceTCKMmek1WrXopvbzNTvYqANy"
//}
```

```swift
let (network, pubkey) = try Account.parseAccountNumber(accountNumber)
```

```java
```

```go
network, pubkey := sdk.Account.ParseAccountNumber(accountNumber)
```

The function parses an account number and returns its network and public key.
