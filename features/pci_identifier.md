## Matching Interface using PCI address

New feature in nmstate 2.2.47.

When identifier been set to `pci-address`, nmstate will not use
`interface.name` for NIC matching but use the `interface.pci-address`,
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
    identifier: pci-address
    pci-address: 0000:07:00.0
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

You can also refer port by profile name in OVS bridge, linux bridge, VRF and
bond.

Example YAML for referring bond port by PCI address:

```yml
---
interfaces:
- name: port1
  type: ethernet
  pci-address: 0000:07:00.0
  identifier: pci-address
- name: port2
  type: ethernet
  pci-address: 0000:07:00.1
  identifier: pci-address
- name: bond0
  type: bond
  state: up
  link-aggregation:
    mode: balance-rr
    ports:
      - port1
      - port2
```


You can also refer to base-interface by profile name in VLAN, VxLAN, MacSec,
MacVtap, MacVlan. Example on using PCI address to identify VLAN base interface:

```yml
---
interfaces:
  - name: port1
    type: ethernet
    identifier: pci-address
    pci-address: 0000:07:00.0
  - name: vlan101
    type: vlan
    state: up
    vlan:
      base-iface: port1
      id: 101
```

You can also refer to route next-hop-interface by profile name. For example:

```yml
---
interfaces:
  - name: port1
    type: ethernet
    state: up
    pci-address: 0000:07:00.0
    identifier: pci-address
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
