# Linux bridge on top of default gw NIC with DHCP

### policy

```yaml
capture:
  default-gw: routes.running.destination=="0.0.0.0/0"
  base-iface: interfaces.name==capture.default-gw.routes.running.0.next-hop-interface
desired:
  interfaces:
  - name: br1
    description: DHCP aware Linux bridge to connect a nic that is referenced by a default gateway
    type: linux-bridge
    state: up
    mac-address: "{{ capture.base-iface.interfaces.0.mac-address }}"
    ipv4:
      dhcp: true
      enabled: true
    bridge:
      options:
        stp:
          enabled: false
        port:
        - name: "{{ capture.base-iface.interfaces.0.name }}"
```

### current state
```yaml
routes:
  running:
  - destination: 0.0.0.0/0
    next-hop-address: 192.168.100.1
    next-hop-interface: eth1
    table-id: 254
  - destination: 1.1.1.0/24
    next-hop-address: 192.168.100.1
    next-hop-interface: eth1
    table-id: 254
interfaces:
- name: eth1
  type: ethernet
  state: up
  mac-address: 00:00:5E:00:00:01
  ipv4:
    address:
    - ip: 10.244.0.1
      prefix-length: 24
    - ip: 169.254.1.0
      prefix-length: 16
    dhcp: true
    enabled: true
```

### generated state
```yaml
interfaces:
- name: br1
  description: DHCP aware Linux bridge to connect a nic that is referenced by a default gateway
  type: linux-bridge
  state: up
  mac-address: 00:00:5E:00:00:01
  ipv4:
    dhcp: true
    enabled: true
  bridge:
    options:
      stp:
        enabled: false
      port:
      - name: eth1
```
