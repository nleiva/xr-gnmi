# Telemetry Collector

This is the minimun set of parameter to pass to the compiled binary.

```bash
$ ./gnmi_collector -config_file example.cfg \
  -cert_file=cert.pem \
  -key_file=key.pem
```

## Interfaces sample stream

You need to specify `encoding`, `mode` and `sample_interval` to work with IOS XR devices.

### config.cfg

```json
request: <
  key: "interfaces"
  value: <
    subscribe: <
      encoding: PROTO
      prefix: <
        origin: "openconfig"
      >
      subscription: <
        mode: SAMPLE
        sample_interval: 30000000000
        path: <
          elem: <
            name: "interfaces"
          >
        >
      >
    >
  >
>
target: <
  key: "target1"
  value: <
    address: "10.87.89.122:57344"
    request: "interfaces"
    credentials: <
      username: "cisco"
      password: "cisco"
    >
  >
>
```

### Output

Well, it won't print out anything to the terminal (it  caches the data), so let's hack `handleUpdate` with a `fmt.Printf("%v\n", resp.Response)` to see some action.

```go
func (s *state) handleUpdate(msg proto.Message) error {
...
	switch v := resp.Response.(type) {
	case *gnmipb.SubscribeResponse_Update:
		fmt.Printf("%v\n", resp.Response)
...
}
```

```json
$ ./gnmi_collector -config_file example.cfg \
   -cert_file=cert.pem \
   -key_file=key.pem
&{timestamp:1543357371252000000 prefix:<origin:"openconfig" elem:<name:"interfaces" > > update:<path:<elem:<name:"interface" key:<key:"name" value:"MgmtEth0/RP0/CPU0/0" > > elem:<name:"openconfig-if-ethernet:ethernet" > elem:<name:"state" > elem:<name:"counters" > elem:<name:"in-8021q-frames" > > val:<uint_val:0 > > update:<path:<elem:<name:"interface" key:<key:"name" value:"MgmtEth0/RP0/CPU0/0" > > elem:<name:"openconfig-if-ethernet:ethernet" > elem:<name:"state" > elem:<name:"counters" > elem:<name:"in-mac-pause-frames" > > val:<uint_val:0 > > update:<path:<elem:<name:"interface" key:<key:"name" value:"MgmtEth0/RP0/CPU0/0" > > elem:<name:"openconfig-if-ethernet:ethernet" > elem:<name:"state" > elem:<name:"counters" > elem:<name:"in-oversize-frames" > > val:<uint_val:0 > > update:<path:<elem:<name:"interface" key:<key:"name" value:"MgmtEth0/RP0/CPU0/0" > > elem:<name:"openconfig-if-ethernet:ethernet" > elem:<name:"state" > elem:<name:"counters" > elem:<name:"in-jabber-frames" > > val:<uint_val:0 > > update:<path:<elem:<name:"interface" key:<key:"name" value:"MgmtEth0/RP0/CPU0/0" > > elem:<name:"openconfig-if-ethernet:ethernet" > elem:<name:"state" > elem:<name:"counters" > elem:<name:"in-fragment-frames" > > val:<uint_val:0 > > update:<path:<elem:<name:"interface" key:<key:"name" value:"MgmtEth0/RP0/CPU0/0" > > elem:<name:"openconfig-if-ethernet:ethernet" > elem:<name:"state" > elem:<name:"counters" > elem:<name:"in-crc-errors" > > val:<uint_val:0 > > update:<path:<elem:<name:"interface" key:<key:"name" value:"MgmtEth0/RP0/CPU0/0" > > elem:<name:"openconfig-if-ethernet:ethernet" > elem:<name:"state" > elem:<name:"counters" > elem:<name:"out-8021q-frames" > > val:<uint_val:0 > > update:<path:<elem:<name:"interface" key:<key:"name" value:"MgmtEth0/RP0/CPU0/0" > > elem:<name:"openconfig-if-ethernet:ethernet" > elem:<name:"state" > elem:<name:"counters" > elem:<name:"out-mac-pause-frames" > > val:<uint_val:0 > > }
...
```