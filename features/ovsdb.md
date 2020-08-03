## Open vSwitch Database Plugin

Nmstate introduced `nmstate-plugin-ovsdb` subpackage in nmstate-0.3.2 release.

Please install `python3-openvswitch` and start OVS daemon before applied
changes via `ovsdb` plugin.
For RHEL8, please enable fast datapath repository and install
`python3-openvswitch2.11` or `python3-openvswitch2.13` or later version.

This plugin allows you to modify the Open vSwtich database directly.
Currently only support modifying `external_ids` of OVS interface and OVS
bridge:

```yml
---
interfaces:
- name: br0
  type: ovs-bridge
  state: up
  bridge:
    port:
    - name: ovs0
  ovs-db:
    external_ids:
      foo: 10
- name: ovs0
  type: ovs-interface
  state: up
  ovs-db:
    external_ids:
      foo: abc
```

By default, NetworkManager will save their own key `NM.connection.uuid` which
is not changable by nmstate and will be ignored from desire state passing to
nmstate.

### Limitations

This plugin does not support checkpoint actions yet. We depend on
nmstate NetworkManager plugin handling the checkpoint actions for us.
