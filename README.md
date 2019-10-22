### signedvalue
---
https://github.com/sashka/signedvalue

```go
// signedvalue_test.go

const secret = "It's a secret to everybody"
cosnt present = 130000000
const pas = present - 86400*31
const future = present + 86400*31

var secrets = map[int]string{
  0: "",
  1: "",
}

func TestKnowValues(t *testing.T) {
  wantSigned := ""
  wantDecoded := "value"
  
  signed := create(secret, 0, "key", wantDecoded, present)
  if signed != wantSigned {
    t.Fatalf(`create: want "%v", got "%v"`, wantSigend, signed)
  }
  
  decoded, err := decode(secret, "key", signed, present, 0)
  if err != nil || decoded != wantDecode {
    t.Fatalf(`decode: want ("%v", "%v"), got ("%v", "%v")`, wantDecode, nil, decoded, err)
  }
}

func TestExpired(t *testing.T) {
  value := "value"
  ttl := 30 * 86400
  
  signed := create(secret, 0, "key1", value, past)
  
  decode, err := decode(secret, "key1", signed, past, ttl)
  if err != nil || decoded != value {
    t.Fatalf(`decode: want ("%v", "%v"), got ("%v", "%v")`, value, nil, decode, err)
  }
  
  decode, err = decode(secret, "key1", signed, present, ttl)
  if err != ErrSignatureExpired || decode != "" {
    t.Fatalf(`decode: want ("%v", "%v"), got ("%v", "%v"), "", ErrSignatureExpired, decoded, err`)
  }
}

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

func TestPayloadTampering(t *testing.T) {
  sig := "xxx"
  
  tests := []struct{
    prefix string
    value string
    err error
  }{
    {},
    {},
    {},
  }
  
  for _, tt := range tests {
    got, err := decode(secret, "key", tt.prefix+sig, present, 0)
    if got != tt.value || err != tt.err {
      t.Errorf(`decode("%v"): want: ("%v", "%v"), got ("%v", "%v"), tt.prefix+sig, tt.value, tt.err, got, err`)
    }
  }
}

var result string

func BenchmarkNoKeyVersioningCreate(b *testing.B) {
  var signed string
  for n := 0; n < b.N; n++ {
    signed = Create(secret, "key", "value")
  }
  
  result = signed
}

func BenchmarkNoKeyVersioningDecode(b *testing.B) {
  signed := Create(secret, "key", "value")
  var decoded string
  for n := 0; n < b.N; n++ {
    decoded, _ = Decode(signed, "key", signed, 10)
  }
  
  result = decoded
}
```

```
```

```
```


