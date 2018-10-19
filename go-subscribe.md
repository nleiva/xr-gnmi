## Proto

```go
message SubscribeRequest {
  oneof request {
    SubscriptionList subscribe = 1;
    Poll poll = 3;
    AliasList aliases = 4;
  }
  repeated gnmi_ext.Extension extension = 5;

```

```go
message SubscribeResponse {
  oneof response {
    Notification update = 1;
    bool sync_response = 3;
    Error error = 4 [deprecated=true];
  }
  repeated gnmi_ext.Extension extension = 5;
}
```

```go
message SubscriptionList {
  Path prefix = 1;
  repeated Subscription subscription = 2;
  bool use_aliases = 3;
  QOSMarking qos = 4;
  enum Mode {
    STREAM = 0;
    ONCE = 1;
    POLL = 2;
  }
  Mode mode = 5;
  bool allow_aggregation = 6;
  repeated ModelData use_models = 7;
  Encoding encoding = 8;
  bool updates_only = 9;
}
```

```go
message Subscription {
  Path path = 1;
  SubscriptionMode mode = 2;
  uint64 sample_interval = 3;
  bool suppress_redundant = 4;
  uint64 heartbeat_interval = 5;
}
```

```go
enum SubscriptionMode {
  TARGET_DEFINED = 0;
  ON_CHANGE      = 1;
  SAMPLE         = 2;
}
```