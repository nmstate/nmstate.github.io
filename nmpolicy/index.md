# Introduction

Nmpolicy is an expressions driven declarative API for dynamic network
configuration.

Nmpolicy contains two sections:
 * `capture`: Rule to filter desire state
 * `desired` or `desiredState`: Use capture data to generate new state.

Using nmpolicy allows user to generate desired state base on current network
state.

Example YAML for changing gateway interface MTU to 1280.

```yaml
capture:
  gw: routes.running.destination=="0.0.0.0/0"
  gw-iface: interfaces.name==capture.gw.routes.running.0.next-hop-interface
desired:
  interfaces:
  - name: "{{ capture.gw-iface.interfaces.0.name}}"
    type: ethernet
    state: up
    mtu: 1280
```

You may applying this YAML directly through `nmstatectl apply` or
use `nmstatectl policy` to generate desired state without apply.

## Documentation

 * [Syntax](./syntax.md)
 * [Example: Linux bridge on top of default gw NIC with DHCP][1]
 * [Example: Linux bridge on top of default gw NIC without DHCP][2]
 * [Example: OVS SLB bond between primary and secondary nics][3]
 * [Example: Set all linux bridges down][4]
 * [Example: Convert DHCP aware interface to static addressing][5]
 * [Example: Enable LLDP on all active interfaces][6]
 * [Example: Linux Bridge using ports referred by description][7]

[1]: ./examples/bridge-on-default-gw-dhcp.md
[2]: ./examples/bridge-on-default-gw-no-dhcp.md
[3]: ./examples/ovs-slb-bond-primary-secondary.md
[4]: ./examples/all-linux-bridges-down.md
[5]: ./examples/convert-dhcp-to-static.md
[6]: ./examples/lldp-on-up-eth.md
[7]: ./examples/bridge-interfaces-by-description.md
