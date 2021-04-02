## Generate Configurations

New feature in nmstate 1.0.2.

When you have no access to the host network stack but would like nmstate
to generate the network configuration files for you save it. You may use
 * Python API: `libnmstate.generate_configurations(desired_state)`
 * CLI: `nmstatectl gc <path_to_state_json_or_yaml_file>`

This function will not access network stack, so user have to provide
full network states and no merge action will be done against current
network status. If any undefined interface mentioned as port of
bond/bridge/team/vlan and etc, nmstate is assuming undefined interface is
with type of `ethernet`.

The output would like in the format like:

```python
{
    "<plugin_name>": [
        (<configuration_file_name>, <configuration_contents>
    ]
}
```

Example against this desired state:

```yaml
---
interfaces:
- name: bond99
  type: bond
  state: up
  link-aggregation:
    mode: balance-rr
    port:
    - eth2
    - eth1
```

The STDOUT of `nmstatectl gc bond.yml` will contain below information
for you to saved the configuration into NetworkManager folder by your own
tools:


```yaml
---
NetworkManager:
- - bond99.nmconnection
  - '[connection]

    id=bond99

    uuid=2a31ed97-6794-45dc-813d-7f948ec098ae

    type=bond

    autoconnect-slaves=1

    interface-name=bond99

    permissions=


    [bond]

    mode=balance-rr


    [ipv4]

    dhcp-client-id=mac

    dns-search=

    method=disabled


    [ipv6]

    addr-gen-mode=eui64

    dhcp-duid=ll

    dhcp-iaid=mac

    dns-search=

    method=disabled


    [proxy]

    '
- - eth2.nmconnection
  - '[connection]

    id=eth2

    uuid=652f5243-c2e1-4a05-a463-d30b3381b47d

    type=ethernet

    interface-name=eth2

    master=bond99

    permissions=

    slave-type=bond


    [ethernet]

    mac-address-blacklist=

    '
- - eth1.nmconnection
  - '[connection]

    id=eth1

    uuid=33d6e6ad-8189-4088-b142-35e40a4c3142

    type=ethernet

    interface-name=eth1

    master=bond99

    permissions=

    slave-type=bond


    [ethernet]

    mac-address-blacklist=

    '
```
