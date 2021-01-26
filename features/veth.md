## Veth interfaces

[Veth interfaces][1] support is introduced by nmstate-1.0.1 and requires
NetworkManager 1.30 or greater.

The veth devices are virtual Ethernet devices. They can act as tunnels between
network namespaces to create a bridge to a physical network device in another
namespace or be used as standalone network devices.

Currently, Nmstate does not have namespaces support so the Nmstate could face
several issues when configuring veth interfaces from different namespaces.

Create a veth interface and a peer with link-up:

```yaml
---
interfaces:
- name: veth1
  type: veth
  state: up
  veth:
    peer: veth2
```

To create a veth with the peers being hold by an ovs bridge and a linux bridge,
the peer must be defined in the desired state too.

```yaml
---
interfaces:
- name: veth1
  type: veth
  state: up
  veth:
    peer: veth2
- name: veth2
  type: veth
  state: up
  veth:
    peer: veth1
- name: ovs-br0
  type: ovs-bridge
  state: up
  bridge:
    options:
      stp: true
    port:
      - name: veth1
- name: linux-br0
  type: linux-bridge
  state: up
  bridge:
    port:
      - name: veth2
```

[1]: https://man7.org/linux/man-pages/man4/veth.4.html
