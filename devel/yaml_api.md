<!-- vim-markdown-toc GFM -->

* [Introduction](#introduction)
    * [Interfaces](#interfaces)
        * [Base Interface](#base-interface)
            * [Name](#name)
            * [Type](#type)
            * [State](#state)
            * [MAC Address](#mac-address)
            * [Permanent MAC Address](#permanent-mac-address)
            * [MTU](#mtu)
            * [Minimum MTU](#minimum-mtu)
            * [Maximum MTU](#maximum-mtu)
            * [Wait IP](#wait-ip)
            * [Interface Controller](#interface-controller)
            * [Accept All MAC addresses](#accept-all-mac-addresses)
            * [Copy MAC From](#copy-mac-from)
        * [IP](#ip)
            * [IP Enable](#ip-enable)
            * [DHCP](#dhcp)
            * [IPv6 Autoconf](#ipv6-autoconf)
            * [IP Address](#ip-address)
            * [Auto DNS](#auto-dns)
            * [Auto Routes](#auto-routes)
            * [Auto Gateway](#auto-gateway)
            * [Auto Route Table ID](#auto-route-table-id)
            * [Auto Route Metric](#auto-route-metric)
        * [Multipath TCP](#multipath-tcp)
        * [Ethtool](#ethtool)
            * [Ethtool feature](#ethtool-feature)
        * [Ethtool coalesce](#ethtool-coalesce)
        * [Ethtool ring](#ethtool-ring)
        * [LLDP](#lldp)
        * [Ethernet Interface](#ethernet-interface)
            * [Ethernet speed](#ethernet-speed)
            * [Ethernet duplex](#ethernet-duplex)
            * [Ethernet Auto Negotiation](#ethernet-auto-negotiation)
        * [SR-IOV](#sr-iov)
            * [SR-IOV Total VFS Count](#sr-iov-total-vfs-count)
            * [SR-IOV VF Specific Settings](#sr-iov-vf-specific-settings)
        * [Bond Interface](#bond-interface)
            * [Bond Mode](#bond-mode)
            * [Bond Options](#bond-options)
            * [Bond Ports](#bond-ports)
        * [VLAN Interface](#vlan-interface)
        * [VxLAN Interface](#vxlan-interface)
        * [Linux Bridge Interface](#linux-bridge-interface)
            * [Linux Bridge Options](#linux-bridge-options)
        * [Linux Bridge Ports](#linux-bridge-ports)
        * [Linux Bridge Port VLAN](#linux-bridge-port-vlan)
        * [OpenvSwitch Bridge Interface](#openvswitch-bridge-interface)
            * [OpenvSwitch Bridge Options](#openvswitch-bridge-options)
        * [OpenvSwitch Bridge Ports](#openvswitch-bridge-ports)
            * [OpenvSwitch DPDK](#openvswitch-dpdk)
        * [OpenvSwitch Internal Interface](#openvswitch-internal-interface)
        * [OpenvSwitch Bridge Patch Interface](#openvswitch-bridge-patch-interface)
        * [Mac VTAP Interface](#mac-vtap-interface)
        * [Mac VLAN Interface](#mac-vlan-interface)
        * [IP over InfiniBand Interface](#ip-over-infiniband-interface)
        * [Virtual Routing and Forwarding (VRF) Interface](#virtual-routing-and-forwarding-vrf-interface)
        * [Linux Virtual Ethernet(veth) Interface](#linux-virtual-ethernetveth-interface)
    * [Routes](#routes)
    * [Route Rules](#route-rules)
    * [DNS Resolver](#dns-resolver)
    * [Hostname](#hostname)
    * [OpenvSwitch Database](#openvswitch-database)

<!-- vim-markdown-toc -->

# Introduction

Nmstate provides a set of bindings for querying and applying the network state
in the format of YAML.

For properties not mentioned desired state, nmstate will try to preserve
current configuration unless changes required by other desired property.

For example, to create an OpenvSwitch bridge:

```python
#!/usr/bin/python3

import yaml
import libnmstate

desire_state = """
interfaces:
- name: eth1
  type: ethernet
  state: up
- name: br0
  type: ovs-bridge
  state: up
  bridge:
    port:
    - name: eth1
    - name: br0
- name: br0
  type: ovs-interface
  state: up
"""

libnmstate.apply(yaml.load(desire_state, Loader=yaml.SafeLoader))
```

```rust
// SPDX-License-Identifier: Apache-2.0

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let desire_state: nmstate::NetworkState = serde_yaml::from_str(
        r#"---
interfaces:
- name: eth1
  type: ethernet
  state: up
- name: br0
  type: ovs-bridge
  state: up
  bridge:
    port:
    - name: eth1
    - name: br0
- name: br0
  type: ovs-interface
  state: up
"#,
    )?;
    desire_state.apply()?;
    Ok(())
}

```

TODO: provide example code for using C binding.

## Interfaces

The `interfaces` section is a list, for those interface not mentioned in desire
state, nmstate will not modify it unless required(for example, bridge port list
changed).

### Base Interface

There are many properties that are available for all kind of network
interfaces. The example this is a desire state of base interface properties
only:

```yml
interfaces:
- name: eth1
  type: ethernet
  state: up
  mac-address: 1C:FF:EE:DD:3B:D3
  permanent-mac-address: 1C:FF:EE:DD:BB:D3
  mtu: 1500
  min-mtu: 256
  max-mtu: 2304
  wait-ip: any
  accept-all-mac-addresses: false
```

Besides above example, we also have these properties:
 * `controller`
 * `copy-mac-from`

#### Name

The `name` property is the name string used in kernel or user space tool(for
example OpenvSwitch bridge interface name only exist in its user space
database).

You may also use this format to refer a SR-IOV VF interface:

```
sriov:<pf_name>:<vf_id>
```

Please check [this document][sriov_vf_name] for detail.

#### Type

The `type` property is the type name of interface. Please read on to get all
supported interface types.

#### State

All possible states of interface are:

 * `up`: Indicate network backend is managing this interface and activated
   configuration of specified interface.

 * `down`: Indicate network backend deactivate configuration of specified
   interface.

 * `ignore`: Indicate network backend is ignoring this interface. When
   applying state with interface marked as `ignore`, the verification will
   ignore this interface also.

 * `absent`: Only used when applying to instruct network backend to delete
    configuration of specified interface, this might or might not lead to
    the deletion in kernel.

#### MAC Address

The `mac-address` property holds the MAC address of this interface. Please be
aware the IP over InfiniBand interface have 20 bytes which result in a string
like:

```
20:00:55:04:01:FE:80:00:00:00:00:00:00:00:02:C9:02:00:23:13:92
```

The loopback interface will hold MAC address as `00:00:00:00:00:00`.

When showing, nmstate use upper case string. When applying, it is
case-insensitive.

#### Permanent MAC Address

The `permanent-mac-address` property holds the MAC addresses stored in
firmware of network card which will be persistent after system rebooted.

This is query only property and will be ignored when applying.

#### MTU

The `mtu` value is the Maximum Transmission Unit of specified interface.

It should be in the range between `min_mtu` and `max_mtu`.

#### Minimum MTU

Available since 2.2.0.

The minimum MTU supported by specified interface.

This is query only property and will be ignored when applying.

#### Maximum MTU

Available since 2.2.0.

The minimum MTU supported by specified interface.

This is query only property and will be ignored when applying.

#### Wait IP

Available since 2.1.3.

The `wait-ip` property defines which IP stack should network backend wait
before considering the interface activation finished.

It supports these values:

 * `any` -- (Default value) The activation is considered done once IPv4 stack
   or IPv6 stack is configure.
 * `ipv4` -- The activation is considered done once IPv4 stack is configured.
 * `ipv6` -- The activation is considered done once IPv6 stack is configured.
 * `ipv4+ipv6` -- The activation is considered done once both IPv4 and IPv6
   stack are configured.

#### Interface Controller

Available since 2.2.2.

The `controller` properties defined the controller of specified interface.
This property is hidden when querying and only used for controller attaching
and detaching. Setting as `controller: ''` means detaching from current
controller.

This property might conflict with port list of controller, an error will be
trigger when it happens.

Example yaml for attaching `eth1` to `br0` interface:

```yml
---
interfaces:
- name: eth1
  state: up
  controller: br0
```

#### Accept All MAC addresses

The `accept-all-mac-addresses` properties is the boolean switch for enabling
the `promiscuous` mode of specified interface which causing the kernel
to accept ethernet packages even its destination MAC address is not for
current interface.

#### Copy MAC From

The apply only property `copy-mac-from` is for clone MAC address of other
interface.

For example, to create bridge using the MAC address of eth1:

```yml
---
interfaces:
  - name: br0
    type: linux-bridge
    state: up
    copy-mac-from: eth2
    bridge:
      port:
      - name: eth2
      - name: eth1
```

### IP

This is the example of interface with static IP addresses:

```yml
---
interfaces:
- name: eth1
  state: up
  ipv4:
    enabled: true
    dhcp: false
    address:
    - ip: 192.0.2.252
      prefix-length: 24
      mptcp-flags:
      - signal
      - subflow
    - ip: 192.0.2.251
      prefix-length: 24
      mptcp-flags:
      - signal
      - subflow
  ipv6:
    enabled: true
    autoconf: false
    dhcp: false
    address:
    - ip: 2001:db8:2::1
      prefix-length: 64
      mptcp-flags:
      - signal
      - subflow
    - ip: 2001:db8:1::1
      prefix-length: 64
      mptcp-flags:
      - signal
      - subflow
```

This is the example of interface with dynamic IP addresses:

```
---
interfaces:
  - name: eth1
    type: ethernet
    state: up
    ipv4:
      enabled: true
      dhcp: true
      auto-dns: true
      auto-gateway: true
      auto-routes: true
      auto-route-table-id: 0
      auto-route-metric: 101
    ipv6:
      enabled: true
      dhcp: true
      autoconf: true
      auto-dns: true
      auto-gateway: true
      auto-routes: true
      auto-route-table-id: 0
      auto-route-metric: 102
```

#### IP Enable

The `enabled` property is the boolean switch to enable and disable the whole IP
stack.

For IPv6 with `enabled: false`, the link local address will also be removed
from this interface.

If explicitly set to `enabled: false`, all other IP parameters will be ignored
and will not fail the verification.

#### DHCP

The `dhcp` property is the boolean switch for DHCP on specified interface.

When set to `dhcp: false` for IPv4, these properties will be ignored and will
not fail the verification:
 * `auto-dns`
 * `auto-gateway`
 * `auto-routes`
 * `auto-route-table-id`
 * `auto-route-metric`

When dynamic configuration disabled(IPv4 dhcp off, IPv6 dhcp off and autoconf
off),  these properties will be ignored and will not fail the verification:

 * `auto-dns`
 * `auto-gateway`
 * `auto-routes`
 * `auto-route-table-id`
 * `auto-route-metric`

#### IPv6 Autoconf

The `autoconf` property is the boolean switch for IPv6 autoconf via Router
Advertisement (RA).

Limitation:

 * Currently nmstate does not allows `autoconf: true` and `dhcp: false`.

#### IP Address

The `address` property holds a list of object for IP address which contains
these parameters:
 * `ip`
 * `prefix-length`
 * `mptcp-flags` -- Query only, list of MPTCP flag string.

The order of IP address will be preserved when applying to kernel.

#### Auto DNS

The `auto-dns` property is the boolean switch for whether apply DNS
configurations retried from dynamic configuration method.

Set to true by default unless current state set to false explicitly.

#### Auto Routes

The `auto-routes` property is the boolean switch for whether apply routes
entries(including default gateway) retried from dynamic configuration method.

Set to true by default unless current state set to false explicitly.

#### Auto Gateway

The `auto-routes` property is the boolean switch for whether apply default
gateway retried from dynamic configuration method.

Set to true by default unless current state set to false explicitly.

#### Auto Route Table ID

The `auto-route-table-id` property is the integer for route table ID of
the routes retried from dynamic configuration method.

If not mentioned in desire state or set to 0, nmstate will use the main table
ID 254.

#### Auto Route Metric

Available since nmstate 2.2.1.

The `auto-route-metric` property is the integer for route metric of
the routes retried from dynamic configuration method.

If not mentioned in desire state or set to -1, nmstate will let network backend
to determine the metric.

### Multipath TCP

Available since nmstate 2.2.0.

The `mptcp` section of interface holds the Multipath TCP interface level
flags, it will apply flags to all valid IP addresses(both static and dynamic).
Network backend might has its own rule on excusing special scope of IP
address, e.g. IPv6 multicast address.

For example:

```
---
interfaces:
  - name: eth1
    type: ethernet
    state: up
    mptcp:
      address-flags:
      - backup
      - signal
    ipv4:
      enabled: true
      dhcp: false
      address:
        - ip: 192.0.2.2
          prefix-length: 24
```

The only supported property of `mptcp` section is `address-flags` for now.
The `address-flags` property holds the list of MPTCP flags which could be:
 * `signal`
 * `subflow`
 * `backup`
 * `fullmess`

Limitations:
 * Per IP address MPTCP flags is ignored with an warning message when
   different from interface level MPTCP flags.
 * Cannot set `signal` along with `fullmesh` due to kernel restriction.
 * If sysctl has `net.mptcp.enabled` set to 0, NetworkManager cannot
   apply MPTCP flags. User will get an failure suggest them to use
   third-party tools.
 * Cannot reapply on MPTCP changes, full activation will invoked when
   MPTCP flags changed.
 * When MPTCP not supported by NetworkManager, the query will hide
   interface level MPTCP flags even it been enabled by third party
   tool.

### Ethtool

Available since 1.1.0

The `ethtool` section of interface holds the ethtool settings of network cards,
including these subsections.

 * `feature` -- configurable offload protocol and other features
 * `ring` -- RX/TX ring parameters
 * `coalesce` -- Coalescing parameters

This is the example query output of `netdevsim` interface:

```yml
interfaces:
- name: sim0
  type: ethernet
  state: up
  ethtool:
    pause:
      rx: true
      tx: true
      autoneg: false
    feature:
      rx-gro: true
      tx-nocache-copy: false
      hw-tc-offload: false
      rx-udp_tunnel-port-offload: true
      tx-generic-segmentation: true
      rx-udp-gro-forwarding: false
      rx-gro-list: false
    coalesce:
      adaptive-rx: false
      adaptive-tx: false
      pkt-rate-high: 0
      pkt-rate-low: 0
      rx-frames: 0
      rx-frames-high: 0
      rx-frames-low: 0
      rx-usecs: 0
      rx-usecs-high: 0
      rx-usecs-irq: 0
      rx-usecs-low: 0
      sample-interval: 0
      stats-block-usecs: 0
      tx-frames: 0
      tx-frames-high: 0
      tx-frames-irq: 0
      tx-frames-low: 0
      tx-usecs: 0
      tx-usecs-high: 0
      tx-usecs-irq: 0
      tx-usecs-low: 0
    ring:
      rx: 0
      rx-max: 4096
      rx-jumbo: 0
      rx-jumbo-max: 4096
      rx-mini: 0
      rx-mini-max: 4096
      tx: 0
      tx-max: 4096
```

#### Ethtool feature

The `feature` subsection of `ethtool` holds the __changeable__ features of
specified interface using feature name as key string and boolean as value.

### Ethtool coalesce

The `coalesce` subsection supports these parameters:

 * `adaptive-rx`: boolean
 * `adaptive-tx`: boolean
 * `pkt-rate-high`: 32 bits unsigned integer
 * `pkt-rate-low`: 32 bits unsigned integer
 * `rx-frames`: 32 bits unsigned integer
 * `rx-frames-high`: 32 bits unsigned integer
 * `rx-frames-low`: 32 bits unsigned integer
 * `rx-usecs`: 32 bits unsigned integer
 * `rx-usecs-high`: 32 bits unsigned integer
 * `rx-usecs-irq`: 32 bits unsigned integer
 * `rx-usecs-low`: 32 bits unsigned integer
 * `sample-interval`: 32 bits unsigned integer
 * `stats-block-usecs`: 32 bits unsigned integer
 * `tx-frames`: 32 bits unsigned integer
 * `tx-frames-high`: 32 bits unsigned integer
 * `tx-frames-irq`: 32 bits unsigned integer
 * `tx-frames-low`: 32 bits unsigned integer
 * `tx-usecs`: 32 bits unsigned integer
 * `tx-usecs-high`: 32 bits unsigned integer
 * `tx-usecs-irq`: 32 bits unsigned integer
 * `tx-usecs-low`: 32 bits unsigned integer

### Ethtool ring

The `ring` subsection supports these parameters:
 * `rx`
 * `rx-max`
 * `rx-jumbo`
 * `rx-jumbo-max`
 * `rx-mini`
 * `rx-mini-max`
 * `tx`
 * `tx-max`

All of them are 32 bits unsigned integer.

### LLDP

Query only. TODO

### Ethernet Interface

The `ethernet` section of interface holds the parameters of ethernet specific
configurations. For example:

```yml
---
interfaces:
  - name: eth1
    type: ethernet
    state: up
    ethernet:
      speed: 10000
      duplex: full
      auto-negotiation: false
```

#### Ethernet speed

The `speed` property is the integer speed in Mbps.

This property is ignored when applying if `auto-negotiation: true`.

#### Ethernet duplex

The `duplex` property supports:

 * `full`
 * `half`

This property is ignored when applying if `auto-negotiation: true`.

#### Ethernet Auto Negotiation

The `auto-negotiation` property is the boolean switch for enabling
auto negotiation on speed and duplex.

Default to `true`.


### SR-IOV

The `sr-iov` is subsection of `ethernet` interface. For example:

```yaml
---
interfaces:
  - name: eth1
    type: ethernet
    state: up
    ethernet:
      sr-iov:
        total-vfs: 2
        vfs:
        - id: 0
          mac-address: 00:11:22:33:00:FF
          spoof-check: true
          trust: false
          min-tx-rate: 0
          max-tx-rate: 0
          vlan-id: 0
          qos: 0
        - id: 1
          mac-address: 00:11:22:33:00:EF
          spoof-check: true
          trust: false
          min-tx-rate: 0
          max-tx-rate: 0
          vlan-id: 0
          qos: 0
```

#### SR-IOV Total VFS Count

The `total-vfs` property defines the SRIOV VF count of PF interface.

#### SR-IOV VF Specific Settings

The `vfs` section holds VF specific settings:

 * `id` -- VF ID
 * `mac-address` -- VF MAC address
 * `spoof-check` -- Whether spoof check is enabled
 * `trust` -- Is Privileged (trusted) VF or not
 * `min-tx-rate`
 * `max-tx-rate`
 * `vlan-id`
 * `qos`

Once the `vfs` section is defined in desire state, user is required to provide
__all__ VFS configuration, nmstate will not merge it with current status.

### Bond Interface

The `link-aggregation` section of interface holds the parameters of bond
specific configurations. For example:

```yml
---
interfaces:
- name: bond99
  type: bond
  state: up
  link-aggregation:
    mode: balance-rr
    options:
      all_slaves_active: dropped
      arp_all_targets: any
      arp_interval: 0
      arp_validate: none
      downdelay: 0
      lp_interval: 1
      miimon: 100
      min_links: 0
      packets_per_slave: 1
      primary_reselect: always
      resend_igmp: 1
      updelay: 0
      use_carrier: true
    port:
    - eth1
    - eth2
```

#### Bond Mode

The `mode` property defines the bond mode:

 * `balance-rr` or 0
 * `active-backup` or 1
 * `balance-xor` or 2
 * `broadcast` or 3
 * `802.3ad` or 4
 * `balance-tlb` or 5
 * `balance-alb` or 6

Please check kernel document for detail meaning of each mode.

#### Bond Options

The `options` property of bond supports:

 * `ad_actor_sys_prio`
 * `ad_actor_system`
 * `ad_select`
 * `ad_user_port_key`
 * `all_slaves_active`
 * `arp_all_targets`
 * `arp_interval`
 * `arp_ip_target`
 * `arp_validate`
 * `downdelay`
 * `fail_over_mac`
 * `lacp_rate`
 * `lp_interval`
 * `miimon`
 * `min_links`
 * `num_grat_arp`
 * `num_unsol_na`
 * `packets_per_slave`
 * `primary`
 * `primary_reselect`
 * `resend_igmp`
 * `tlb_dynamic_lb`
 * `updelay`
 * `use_carrier`
 * `xmit_hash_policy`
 * `balance_slb`

Please check kernel document for their allowed value and combinations.

The `balance_slb` is not kernel option of bond, but user space configuration,
only supported by nmstate 2.2.1+, NetworkManager 1.41+ on patched linux kernel.

The verification process of nmstate will ignore bond option miss-match.

#### Bond Ports

The `port` property of bond defines a list of bond ports.

When defined in desire state, user is required to provide __all__ bond ports.

You may use `ports` also for applying since 2.1.4.

### VLAN Interface

The `vlan` section of interface holds the parameters of VLAN specific
configurations. For example:

```yml
---
interfaces:
  - name: eth1.101
    type: vlan
    state: up
    vlan:
      base-iface: eth1
      id: 101
```

The `vlan` sections contains two parameters:
 * `base-iface`: The parent interface name.
 * `id`: VLAN ID integer.

Currently, we only support VLAN based on IEEE 802.1Q protocol.

### VxLAN Interface

The `vxlan` section of interface holds the parameters of VxLAN specific
configurations. For example:

```yml
interfaces:
- name: eth1.102
  type: vxlan
  state: up
  ipv6:
    enabled: true
  vxlan:
    base-iface: eth1
    id: 102
    remote: 192.0.2.251
    destination-port: 1235
```

The `vxlan` sections contains two parameters:
 * `base-iface`: The parent interface name.
 * `id`: VLAN ID integer.
 * `remote`: The destination IP address to use in outgoing packages when the
   destination link layer address is not known in the VxLAN device forwarding
   database. Could be unicast IP or multicast IP.
 * `destination-port`: The UDP destination port.

Currently, we only support VxLAN based on IEEE 802.1Q protocol.

### Linux Bridge Interface

The `bridge` section of interface could hold parameters of Linux Bridge
specific configurations. For example:

```yml
interfaces:
- name: br0
  type: linux-bridge
  state: up
  bridge:
    options:
      gc-timer: 29657
      group-addr: 01:80:C2:00:00:00
      group-forward-mask: 0
      group-fwd-mask: 0
      hash-max: 4096
      hello-timer: 0
      mac-ageing-time: 300
      multicast-last-member-count: 2
      multicast-last-member-interval: 100
      multicast-membership-interval: 26000
      multicast-querier: false
      multicast-querier-interval: 25500
      multicast-query-interval: 12500
      multicast-query-response-interval: 1000
      multicast-query-use-ifaddr: false
      multicast-router: auto
      multicast-snooping: true
      multicast-startup-query-count: 2
      multicast-startup-query-interval: 3125
      stp:
        enabled: false
        forward-delay: 15
        hello-time: 2
        max-age: 20
        priority: 32768
      vlan-protocol: 802.1q
    port:
    - name: eth1
      stp-hairpin-mode: false
      stp-path-cost: 100
      stp-priority: 32
```

#### Linux Bridge Options

The `options` subsection of Linux bridge interface `bridge` section holds these
options:

 * `gc_timer`: Integer. Ignored when applying.
 * `group-addr`: String.
 * `group-forward-mask`: Integer.
 * `group-fwd-mask`: alias of `group-forward-mask`.
 * `hash-max`: Integer.
 * `hello-timer`: Integer. Ignored when applying.
 * `mac-ageing-time`: Integer.
 * `multicast-last-member-count`: Integer.
 * `multicast-last-member-interval`: Integer.
 * `multicast-membership-interval`: Integer.
 * `multicast-querier`: Boolean.
 * `multicast-querier-interval`: Integer.
 * `multicast-query-interval`: Integer.
 * `multicast-query-response-interval`: Integer.
 * `multicast-query-use-ifaddr`: Boolean.
 * `multicast-router`: `auto`, `disabled` or `enabled`.
 * `multicast-snooping`: Boolean.
 * `multicast-startup-query-count`: Integer.
 * `multicast-startup-query-interval`: Integer.
 * `vlan-default-pvid`: Integer.
 * `stp`:
    * `enabled`: Boolean. Other STP options will be ignored if set `false`.
    * `forward-delay`: Integer
    * `hello-time`: Integer
    * `max-age`: Integer
    * `priority`: Integer
 * `vlan-protocol`: `802.1q`(default) or `802.1ad`

### Linux Bridge Ports

The `port` subsection of `bridge` section defines a list of linux bridge ports.

Each linux bridge port contains these properties:
 * `name`: Interface name.
 * `stp-hairpin-mode`: Boolean.
 * `stp-path-cost`: Integer.
 * `stp-priority`: Integer.
 * `vlan`: Please check `Linux Bridge Port VLAN` section.

### Linux Bridge Port VLAN

The `vlan` subsection of `port` section of linux bridge holds the VLAN
filtering of linux bridge. For example:

```yml
---
interfaces:
  - name: br0
    type: linux-bridge
    state: up
    bridge:
      port:
        - name: eth1
          vlan:
            mode: trunk
            trunk-tags:
            - id: 101
            - id-range:
                min: 500
                max: 599
            tag: 100
            enable-native: true
        - name: eth2
          vlan:
            mode: trunk
            trunk-tags:
            - id: 500
```

The `vlan` subsection holds these parameters:

 * `mode`: `access` or `trunk`
 * `tag`: Integer. VLAN Tag for native VLAN.
 * `enable-native`: Boolean. Enable native VLAN or not.
 * `trunk-tags`: List of VLAN IDs or ID ranges:
    * `id`: Integer.
    * `id-range`:
        * `min`: Minimum VLAN ID(inclusive).
        * `max`: Maximum VLAN ID(inclusive).

When `vlan` section not defined in desire state when applying, current VLAN
filtering settings will be preserved for specified interface, once defined,
nmstate will override all VLAN filter settings of specified interface with
desired without merging from current.

### OpenvSwitch Bridge Interface

The `bridge` section of could hold parameters of
OpenvSwitch(Short to OVS below) Bridge specific configurations. For example:

```yml
interfaces:
- name: br0
  type: ovs-bridge
  state: up
  wait-ip: any
  bridge:
    options:
      stp: false
      rstp: false
      mcast-snooping-enable: false
    port:
    - name: ovs0
    - name: eth1
```

#### OpenvSwitch Bridge Options

The `options` subsection of OVS bridge interface `bridge` section holds these
options:

 * `stp`: Boolean.
 * `rstp`: Boolean.
 * `mcast-snooping-enable`: String.
 * `fail-mode`: String.
 * `datapath`: String. Only for DPDK.

### OpenvSwitch Bridge Ports

The `port` subsection of `bridge` section defines a list of OVS bridge
interfaces.

There are three type of OVS interfaces:
 * `internal`: Represent the bridge itself in kernel.
 * `system`: Interface exists in kernel and attached to this bridge.
 * `bond`: OVS bond interface.

This is the example YAML contains these three OVS interface types.

```yml
- name: br0
  type: ovs-bridge
  state: up
  bridge:
    port:
    - name: veth1
    - name: ovs0
    - name: bond1
      link-aggregation:
        mode: balance-slb
        port:
          - name: eth2
          - name: eth1
      vlan:
        mode: access
        tag: 100
```

Each OVS interface config could have:
 * `name`: String
 * `link-aggregation`: Bond config.
 * `vlan`: Vlan config. Please check `Linux Bridge Port VLAN` section.

Limitations:
 * Only support access mode VLAN for now.

#### OpenvSwitch DPDK

Below yaml could be used to enable DPDK using PCI device `0000:af:00.1`:

```yml
---
interfaces:
- name: ovs0
  type: ovs-interface
  state: up
  dpdk:
    devargs: "0000:af:00.1"
    rx-queue: 100
- name: br0
  type: ovs-bridge
  state: up
  bridge:
    options:
      datapath: "netdev"
    port:
    - name: ovs0
ovs-db:
  other_config:
    dpdk-init: "true"
```

Please refer to OpenvSwitch document for technical details.

### OpenvSwitch Internal Interface

The OVS internal interface is the virtual interface representing the OVS bridge
in kernel. For example:

```yml
- name: ovs0
  type: ovs-interface
  state: up
  mac-address: 92:7C:8B:BF:AA:D8
  mtu: 1500
  min-mtu: 68
  max-mtu: 65535
  wait-ip: any
  ipv4:
    enabled: false
  ipv6:
    enabled: false
```

There is no special config for OVS internal interface.

### OpenvSwitch Bridge Patch Interface

The `patch` section of `ovs-interface` could be used for OVS bridge patching.

This is the example on using OVS patch interface to connect two OVS bridges:

```yml
---
interfaces:
- name: patch0
  type: ovs-interface
  state: up
  patch:
    peer: patch1
- name: ovs-br0
  type: ovs-bridge
  state: up
  bridge:
    port:
    - name: patch0
- name: patch1
  type: ovs-interface
  state: up
  patch:
    peer: patch0
- name: ovs-br1
  type: ovs-bridge
  state: up
  bridge:
    port:
    - name: patch1
```

### Mac VTAP Interface

The `mac-vtap` section of `mac-vtap` interface defines configurations of
MAC VTAP interface.

For example:

```yml
interfaces:
- name: mac0
  type: mac-vtap
  state: up
  mac-vtap:
    base-iface: eth1
    mode: passthru
    promiscuous: true
```

The `mac-vtap` section contains these parameters:
 * `base-iface`: String. Parent interface name.
 * `mode`: String. Should be `vepa`, `bridge`, `private`, `passthru` or
   `source`.
 * `promiscuous`: Boolean.

### Mac VLAN Interface

The `mac-vlan` section of `mac-vlan` interface defines configurations of
MAC VLAN interface.

For example:

```yml
interfaces:
- name: mac0
  type: mac-vlan
  state: up
  mac-vlan:
    base-iface: eth1
    mode: passthru
    promiscuous: true
```

The `mac-vlan` section contains these parameters:
 * `base-iface`: String. Parent interface name.
 * `mode`: String. Should be `vepa`, `bridge`, `private`, `passthru` or
   `source`.
 * `promiscuous`: Boolean.

### IP over InfiniBand Interface

The `infiniband` section of `infiniband` interface defines configurations of
IP over InfiniBand Interface.

For example:

```yml
---
interfaces:
  - name: ib2.8001
    type: infiniband
    state: up
    mtu: 1280
    infiniband:
      pkey: "0x8001"
      mode: "connected"
      base-iface: "ib2"
```

The `infiniband` sections contains these parameters:
 * `pkey`: String or integer. P-key of sub-interface. None and `0xffff` both
   indicate the base interface.
 * `base-iface`: Base interface name. Empty for base interface itself.
 * `mode`: `connected` or `datagram`.

### Virtual Routing and Forwarding (VRF) Interface

The `vrf` section of `vrf` interface contains configurations of Linux kernel
Virtual Routing and Forwarding(VRF) interface. For example:

```yml
- name: vrf0
  type: vrf
  state: up
  vrf:
    port:
    - eth1
    - eth2
    route-table-id: 100
```

The `vrf` section contains:
 * `port`: List of String. VRF Ports.
 * `route-table-id`: Which route table ID should this VRF attach to.

### Linux Virtual Ethernet(veth) Interface

Besides holding configurations of Ethernet interface, the `veth` interface
could also hold `veth` section. For example:

```yml
---
interfaces:
- name: veth1
  type: veth
  state: up
  veth:
    peer: veth1peer
```

## Routes

The `routes` top section of network state contains two type routes:
 * `running`: Running effected routes containing route from universe or link scope,
    and only from these protocols:
    * boot (often used by `iproute` command)
    * static
    * ra
    * dhcp
    * mrouted
    * keepalived
    * babel

 * `config`: Static routes containing route from universe or link scope,
    and only from these protocols:
     * boot (often used by `iproute` command)
     * static

For example:

```yml
routes:
  running:
  - destination: 0.0.0.0/0
    next-hop-interface: eth1
    next-hop-address: 192.0.2.1
    table-id: 254
  config:
  - destination: 0.0.0.0/0
    next-hop-interface: eth1
    next-hop-address: 192.0.2.1
    table-id: 254
```

When applying, the `running` routes are ignored, the `config` routes in desire
state is **appended** to existing routes. To change or override existing
routes, you need to delete old route entry and add new one(could in single
desire state). For example, to change default gateway of `eth1`:

```yml
routes:
  config:
  - destination: 0.0.0.0/0
    next-hop-interface: eth1
    state: absent
  - destination: 0.0.0.0/0
    next-hop-address: 192.0.2.1
    next-hop-interface: eth1
```

The `state: absent` is used to delete all matching route entry. Below route
will delete all routes with `next-hop-interface: eth1`:

```yml
routes:
  config:
  - destination: 0.0.0.0/0
    next-hop-interface: eth1
    state: absent
```

Each route entry could have these parameters:

 * `state`: Only set to `absent` for deleting matching routes when applying.
 * `destination`: String. Mandatory for every non-absent routes.
 * `next-hop-interface`: String. Mandatory for every non-absent routes
 * `next-hop-address`: String. When setting this as empty string for absent
   route, it will only delete routes __without__ `next-hop-address`.
 * `metric`: Integer. Set to -1 for let backend decide.
 * `table-id`: Integer. Set to 0 for main table id 254.

## Route Rules

This is the example of IP route rule:

```yml
route-rules:
  config:
  - ip-from: 192.168.3.2/32
    priority: 1000
    route-table: 200
  - ip-from: 2001:db8:b::/64
    priority: 1000
    route-table: 200
routes:
  config:
  - destination: 192.168.2.0/24
    next-hop-interface: eth1
    next-hop-address: 192.168.1.3
    metric: 108
    table-id: 200
  - destination: 2001:db8:a::/64
    next-hop-interface: eth1
    next-hop-address: 2001:db8:1::2
    metric: 108
    table-id: 200
```

Like `routes`, the `route-rules` is also using `state: absent` for partial
editing. For example:

```yml
---
route-rules:
  config:
    - ip-from: 192.168.3.2/32
      route-table: 200
      state: absent
```

Each route rule entry could contains these parameters:
 * `family`: `ipv4` or `ipv6`. Only used when you would like to do
   `from any to any` matching.
 * `state`: Only set to `absent` for deleting matching route rules.
 * `ip-from`: String. When setting to empty string in absent route rule, it
   will only delete route rule __without__ `ip-from`.
 * `ip-to`: String. When setting to empty string in absent route rule, it will
   only delete route rule __without__ `ip-to`.
 * `priority`: Integer. Strongly suggested user to manually set priority
   to make sure route rule acting in desired order. If not defined, nmstate
   will add desired routes __after__ existing route rules.
 * `route-table`: Integer. Redirect matching packages to route table ID. Unset
   or 0 will be changed to main table ID 254.
 * `fwmark`: Integer or hex string. Select the `fwmark` value to match.
 * `fwmask`: Integer or hex string. Select the `fwmask` value to match.
 * `iif`: String. New in version 2.2.2. Incoming interface name.
 * `action`: New in version 2.2.2. `blackhole` or `unreachable` or `prohibit`
   or undefined.

## DNS Resolver

The `dns-resolver` sections contains two sub-sections:
 * `running`: The running effective state. The DNS server might be from DHCP(IPv6
   autoconf) or manual setup.
 * `config`:  The static saved DNS resolver config.  When applying, if this not
   mentioned, current static DNS config will be preserved as it was. If
   defined, will override current static DNS config.

For example:

```yaml
dns-resolver:
  running:
    server:
    - 2001:4860:4860::8888
    - 2001:4860:4860::8844
    - 8.8.4.4
    search:
    - example.com
    - example.org
  config:
    server:
    - 2001:4860:4860::8888
    - 2001:4860:4860::8844
    - 8.8.4.4
    search:
    - example.com
    - example.org
```

Due to NetworkManager limitations, when using NetworkManager backend, nmstate
is required to find a interface to hold DNS settings in the order of:
 * First interface in desire state with static IP or auto IP with
   `auto-dns: false`.
 * First unchanged interface in current state with static IP or auto IP with
   `auto-dns: false`.

## Hostname

Only available on nmstate version 2.1.1+.
The `hostname` section contains these parameters:

 * `running`: The running active hostname.
 * `config`: The saved static hostname.


For example:

```yml
hostname:
  running: "c9sr"
  config: "c9ss"
```

To remove static hostname, please set `running` or `config` as empty string.


## OpenvSwitch Database

Nmstate supports two types of OpenvSwitch Database modification:
 * Interface level
 * Global level.

For Interface level, only `external_ids` is supported. For example:

```yml
---
interfaces:
- name: br0
  type: ovs-bridge
  state: up
  bridge:
    port:
    - name: ovs0
    - name: eth1
  ovs-db:
    external_ids:
      gris: 10
- name: ovs0
  type: ovs-interface
  state: up
  ovs-db:
    external_ids:
      gris: abc
- name: eth1
  ovs-db:
    external_ids:
      gris: xyz
```

For global config, both `external_ids` and `other_config` are supported.
For example:

```yml
ovs-db:
  other_config:
    stats-update-interval: "1000"
  external_ids:
    ovn-localnet-bridge-mappings: "ovn-external:breth0"
```

[sriov_vf_name]: ../features/iface_vf_id.md
