# Store Seed

Store key in mobile application is very important to ensure the security, protect user data against attackers. **Bitmark** is a blockchain platform, everything is decentralized, so we need to keep the user's key more secure to guarantee the ownership. So we provide the utility for storing key securely in mobile application, support **Android/iOS** platform.

## Android

```java
// Storing your account
Account account = new Account();
account.saveToKeyStore(activity/* the activity that is instance of StatefulActivity*/, alias /* the account alias, avoid it with default account number*/, isAuthenticationRequired/* need to authenticate each time using key*/, new Callback0() {
                @Override
                public void onSuccess() {
                    
                }

                @Override
                public void onError(Throwable throwable) {
                    
                }
            });

// Getting your account
Account.loadFromKeyStore(activity, alias /*Simplify with your account number*/, new Callback1<Account>() {
                @Override
                public void onSuccess(Account account) {
                    
                }

                @Override
                public void onError(Throwable throwable) {
                    
                }
            });

// Deleting your account
account.removeFromKeyStore(activity, new Callback0() {
                @Override
                public void onSuccess() {
                    
                }

                @Override
                public void onError(Throwable throwable) {
                    
                }
            });

```

Bitmark Android SDK supports storing key from Android **API 21(L)**. However, we recommend you for using store seed utility from Android **API 23(M) and above** because of the security level. Of course, you can use with prior M as well. 

Bitmark Android SDK always authenticate user **each time** they make a key using requesting, so protects user against attackers. We use Android Key Store system with a lot of security algorithm for protecting user's key. 

**NOTE**: 

- You **need** to `extends StatefulActivity` in the host activity you pass for storing/getting/deleting the key.
- **React Native** is also supported as the *experimental*. You **need** to `extends StatefulReactActivity` in the host activity you want to pass for storing/getting/deleting the key.
- Some exception will be thrown if you missed some handling such as lock screen required, fingerprint enrolled required, etc...

## iOS

```swift
```