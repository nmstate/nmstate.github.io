## Matching Interface using MAC address

New feature in nmstate 2.2.10.

Introduce two properties to base interface:
 * `identifier` -- `name` or `mac-address`, default to `name`
 * `profile-name`: string

When identifier been set to `mac-address`, nmstate will not use
`interface.name` for NIC matching but use the `interface.mac-address`,
The `interface.name` will be used for `profile-name` when storing the
network configuration in backend.

The `profile-name` will be hide if it is equal to interface name.

Example on creating MAC based profile:

```yml
---
interfaces:
  - name: wan0
    type: ethernet
    state: up
    identifier: mac-address
    mac-address: 1e:bd:23:e9:fb:94
    ipv4:
      enabled: true
      dhcp: true
    ipv6:
      enabled: true
      dhcp: true
      autoconf: true
```

Above profile could be delete by `state:absent` using `name: wan0` or its real
interface name.

Since nmstate version 2.2.41, you can also refer port by profile name in OVS
bridge, linux bridge, VRF and bond.

Example YAML for referring bond port by MAC address:

```yml
---
interfaces:
- name: port1
  type: ethernet
  mac-address: 00:23:45:67:89:1a
  identifier: mac-address
- name: port2
  type: ethernet
  mac-address: 00:23:45:67:89:1b
  identifier: mac-address
- name: bond0
  type: bond
  state: up
  link-aggregation:
    mode: balance-rr
    ports:
      - port1
      - port2
```

Since nmstate version 2.2.41, you can also refer to base-interface by profile
name in VLAN, VxLAN, MacSec, MacVtap, MacVlan. Example on using MAC address to
identify VLAN base interface:

```yml
---
interfaces:
  - name: port1
    type: ethernet
    identifier: mac-address
    mac-address: 00:23:45:67:89:1a
  - name: vlan101
    type: vlan
    state: up
    vlan:
      base-iface: port1
      id: 101
```

Since nmstate version 2.2.41, you can also refer to route next-hop-interface by
profile name. For example:

```yml
---
interfaces:
  - name: port1
    type: ethernet
    state: up
    mac-address: 00:23:45:67:89:1a
    identifier: mac-address
    ipv4:
      enabled: true
      address:
      - ip: 192.0.2.251
        prefix-length: 24
      dhcp: false
routes:
  config:
  - destination: 0.0.0.0/0
    next-hop-address: 192.0.2.1
    next-hop-interface: port1
```
