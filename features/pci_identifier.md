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
