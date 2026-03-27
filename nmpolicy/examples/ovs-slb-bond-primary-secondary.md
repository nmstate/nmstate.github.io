# OVS SLB bond between primary and secondary nics

### policy
```yaml
capture:
  primary-nic: interfaces.description == "primary"
  secondary-nic: interfaces.description == "secondary"
desired:
  interfaces:
  - name: br1-iface
    type: ovs-interface
    state: up
    mac-address: "{{ capture.primary-nic.interfaces.0.mac-address }}"
    ipv4: "{{ capture.primary-nic.interfaces.0.ipv4 }}"
  - name: br1
    type: ovs-bridge
    state: up
    bridge:
      options:
        stp: false
        mcast-snooping-enable: false
        rstp: false
      port:
      - name: bond0
        link-aggregation:
          mode: balance-slb
          port:
          - name: "{{ capture.primary-nic.interfaces.0.name }}"
          - name: "{{ capture.secondary-nic.interfaces.0.name }}"
      - name: br1-iface
```

### current state
```yaml
interfaces:
- name: eth1
  description: primary
  type: ethernet
  state: up
  mac-address: 00:00:5E:00:00:01
  ipv4:
    dhcp: true
    enabled: true
- name: eth2
  description: secondary
  type: ethernet
  state: up
  mac-address: 00:00:5E:00:00:02
  ipv4:
    dhcp: true
    enabled: true
```

### generated state
```yaml
interfaces:
- name: br1-iface
  type: ovs-interface
  state: up
  mac-address: 00:00:5E:00:00:01
  ipv4:
    dhcp: true
    enabled: true
- name: br1
  type: ovs-bridge
  state: up
  bridge:
    options:
      stp: false
      mcast-snooping-enable: false
      rstp: false
    port:
    - name: bond0
      link-aggregation:
        mode: balance-slb
        port:
        - name: eth1
        - name: eth2
    - name: br1-iface
```
