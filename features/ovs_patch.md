<!-- vim-markdown-toc GFM -->

* [Open vSwitch Patch Port](#open-vswitch-patch-port)

<!-- vim-markdown-toc -->

## Open vSwitch Patch Port

[Open vSwitch patch port][1] support is introduced by nmstate-0.3.2.

The OVS patch port is used for connecting two bridges.
This is the example of patching ovs bridge `br0` with `br1` via `patch0-patch1`
pair.

```yml
interfaces:
- name: ovs-br0
  type: ovs-bridge
  state: up
    port:
    - name: patch0
- name: ovs-br1
  type: ovs-bridge
  state: up
  bridge:
    port:
    - name: patch1
- name: patch0
  type: ovs-interface
  state: up
  patch:
    peer: patch1
- name: patch1
  type: ovs-interface
  state: up
  patch:
    peer: patch0
```

[1]: http://docs.openvswitch.org/en/latest/faq/configuration/
