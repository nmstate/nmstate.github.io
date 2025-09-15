<!-- vim-markdown-toc GFM -->

* [Infiniband Over IP](#infiniband-over-ip)
* [Bond Over Infiniband](#bond-over-infiniband)

<!-- vim-markdown-toc -->

# Infiniband Over IP

This is the example YAML for creating PKEY Infiniband over IP

```yml
---
interfaces:
- name: mlx5_ib0.80fe
  type: infiniband
  state: up
  infiniband:
    base-iface: mlx5_ib0
    mode: datagram
    pkey: '0x80fe'
```

Pleas refer to [YAML API document][1] for detail meaning of each property.

# Bond Over Infiniband

Only the `active-backup` mode of bond is supported when attaching infiniband
over IP to bond. Example YAML:

```yml
---
interfaces:
- name: mlx5_ib0.80fe
  type: infiniband
  state: up
  infiniband:
    base-iface: mlx5_ib0
    mode: datagram
    pkey: '0x80ff'
- name: mlx5_ib1.80fe
  type: infiniband
  state: up
  infiniband:
    base-iface: mlx5_ib1
    mode: datagram
    pkey: '0x80fe'
- name: bond0
  type: bond
  state: up
  bond:
    mode: active-backup
    port:
    - mlx5_ib0.80fe
    - mlx5_ib1.80fe
```

[1]: https://nmstate.io/devel/yaml_api.html#ip-over-infiniband-interface
