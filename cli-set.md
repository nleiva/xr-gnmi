## Examples

We will use the follwoing command for all the examples, we will only modify the content of the `test.proto` file.

```bash
./gnmi_cli -set --address=mrstn-5502-2.cisco.com:57344 \
  -proto "$(cat test.proto)" \
  -with_user_pass \
  -insecure \
  -ca_crt=ca.cert \
  -client_crt=ems.pem \
  -client_key=ems.key \
  -timeout=5s
```

### Replace BGP config with full JSON data

#### test.proto

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

#### Output

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

#### Final router config

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

### Replace BGP config with partial JSON data

Be careful.

#### test.proto

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

#### Final router config

```c
router bgp 7922
!
```

### Add a new BGP neighbor with JSON data

#### test.proto

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

#### Final router config

```c
router bgp 7922
 neighbor 198.51.100.4
  remote-as 7922
  description another peer
 !
!
```

## Apendix

```python
# pwd
/usr/src/openconfig/public/release/models
# pyang -f tree network-instance/openconfig-network-instance.yang -p ../
module: openconfig-network-instance
  +--rw network-instances
     +--rw network-instance* [name]
        +--rw name                       -> ../config/name
        +--rw fdb
        |  +--rw config
        |  |  +--rw mac-learning?      boolean
        |  |  +--rw mac-aging-time?    uint16
        |  |  +--rw maximum-entries?   uint16
        |  +--ro state
        |  |  +--ro mac-learning?      boolean
        |  |  +--ro mac-aging-time?    uint16
        |  |  +--ro maximum-entries?   uint16
        |  +--rw mac-table
        |     +--rw entries
        |        +--rw entry* [mac-address vlan]
        |           +--rw mac-address    -> ../config/mac-address
        |           +--rw vlan           -> ../config/vlan
...
      +--rw config
        |  +--rw name?                       string
        |  +--rw type?                       identityref
        |  +--rw enabled?                    boolean
        |  +--rw description?                string
        |  +--rw router-id?                  yang:dotted-quad
        |  +--rw route-distinguisher?        oc-ni-types:route-distinguisher
        |  +--rw enabled-address-families*   identityref
        |  +--rw mtu?                        uint16
        +--ro state
        |  +--ro name?                       string
        |  +--ro type?                       identityref
        |  +--ro enabled?                    boolean
...
    +--rw protocols
           +--rw protocol* [identifier name]
              +--rw identifier          -> ../config/identifier
              +--rw name                -> ../config/name
              +--rw config
              |  +--rw identifier?       identityref
              |  +--rw name?             string
              |  +--rw enabled?          boolean
              |  +--rw default-metric?   uint32
              +--ro state
              |  +--ro identifier?       identityref
              |  +--ro name?             string
              |  +--ro enabled?          boolean
              |  +--ro default-metric?   uint32
              +--rw static-routes
              |  +--rw static* [prefix]
              |     +--rw prefix       -> ../config/prefix
              |     +--rw config
              |     |  +--rw prefix?    inet:ip-prefix
              |     |  +--rw set-tag?   oc-pt:tag-type
...
              +--rw bgp
              |  +--rw global
              |  |  +--rw config
              |  |  |  +--rw as           oc-inet:as-number
              |  |  |  +--rw router-id?   oc-yang:dotted-quad
              |  |  +--ro state
              |  |  |  +--ro as                oc-inet:as-number
              |  |  |  +--ro router-id?        oc-yang:dotted-quad
              |  |  |  +--ro total-paths?      uint32
              |  |  |  +--ro total-prefixes?   uint32
              |  |  +--rw default-route-distance
              |  |  |  +--rw config
              |  |  |  |  +--rw external-route-distance?   uint8
              |  |  |  |  +--rw internal-route-distance?   uint8
              |  |  |  +--ro state
              |  |  |     +--ro external-route-distance?   uint8
              |  |  |     +--ro internal-route-distance?   uint8
              |  |  +--rw confederation
              |  |  |  +--rw config
              |  |  |  |  +--rw identifier?   oc-inet:as-number
              |  |  |  |  +--rw member-as*    oc-inet:as-number
              |  |  |  +--ro state
...
              |  +--rw neighbors
              |  |  +--rw neighbor* [neighbor-address]
              |  |     +--rw neighbor-address      -> ../config/neighbor-address
              |  |     +--rw config
              |  |     |  +--rw peer-group?           -> ../../../../peer-groups/peer-group/peer-group-name
              |  |     |  +--rw neighbor-address?     oc-inet:ip-address
              |  |     |  +--rw enabled?              boolean
              |  |     |  +--rw peer-as?              oc-inet:as-number
              |  |     |  +--rw local-as?             oc-inet:as-number
              |  |     |  +--rw peer-type?            oc-bgp-types:peer-type
              |  |     |  +--rw auth-password?        oc-types:routing-password
              |  |     |  +--rw remove-private-as?    oc-bgp-types:remove-private-as-option
              |  |     |  +--rw route-flap-damping?   boolean
              |  |     |  +--rw send-community?       oc-bgp-types:community-type
              |  |     |  +--rw description?          string
              |  |     +--ro state
              |  |     |  +--ro peer-group?                -> ../../../../peer-groups/peer-group/peer-group-name
              |  |     |  +--ro neighbor-address?          oc-inet:ip-address
              |  |     |  +--ro enabled?                   boolean
              |  |     |  +--ro peer-as?                   oc-inet:as-number
...

```