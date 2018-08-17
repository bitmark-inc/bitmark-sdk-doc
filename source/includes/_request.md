# Request / Payload / Parameter

## Register asset data



```javascript
```

```swift
```

```java
```

```go
a := sdk.NewAssetRequest("asset_name", map[string]string{"desc": "sdk example"})

file, _ := os.Open("path/file.ext")
a.setFingerprint(file)

a.sign(account)
```