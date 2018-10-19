## Proto

```go
message GetRequest {
  Path prefix = 1;
  repeated Path path = 2;
  enum DataType {
    ALL = 0;
    CONFIG = 1;
    STATE = 2;
    OPERATIONAL = 3;
  }
  DataType type = 3;
  Encoding encoding = 5;
  repeated ModelData use_models = 6;
  repeated gnmi_ext.Extension extension = 7;
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
message PathElem {
  string name = 1;
  map<string, string> key = 2;
}
```

## Go

```go
r := GetRequest{
	Prefix: &Path{
		Elem: []*PathElem{
			{Name: "test1"},
			{Name: "test2"},
		},
	},
	Path: []*Path{
		{
			Elem: []*PathElem{
				{Name: "test3"},
				{Name: "test4"},
			},
		},
		{
			Elem: []*PathElem{
				{Name: "test5"},
				{Name: "test6"},
			},
		},
	},
}
```

```bash
$ cat request.data | protoc --decode_raw
1 {
  3 {
    1: "test1"
  }
  3 {
    1: "test2"
  }
}
2 {
  3 {
    1: "test3"
  }
  3 {
    1: "test4"
  }
}
2 {
  3 {
    1: "test5"
  }
  3 {
    1: "test6"
  }
}
```

