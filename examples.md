# NMState state examples
This page includes various configuration state examples of various entities.
For readability, the examples are shown in yaml format.

## Interfaces: Generic state manipulation
The examples below show how to control the iface state, regardless of
its type.

### Setting the iface up:
The `up` state also covers the creation of a virtual iface.
```yaml
interfaces:
- name: eth0
  type: ethernet
  state: up
```

### Setting the iface down
For virtual ifaces, it removes the iface. Nevertheless, the correct way to
remove an interface is using the `absent` keyword.
```yaml
interfaces:
- name: foo0
  type: unknown
  state: down
```

### Removing an iface:
For a physical device (like an ethernet NIC), it just removes any
configuration from the iface but does not (or can) physically delete it.
```yaml
interfaces:
- name: dummy0
  type: dummy
  state: absent
```

## Interfaces: ethernet
The example includes 3 ethernet interfaces, two with static IPv4 and IPv6 and
the 3rd with no IP and with the state set to down.

```yaml
interfaces:
- name: eth0
  type: ethernet
  state: up
  ipv4:
    address:
    - ip: 192.168.122.250
      prefix-length: 24
    enabled: true
  ipv6:
    address:
    - ip: 2001:db8::1:1
      prefix-length: 64
    enabled: true
- name: eth1
  type: ethernet
  state: up
  ipv4:
    address:
    - ip: 192.168.100.192
      prefix-length: 24
    enabled: true
  ipv6:
    address:
    - ip: 2001:db8::2:1
      prefix-length: 64
    enabled: true
- name: eth2
  type: ethernet
  state: down
  ipv4:
    enabled: false
```

## Interfaces: bond
The example defines a bond with two slaves and an IPv4 static address.

The bond mode is specified to be `balance-rr` and the `miimon` options is
specified.

```yaml
interfaces:
- name: eth2
  type: ethernet
  state: up
  ipv4:
    enabled: false
- name: eth3
  type: ethernet
  state: up
  ipv4:
    enabled: false
- name: bond99
  type: bond
  state: up
  ipv4:
    address:
    - ip: 10.10.10.10
      prefix-length: 24
    enabled: true
  link-aggregation:
    mode: balance-rr
    options:
      miimon: '140'
    slaves:
    - eth3
    - eth2

```

## Interfaces: ovs-bridge
The example defines an openvswitch bridge and attaches to it the
eth3 interface (as a port).

The bridge has spanning tree (stp) enabled.

```yaml
interfaces:
- name: eth3
  type: ethernet
  state: up
  ipv4:
    enabled: false
- name: ovs-br0
  type: ovs-bridge
  state: up
  bridge:
    options:
      fail-mode: ''
      mcast-snooping-enable: false
      rstp: false
      stp: true
    port:
    - name: eth3
      type: system
  ipv4:
    enabled: false
```

## Interfaces: dummy

```yaml
interfaces:
- name: ;vdsmdummy;
  type: unknown
  state: down
  ipv4:
    enabled: false
```

## Interface: VLAN

```yaml
---
interfaces:
  - name: eth1.101
    type: vlan
    state: up
    vlan:
      base-iface: eth1
      id: 101
```

## Interface: Linux Bridge

```yaml
---
interfaces:
  - name: eth1
    type: ethernet
    state: up
  - name: linux-br0
    type: linux-bridge
    state: up
    bridge:
      options:
        group-forward-mask: 0
        mac-ageing-time: 300
        multicast-snooping: true
        stp:
          enabled: true
          forward-delay: 15
          hello-time: 2
          max-age: 20
          priority: 32768
      port:
        - name: eth1
          stp-hairpin-mode: false
          stp-path-cost: 100
          stp-priority: 32
```

## Route

```yaml
---
interfaces:
  - name: eth1
    type: ethernet
    state: up
    ipv4:
      address:
      - ip: 192.0.2.251
        prefix-length: 24
      dhcp: false
      enabled: true

routes:
  config:
  - destination: 198.51.100.0/24
    metric: 150
    next-hop-address: 192.0.2.1
    next-hop-interface: eth1
    table-id: 254
```

## DNS

```yaml
---
dns-resolver:
  config:
    search:
    - example.com
    - example.org
    server:
    - 2001:4860:4860::8888
    - 8.8.8.8
```
