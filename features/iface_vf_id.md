## Referring interface using SR-IOV PF name and VF ID

The SR-IOV VF interface name could be unpredictable due to complex naming rule
of systemd/udev. To simplify the effort on referring the SR-IOV VF interface,
with nmstate 2.2.0, we introduce the support of this special interface name
format:

```
sriov:<pf_name>:<vf_id>
```


For example, to set MTU of VF 1 of PF eth1:


```yaml
interfaces:
  - name: sriov:eth1:1
    type: ethernet
    state: up
    mtu: 150
```

You may also use this naming format in bond, Linux bridge or OVS bridge as port
name:

```yaml
---
interfaces:
  - name: br0
    type: linux-bridge
    state: up
    bridge:
      port:
        - name: sriov:eth1:2
```

```yaml
---
interfaces:
- name: bond99
  type: bond
  state: up
  link-aggregation:
    mode: balance-rr
    port:
    - sriov:eth1:1
```

```yaml
---
interfaces:
  - name: br0
    type: ovs-bridge
    state: up
    bridge:
      port:
      - name: ovs0
      - name: sriov:eth1:1
```
