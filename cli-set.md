# Set CLI

We will use the follwoing command for all the examples, we will only modify the content of the `test.proto` file.

```bash
$ ./gnmi_cli -set --address=mrstn-5502-2.cisco.com:57344 \
  -proto "$(cat test.proto)" \
  -with_user_pass \
  -insecure \
  -ca_crt=ca.cert \
  -client_crt=ems.pem \
  -client_key=ems.key \
  -timeout=5s
```

## Replace BGP config with full JSON data

### test.proto

```json
replace: <
    path: <
        origin: "openconfig-network-instance"
        elem: <
            name: "network-instances"
        >
        elem: <
            name: "network-instance"
            key: <
                key: "name"
                value: "default"
            >
        >
        elem: <
            name: "protocols"
        >
        elem: <
            name: "protocol"
            key: <
                key: "identifier"
                value: "BGP"
            >
            key: <
                key: "name"
                value: "default"
            >
        >
        elem: <
            name: "bgp"
        >
    >
    val: <
        json_ietf_val: '{"global":{"config":{"as":7922,"router-id":"203.0.113.1"},"afi-safis":{"afi-safi":[{"afi-safi-name":"IPV4_UNICAST","config":{"afi-safi-name":"IPV4_UNICAST","enabled":true},"use-multiple-paths":{"ebgp":{"config":{"maximum-paths":16}},"ibgp":{"config":{"maximum-paths":16}}}},{"afi-safi-name":"IPV6_UNICAST","config":{"afi-safi-name":"IPV6_UNICAST","enabled":true},"use-multiple-paths":{"ebgp":{"config":{"maximum-paths":16}},"ibgp":{"config":{"maximum-paths":16}}}}]}},"peer-groups":{"peer-group":[{"peer-group-name":"iBGP-PEER","config":{"peer-group-name":"iBGP-PEER","peer-as":7922},"timers":{"config":{"keepalive-interval":"100","hold-time":"300"}},"route-reflector":{"config":{"route-reflector-cluster-id":"203.0.113.1"}},"afi-safis":{"afi-safi":[{"afi-safi-name":"IPV4_UNICAST","config":{"afi-safi-name":"IPV4_UNICAST","enabled":true},"apply-policy":{"config":{"import-policy":["PASS-ALL"]}}},{"afi-safi-name":"IPV6_UNICAST","config":{"afi-safi-name":"IPV6_UNICAST","enabled":true},"apply-policy":{"config":{"import-policy":["PASS-ALL"]}}}]}}]},"neighbors":{"neighbor":[{"neighbor-address":"192.0.2.18","config":{"neighbor-address":"192.0.2.18","peer-group":"iBGP-PEER","description":"imaginary peer"}}]}}'
    >
>
```

### Output

```bash
$ ./gnmi_cli -set --address=mrstn-5502-2.cisco.com:57344 \
...
response: <
  path: <
    origin: "openconfig-network-instance"
    elem: <
      name: "network-instances"
    >
    elem: <
      name: "network-instance"
      key: <
        key: "name"
        value: "default"
      >
    >
    elem: <
      name: "protocols"
    >
    elem: <
      name: "protocol"
      key: <
        key: "identifier"
        value: "BGP"
      >
      key: <
        key: "name"
        value: "default"
      >
    >
    elem: <
      name: "bgp"
    >
  >
  message: <
  >
  op: REPLACE
>
message: <
>
timestamp: 1539818324536749460
```

### Final router config

```c
router bgp 7922
 bgp router-id 203.0.113.1
 address-family ipv4 unicast
  maximum-paths ebgp 16
  maximum-paths ibgp 16
 !
 address-family ipv6 unicast
  maximum-paths ebgp 16
  maximum-paths ibgp 16
 !
 neighbor-group iBGP-PEER
  remote-as 7922
  timers 100 300
  cluster-id 203.0.113.1
  address-family ipv4 unicast
   route-policy PASS-ALL in
  !
  address-family ipv6 unicast
   route-policy PASS-ALL in
  !
 !
 neighbor 192.0.2.18
  use neighbor-group iBGP-PEER
  description imaginary peer
 !
!
```

## Replace BGP config with partial JSON data

Be careful.

### test.proto

```json
replace: <
    path: <
        origin: "openconfig-network-instance"
        elem: <
            name: "network-instances"
        >
        elem: <
            name: "network-instance"
            key: <
                key: "name"
                value: "default"
            >
        >
        elem: <
            name: "protocols"
        >
        elem: <
            name: "protocol"
            key: <
                key: "identifier"
                value: "BGP"
            >
            key: <
                key: "name"
                value: "default"
            >
        >
        elem: <
            name: "bgp"
        >
    >
    val: <
        json_ietf_val: '{"global":{"config":{"as":7922}}}'
    >
>
```

```json
$ ./gnmi_cli -set --address=mrstn-5502-2.cisco.com:57344 \
...
response: <
  path: <
    origin: "openconfig-network-instance"
    elem: <
      name: "network-instances"
    >
    elem: <
      name: "network-instance"
      key: <
        key: "name"
        value: "default"
      >
    >
    elem: <
      name: "protocols"
    >
    elem: <
      name: "protocol"
      key: <
        key: "identifier"
        value: "BGP"
      >
      key: <
        key: "name"
        value: "default"
      >
    >
    elem: <
      name: "bgp"
    >
  >
  message: <
  >
  op: REPLACE
>
message: <
>
timestamp: 1539878497088782629
```

### Final router config

```c
router bgp 7922
!
```

## Add a new BGP neighbor with JSON data

### test.proto

```json
update: <
    path: <
        origin: "openconfig-network-instance"
        elem: <
            name: "network-instances"
        >
        elem: <
            name: "network-instance"
            key: <
                key: "name"
                value: "default"
            >
        >
        elem: <
            name: "protocols"
        >
        elem: <
            name: "protocol"
            key: <
                key: "identifier"
                value: "BGP"
            >
            key: <
                key: "name"
                value: "default"
            >
        >
        elem: <
            name: "bgp"
        >
    >
    val: <
        json_ietf_val: '{"global":{"config":{"as":7922}},"neighbors":{"neighbor":[{"neighbor-address":"198.51.100.4","config":{"neighbor-address":"198.51.100.4","peer-as":7922,"description":"another peer"}}]}}'
    >
>
```

```bash
response: <
  path: <
    origin: "openconfig-network-instance"
    elem: <
      name: "network-instances"
    >
    elem: <
      name: "network-instance"
      key: <
        key: "name"
        value: "default"
      >
    >
    elem: <
      name: "protocols"
    >
    elem: <
      name: "protocol"
      key: <
        key: "identifier"
        value: "BGP"
      >
      key: <
        key: "name"
        value: "default"
      >
    >
    elem: <
      name: "bgp"
    >
  >
  message: <
  >
  op: UPDATE
>
message: <
>
timestamp: 1539877485292066739
```

### Final router config

```c
router bgp 7922
 neighbor 198.51.100.4
  remote-as 7922
  description another peer
 !
!
```