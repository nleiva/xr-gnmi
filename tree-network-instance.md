## Network Instance tree

```bash
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