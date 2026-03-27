# Linux Bridge using ports referred by description

### policy
```yaml
capture:
  primary-nic: interfaces.description == "primary"
  secondary-nic: interfaces.description == "secondary"
desired:
  interfaces:
  - name: br1
    type: linux-bridge
    state: up
    mac-address: "{{ capture.primary-nic.interfaces.0.mac-address }}"
    ipv4:
      dhcp: true
      enabled: true
    bridge:
      options:
        stp:
          enabled: false
      port:
      - name: "{{ capture.primary-nic.interfaces.0.name }}"
      - name: "{{ capture.secondary-nic.interfaces.0.name }}"
```

### current state
```yaml
interfaces:
- name: eth0
  description: primary
  type: ethernet
  state: up
  mac-address: 00:00:5E:00:00:01
- name: eth1
  description: secondary
  type: ethernet
  state: up
  mac-address: 00:00:5E:00:00:02
```

### generated state
```yaml
interfaces:
- name: br1
  type: linux-bridge
  state: up
  mac-address: 00:00:5E:00:00:01
  ipv4:
    enabled: true
    dhcp: true
  bridge:
    options:
      stp:
        enabled: false
    port:
    - name: eth0
    - name: eth1
```
