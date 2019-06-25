# Capabilities CLI

We will use the following command.

```bash
$ ./gnmi_cli --capabilities --address=mrstn-5502-2.cisco.com:57344 \
  -with_user_pass \
  -insecure \
  -ca_crt=ca.cert \
  -client_crt=ems.pem \
  -client_key=ems.key \
  -timeout=5s
```

If you don't want to validate the certificate, you probably don't need `ca_crt`. Similarly, if you are not using mutual authentication, `client_crt` and `client_key` might not be requiered so the command below should also work.

```bash
./gnmi_cli --capabilities --address=mrstn-5502-2.cisco.com:57344 \
  -with_user_pass \
  -insecure \
  -timeout=5s
```

## Output

```bash
$ ./gnmi_cli --capabilities--address=mrstn-5502-2.cisco.com:57344 \
...
response: <
supported_models: <
  name: "Cisco-IOS-XR-ifmgr-oper"
  organization: "Cisco Systems, Inc."
  version: "2015-07-30"
>
supported_models: <
  name: "Cisco-IOS-XR-ifmgr-oper-sub2"
  organization: "Cisco Systems, Inc."
  version: "2015-07-30"
>
supported_models: <
  name: "Cisco-IOS-XR-ifmgr-oper-sub1"
  organization: "Cisco Systems, Inc."
  version: "2015-07-30"
>
supported_models: <
  name: "openconfig-packet-match-types"
  organization: "OpenConfig working group"
  version: "1.0.0"
>
...
supported_models: <
  name: "cisco-xr-openconfig-if-ip-deviations"
  organization: "Cisco Systems, Inc."
  version: "2017-02-07"
>
supported_models: <
  name: "Cisco-IOS-XR-sysadmin-clear-ncs5502"
  organization: "Cisco Systems, Inc."
  version: "2017-10-11"
>
supported_encodings: JSON_IETF
supported_encodings: ASCII
gNMI_version: "0.4.0"
```
