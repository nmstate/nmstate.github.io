<!-- vim-markdown-toc GFM -->

* [Introduction](#introduction)
    * [Query network state](#query-network-state)
    * [Apply network state](#apply-network-state)
    * [Change Network State with Manual Transaction Control](#change-network-state-with-manual-transaction-control)
    * [Error handling](#error-handling)
        * [NmstateError](#nmstateerror)
        * [NmstateDependencyError](#nmstatedependencyerror)
        * [NmstateValueError](#nmstatevalueerror)
        * [NmstatePermissionError](#nmstatepermissionerror)
        * [NmstateConflictError](#nmstateconflicterror)
        * [NmstateLibnmError](#nmstatelibnmerror)
        * [NmstateVerificationError](#nmstateverificationerror)
        * [NmstateNotImplementedError](#nmstatenotimplementederror)
        * [NmstateInternalError](#nmstateinternalerror)
    * [Schema](#schema)
* [Limitations](#limitations)
* [Network State Main layout](#network-state-main-layout)
* [Basic Interface](#basic-interface)
    * [Basic Interface: Example](#basic-interface-example)
    * [`Interface.NAME`](#interfacename)
    * [`Interface.TYPE`](#interfacetype)
    * [`Interface.STATE`](#interfacestate)
    * [`Interface.MTU`](#interfacemtu)
    * [`Interface.MAC_ADDRESS`](#interfacemac_address)
    * [`InterfaceIP`](#interfaceip)
    * [`Interface.IPV4`](#interfaceipv4)
        * [`InterfaceIPv4.ENABLED`](#interfaceipv4enabled)
        * [`InterfaceIPv4.DHCP`](#interfaceipv4dhcp)
        * [`InterfaceIPv4.AUTO_ROUTES`](#interfaceipv4auto_routes)
        * [`InterfaceIPv4.AUTO_GATEWAY`](#interfaceipv4auto_gateway)
        * [`InterfaceIPv4.AUTO_DNS`](#interfaceipv4auto_dns)
        * [`InterfaceIPv4.ADDRESS`](#interfaceipv4address)
            * [`InterfaceIPv4.ADDRESS_IP`](#interfaceipv4address_ip)
            * [`InterfaceIPv4.ADDRESS_PREFIX_LENGTH`](#interfaceipv4address_prefix_length)
    * [`Interface.IPV6`](#interfaceipv6)
        * [`InterfaceIPv6.ENABLED`](#interfaceipv6enabled)
        * [`InterfaceIPv6.DHCP`](#interfaceipv6dhcp)
        * [`InterfaceIPv6.AUTOCONF`](#interfaceipv6autoconf)
        * [`InterfaceIPv6.AUTO_ROUTES`](#interfaceipv6auto_routes)
        * [`InterfaceIPv6.IGNORE_GATEWAY`](#interfaceipv6ignore_gateway)
        * [`InterfaceIPv6.IGNORE_DNS`](#interfaceipv6ignore_dns)
        * [`InterfaceIPv6.ADDRESS`](#interfaceipv6address)
* [Ethernet](#ethernet)
    * [`Ethernet.AUTO_NEGOTIATION`](#ethernetauto_negotiation)
    * [`Ethernet.DUPLEX`](#ethernetduplex)
    * [`Ethernet.SPEED`](#ethernetspeed)
    * [`Ethernet.SRIOV_SUBTREE`](#ethernetsriov)
        * [`Ethernet.SRIOV.TOTAL_VFS`](#sriovtotal_vfs)
        * [`Ethernet.SRIOV.VFS_SUBTREE`](#sriovvfs)
            * [`Ethernet.SRIOV.VFS.ID`](#sriovvfsid)
            * [`Ethernet.SRIOV.VFS.MAC_ADDRESS`](#sriovvfsmac)
            * [`Ethernet.SRIOV.VFS.SPOOF_CHECK`](#sriovvfsspoof)
            * [`Ethernet.SRIOV.VFS.TRUST`](#sriovvfstrust)
            * [`Ethernet.SRIOV.VFS.MIN_TX_RATE`](#sriovvfsmintx)
            * [`Ethernet.SRIOV.VFS.MAX_TX_RATE`](#sriovvfsmaxtx)
* [VLAN](#vlan)
    * [`VLAN.ID`](#vlanid)
    * [`VLAN.BASE_IFACE`](#vlanbase_iface)
* [VXLAN](#vxlan)
    * [`VXLAN.ID`](#vxlanid)
    * [`VXLAN.BASE_IFACE`](#vxlanbase_iface)
    * [`VXLAN.REMOTE`](#vxlanremote)
    * [`VXLAN.DESTINATION_PORT`](#vxlandestination_port)
* [Linux Bridge](#linux-bridge)
    * [`LinuxBridge.OPTIONS_SUBTREE`](#linuxbridgeoptions_subtree)
        * [`LinuxBridge.Options.GROUP_FORWARD_MASK`](#linuxbridgeoptionsgroup_forward_mask)
        * [`LinuxBridge.Options.MAC_AGEING_TIME`](#linuxbridgeoptionsmac_ageing_time)
        * [`LinuxBridge.Options.MULTICAST_SNOOPING`](#linuxbridgeoptionsmulticast_snooping)
    * [`LinuxBridge.STP_SUBTREE`](#linuxbridgestp_subtree)
        * [`LinuxBridge.STP.ENABLED`](#linuxbridgestp_enabled)
        * [`LinuxBridge.STP.FORWARD_DELAY`](#linuxbridgestp_forward_delay)
        * [`LinuxBridge.STP.HELLO_TIME`](#linuxbridgestp_hello_time)
        * [`LinuxBridge.STP.MAX_AGE`](#linuxbridgestp_max_age)
        * [`LinuxBridge.STP.PRIORITY`](#linuxbridgestp_priority)
    * [`LinuxBridge.PORT_SUBTREE`](#linuxbridgeport_subtree)
        * [`LinuxBridge.Port.NAME`](#linuxbridgeport_name)
        * [`LinuxBridge.Port.STP_PRIORITY`](#linuxbridgeport_stp_priority)
        * [`LinuxBridge.Port.STP_HAIRPIN_MODE`](#linuxbridgeport_stp_hairpin_mode)
        * [`LinuxBridge.Port.STP_PATH_COST`](#linuxbridgeport_stp_path_cost)
        * [`LinuxBridge.Port.VLAN_SUBTREE`](#linuxbridgeport_vlan_subtree)
            * [`LinuxBridge.Port.VLAN.ENABLE_NATIVE`](#linuxbridgevlan_enable_native)
            * [`LinuxBridge.Port.VLAN.MODE`](#linuxbridgevlan_mode)
            * [`LinuxBridge.Port.VLAN.TAG`](#linuxbridgevlan_tag)
            * [`LinuxBridge.Port.VLAN.TRUNK_TAGS`](#linuxbridgevlan_trunk_tags)
* [Interface -- Bond](#interface----bond)
    * [`Bond.MODE`](#bondmode)
    * [`Bond.OPTIONS`](#bondoptions)
    * [`Bond.SLAVES`](#bondslaves)
* [Interface -- Open vSwitch(OVS) Bridge](#interface----open-vswitchovs-bridge)
    * [`OVSBridge.OPTIONS_SUBTREE`](#ovsbridgeoptions_subtree)
        * [`OVSBridge.Options.FAIL_MODE`](#ovsbridgefail_mode)
        * [`OVSBridge.Options.MCAST_SNOOPING_ENABLE`](#ovsbridgemcast_snooping_enable)
        * [`OVSBridge.Options.RSTP`](#ovsbridgerstp)
        * [`OVSBridge.Options.STP`](#ovsbridgestp)
    * [`OVSBridge.PORT_SUBTREE`](#ovsbridgeport_subtree)
        * [`OVSBridge.Port.NAME`](#ovsbridgeport_name)
        * [`OVSBridge.Port.VLAN_SUBTREE`](#ovsbridgeportvlan_subtree)
            * [`OVSBridge.Port.Vlan.TRUNK_TAGS`](#ovsbridgevlan_trunk_tags)
            * [`OVSBridge.Port.Vlan.TAG`](#ovsbridgevlan_tag)
            * [`OVSBridge.Port.Vlan.ENABLE_NATIVE`](#ovsbridgevlan_enable_native)
            * [`OVSBridge.Port.Vlan.MODE`](#ovsbridgevlan_mode)
        * [`OVSBridge.Port.LINK_AGGREGATION_SUBTREE`](#ovsbridgeport_link_aggregation)
            * [`OVSBridge.Port.LinkAggregation.MODE`](#ovsbridgelink_mode)
            * [`OVSBridge.Port.LinkAggregation.SLAVES_SUBTREE`](#ovsbridgelink_slave_subtree)
	        * [`OVSBridge.Port.LinkAggregation.Slave.NAME`](#ovsbridgelink_slave_name)
* [Route](#route)
    * [`Route.TABLE_ID`](#routetable_id)
    * [`Route.DESTINATION`](#routedestination)
    * [`Route.NEXT_HOP_INTERFACE`](#routenext_hop_interface)
    * [`Route.NEXT_HOP_ADDRESS`](#routenext_hop_address)
    * [`Route.METRIC`](#routemetric)
* [DNS client configuration](#dns-client-configuration)
    * [`DNS.SEARCH`](#dnssearch)
    * [`DNS.SERVER`](#dnsserver)

<!-- vim-markdown-toc -->

# Introduction

This document holds all the API detail of the `libnmstate` Python module.
For quick start, you may refer to [Python code examples][py_example]

The `libnmstate` package exposes the following API::
 * Functions:
    * `show()`  -- [Query network state][show]
    * `apply()` -- [Apply network state][apply]
    * `commit()` -- [Commit a transaction][manual]
    * `rollback()` -- [Rollback a transaction][manual]
 * Submodules:
     * `error` -- [Exceptions][error]
     * `schema` -- Schema Constants

## Query network state

Example of querying network state:

```python
import json
import libnmstate
from libnmstate.schema import Interface

net_state = libnmstate.show()
for iface_state in net_state[Interface.KEY]:
    print(iface_state[Interface.NAME])
```

## Apply network state

Nmstate supports partial editing, only the specified data is changed, the
unspecified data is preserved/untouched.
Some properties(e.g. `InterfaceIPv4.ADDRESS` ) do not support partial editing,
please refer to their document for detail.


Example of changing eth1 MTU to 1460:

```python
import libnmstate
from libnmstate.schema import Interface

libnmstate.apply({
    Interface.KEY: [
        {
            Interface.NAME: 'eth1',
            Interface.MTU: 1460
        }
    ]
})
```

## Change Network State with Manual Transaction Control

## Error handling


### NmstateError

All the exceptions are stored in `libnmstate.error` and are all
is inherited from `NmstateError`.

### NmstateDependencyError

Nmstate requires external tools installed and/or started for desired state.

### NmstateValueError

Exception happens at pre-apply check, user should resubmit the amended
desired state. Example:
    * JSON/YAML syntax issue.
    * Nmstate schema issue.
    * Invalid value of desired property, like bond missing slave.

### NmstatePermissionError

Permission deny when applying the desired state.


### NmstateConflictError

An existing Nmstate transaction is in progress.


### NmstateLibnmError

Exception for unexpected NetworkManager libnm failure.


### NmstateVerificationError

After applied desired state, the desired state does not match the refreshed
current state.


### NmstateNotImplementedError

Desired feature is not supported by Nmstate yet.

### NmstateInternalError

Unexpected behaviour happened. It is a bug of libnmstate which should be fixed.

## Schema

The YAML schema file is placed at `libnmstate/schemas/operational-state.yaml`.

In stead of using hard coded string, please use constants defined in
`libnmstate.schmea`.

# Limitations

Please refer to [limitations in code base][limitations]


# Network State Main layout

The network state is represented in a dictionary:

```python
from libnmstate.schema import Interface
from libnmstate.schema import Route
from libnmstate.schema import DNS

network_state = {
    DNS.KEY: {},            # Check section of 'DNS'
    Route.KEY: {},          # Check section of 'Route'
    Interface.KEY: [],      # Check section of 'Interface -- Basic'
}
```

# Basic Interface

An interface represents a hardware or software network interface.
Each interface has these basic properties represented as a dictionary.

## Basic Interface: Example

```python
{
    Interface.NAME: 'eth1',
    Interface.STATE: InterfaceState.UP,
    Interface.TYPE: InterfaceType.ETHERNET,
    Interface.IPV4: {
        InterfaceIPv4.ENABLED: True
        InterfaceIPv4.DHCP: False,
        InterfaceIPv4.ADDRESS: [
            {
                InterfaceIPv4.ADDRESS_IP: '192.0.2.251',
                InterfaceIPv4.ADDRESS_PREFIX_LENGTH: 24,
            }
        ],
    },
    Interface.IPV6: {
        InterfaceIPv6.ENABLED: True
        InterfaceIPv6.DHCP: True,
        InterfaceIPv6.AUTOCONF: True,
        InterfaceIPv6.AUTO_DNS: True,
        InterfaceIPv6.AUTO_GATEWAY: True,
        InterfaceIPv6.AUTO_ROUTES: True,
        InterfaceIPv6.ADDRESS: [],
    }
}
```

## `Interface.NAME`

The kernel name of the interface.

Type: `string`

## `Interface.TYPE`

The interface type.

Type: `string`

Possible values:

 * `InterfaceType.ETHERNET`
 * `InterfaceType.BOND`
 * `InterfaceType.LINUX_BRIDGE`
 * `InterfaceType.OVS_BRIDGE`
 * `InterfaceType.OVS_PORT`
 * `InterfaceType.OVS_INTERFACE`
 * `InterfaceType.VLAN`
 * `InterfaceType.VXLAN`

## `Interface.STATE`

The interface state.

Type: `string`

Possible values:

 * `InterfaceState.UP`

   Bring interface link up.

 * `InterfaceState.DOWN`

   Remove interface. Might change to bring interface link down in future
   release.

 * `InterfaceState.ABSENT`

   Remove interface.


## `Interface.MTU`

The maximum transmission unit of interface.

Type: `interger`

## `Interface.MAC_ADDRESS`

The MAC address of interface.

Type: `String` in the format of `EA:60:E4:08:17:F1`. Case insensitive.

## `InterfaceIP`

The `InterfaceIP` class is holding the share constants between `InterfaceIPv4`
and `InterfaceIPv6`:

 * `InterfaceIP.ENABLED`
 * `InterfaceIP.DHCP`
 * `InterfaceIP.AUTO_DNS`
 * `InterfaceIP.AUTO_GATEWAY`
 * `InterfaceIP.AUTO_ROUTES`
 * `InterfaceIP.ADDRESS`
 * `InterfaceIP.ADDRESS_IP`
 * `InterfaceIP.ADDRESS_PREFIX_LENGTH`

## `Interface.IPV4`

The `Interface.IPV4` property holds the IPv4 stack information in a dictionary.

Example:
```python
{
    Interface.IPV4: {
        InterfaceIPv4.ENABLED: True
        InterfaceIPv4.DHCP: TRUE,
        InterfaceIPv4.ADDRESS: [],
        InterfaceIPv4.AUTO_DNS: True,
        InterfaceIPv4.AUTO_GATEWAY: True,
        InterfaceIPv4.AUTO_ROUTES: True,
    }
}
```

### `InterfaceIPv4.ENABLED`

Type: `bool`

Possible values:

 * `True` - IPv4 stack is enabled.

 * `False` - IPv4 stack is disabled.

When applying network state with IPv4 disabled, all the remaining IPv4
configurations are ignored.

When showing the network state, if IPv4 is disabled, all the remaining
properties are excluded in returned network state.

### `InterfaceIPv4.DHCP`

Type: `bool`

Possible values:

 * `True` - DHCPv4 is enabled.

 * `False` - DHCPv4 is disabled.

### `InterfaceIPv4.AUTO_ROUTES`

Type: `bool`

Possible values:

 * `True`

   Apply routes retrieved from DHCPv4: including gateway and
   [classless static Route][rfc-dhcpv4-classless-route]

 * `False`

   Ignore routes retrieved from DHCPv4: including gateway and
   [classless static Route][rfc-dhcpv4-classless-route]

This property only shows when `Interface.IPV4[InterfaceIPv4.DHCP]` is `True`
when querying.

This property is ignored when `Interface.IPV4[InterfaceIPv4.DHCP]` is
`False` when applying.

### `InterfaceIPv4.AUTO_GATEWAY`

Type: `bool`

Possible values:

 * `True` - Apply gateway retrieved from DHCPv4.

 * `False` - Ignore gateway retrieved from DHCPv4.

This property only shows when `Interface.IPV4[InterfaceIPv4.DHCP]` is `True`
when querying.

This property is ignored when `Interface.IPV4[InterfaceIPv4.DHCP]` is
`False` when applying.

### `InterfaceIPv4.AUTO_DNS`

Type: `bool`

Possible values:

 * `True`

   Apply gateway retrieved from DHCPv4.

 * `False`

   Ignore gateway retrieved from DHCPv4.

This property only shows when `Interface.IPV4[InterfaceIPv4.DHCP]` is `True`
when querying.

This property is ignored when `Interface.IPV4[InterfaceIPv4.DHCP]` is
`False` when applying.

### `InterfaceIPv4.ADDRESS`
The static IPv4 addresses.
This property does not support partial editing, the full list of IP addresses
is required in desired state.

Type: `array of dictionary`.

When `Interface.IPV4[InterfaceIPv4.DHCP]` is set to `True`, the address defined
in this property is also ignored.

Example:
```python
InterfaceIPv4.ADDRESS: [
    {
        InterfaceIPv4.ADDRESS_IP: '192.0.2.251',
        InterfaceIPv4.ADDRESS_PREFIX_LENGTH: 24,
    }
]
```

#### `InterfaceIPv4.ADDRESS_IP`

The static IPv4 address.

Type: `string`

Format: `192.0.2.251`


#### `InterfaceIPv4.ADDRESS_PREFIX_LENGTH`

The prefix length of static IPv4 address.

Type: `interger`.

## `Interface.IPV6`
The `Interface.IPV6` property holds the IPv6 stack information in a dictionary.

Example:
```python
{
    Interface.IPV6: {
        InterfaceIPv6.ENABLED: True
        InterfaceIPv6.DHCP: TRUE,
        InterfaceIPv6.ADDRESS: [],
        InterfaceIPv6.AUTO_DNS: True,
        InterfaceIPv6.AUTO_GATEWAY: True,
        InterfaceIPv6.AUTO_ROUTES: True,
    }
}
```

### `InterfaceIPv6.ENABLED`
Type: `bool`

Possible values:

 * `True` - IPv6 stack is enabled.

 * `False` - IPv6 stack is disabled.

When applying network state with IPv6 disabled, all the remaining IPv6
configurations are ignored.

When showing network state, if IPv6 is disabled, all the remaining properties
are excluded in the returned network state.

### `InterfaceIPv6.DHCP`

Type: `bool`

Possible values:

 * `True` - DHCPv6 is enabled.
 * `False` - DHCPv6 is disabled.

### `InterfaceIPv6.AUTOCONF`
Type: `bool`

Possible values:

 * `True` - Auto configuration(IPv6 router advertisement) is enabled.
 * `False` - Auto configuration(IPv6 router advertisement) is disabled.

### `InterfaceIPv6.AUTO_ROUTES`
Type: `bool`

Possible values:

 * `True`

   Apply routes(including gateway) retrieved from IPv6 router advertisement.

 * `False`

   Ignore routes(including gateway) retrieved from IPv6 router advertisement.

### `InterfaceIPv6.IGNORE_GATEWAY`

Type: `bool`

Possible values:

 * `True` - Apply gateway retrieved from IPv6 router advertisement.

 * `False` - Ignore gateway retrieved from IPv6 router advertisement.

### `InterfaceIPv6.IGNORE_DNS`

Type: `bool`

Possible values:

 * `True`

   Apply DNS client configuration retrieved from IPv6 router advertisement and
   DHCPv6.

 * `False`

   Ignore DNS client configuration retrieved from IPv6 router advertisement and
   DHCPv6.

### `InterfaceIPv6.ADDRESS`
The static IPv6 addresses.
This property does not support partial editing, the full list of IP addresses
is required in desired state.


Type: `array of dictionary`.

When `Interface.IPV6[InterfaceIPv6.DHCP]` or
`Interface.IPV6[InterfaceIPv6.AUTOCONF]` is set to `True`, the address defined
in this property is also ignored.

Example:
```python
InterfaceIPv6.ADDRESS: [
    {
        InterfaceIPv6.ADDRESS_IP: '2001:db8:1::f',
        InterfaceIPv6.ADDRESS_PREFIX_LENGTH: 64,
    }
]
```

# Ethernet

Besides basic interface properties, each Ethernet interface state also contains
a dictionary saved in key `Ethernet.CONFIG_SUBTREE`.

For virtual interface like `virtio_net` or `veth`, there is no
`Ethernet.CONFIG_SUBTREE`.

Example:

```python
{
    Interface.KEY: [
	Interface.NAME: "eth1",
        Interface.TYPE: InterfaceType.ETHERNET,
        Interface.STATE: InterfaceState.UP
        Ethernet.CONFIG_SUBTREE: {
            Ethernet.AUTO_NEGOTIATION: True,
            Ethernet.DUPLEX: Ethernet.FULL_DUPLEX,
            Ethernet.SPEED: 1000
        }
    ]
}
```
## `Ethernet.AUTO_NEGOTIATION`

Type: `bool`

When TRUE, enforce auto-negotiation of speed and duplex mode.
If `Ethernet.SPEED` and `Ethernet.DUPLEX` properties are both specified, only
that single mode is advertised and accepted during the link
auto-negotiation process: this works only for BASE-T 802.3 specifications and
is useful for enforcing gigabits modes, as in these cases link negotiation is
mandatory. When FALSE, `Ethernet.SPEED` and `Ethernet.DUPLEX` properties should
be both set or link configuration is skipped.

## `Ethernet.DUPLEX`

Type: `string`, `Ethernet.FULL_DUPLEX` or `Ethernet.HALF_DUPLEX`.

Ethernet duplex mode.

## `Ethernet.SPEED`

Type: `integer`

Ethernet speed in the unit of Mbps.

## `Ethernet.SRIOV_SUBTREE`

If the ethernet interface supports SR-IOV, the `Ethernet.SRIOV_SUBTREE`
dictionary allows to configure it.  Nmstate supports the following options:

### `Ethernet.SRIOV.TOTAL_VFS`

Type: `integer`

Total number of Virtual Functions per Physical Function. Changing the total VFs
will also change the interfaces that are visible in the Nmstate state. Reducing
the total VFs without setting `Interface.STATE` to `InterfaceState.ABSENT` for
the corresponding interfaces might lead to an undefined system state or error.

### `Ethernet.SRIOV.VFS_SUBTREE`

If `Ethernet.SRIOV.TOTAL_VFS` is 1 or greater, the `Ethernet.SRIOV.VFS_SUBTREE`
dictionary list allows to configure the VFS special options. Nmstate supports the
following VF options:

#### `Ethernet.SRIOV.VFS.ID`

Type: `integer`

ID of the VF to configure.

#### `Ethernet.SRIOV.VFS.MAC_ADDRESS`

Type: `string` in the format of `11:22:33:44:55:66`. Case insensitive.

#### `Ethernet.SRIOV.VFS.SPOOF_CHECK`

Type: `boolean`

Enable or disable spoof checking on the VF.

#### `Ethernet.SRIOV.VFS.TRUST`

Type: `boolean`

Enable or disable trust option on the VF.

#### `Ethernet.SRIOV.VFS.MIN_TX_RATE`

Type: `integer`

Set the minimum TX rate on the VF. This value must be greater or equal than 0.

#### `Ethernet.SRIOV.VFS.MAX_TX_RATE`

Type: `integer`

Set the maximum TX rate on the VF. This value must be greater or equal than 0.

Example:
```python
{
    Interface.KEY: [
	{
		Interface.NAME: "eth1",
		Interface.TYPE: InterfaceType.ETHERNET,
		Interface.STATE: InterfaceState.UP
		Ethernet.CONFIG_SUBTREE: {
			Ethernet.SRIOV_SUBTREE: {
				Ethernet.SRIOV.TOTAL_VFS: 3
				Ethernet.SRIOV.VFS_SUBTREE: [
					{
						Ethernet.SRIOV.VFS.ID: 0,
						Ethernet.SRIOV.VFS.MAC_ADDRESS: "11:22:33:44:55:66",
						Ethernet.SRIOV.VFS.TRUST: True,
					},
					{
						Ethernet.SRIOV.VFS.ID: 1,
						Ethernet.SRIOV.VFS.SPOOF_CHECK: False,
						Ethernet.SRIOV.VFS.MAX_TX_RATE: 1000,
						Ethernet.SRIOV.VFS.MIN_TX_RATE: 100,
					}
				]
			}
		}
	}
    ]
}
```

# VLAN
Besides basic interface properties, each VLAN interface state also contains
a dictionary saved in key `VLAN.CONFIG_SUBTREE`.

Example:

```python
{
    Interface.KEY: [
        Interface.NAME: "eth1.101",
        Interface.TYPE: VLAN.TYPE,
        Interface.STATE: InterfaceState.UP
        VLAN.CONFIG_SUBTREE: {
            VLAN.ID: 101,
            VLAN.BASE_IFACE: 'eth1'
        }
    ]
}
```

## `VLAN.ID`

Type: `integer`

The VLAN ID.

To change VLAN ID, please use `InterfaceState.ABSENT` for existing one and add
new one after that.


## `VLAN.BASE_IFACE`

Type: `string`

The interface name which current VLAN is based on.

# VXLAN
Besides basic interface properties, each VXLAN interface state also contains
a dictionary saved in key `VXLAN.CONFIG_SUBTREE`.

Example:

```python
{
    Interface.KEY: [{
        Interface.NAME: 'eth1.101',
        Interface.TYPE: VXLAN.TYPE,
        Interface.STATE: InterfaceState.UP,
        VXLAN.CONFIG_SUBTREE: {
            VXLAN.ID: 101,
            VXLAN.BASE_IFACE: 'eth1',
            VXLAN.REMOTE: '192.0.2.2',
            VXLAN.DESTINATION_PORT: 4790 # optional
        }
    }]
}
```

## `VXLAN.ID`

Type: `integer`

The VXLAN ID.

To change the VXLAN ID, please remove the interface first by setting the state
to `InterfaceState.ABSENT` and then add a new one.


## `VXLAN.BASE_IFACE`

Type: `string`

The name of the underlying interface for the VXLAN.

## `VXLAN.REMOTE`

Type: `string`

The unicast destination IP address to use in outgoing packets when the destination link layer address is not known in the VXLAN device forwarding database, or the multicast IP address to join.

## `VXLAN.DESTINATION_PORT`

Type: `integer`

The UDP destination port to communicate to the remote VXLAN tunnel endpoint.
Default: 4789


# Linux Bridge
Besides basic interface properties, each Linux Bridge interface state also
contains a dictionary saved in key `LinuxBridge.CONFIG_SUBTREE`.

Example:

```python
{
    Interface.KEY: [{
        Interface.NAME: "linux-br0",
        Interface.TYPE: LinuxBridge.TYPE,
        Interface.STATE: InterfaceState.UP
        LinuxBridge.CONFIG_SUBTREE: {
            LinuxBridge.OPTIONS_SUBTREE: {
                LinuxBridge.Options.GROUP_FORWARD_MASK: 0,
                LinuxBridge.Options.MAC_AGEING_TIME: 300,
                LinuxBridge.Options.MULTICAST_SNOOPING: true,
                LinuxBridge.STP_SUBTREE: {
                    LinuxBridge.STP.ENABLED: true,
                    LinuxBridge.STP.FORWARD_DELAY: 15,
                    LinuxBridge.STP.HELLO_TIME: 2,
                    LinuxBridge.STP.MAX_AGE: 20,
                    LinuxBridge.STP.PRIORITY: 32768
                }
            },
            PORT_SUBTREE: [
                {
                    LinuxBridge.Port.NAME: "eth1",
                    LinuxBridge.Port.STP_PRIORITY: 32,
                    LinuxBridge.Port.STP_HAIRPIN_MODE: false,
                    LinuxBridge.Port.STP_PATH_COST: 100
                }
            ]
        }
    }]
}
```

## `LinuxBridge.Options.GROUP_FORWARD_MASK`

Type: `integer`

The mask of group address to forward.

## `LinuxBridge.Options.MAC_AGEING_TIME`

Type: `integer`

The Spanning Tree Protocol (STP) maximum message age, in seconds.

## `LinuxBridge.Options.MULTICAST_SNOOPING`

Type: `bool`

Whether IGMP snooping is enabled for this bridge.

## `LinuxBridge.STP.ENABLED`

Type: `bool`

Whether Spanning Tree Protocol (STP) is enabled for this bridge.

## `LinuxBridge.STP.FORWARD_DELAY`

Type: `integer`

The Spanning Tree Protocol (STP) forwarding delay, in seconds.

## `LinuxBridge.STP.HELLO_TIME`

Type: `integer`

The Spanning Tree Protocol (STP) hello time, in seconds.

## `LinuxBridge.STP.MAX_AGE`

Type: `integer`

The Spanning Tree Protocol (STP) maximum message age, in seconds.

## `LinuxBridge.STP.PRIORITY`

Type: `integer`

Sets the Spanning Tree Protocol (STP) priority for this bridge. Lower values
are "better"; the lowest priority bridge will be elected the root bridge.

## `LinuxBridge.Port.SUBTREE`

Type: List of dictionary.

This property does not support partial editing, the full list of ports is
required in desired state.

## `LinuxBridge.Port.NAME`

Type: `string`

The interface name of the linux bridge port.

## `LinuxBridge.Port.STP_PRIORITY`

Type: `integer`

The Spanning Tree Protocol (STP) priority of this bridge port. Lower values
are "better".

## `LinuxBridge.Port.STP_HAIRPIN_MODE`

Type: `bool`

Whether [hairpin mode][hairpin] is enabled or not for the port.

## `LinuxBridge.Port.STP_PATH_COST`

Type: `integer`

The Spanning Tree Protocol (STP) port cost for destinations via this port.

# Interface -- Bond

Besides basic interface properties, each bond interface state also
contains a dictionary saved in key `BOND.CONFIG_SUBTREE`.

Example:

```python

{
    Interface.KEY: [
        Interface.NAME: "bond99",
        Interface.TYPE: InterfaceType.BOND,
        Interface.STATE: InterfaceState.UP
        Bond.CONFIG_SUBTREE: {
            Bond.MODE: "balance-rr",
            Bond.OPTIONS: {},
            Bond.SLAVES: ["eth1"]
        },
    ]
}
```

## `Bond.MODE`

Type: `string`

The bond mode in kernel. Possible values are:
 * `balance-rr`
 * `active-backup`
 * `balance-xor`
 * `broadcast`
 * `802.3ad`
 * `balance-alb`

Please refer to [kernel document for detail][bond-kernel-doc].

## `Bond.OPTIONS`

Type: `dictionary`

The dictionary of bond options.
Please refer to [kernel document for detail][bond-kernel-doc].


## `Bond.SLAVES`

Type: list of string

The names of bond slave interfaces.
This property does not support partial editing, full list of salves is required
in desired state.

# Interface -- Open vSwitch(OVS) Bridge

Besides basic interface properties, each OVS bridge interface state also
contains a dictionary saved in key `OVSBridge.CONFIG_SUBTREE`.

Example:

```python

{
    Interface.KEY: [{
        Interface.NAME: "ovs-br0",
        Interface.TYPE: InterfaceType.BOND,
        Interface.STATE: InterfaceState.UP
        OVSBridge.CONFIG_SUBTREE: {
            OVSBridge.OPTIONS_SUBTREE: {
                OVSBridge.FAIL_MODE: "",
                OVSBridge.MCAST_SNOOPING_ENABLE: false,
                OVSBridge.RSTP: false,
                OVSBridge.STP: true
            },
            OVSBridge.PORT_SUBTREE: [
                {
                    OVSBridge.PORT_NAME: "ovs0",
                },
                {
                    OVSBridge.PORT_NAME: "eth1",
                }
            ]
        },
    }]
}
```

## `OVSBridge.Options.FAIL_MODE`

Type: `string`, 'secure' or 'standalone' or empty.

The bridge failure mode.

## `OVSBridge.Options.MCAST_SNOOPING_ENABLE`

Type: `bool`

Enable or disable multicast snooping.

## `OVSBridge.Options.RSTP`

Type: `bool`

Enable or disable RSTP.

## `OVSBridge.Options.STP`

Type: `bool`

Enable or disable STP.

## `OVSBridge.PORT_SUBTREE`

Type: List of dictionary.

This property does not support partial editing, the full list of ports is
required in desired state.


## `OVSBridge.Port.NAME`

Type: `string`

The OVS bridge port name.

# Route

Nmstate provides both running and in-config routes via `libnmstate.show()`.
Example:

```python
{
    Route.KEY: {
        Route.CONFIG: [
            {
                Route.TABLE_ID: 0,
                Route.DESTINATION: "0.0.0.0/0",
                Route.NEXT_HOP_INTERFACE: "eth1",
                Route.NEXT_HOP_ADDRESS: "192.0.2.1",
                Route.METRIC: -1
            }
        ],
        Route.RUNNING: [
            {
                Route.TABLE_ID: 254,
                Route.DESTINATION: "192.0.2.0/24",
                Route.NEXT_HOP_INTERFACE: "eth1",
                Route.NEXT_HOP_ADDRESS: "",
                Route.METRIC: 100
            },
    }
}
```

The running routes contains route entries in kernel, which might contain:
 * Routes from DHCP or IPv6-RA.
 * Routes generated by kernel automatically. Like multicast, LAN and etc.

The config routes are the static routes saved in configuration files.

The `libnmstate.apply(desire_state)` merges current config routes into
`desire_state`, to remove config route entry, please use
`Route.STATE: Route.STATE_ABSENT` in the route entry:

* Remove specific route config entry:

  Just add `Route.STATE: Route.STATE_ABSENT` to the route entry to be
  removed.

* Remove all route config entry that matches:

  If any property is not defined in config absent route, it is treat as
  wildcard.
  For example, the following route entry removes all routes whose next hop is
  'eth1'.

```python
    {
        Route.STATE: Route.STATE_ABSENT,
        Route.NEXT_HOP_INTERFACE: "eth1"
    }
```

## `Route.TABLE_ID`

Type: `integer`

The route table ID.
If not defined, set as `Route.USE_DEFAULT_ROUTE_TABLE` to use the `default`
route table.

## `Route.DESTINATION`

Type: `string`

The route destination in the format like `192.0.2.0/24` or `2001:db8:1::/64`.

## `Route.NEXT_HOP_INTERFACE`

Type: `string`

The next hop interface. Mandatory.

## `Route.NEXT_HOP_ADDRESS`

Type: `string`

The next hop address.

## `Route.METRIC`

Type: `integer`

The route metric.
If not define, set to `Route.USE_DEFAULT_METRIC` for the default metric.

# DNS client configuration

Nmstate provides both running and in-config DNS client configuration via
`libnmstate.show()`. Example:

```python
{
    DNS.KEY: {
        DNS.CONFIG: {
            DNS.SEARCH: [
                "example.org",
                "example.com",
            ],
            DNS.SERVER: [
                "192.0.2.1"
            ]
        },
        DNS.RUNNING: {
            DNS.SEARCH: [
                "example.org",
                "example.com",
            ],
            DNS.SERVER: [
                "192.0.2.1"
            ]
        }
    },
}
```

The running DNS config it might contains DNS config from DHCP or IPv6-RA.
The config DNS is the static configuration saved in configuration files.

The `libnmstate.apply(desire_state)` copies current DNS config to
`desire_state` if `DNS.KEY` is missing from `desire_state`. Otherwise, DNS
config in `desire_state` overrides current DNS configurations.

## `DNS.SEARCH`

Type: list of string

Search list for host-name lookup.

## `DNS.SERVER`

Type: list of string

Name server IP addresses.


[py_example]: ./py_example.md
[rfc-dhcpv4-classless-route]: https://tools.ietf.org/html/rfc3442
[show]: ./api.md#query-network-state
[apply]: ./api.md#apply-network-state
[manual]: ./api.md#manual-transaction-control
[error]: ./api.md#error-handling
[bond-kernel-doc]: https://www.kernel.org/doc/Documentation/networking/bonding.txt
[hairpin]: https://lwn.net/Articles/347344/
[limitations]: https://github.com/nmstate/nmstate/blob/master/LIMITATIONS.md
