## Proto

```go
message CapabilityRequest {
  repeated gnmi_ext.Extension extension = 1;
```

```go
message CapabilityResponse {
  repeated ModelData supported_models = 1;
  repeated Encoding supported_encodings = 2;
  string gNMI_version = 3;
  repeated gnmi_ext.Extension extension = 4;
}
```

```go
message ModelData {
  string name = 1;
  string organization = 2;
  string version = 3;
}
```

```go
enum Encoding {
  JSON = 0;
  BYTES = 1;
  PROTO = 2;
  ASCII = 3;
  JSON_IETF = 4;
}
```