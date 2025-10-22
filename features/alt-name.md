## Interface Alternative Names

New feature in nmstate 2.2.51.

Interface alternative name is a kernel property of network interface which is
treat equally as interface kernel name in netlink protocol.

Unlike kernel interface name limitation on 15 characters, alternative name
length limitation is 255(might change in the future by kernel team).

Like kernel interface name, alternative name should unique among all interface
names and all alternative names. Nmstate will raise error otherwise.

Nmstate does not override existing alternative names, you can only append
and remove explicitly.

Example YAML for adding alternative names:

```rust
---
interfaces:
  - name: enp7s0
    alt-names:
      - name: port1
      - name: veryveryveryverylonglonglongname
```

Example YAML for removing alternative names:

```
---
interfaces:
  - name: enp7s0
    alt-names:
      - name: port1
        state: absent
      - name: veryveryveryverylonglonglongname
        state: absent
```
