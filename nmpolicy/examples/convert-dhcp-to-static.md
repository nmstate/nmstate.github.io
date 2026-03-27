# Convert DHCP aware interface to static addressing

### policy
```yaml
capture:
  eth1-iface: interfaces.name == "eth1"
  eth1-routes: routes.running.next-hop-interface == "eth1"
  dns: dns-resolver.running

desired:
  interfaces:
  - name: eth1
    type: ethernet
    state: up
    ipv4:
      # When converting from DHCP to static without address mentioned,
      # nmstate will convert existing dynamic IP addresses to static
      dhcp: false
      enabled: true
  routes:
    config: "{{ capture.eth1-routes.routes.running }}"
  dns-resolver:
    config: "{{ capture.dns.dns-resolver.running }}"
```

### current state
```yaml
dns-resolver:
  running:
    search:
    - example.com
    - example.org
    server:
    - 8.8.8.8
    - 2001:4860:4860::8888
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
  ipv4:
    address:
    - ip: 10.244.0.1
      prefix-length: 24
      preferred-life-time: 60sec
      valid-life-time: 60sec
    - ip: 169.254.1.0
      prefix-length: 16
      preferred-life-time: 60sec
      valid-life-time: 60sec
    dhcp: true
    enabled: true
```

### generated state

```yaml
dns-resolver:
  running:
    search:
    - example.com
    - example.org
    server:
    - 8.8.8.8
    - 2001:4860:4860::8888
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
  ipv4:
    dhcp: true
    enabled: true
```
