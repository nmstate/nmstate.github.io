## Interface Alternative Names

New feature in nmstate 2.2.51.

Interface alternative name is a kernel property of network interface which is
treated equally to the interface kernel name by the netlink protocol.

Unlike the kernel interface name limitation of 15 characters, interface alternative names
are limited to 127 characters (although this could be changed in the future by the kernel team).

Like kernel interface name, alternative name should unique among all interface
names and all alternative names. Nmstate will raise an error otherwise.

Nmstate does not override existing alternative names, you can only append
and remove explicitly.

Example YAML for adding alternative names:

```yml
---
interfaces:
  - name: enp7s0
    alt-names:
      - name: port1
      - name: veryveryveryverylonglonglongname
```

Example YAML for removing alternative names:

```yml
---
interfaces:
  - name: enp7s0
    alt-names:
      - name: port1
        state: absent
      - name: veryveryveryverylonglonglongname
        state: absent
```


Since nmstate 2.2.58, you may refer alt-name as interface name:

```yml
---
interfaces:
  - name: veryveryveryverylonglonglongname
    tyep: ethernet
    state: up
    ipv4:
      enabled: false
```

Since nmstate 2.2.58, you may refer alt-name as route next-hop-interface:

```yml
---
routes:
  config:
  - destination: 198.51.100.0/24
    metric: 150
    next-hop-address: 192.0.2.1
    next-hop-interface: veryveryveryverylonglonglongname
    table-id: 254
```

Since nmstate 2.2.58, you may refer alt-name as
bond/linux-bridge/ovs-bridge/vrf port. Example on linux bridge:

```yml
---
interfaces:
  - name: br0
    type: linux-bridge
    state: up
    bridge:
      port:
        - name: veryveryveryverylonglonglongname
      options:
        stp:
          enabled: false
```

Since nmstate 2.2.58, you may refer alt-name as base-iface of
VLAN/VxLAN/MacSec/MacVlan:

```yml
---
interfaces:
  - name: vlan101
    type: vlan
    state: up
    vlan:
      base-iface: veryveryveryverylonglonglongname
      id: 101
```
