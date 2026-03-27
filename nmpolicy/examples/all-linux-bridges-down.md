# Set all linux bridges down

### policy
```yaml
capture:
  linux-bridges: interfaces.type=="linux-bridge"
  linux-bridges-down: capture.linux-bridges | interfaces.state:="down"
desired:
  interfaces: "{{ capture.linux-bridges-down.interfaces }}"
```

### current state
```yaml
interfaces:
- name: eth0
  type: ethernet
- name: br1
  type: linux-bridge
  state: up
- name: br2
  type: linux-bridge
  state: up
- name: br3
  type: linux-bridge
  state: up
```

### generated state
```yaml
interfaces:
- name: br1
  type: linux-bridge
  state: down
- name: br2
  type: linux-bridge
  state: down
- name: br3
  type: linux-bridge
  state: down
