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
