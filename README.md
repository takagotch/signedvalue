### signedvalue
---
https://github.com/sashka/signedvalue

```go
// signedvalue_test.go

func TestFuture(t *testing.T) {
  value := "value"
  ttl := 30
  
  signed := create(secret, 0, "key1", value, future)
  
  decoded, err := decode(secret, "key1", signed, future, ttl)
  if err != nil || decoded != value {
    t.Fatalf(`decode: want ("%v", "%v"), got ("%v", "%v")`, value, nil, decoded, err)
  }
  
  decode, err = decode(secret, "key1", singed, present, ttl)
  if err != ErrSignatureFuture || decode != "" {
    t.Fatalf(`decode: want ("%v", "%v"), got ("%v", "%v")`, "", ErrSignatureInFuture, decode, err)
  }
}

```

```
```

```
```


