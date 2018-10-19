
# Get CLI

We will use the follwoing command for all the examples, we will only modify the content of the `test.proto` file.

```bash
./gnmi_cli -get --address=mrstn-5502-2.cisco.com:57344 \
  -proto "$(cat test.proto)" \
  -with_user_pass \
  -insecure \
  -ca_crt=ca.cert \
  -client_crt=ems.pem \
  -client_key=ems.key \
  -timeout=5s
```

## Constants

Encoding

```go
JSON        = 0
BYTES       = 1
PROTO       = 2
ASCII       = 3
JSON_IETF   = 4
```

DataType

```go
ALL         = 0
CONFIG      = 1
STATE       = 2
OPERATIONAL = 3
```

## Single path

### test.proto

```json
prefix: <
    origin: "openconfig-network-instance"
    >
path: <
    elem: <
      name: "network-instances"
    >
    elem: <
      name: "network-instance"
    >
  >
encoding: 4
type: 1
```

Which is equivalent to

```bash
prefix {
    origin: "openconfig-network-instance"
  }
path {
    elem {
      name: "network-instances"
    }
    elem {
      name: "network-instance"
    }
  }
encoding: 4
type: 1
```

### Output

```bash
$ ./gnmi_cli -get --address=mrstn-5502-2.cisco.com:57344 \
→   -proto "$(cat test.proto)" \
→   -with_user_pass \
→   -insecure \
→   -ca_crt=ca.cert \
→   -client_crt=ems.pem \
→   -client_key=ems.key \
→   -timeout=5s
username: cisco
password: 
notification: <
  timestamp: 1539724569211203288
  update: <
    path: <
      origin: "openconfig-network-instance"
      elem: <
        name: "network-instances"
      >
      elem: <
        name: "network-instance"
      >
    >
    val: <
      json_ietf_val: "[{"name":"default","protocols":{"protocol":[{"identifier":"openconfig-policy-types:ISIS","name":"BB2","config":{"identifier":"openconfig-policy-types:ISIS","name":"BB2"},"isis":{"global":{"config":{"level-capability":"LEVEL_2","net":["49.0000.0162.0151.0250.0002.00"]},"nsr":{"config":{"enabled":false}},"graceful-restart":{"config":{"enabled":false}},"segment-routing":{"config":{"enabled":false}},"lsp-bit":{"attached-bit":{"config":{"suppress-bit":false}}},"timers":{"config":{"lsp-lifetime-interval":4000,"lsp-refresh-interval":3600}}},
      ...
      "config":{"name":"default"}}]"
    >
  >
>
error: <
>
```

## Two paths on a request

### test.proto

```json
prefix: <
    origin: "openconfig-interfaces"
    >
path: <
    elem: <
      name: "interfaces"
    >
    elem: <
      name: "interface"
      key: <
        key: "name"
        value: "Loopback60"
      >
    >
>
path: <
    elem: <
      name: "interfaces"
    >
    elem: <
      name: "interface"
      key: <
        key: "name"
        value: "HundredGigE0/0/0/0"
      >
    >
>
encoding: 4
type: 1
```

### Output

```bash
$ ./gnmi_cli -get --address=mrstn-5502-2.cisco.com:57344 \
...
notification: <
  timestamp: 1539725721033018890
  update: <
    path: <
      origin: "openconfig-interfaces"
      elem: <
        name: "interfaces"
      >
      elem: <
        name: "interface"
        key: <
          key: "name"
          value: "Loopback60"
        >
      >
    >
    val: <
      json_ietf_val: "[{"name":"Loopback60","config":{"name":"Loopback60","type":"iana-if-type:softwareLoopback","enabled":true},"subinterfaces":{"subinterface":[{"index":0,"openconfig-if-ip:ipv4":{"addresses":{"address":[{"ip":"203.0.113.22","config":{"ip":"203.0.113.22","prefix-length":32}}]}},"openconfig-if-ip:ipv6":{"addresses":{"address":[{"ip":"2001:558:2::2","config":{"ip":"2001:558:2::2","prefix-length":128}}]}}}]}}]"
    >
  >
>
notification: <
  timestamp: 1539725721061956402
  update: <
    path: <
      origin: "openconfig-interfaces"
      elem: <
        name: "interfaces"
      >
      elem: <
        name: "interface"
        key: <
          key: "name"
          value: "HundredGigE0/0/0/0"
        >
      >
    >
    val: <
      json_ietf_val: "[{"name":"HundredGigE0/0/0/0","config":{"name":"HundredGigE0/0/0/0","type":"iana-if-type:ethernetCsmacd","enabled":true,"description":"TRANSPORT: mrstn-5502-1.cisco.com","mtu":9192},"openconfig-if-ethernet:ethernet":{"config":{"auto-negotiate":false}},"subinterfaces":{"subinterface":[{"index":0,"openconfig-if-ip:ipv4":{"addresses":{"address":[{"ip":"192.0.0.2","config":{"ip":"192.0.0.2","prefix-length":24}}]}},"openconfig-if-ip:ipv6":{"addresses":{"address":[{"ip":"2001:f00:2122::2","config":{"ip":"2001:f00:2122::2","prefix-length":64}}]}}}]}}]"
    >
  >
>
error: <
>
```

## More specific path (only IPv6 info)

### test.proto


```json
prefix: <
    origin: "openconfig-interfaces"
    >
path: <
    elem: <
      name: "interfaces"
    >
    elem: <
      name: "interface"
      key: <
        key: "name"
        value: "Loopback60"
      >
    >
    elem: <
      name: "subinterfaces"
    >
    elem: <
      name: "subinterface"
      key: <
        key: "index"
        value: "0"
      >
    >
    elem: <
        name: "openconfig-if-ip:ipv6"
    >
>
encoding: 4
type: 1
```

### Output

```bash
$ ./gnmi_cli -get --address=mrstn-5502-2.cisco.com:57344 \
...
notification: <
  timestamp: 1539726043424541538
  update: <
    path: <
      origin: "openconfig-interfaces"
      elem: <
        name: "interfaces"
      >
      elem: <
        name: "interface"
        key: <
          key: "name"
          value: "Loopback60"
        >
      >
      elem: <
        name: "subinterfaces"
      >
      elem: <
        name: "subinterface"
        key: <
          key: "index"
          value: "0"
        >
      >
      elem: <
        name: "openconfig-if-ip:ipv6"
      >
    >
    val: <
      json_ietf_val: "{"addresses":{"address":[{"ip":"2001:558:2::2","config":{"ip":"2001:558:2::2","prefix-length":128}}]}}"
    >
  >
>
error: <
>
```