```go
message SetRequest {
  Path prefix = 1;
  repeated Path delete = 2;
  repeated Update replace = 3;
  repeated Update update = 4;
  repeated gnmi_ext.Extension extension = 5;
}
```

```go
message Path {
  repeated string element = 1 [deprecated=true];
  string origin = 2;
  repeated PathElem elem = 3;
  string target = 4;
}
```

```go
message Update {
  Path path = 1;
  Value value = 2 [deprecated=true];
  TypedValue val = 3;
  uint32 duplicates = 4;
}
```

```go
message TypedValue {
  oneof value {
    string string_val = 1;
    int64 int_val = 2;
    uint64 uint_val = 3;
    bool bool_val = 4;
    bytes bytes_val = 5;
    float float_val = 6;
    Decimal64 decimal_val = 7;
    ScalarArray leaflist_val = 8;
    google.protobuf.Any any_val = 9;
    bytes json_val = 10;
    bytes json_ietf_val = 11;
    string ascii_val = 12;
    bytes proto_bytes = 13;
  }
}
```