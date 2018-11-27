# Subscribe CLI

We will use the follwoing command for all the examples, we will only modify the content of the `test.proto` file.

```bash
$ ./gnmi_cli --address=mrstn-5502-2.cisco.com:57344 \
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

SubscriptionMode is the mode of the subscription, specifying how the
// target must return values in a subscription.

```go
TARGET_DEFINED = 0
ON_CHANGE      = 1
SAMPLE         = 2
```

Mode of the subscription

```go
STREAM  = 0
ONCE    = 1
POLL    = 2
```

## Interfaces sample stream

### test.proto

```json
 subscribe: <
    prefix: <>
    subscription: <
      path: <
            elem: <
                name: "interfaces"
            >
            elem: <
                name: "interface"
            >
        >
      mode: SAMPLE
      sample_interval: 30000000000
    >
    mode: STREAM
    encoding: PROTO
>
```

### Output

```json
$ ./gnmi_cli --address=mrstn-5502-2.cisco.com:57344 \
→   -proto "$(cat test.proto)" \
→   -with_user_pass \
→   -insecure \
→   -ca_crt=ca.cert \
→   -client_crt=ems.pem \
→   -client_key=ems.key \
→   -timeout=5s
username: cisco
password:
{
  "openconfig": {
    "interfaces": {
      "interface": {
        "HundredGigE0/0/0/0": {
          "hold-time": {
            "state": {
              "down": 0,
              "up": 10
            }
          },
          "openconfig-if-ethernet:ethernet": {
            "state": {
              "counters": {
                "in-8021q-frames": 0,
                "in-crc-errors": 0,
                "in-fragment-frames": 0,
                "in-jabber-frames": 0,
                "in-mac-pause-frames": 0,
                "in-oversize-frames": 1,
                "out-8021q-frames": 0,
                "out-mac-pause-frames": 0
              }
            }
          },
          "state": {
            "admin-status": "UP",
            "counters": {
              "in-broadcast-pkts": 2,
              "in-discards": 0,
              "in-errors": 1,
              "in-multicast-pkts": 502393,
              "in-octets": 1527040826,
              "in-unicast-pkts": 1099651,
              "in-unknown-protos": 1,
              "last-clear": "2018-09-27T21:16:11Z",
              "out-broadcast-pkts": 1,
              "out-discards": 0,
              "out-errors": 0,
              "out-multicast-pkts": 502398,
              "out-octets": 1530596333,
              "out-unicast-pkts": 1099659
            },
            "description": "TRANSPORT: mrstn-5502-1.cisco.com",
            "enabled": true,
            "ifindex": 53,
            "mtu": 9192,
            "name": "HundredGigE0/0/0/0",
            "oper-status": "UP",
            "type": "ethernetCsmacd"
          }
        },
        "HundredGigE0/0/0/1": {
          "hold-time": {
            "state": {
              "down": 0,
              "up": 10
            }
          },
          "openconfig-if-ethernet:ethernet": {
            "state": {
              "counters": {
                "in-8021q-frames": 0,
                "in-crc-errors": 0,
                "in-fragment-frames": 0,
                "in-jabber-frames": 0,
                "in-mac-pause-frames": 0,
                "in-oversize-frames": 0,
                "out-8021q-frames": 0,
                "out-mac-pause-frames": 0
              }
            }
          },
          "state": {
            "admin-status": "UP",
            "counters": {
              "in-broadcast-pkts": 0,
              "in-discards": 0,
              "in-errors": 0,
              "in-multicast-pkts": 0,
              "in-octets": 0,
              "in-unicast-pkts": 0,
              "in-unknown-protos": 0,
              "last-clear": "2018-09-27T21:16:11Z",
              "out-broadcast-pkts": 0,
              "out-discards": 0,
              "out-errors": 0,
              "out-multicast-pkts": 0,
              "out-octets": 0,
              "out-unicast-pkts": 0
            },
            "description": "DIRECT: mrstn-5501-1.cisco.com",
            "enabled": true,
            "ifindex": 99,
            "mtu": 9192,
            "name": "HundredGigE0/0/0/1",
            "oper-status": "DOWN",
            "type": "ethernetCsmacd"
          }
        },
        ...
        "Loopback60": {
          "state": {
            "admin-status": "UP",
            "enabled": true,
            "ifindex": 101,
            "mtu": 1500,
            "name": "Loopback60",
            "oper-status": "UP",
            "type": "softwareLoopback"
          }
        },
        "MgmtEth0/RP0/CPU0/0": {
          "hold-time": {
            "state": {
              "down": 0,
              "up": 10
            }
          },
          "openconfig-if-ethernet:ethernet": {
            "state": {
              "counters": {
                "in-8021q-frames": 0,
                "in-crc-errors": 0,
                "in-fragment-frames": 0,
                "in-jabber-frames": 0,
                "in-mac-pause-frames": 0,
                "in-oversize-frames": 0,
                "out-8021q-frames": 0,
                "out-mac-pause-frames": 0
              }
            }
          },
          "state": {
            "admin-status": "UP",
            "counters": {
              "in-broadcast-pkts": 4166981,
              "in-discards": 0,
              "in-errors": 0,
              "in-multicast-pkts": 506100,
              "in-octets": 417151384,
              "in-unicast-pkts": 9647345,
              "in-unknown-protos": 0,
              "last-clear": "2018-09-27T21:12:55Z",
              "out-broadcast-pkts": 0,
              "out-discards": 0,
              "out-errors": 0,
              "out-multicast-pkts": 0,
              "out-octets": 24464109,
              "out-unicast-pkts": 275014
            },
            "enabled": true,
            "ifindex": 2,
            "mtu": 1514,
            "name": "MgmtEth0/RP0/CPU0/0",
            "oper-status": "UP",
            "type": "ethernetCsmacd"
          }
        }
      }
    }
  }
}
...
```