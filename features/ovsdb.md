## Open vSwitch Database Plugin

Nmstate introduced `nmstate-plugin-ovsdb` subpackage in nmstate-0.3.2 release.
Please install `python3-openvswitch` and start OVS daemon before applied
changes via `ovsdb` plugin.

For RHEL8, please enable fast datapath repository and install
`python3-openvswitch2.11` or `python3-openvswitch2.13` or later version.

This plugin allows you to modify the OpenvSwitch database global parameters.
For modifying OpenvSwitch interface level parameters, this plugin is not
required, NetworkManager will handle the works.

Since nmstate 2.1, ovsdb modification does not require this plugin any more.

Please check [YAML API document][yaml_api_doc] for detail.

Example YAML containing both global and interface level parameters:

```yml
---
ovs-db:
  external_ids:
      ovn-localnet-bridge-mappings: "ovn-external:breth0"
  other_config:
    stats-update-interval: "1000"
interfaces:
- name: br0
  type: ovs-bridge
  state: up
  bridge:
    port:
    - name: ovs0
    - name: eth1
  ovs-db:
    external_ids:
      gris: 10
- name: ovs0
  type: ovs-interface
  state: up
  ovs-db:
    external_ids:
      gris: abc
- name: eth1
  ovs-db:
    external_ids:
      gris: xyz
```

[yaml_api_doc]: ./yaml_api.md
