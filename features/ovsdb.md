## Open vSwitch Database Plugin

Nmstate introduced `nmstate-plugin-ovsdb` subpackage in nmstate-0.3.2 release.

Please install `python3-openvswitch` and start OVS daemon before applied
changes via `ovsdb` plugin.
For RHEL8, please enable fast datapath repository and install
`python3-openvswitch2.11` or `python3-openvswitch2.13` or later version.

This plugin allows you to modify the Open vSwtich database directly.
Currently only support modifying `external_ids` of OVS interface and OVS
bridge.

### Setting OVSDB `external_ids`

Currently, ovsdb plugin will not merge external_ids from current state,
When you defined "ovsdb" section, it will override the current ovsdb settings.
If "ovs-db" or "external_ids" is not defined in desire state, current state
will be preserved.

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
is not changeable by nmstate and will be ignored from desire state passing to
nmstate.

### Remove OVSDB `external_ids`

This yaml will remove existing ovsdb `external_ids`:

```yaml
- name: ovs0
  type: ovs-interface
  state: up
  ovs-db:
    external_ids: {}
```

### Limitations

#### Checkpoint
This plugin does not support checkpoint actions yet. We depend on
nmstate NetworkManager plugin handling the checkpoint actions for us.

#### Memory only profile
Currently, you cannot define `save_to_disk=False` in `libnmstate.apply()` or
`nmstatectl --memory-only` for OVSDB changes.

#### Lose OVSDB customization after reboot or nmcli command

Due to [bug of NetworkManager][1], NetworkManager will remove the OVSDB entry
when connection/profile deactivated or reactivated(including `nmcli c up`).

You have to run the nmstate again with desired OVSDB customization after
reboot or any nmcli command.


[1]: https://bugzilla.redhat.com/show_bug.cgi?id=1866227
