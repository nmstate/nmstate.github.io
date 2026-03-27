# Enable LLDP on all active interfaces

### policy
```yaml
capture:
  ethernets: interfaces.type=="ethernet"
  ethernets-up: capture.ethernets.interfaces.state=="up"
  ethernets-lldp: capture.ethernets-up | interfaces.lldp.enabled:=true

desired:
  interfaces: "{{ capture.ethernets-lldp.interfaces }}"
```

### current state
```yaml
interfaces:
- name: eth0
  type: ethernet
  state: up
  mac-address: 52:55:00:D1:55:01
  accept-all-mac-addresses: false
  lldp:
    enabled: false
- name: eth1
  type: ethernet
  state: down
  mac-address: 52:55:00:D1:56:02
  accept-all-mac-addresses: false
  lldp:
    enabled: false
- name: eth2
  type: ethernet
  state: down
  mac-address: 52:55:00:D1:56:04
  accept-all-mac-addresses: false
- name: eth4
  type: ethernet
  state: up
  mac-address: 52:55:00:D1:57:03
  accept-all-mac-addresses: false
```

### generated state
```yaml
interfaces:
- name: eth0
  type: ethernet
  state: up
  mac-address: 52:55:00:D1:55:01
  accept-all-mac-addresses: false
  lldp:
    enabled: true
- name: eth4
  type: ethernet
  state: up
  mac-address: 52:55:00:D1:57:03
  accept-all-mac-addresses: false
  lldp:
    enabled: true
```
