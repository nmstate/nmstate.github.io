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
        * [NmstateNotSupportedError](#nmstatenotsupportederror)
        * [NmstateTimeoutError](#nmstatetimeouterror)
        * [NmstatePluginError](#nmstatepluginerror)
    * [Schema](#schema)
* [Network State Main layout](#network-state-main-layout)
* [Basic Interface](#basic-interface)
    * [Basic Interface: Example](#basic-interface-example)
    * [`Interface.NAME`](#interfacename)
    * [`Interface.TYPE`](#interfacetype)
    * [`Interface.STATE`](#interfacestate)
    * [`Interface.MTU`](#interfacemtu)
    * [`Interface.MAC`](#interfacemac)
    * [`Interface.MIN_MTU`](#interfacemin_mtu)
    * [`Interface.MAX_MTU`](#interfacemax_mtu)
    * [`Interface.COPY_MAC_FROM`](#interfacecopy_mac_from)
    * [`Interface.DESCRIPTION`](#interfacedescription)
    * [`InterfaceIP`](#interfaceip)
    * [`Interface.IPV4`](#interfaceipv4)
        * [`InterfaceIPv4.ENABLED`](#interfaceipv4enabled)
        * [`InterfaceIPv4.DHCP`](#interfaceipv4dhcp)
        * [`InterfaceIPv4.AUTO_ROUTES`](#interfaceipv4auto_routes)
        * [`InterfaceIPv4.AUTO_GATEWAY`](#interfaceipv4auto_gateway)
        * [`InterfaceIPv4.AUTO_DNS`](#interfaceipv4auto_dns)
        * [`InterfaceIPv4.AUTO_ROUTE_TABLE_ID`](#interfaceipv4auto_route_table_id)
        * [`InterfaceIPv4.AUTO_ROUTE_METRIC`](#interfaceipv4auto_route_metric)
        * [`InterfaceIPv4.ADDRESS`](#interfaceipv4address)
            * [`InterfaceIPv4.ADDRESS_IP`](#interfaceipv4address_ip)
            * [`InterfaceIPv4.ADDRESS_PREFIX_LENGTH`](#interfaceipv4address_prefix_length)
    * [`Interface.IPV6`](#interfaceipv6)
        * [`InterfaceIPv6.ENABLED`](#interfaceipv6enabled)
        * [`InterfaceIPv6.DHCP`](#interfaceipv6dhcp)
        * [`InterfaceIPv6.AUTOCONF`](#interfaceipv6autoconf)
        * [`InterfaceIPv6.AUTO_ROUTES`](#interfaceipv6auto_routes)
        * [`InterfaceIPv6.AUTO_GATEWAY`](#interfaceipv6auto_gateway)
        * [`InterfaceIPv6.AUTO_DNS`](#interfaceipv6auto_dns)
        * [`InterfaceIPv6.AUTO_ROUTE_TABLE_ID`](#interfaceipv6auto_route_table_id)
        * [`InterfaceIPv6.AUTO_ROUTE_METRIC`](#interfaceipv6auto_route_metric)
        * [`InterfaceIPv6.ADDRESS`](#interfaceipv6address)
            * [`InterfaceIPv6.ADDRESS_IP`](#interfaceipv6address_ip)
            * [`InterfaceIPv6.ADDRESS_PREFIX_LENGTH`](#interfaceipv6address_prefix_length)
    * [`Interface.ACCEPT_ALL_MAC_ADDRESSES`](#interfaceaccept_all_mac_addresses)
    * [`Interface.WAIT_IP`](#interfacewait_ip)
    * [`Interface.CONTROLLER`](#interfacecontroller)
    * [`Interface.PROFILE_NAME`](#interfaceprofile_name)
    * [`Interface.IDENTIFIER`](#interfaceidentifier)
    * [`Interface.IDENTIFIER_NAME`](#interfaceidentifier_name)
    * [`Interface.IDENTIFIER_MAC`](#interfaceidentifier_mac)
* [Ethernet](#ethernet)
    * [`Ethernet.AUTO_NEGOTIATION`](#ethernetauto_negotiation)
    * [`Ethernet.DUPLEX`](#ethernetduplex)
    * [`Ethernet.SPEED`](#ethernetspeed)
    * [`Ethernet.SRIOV_SUBTREE`](#ethernetsriov_subtree)
        * [Limitations of SR-IOV configuration](#limitations-of-sr-iov-configuration)
        * [`Ethernet.SRIOV.TOTAL_VFS`](#ethernetsriovtotal_vfs)
        * [`Ethernet.SRIOV.VFS_SUBTREE`](#ethernetsriovvfs_subtree)
            * [`Ethernet.SRIOV.VFS.ID`](#ethernetsriovvfsid)
            * [`Ethernet.SRIOV.VFS.MAC_ADDRESS`](#ethernetsriovvfsmac_address)
            * [`Ethernet.SRIOV.VFS.SPOOF_CHECK`](#ethernetsriovvfsspoof_check)
            * [`Ethernet.SRIOV.VFS.TRUST`](#ethernetsriovvfstrust)
            * [`Ethernet.SRIOV.VFS.MIN_TX_RATE`](#ethernetsriovvfsmin_tx_rate)
            * [`Ethernet.SRIOV.VFS.VLAN_ID`](#ethernetsriovvfsvlan_id)
            * [`Ethernet.SRIOV.VFS.QOS`](#ethernetsriovvfsqos)
* [VLAN](#vlan)
    * [`VLAN.ID`](#vlanid)
    * [`VLAN.BASE_IFACE`](#vlanbase_iface)
* [VXLAN](#vxlan)
    * [`VXLAN.ID`](#vxlanid)
    * [`VXLAN.BASE_IFACE`](#vxlanbase_iface)
    * [`VXLAN.REMOTE`](#vxlanremote)
    * [`VXLAN.DESTINATION_PORT`](#vxlandestination_port)
* [Linux Bridge](#linux-bridge)
    * [Limitations of Linux Bridge Interface](#limitations-of-linux-bridge-interface)
    * [`LinuxBridge.Options.GROUP_FORWARD_MASK`](#linuxbridgeoptionsgroup_forward_mask)
    * [`LinuxBridge.Options.MAC_AGEING_TIME`](#linuxbridgeoptionsmac_ageing_time)
    * [`LinuxBridge.Options.MULTICAST_SNOOPING`](#linuxbridgeoptionsmulticast_snooping)
    * [`LinuxBridge.Options.GROUP_ADDR`](#linuxbridgeoptionsgroup_addr)
    * [`LinuxBridge.Options.GROUP_FWD_MASK`](#linuxbridgeoptionsgroup_fwd_mask)
    * [`LinuxBridge.Options.HASH_ELASTICITY`](#linuxbridgeoptionshash_elasticity)
    * [`LinuxBridge.Options.HASH_MAX`](#linuxbridgeoptionshash_max)
    * [`LinuxBridge.Options.MULTICAST_ROUTER`](#linuxbridgeoptionsmulticast_router)
    * [`LinuxBridge.Options.MULTICAST_LAST_MEMBER_COUNT`](#linuxbridgeoptionsmulticast_last_member_count)
    * [`LinuxBridge.Options.MULTICAST_LAST_MEMBER_INTERVAL`](#linuxbridgeoptionsmulticast_last_member_interval)
    * [`LinuxBridge.Options.MULTICAST_MEMBERSHIP_INTERVAL`](#linuxbridgeoptionsmulticast_membership_interval)
    * [`LinuxBridge.Options.MULTICAST_QUERIER`](#linuxbridgeoptionsmulticast_querier)
    * [`LinuxBridge.Options.MULTICAST_QUERIER_INTERVAL`](#linuxbridgeoptionsmulticast_querier_interval)
    * [`LinuxBridge.Options.MULTICAST_QUERY_USE_IFADDR`](#linuxbridgeoptionsmulticast_query_use_ifaddr)
    * [`LinuxBridge.Options.MULTICAST_QUERY_INTERVAL`](#linuxbridgeoptionsmulticast_query_interval)
    * [`LinuxBridge.Options.MULTICAST_QUERY_RESPONSE_INTERVAL`](#linuxbridgeoptionsmulticast_query_response_interval)
    * [`LinuxBridge.Options.MULTICAST_STARTUP_QUERY_COUNT`](#linuxbridgeoptionsmulticast_startup_query_count)
    * [`LinuxBridge.Options.MULTICAST_STARTUP_QUERY_INTERVAL`](#linuxbridgeoptionsmulticast_startup_query_interval)
    * [`LinuxBridge.Options.VLAN_DEFAULT_PVID`](#linuxbridgeoptionsvlan_default_pvid)
    * [`LinuxBridge.STP.ENABLED`](#linuxbridgestpenabled)
    * [`LinuxBridge.STP.FORWARD_DELAY`](#linuxbridgestpforward_delay)
    * [`LinuxBridge.STP.HELLO_TIME`](#linuxbridgestphello_time)
    * [`LinuxBridge.STP.MAX_AGE`](#linuxbridgestpmax_age)
    * [`LinuxBridge.STP.PRIORITY`](#linuxbridgestppriority)
    * [`LinuxBridge.Port.SUBTREE`](#linuxbridgeportsubtree)
    * [`LinuxBridge.Port.NAME`](#linuxbridgeportname)
    * [`LinuxBridge.Port.STP_PRIORITY`](#linuxbridgeportstp_priority)
    * [`LinuxBridge.Port.STP_HAIRPIN_MODE`](#linuxbridgeportstp_hairpin_mode)
    * [`LinuxBridge.Port.STP_PATH_COST`](#linuxbridgeportstp_path_cost)
    * [`LinuxBridge.Port.VLAN_SUBTREE`](#linuxbridgeportvlan_subtree)
        * [`LinuxBridge.Port.Vlan.MODE`](#linuxbridgeportvlanmode)
        * [`LinuxBridge.Port.Vlan.ENABLE_NATIVE`](#linuxbridgeportvlanenable_native)
        * [`LinuxBridge.Port.Vlan.TAG`](#linuxbridgeportvlantag)
        * [`LinuxBridge.Port.Vlan.TRUNK_TAGS`](#linuxbridgeportvlantrunk_tags)
* [Interface - Bond](#interface---bond)
    * [`Bond.MODE`](#bondmode)
    * [`Bond.OPTIONS_SUBTREE`](#bondoptions_subtree)
    * [`Bond.PORT`](#bondport)
* [Interface - Open vSwitch(OVS) Bridge](#interface---open-vswitchovs-bridge)
    * [Limitations of OpenvSwitch Interfaces](#limitations-of-openvswitch-interfaces)
    * [`OVSBridge.Options.FAIL_MODE`](#ovsbridgeoptionsfail_mode)
    * [`OVSBridge.Options.MCAST_SNOOPING_ENABLE`](#ovsbridgeoptionsmcast_snooping_enable)
    * [`OVSBridge.Options.RSTP`](#ovsbridgeoptionsrstp)
    * [`OVSBridge.Options.STP`](#ovsbridgeoptionsstp)
    * [`OVSBridge.PORT_SUBTREE`](#ovsbridgeport_subtree)
        * [`OVSBridge.Port.NAME`](#ovsbridgeportname)
* [Interface - Open vSwitch(OVS) Internal](#interface---open-vswitchovs-internal)
    * [`OVSInterface.Patch.PEER`](#ovsinterfacepatchpeer)
* [MAC VTAP](#mac-vtap)
    * [`MacVtap.BASE_IFACE`](#macvtapbase_iface)
    * [`MacVtap.MODE`](#macvtapmode)
    * [`MacVtap.PROMISCUOUS`](#macvtappromiscuous)
* [MAC VLAN](#mac-vlan)
    * [`MacVlan.BASE_IFACE`](#macvlanbase_iface)
    * [`MacVlan.MODE`](#macvlanmode)
    * [`MacVlan.PROMISCUOUS`](#macvlanpromiscuous)
* [IP over InfiniBand(IPoIB)](#ip-over-infinibandipoib)
    * [`InfiniBand.BASE_IFACE`](#infinibandbase_iface)
    * [`InfiniBand.MODE`](#infinibandmode)
    * [`InfiniBand.PKEY`](#infinibandpkey)
* [Virtual Routing and Forwarding (VRF)](#virtual-routing-and-forwarding-vrf)
    * [`VRF.ROUTE_TABLE_ID`](#vrfroute_table_id)
    * [`VRF.PORT_SUBTREE`](#vrfport_subtree)
* [Team](#team)
    * [Limitations of Team interface](#limitations-of-team-interface)
    * [`Team.Port.NAME`](#teamportname)
    * [`Team.Runner.NAME`](#teamrunnername)
* [Linux Virtual Ethernet(veth)](#linux-virtual-ethernetveth)
    * [`Veth.CONFIG_SUBTREE`](#vethconfig_subtree)
        * [`Veth.PEER`](#vethpeer)
* [LLDP](#lldp)
    * [`LLDP.CONFIG_SUBTREE`](#lldpconfig_subtree)
        * [`LLDP.ENABLED`](#lldpenabled)
        * [`LLDP.NEIGHBORS_SUBTREE`](#lldpneighbors_subtree)
        * [`LLDP.Nieghbors.DESCRIPTION`](#lldpnieghborsdescription)
        * [`LLDP.Neighbors.TLV_TYPE`](#lldpneighborstlv_type)
        * [`LLDP.Neighbors.TLV_SUBTYPE`](#lldpneighborstlv_subtype)
        * [`LLDP.Neighbors.ORGANIZATION_CODE`](#lldpneighborsorganization_code)
* [Route](#route)
    * [`Route.TABLE_ID`](#routetable_id)
    * [`Route.DESTINATION`](#routedestination)
    * [`Route.NEXT_HOP_INTERFACE`](#routenext_hop_interface)
    * [`Route.NEXT_HOP_ADDRESS`](#routenext_hop_address)
    * [`Route.METRIC`](#routemetric)
    * [`Route.WEIGHT`](#routeweight)
* [Route Rule](#route-rule)
    * [`RouteRule.CONFIG`](#routeruleconfig)
    * [`RouteRule.IP_FROM`](#routeruleip_from)
    * [`RouteRule.IP_TO`](#routeruleip_to)
    * [`RouteRule.PRIORITY`](#routerulepriority)
    * [`RouteRule.ROUTE_TABLE`](#routeruleroute_table)
    * [`RouteRule.USE_DEFAULT_PRIORITY`](#routeruleuse_default_priority)
    * [`RouteRule.USE_DEFAULT_ROUTE_TABLE`](#routeruleuse_default_route_table)
    * [`RouteRule.FWMARK`](#routerulefwmark)
    * [`RouteRule.FWMASK`](#routerulefwmask)
    * [`RouteRule.FAMILY`](#routerulefamily)
    * [`RouteRule.FAMILY_IPV4`](#routerulefamily_ipv4)
    * [`RouteRule.FAMILY_IPV6`](#routerulefamily_ipv6)
    * [`RouteRule.IFF`](#routeruleiff)
    * [`RouteRule.ACTION`](#routeruleaction)
* [DNS client configuration](#dns-client-configuration)
    * [Limitations of static DNS configuration](#limitations-of-static-dns-configuration)
    * [`DNS.SEARCH`](#dnssearch)
    * [`DNS.SERVER`](#dnsserver)
* [Multipath TCP](#multipath-tcp)

<!-- vim-markdown-toc -->

# Introduction

This document holds all the API detail of the `libnmstate` Python module.
For rust crate API, please refer to [our doc in docs.rs][docs_rs_url]

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

All constants and functions are available since version 1.0.0 unless state
explicitly.

Besides using python dictionary with constants in `libnmstate.schema` as desire
state for `libnmstate.apply()`, you may also use `yaml.load()` or
`json.loads()` to convert desire state from YAML/JSON to dictionary
`libnmstate.apply()` needs. Please use [YAML API document][yaml_api_doc]
if you prefer do show/apply the state in YAML format.

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
    * Invalid value of desired property, like bond missing port.

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

### NmstateNotSupportedError

A resource like a device does not support the requested feature.

### NmstateTimeoutError

The transaction execution timed out.

### NmstatePluginError

Unexpected plugin behaviour happens, it is a bug of the plugin.

## Schema

The YAML schema file is placed at `libnmstate/schemas/operational-state.yaml`.

In stead of using hard coded string, please use constants defined in
`libnmstate.schema`.

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
        InterfaceIPv4.ENABLED: True,
        InterfaceIPv4.DHCP: False,
        InterfaceIPv4.ADDRESS: [
            {
                InterfaceIPv4.ADDRESS_IP: '192.0.2.251',
                InterfaceIPv4.ADDRESS_PREFIX_LENGTH: 24,
            }
        ],
    },
    Interface.IPV6: {
        InterfaceIPv6.ENABLED: True,
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
 * `InterfaceType.TEAM`     (deprecated since 1.3 and removed since 2.0)
 * `InterfaceType.VRF`
 * `InterfaceType.INFINIBAND`
 * `InterfaceType.MAC_VLAN`
 * `InterfaceType.MAC_VTAP`
 * `InterfaceType.VETH`     (new in 1.0.1)
 * `InterfaceType.LOOPBACK`

## `Interface.STATE`

The interface state.

Type: `string`

Possible values:

 * `InterfaceState.UP`

   Bring interface link up.

 * `InterfaceState.DOWN`

   Deactivate the interface but keep the configure.
   Software based interface will be removed from kernel.
   You may reactivate the interface with old config again by using
   `InterfaceState.UP`.
   The interface will be activated again after reboot.

 * `InterfaceState.ABSENT`

   Deactivate the interface and remove its persisted configurations.


## `Interface.MTU`

The maximum transmission unit of interface.

Type: `interger`

## `Interface.MIN_MTU`

The minimum MTU supported by specified interface. This is a query only property
and will be ignored when applying.

Type: `integer`

## `Interface.MAX_MTU`

The maximum MTU supported by specified interface. This is a query only property
and will be ignored when applying.

Type: `integer`

## `Interface.MAC`

The MAC address of interface.

Type: `String` in the format of `EA:60:E4:08:17:F1`. Case insensitive.

## `Interface.COPY_MAC_FROM`

New in 1.0.2.

Only valid for `InterfaceType.BOND`, `InterfaceType.LINUX_BRIDGE` and
`InterfaceType.OVS_INTERFACE`.
The `Interface.COPY_MAC_FROM` could hold the name of port which you
want this controller interface to use the same MAC with.

The MAC address is read from permanent hardware MAC address and fallback
to current MAC address.

For example, below yaml state will create/change linux bridge `br0` to
use the MAC address of `eth2`.

```yaml
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

## `Interface.DESCRIPTION`

The description of the interface.

Type: `String`

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
 * `InterfaceIP.AUTO_ROUTE_METRIC` (new in 1.4+ and 2.2+)

## `Interface.IPV4`

The `Interface.IPV4` property holds the IPv4 stack information in a dictionary.

Example:
```python
{
    Interface.IPV4: {
        InterfaceIPv4.ENABLED: True,
        InterfaceIPv4.DHCP: True,
        InterfaceIPv4.ADDRESS: [],
        InterfaceIPv4.AUTO_DNS: True,
        InterfaceIPv4.AUTO_GATEWAY: True,
        InterfaceIPv4.AUTO_ROUTES: True,
        InterfaceIPv4.AUTO_ROUTE_TABLE_ID: 100,
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

### `InterfaceIPv4.AUTO_ROUTE_TABLE_ID`

The table where the route will be configured when `InterfaceIPv4.AUTO_ROUTES`
is enabled.

Type: `integer`

### `InterfaceIPv4.AUTO_ROUTE_METRIC`

New in 1.4 and 2.2.

The metric number assigned to routes retried from DHCPv4 or IPv6 autoconf.

Type: `integer`

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
        InterfaceIPv6.ENABLED: True,
        InterfaceIPv6.DHCP: True,
        InterfaceIPv6.ADDRESS: [],
        InterfaceIPv6.AUTO_DNS: True,
        InterfaceIPv6.AUTO_GATEWAY: True,
        InterfaceIPv6.AUTO_ROUTES: True,
        InterfaceIPv6.AUTO_ROUTE_TABLE_ID: 100,
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

### `InterfaceIPv6.AUTO_GATEWAY`

Type: `bool`

Possible values:

 * `True` - Apply gateway retrieved from IPv6 router advertisement.

 * `False` - Ignore gateway retrieved from IPv6 router advertisement.

### `InterfaceIPv6.AUTO_DNS`

Type: `bool`

Possible values:

 * `True`

   Ignore DNS client configuration retrieved from IPv6 router advertisement and
   DHCPv6.

 * `False`

   Apply DNS client configuration retrieved from IPv6 router advertisement and
   DHCPv6.

### `InterfaceIPv6.AUTO_ROUTE_TABLE_ID`

The table where the route will be configured when `InterfaceIPv6.AUTO_ROUTES`
is enabled.

Type: `integer`

### `InterfaceIPv6.AUTO_ROUTE_METRIC`

New in 1.4 and 2.2.

The metric number assigned to routes retried from DHCPv4 or IPv6 autoconf.

Type: `integer`

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
#### `InterfaceIPv6.ADDRESS_IP`

The static IPv6 address.

Type: `string`

Format: `2001:db8:1::f`


#### `InterfaceIPv6.ADDRESS_PREFIX_LENGTH`

The prefix length of static IPv6 address.

Type: `interger`.

# Ethernet

Besides basic interface properties, each Ethernet interface state also contains
a dictionary saved in key `Ethernet.CONFIG_SUBTREE`.

For virtual interface like `virtio_net` or `veth`, there is no
`Ethernet.CONFIG_SUBTREE`.

Example:

```python
{
    Interface.KEY: [
        {
            Interface.NAME: "eth1",
            Interface.TYPE: InterfaceType.ETHERNET,
            Interface.STATE: InterfaceState.UP,
            Ethernet.CONFIG_SUBTREE: {
                Ethernet.AUTO_NEGOTIATION: True,
                Ethernet.DUPLEX: Ethernet.FULL_DUPLEX,
                Ethernet.SPEED: 1000
            }
        }
    ]
}
```

## `Interface.ACCEPT_ALL_MAC_ADDRESSES`

Type: `bool`

The `Interface.ACCEPT_ALL_MAC_ADDRESSES` causes the kernel to accept ethernet
packages even its destination MAC address is not for current interface.

## `Interface.WAIT_IP`

Type: `string`

The `Interface.WAIT_IP`property defines which IP stack should network backend
wait before considering the interface activation finished.

It supports these values: `any`, `ipv4`, `ipv6` and `ipv4+ipv6`. For more
details please check the [YAML API
documentation](https://nmstate.io/devel/yaml_api.html#wait-ip)

## `Interface.CONTROLLER`

Type: `string`

The `Interface.CONTROLLER` property defines the controller of the specified
interface. This property is hidden when querying and only used for controller
attaching and detaching. Setting as empty means deatching from current
controller.

## `Interface.PROFILE_NAME`

Type: `string`

The profile name used by network backed.

## `Interface.IDENTIFIER`

Type: `string`

The `Interface.IDENTIFIER` property is only valid for applying or generating
configurations. The valid values are: `Interface.IDENTIFIER_NAME` or
`Interface.IDENTIFIER_MAC`. For further information, please read the [YAML API
documentation](https://nmstate.io/devel/yaml_api.html#interface-identifier).

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
dictionary allows to configure it.

### Limitations of SR-IOV configuration

* Some NICs don't use the standard kernel API to create VF or change the number
 of VF, such as mlx4_en, cxgb4 etc. Try running
 `echo 1 > /sys/class/net/${interface}/device/sriov_numvfs` to create a VF, if
 fails, then nmstate might not support manipulating SR-IOV on these NICs.
* For some NICs, the MAC address of a VF might vary automatically even though
you have specified it by nmstate, because PF enforces a MAC override. For such
NICs, configuring MAC on the corresponding interface of a VF, instead of on the
VF itself, is recommended.
* Not all the SR-IOV options listed below can be configured, it depends on a
NIC's SR-IOV capabilities.

Nmstate supports the following options:

### `Ethernet.SRIOV.TOTAL_VFS`

Type: `integer`

Total number of Virtual Functions per Physical Function. Changing the total VFs
will also change the interfaces that are visible in the Nmstate state. Reducing
the total VFs without setting `Interface.STATE` to `InterfaceState.ABSENT` for
the corresponding interfaces might lead to an undefined system state or error.

Example:
```python
{
    Interface.KEY: [
    {
        Interface.NAME: "eth1",
        Interface.TYPE: InterfaceType.ETHERNET,
        Interface.STATE: InterfaceState.UP,
        Ethernet.CONFIG_SUBTREE: {
            Ethernet.SRIOV_SUBTREE: {
                Ethernet.SRIOV.TOTAL_VFS: 1,
                Ethernet.SRIOV.VFS_SUBTREE: [
                    {
                        Ethernet.SRIOV.VFS.ID: 0,
                        Ethernet.SRIOV.VFS.MAC_ADDRESS: 76:A5:0B:F1:8A:C6,
                        Ethernet.SRIOV.VFS.MAX_TX_RATE: 1000,
                        Ethernet.SRIOV.VFS.MIN_TX_RATE: 100,
                        Ethernet.SRIOV.VFS.TRUST: False,
                    }
                ],
            }
        }
    }
    ]
}
```

### `Ethernet.SRIOV.VFS_SUBTREE`

If the ethernet SR-IOV is enabled and the `Ethernet.SRIOV.TOTAL_VFS` is greater
than  0, this will contain a list of dictionaries that allows to configure each
VF. Nmstate supports the following options:

#### `Ethernet.SRIOV.VFS.ID`

The VF id. This parameter is read-only.

Type: `integer`

#### `Ethernet.SRIOV.VFS.MAC_ADDRESS`

The MAC address of the VF.

Type: `string` in the format of `EA:60:E4:08:17:F1`. Case insensitive.

#### `Ethernet.SRIOV.VFS.SPOOF_CHECK`

Enable or disable spoof check in the VF.

Type: `bool`

#### `Ethernet.SRIOV.VFS.TRUST`

Enable or disable trust in the VF.

Type: `bool`

#### `Ethernet.SRIOV.VFS.MIN_TX_RATE`

The minimum TX rate. Please, note that not all the NICs support this option.

Type: `integer`

#### `Ethernet.SRIOV.VFS.VLAN_ID`

Available since 1.2.1

The VLAN ID of SRIOV VF.

Type: `integer`

#### `Ethernet.SRIOV.VFS.QOS`

Available since 1.2.1

The QoS value of SRIOV VF.

Type: `integer`

# VLAN
Besides basic interface properties, each VLAN interface state also contains
a dictionary saved in key `VLAN.CONFIG_SUBTREE`.

Example:

```python
{
    Interface.KEY: [
        {
            Interface.NAME: "eth1.101",
            Interface.TYPE: VLAN.TYPE,
            Interface.STATE: InterfaceState.UP,
            VLAN.CONFIG_SUBTREE: {
                VLAN.ID: 101,
                VLAN.BASE_IFACE: 'eth1'
            }
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
        Interface.STATE: InterfaceState.UP,
        LinuxBridge.CONFIG_SUBTREE: {
            LinuxBridge.OPTIONS_SUBTREE: {
                LinuxBridge.Options.GROUP_FORWARD_MASK: 0,
                LinuxBridge.Options.MAC_AGEING_TIME: 300,
                LinuxBridge.Options.MULTICAST_SNOOPING: True,
                LinuxBridge.STP_SUBTREE: {
                    LinuxBridge.STP.ENABLED: True,
                    LinuxBridge.STP.FORWARD_DELAY: 15,
                    LinuxBridge.STP.HELLO_TIME: 2,
                    LinuxBridge.STP.MAX_AGE: 20,
                    LinuxBridge.STP.PRIORITY: 32768
                }
            },
            LinuxBridge.PORT_SUBTREE: [
                {
                    LinuxBridge.Port.NAME: "eth1",
                    LinuxBridge.Port.STP_PRIORITY: 32,
                    LinuxBridge.Port.STP_HAIRPIN_MODE: False,
                    LinuxBridge.Port.STP_PATH_COST: 100
                }
            ]
        }
    }]
}
```
## Limitations of Linux Bridge Interface

If `Interface.MAC_ADDRESS` is not defined in desire state of Linux Bridge,
nmstate will trust kernel and systemd to determine the MAC address of this
bridge:

 * For RHEL/CentOS 8, the MAC of first attached port will be used.
 * For Fedora 31+ or other distribution with systemd 242+, systemd will
   generate random MAC address with interface name as seed.

If you want to enforcing bridge to use MAC address of first attached port,
you may create `/etc/systemd/network/98-bridge-inherit-mac.link` with below
contents:

```
[Match]
Type=bridge

[Link]
MACAddressPolicy=none
```

Of course, you can always explicitly define assign a MAC address to a bridge
without worrying above.

## `LinuxBridge.Options.GROUP_FORWARD_MASK`

Type: `integer`

The mask of group address to forward.

## `LinuxBridge.Options.MAC_AGEING_TIME`

Type: `integer`

The Spanning Tree Protocol (STP) maximum message age, in seconds.

## `LinuxBridge.Options.MULTICAST_SNOOPING`

Type: `bool`

Whether IGMP snooping is enabled for this bridge.

## `LinuxBridge.Options.GROUP_ADDR`

Type: `string` in the format of `01:80:C2:00:00:00`.

The MAC address of the multicast group this bridge uses for STP.

## `LinuxBridge.Options.GROUP_FWD_MASK`

Type: `integer`

A mask of group addresses to forward.

## `LinuxBridge.Options.HASH_ELASTICITY`

Type: `integer`

## `LinuxBridge.Options.HASH_MAX`

Type: `integer`

## `LinuxBridge.Options.MULTICAST_ROUTER`

Type: `string`

Sets bridge’s multicast router. Multicast-snooping must be enabled for this
option to work.

## `LinuxBridge.Options.MULTICAST_LAST_MEMBER_COUNT`

Type: `integer`

Set the number of queries the bridge will send before stopping forwarding a
multicast group after a “leave” message has been received.

## `LinuxBridge.Options.MULTICAST_LAST_MEMBER_INTERVAL`

Type: `integer`

Set interval (in deciseconds) between queries to find remaining members of a
group, after a “leave” message is received.

## `LinuxBridge.Options.MULTICAST_MEMBERSHIP_INTERVAL`

Type: `integer`

Set delay (in deciseconds) after which the bridge will leave a group, if no
membership reports for this group are received.

## `LinuxBridge.Options.MULTICAST_QUERIER`

Type: `bool`

Enable or disable sending of multicast queries by the bridge. If not specified
the option is disabled.

## `LinuxBridge.Options.MULTICAST_QUERIER_INTERVAL`

Type: `integer`

If no queries are seen after this delay (in deciseconds) has passed, the bridge
will start to send its own queries.

## `LinuxBridge.Options.MULTICAST_QUERY_USE_IFADDR`

Type: `bool`

If enabled the bridge’s own IP address is used as the source address for IGMP
queries otherwise the default of 0.0.0.0 is used.

## `LinuxBridge.Options.MULTICAST_QUERY_INTERVAL`

Type: `integer`

Interval (in deciseconds) between queries sent by the bridge after the end of
the startup phase.

## `LinuxBridge.Options.MULTICAST_QUERY_RESPONSE_INTERVAL`

Type: `integer`

Set the Max Response Time/Max Response Delay (in deciseconds) for IGMP/MLD
queries sent by the bridge.

## `LinuxBridge.Options.MULTICAST_STARTUP_QUERY_COUNT`

Type: `integer`

Set the number of IGMP queries to send during startup phase.

## `LinuxBridge.Options.MULTICAST_STARTUP_QUERY_INTERVAL`

Type: `integer`

Sets the time (in deciseconds) between queries sent out at startup to determine
membership information.

## `LinuxBridge.Options.VLAN_DEFAULT_PVID`

Type: `integer`

The drfault PVID for the ports of the bridge, that is the VLAN id assigned to
incoming untagged frames.

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

## `LinuxBridge.Port.VLAN_SUBTREE`
Each Linux Bridge port also features a subtree to configure the allowed
VLANs passing through it. If the VLAN subtree is not configured, all the VLANs
are allowed; when the VLAN subtree is specified, the user needs to provide the
full list of bridge ports.

Example:

```python
{
    LinuxBridge.Port.VLAN_SUBTREE: {
        LinuxBridge.Port.Vlan.MODE: LinuxBridge.Port.Vlan.Mode.TRUNK,
        LinuxBridge.Port.Vlan.ENABLE_NATIVE: True,
        LinuxBridge.Port.Vlan.TAG: 105,
        LinuxBridge.Port.Vlan.TRUNK_TAGS: [
            LinuxBridge.Port.TrunkTags.ID: 100,
            LinuxBridge.Port.TrunkTags.ID_RANGE: {
                LinuxBridge.Port.TrunkTags.MIN_RANGE: 500,
                LinuxBridge.Port.TrunkTags.MAX_RANGE: 599,
            }
        ]
    }
}
```

### `LinuxBridge.Port.Vlan.MODE`

Type: `string`

The operational mode of the port. Allowed values are:
* LinuxBridge.Port.Vlan.Mode.ACCESS
* LinuxBridge.Port.Vlan.Mode.TRUNK

### `LinuxBridge.Port.Vlan.ENABLE_NATIVE`

Type: `bool`
Default: `false`

Can only be used for trunk ports. When enabled, it configures the VLAN specified by
[`LinuxBridge.Port.Vlan.TAG`](#linuxbridgevlan_tag) as a native VLAN.

### `LinuxBridge.Port.Vlan.TAG`

Type: `integer`

The access tag of the access port, or the native vlan of the trunk port.

### `LinuxBridge.Port.Vlan.TRUNK_TAGS`

Type: `array of dictionary`.

Defines the white-list of VLANs accepted on this port. It accepts two formats:
* single tag - a dictionary with a single `id` key, and an integer value (0-4095)
* tag ranges - a dictionary with a single `id-range` key, whose value is a dict
  with two keys: `min` and `max`. Both values are integers, in the (0-4095)
  range.

# Interface - Bond

Besides basic interface properties, each bond interface state also
contains a dictionary saved in key `BOND.CONFIG_SUBTREE`.

Example:

```python
{
    Interface.KEY: [
        {
            Interface.NAME: "bond99",
            Interface.TYPE: InterfaceType.BOND,
            Interface.STATE: InterfaceState.UP,
            Bond.CONFIG_SUBTREE: {
                Bond.MODE: "balance-rr",
                Bond.OPTIONS_SUBTREE: {},
                Bond.PORT: ["eth1"]
            },
        }
    ]
}
```

## `Bond.MODE`

Type: `string`

The bond mode in kernel. Possible values are:
 * `BondMode.ROUND_ROBIN`
 * `BondMode.ACTIVE_BACKUP`
 * `BondMode.XOR`
 * `BondMode.BROADCAST`
 * `BondMode.LACP`
 * `BondMode.TLB`
 * `BondMode.ALB`

Please refer to [kernel document for detail][bond-kernel-doc].

When changing bond mode, the `Bond.OPTIONS_SUBTREE` will not merge from current
state.  User is required to provide full desire bond options when switching
bond mode.

## `Bond.OPTIONS_SUBTREE`

Type: `dictionary`

The dictionary of bond options.
Please refer to [kernel document for detail][bond-kernel-doc].

Set to `Bond.OPTIONS_SUBTREE: {}` will revert all bond options back to kernel
defaults.

Nmstate will not perform verification on whether desired bond options is
applied to kernel due to complexity of it, please verify the bond options by
yourself.

## `Bond.PORT`

Type: list of string

The names of bond port interfaces.
This property does not support partial editing, full list of ports is required
in desired state. If not defined in desire state, current status will be
preserved.

# Interface - Open vSwitch(OVS) Bridge

Besides basic interface properties, each OVS bridge interface state also
contains a dictionary saved in key `OVSBridge.CONFIG_SUBTREE`.

Example:

```python

{
    Interface.KEY: [{
        Interface.NAME: "ovs-br0",
        Interface.TYPE: InterfaceType.BOND,
        Interface.STATE: InterfaceState.UP,
        OVSBridge.CONFIG_SUBTREE: {
            OVSBridge.OPTIONS_SUBTREE: {
                OVSBridge.Options.FAIL_MODE: "",
                OVSBridge.Options.MCAST_SNOOPING_ENABLE: False,
                OVSBridge.Options.RSTP: False,
                OVSBridge.Options.STP: True
            },
            OVSBridge.PORT_SUBTREE: [
                {
                    OVSBridge.Port.NAME: "ovs0",
                },
                {
                    OVSBridge.Port.NAME: "eth1",
                }
            ]
        },
    }]
}
```
## Limitations of OpenvSwitch Interfaces

When certain OVS interfaces interface is not created by NetworkManager or
nmstate, the `libnmstate.show()` might showing the incorrect running state of
it.

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


### `OVSBridge.Port.NAME`

Type: `string`

The OVS bridge port name.

# Interface - Open vSwitch(OVS) Internal

Each OVS internal interface state contains a dictionary saved in key
`OVSInterface.PATCH_CONFIG_SUBTREE`.

## `OVSInterface.Patch.PEER`

Type: `string`

Interface patch peer name.

# MAC VTAP

Besides basic interface properties, each Macvtap interface state also contains
a dictionary saved in key `MacVtap.CONFIG_SUBTREE` holding information
for `MacVtap.BASE_IFACE`, `MacVtap.MODE` and `MacVtap.PROMISCUOUS`.

## `MacVtap.BASE_IFACE`

The interface name which current Macvtap is based on.

## `MacVtap.MODE`

The mode of Macvtap interface. Possible values are:
 * `MacVtap.Mode.VEPA`
 * `MacVtap.Mode.BRIDGE`
 * `MacVtap.Mode.PRIVATE`
 * `MacVtap.Mode.PASSTHRU`
 * `MacVtap.Mode.SOURCE`

## `MacVtap.PROMISCUOUS`

Bool. Whether accept all mac addresses mode is enabled or not.

# MAC VLAN

Besides basic interface properties, each Macvvlan interface state also contains
a dictionary saved in key `MacVlan.CONFIG_SUBTREE` holding information
for `MacVlan.BASE_IFACE`, `MacVlan.MODE` and `MacVlan.PROMISCUOUS`.

## `MacVlan.BASE_IFACE`

The interface name which current Macvvlan is based on.

## `MacVlan.MODE`

The mode of Macvvlan interface. Possible values are:
 * `MacVlan.Mode.VEPA`
 * `MacVlan.Mode.BRIDGE`
 * `MacVlan.Mode.PRIVATE`
 * `MacVlan.Mode.PASSTHRU`
 * `MacVlan.Mode.SOURCE`

## `MacVlan.PROMISCUOUS`

Bool. Whether accept all mac addresses mode is enabled or not.

# IP over InfiniBand(IPoIB)

Besides basic interface properties, each IP over InfiniBand interface state
also contains a dictionary saved in key `InfiniBand.CONFIG_SUBTREE` holding
information for `InfiniBand.BASE_IFACE`, `InfiniBand.MODE` and
`InfiniBand.PKEY`.

## `InfiniBand.BASE_IFACE`

The parent interface current IPoIB interface is based on.

## `InfiniBand.MODE`

IPoIB can run in two modes of operation: Connected
mode(`InfiniBand.Mode.CONNECTED`) and Datagram
mode(`InfiniBand.Mode.DATAGRAM`).

## `InfiniBand.PKEY`

The Partition Keys (PKeys) used to partition IPoIB communication. You may
use integer or hex string.
The `InfiniBand.DEFAULT_PKEY` indicating this IPoIB interface is not using
pkey partitioning.

# Virtual Routing and Forwarding (VRF)

Besides basic interface properties, each VRF interface state
also contains a dictionary saved in key `VRF.CONFIG_SUBTREE` holding
information for `VRF.ROUTE_TABLE_ID`, `VRF.PORT_SUBTREE`.

## `VRF.ROUTE_TABLE_ID`

Integer, the route table id this VRF is assigned to.

## `VRF.PORT_SUBTREE`

List of string, the name of interfaces assigned to this VRF interface.

# Team

Besides basic interface properties, each Team interface state also contains a
dictionary saved in key `Team.CONFIG_SUBTREE`. In addition, this dictionary
contains a list of dictionaries saved in key `Team.PORT_SUBTREE` and a
dictionary saved in key `Team.RUNNER_SUBTREE`.

Example:

```python
{
    Interface.KEY : [
        Interface.NAME: "team0",
        Interface.TYPE: InterfaceType.TEAM,
        Interface.STATE: InterfaceState.UP,
        Team.CONFIG_SUBTREE: {
            Team.PORT_SUBTREE: [
                {
                    Team.Port.NAME: "eth1"
                },
                {
                    Team.Port.NAME: "eth2"
                }
            ],
            Team.RUNNER_SUBTREE: {
                Team.Runner.NAME: Team.Runner.RunnerMode.LOAD_BALANCE
            }
        }
    ]
}
```

These values are based on the teamd JSON API. For further information please
refer to [teamd manual
page](https://github.com/jpirko/libteam/blob/master/man/teamd.conf.5)

## Limitations of Team interface

When certain team interface is not created by NetworkManager or nmstate,
the `libnmstate.show()` might showing the incorrect running state of it.

## `Team.Port.NAME`

Type: `string`

The interface name of the port.

## `Team.Runner.NAME`

Type: `string`

The mode in which the interface is operating. Currently we are supporting the
following values:

 * `Team.Runner.RunnedMode.LOAD_BALANCE`

# Linux Virtual Ethernet(veth)

New in version 1.0.1.

Besides basic interface properties, each Veth interface state also contains a
dictionary saved in key `Veth.CONFIG_SUBTREE`. In addition, this dictionary
contains a property named `Veth.PEER`.

Example:

```python
{
    Interface.KEY : [
        Interface.NAME: "veth0",
        Interface.TYPE: InterfaceType.VETH,
        Interface.STATE: InterfaceState.UP,
        Veth.CONFIG_SUBTREE: {
            Veth.PEER: "veth1"
        }
    ]
}
```

## `Veth.CONFIG_SUBTREE`

It contains the option to set the veth peer.

### `Veth.PEER`

Type: `string`

Set the veth interface peer.

# LLDP

Nmstate provides Link Layer Discovery Protocol information for each interface.

## `LLDP.CONFIG_SUBTREE`

It contains the option to enable LLDP and the neighbors information.

### `LLDP.ENABLED`

Type: `bool`

Enable or disable LLDP on the interface.

### `LLDP.NEIGHBORS_SUBTREE`

Contains a list of lists. Each element of the list is a neighbor announced by
LLDP and each element of the neighbor is a dictionary containing the TLV
information.

### `LLDP.Nieghbors.DESCRIPTION`

Type: `string`

Description of the TLV.

### `LLDP.Neighbors.TLV_TYPE`

Type: `integer`

### `LLDP.Neighbors.TLV_SUBTYPE`

Type: `integer`

### `LLDP.Neighbors.ORGANIZATION_CODE`

Type: `string` in the format 00:80:c2

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

## `Route.WEIGHT`

Type: `integer`

The route weight. Please, notice that this property is only effective on ECMP
route. Nmstate will configure ECMP route when possible on kernel.

# Route Rule

Nmstate provides routes rules via `libnmstate.show()`.
Example:

```python
{
    RouteRule.KEY: {
        RouteRule.CONFIG: [
            {
                RouteRule.IP_TO: 192.0.2.0/24,
                RouteRule.PRIORITY: 1000,
                RouteRule.ROUTE_TABLE: 50,
            },
        ],
    },
}
```

## `RouteRule.CONFIG`

A list containing the multiple rule configurations.

## `RouteRule.IP_FROM`

Type: `string` in the format 192.0.2.1/24

## `RouteRule.IP_TO`

Type: `string` in the format 192.0.2.1/24

## `RouteRule.PRIORITY`

Type: `integer`

The priority of the rule over the others.

## `RouteRule.ROUTE_TABLE`

Type: `integer`

The table id where the rule should be applied.

## `RouteRule.USE_DEFAULT_PRIORITY`

Type: `integer`

## `RouteRule.USE_DEFAULT_ROUTE_TABLE`

Type: `integer`

## `RouteRule.FWMARK`

Type: `integer`

The fwmark value used. The format reported by Nmstate is hexadecimal as `0x30`
and it is also supported for applying the state.

## `RouteRule.FWMASK`

Type: `integer`

The fwmask value used. The format reported by Nmstate is hexadecimal as `0x30`
and it is also supported for applying the state. In addition, notice that this
value is optional and only valid when `fwmark` is set.

## `RouteRule.FAMILY`

Type: `string`

The address family for this route rule. This option is specially useful when
configuring `from all to all` routing policy. The supported values are `ipv4`
and `ipv6`.

## `RouteRule.FAMILY_IPV4`

Type: `string`

## `RouteRule.FAMILY_IPV6`

Type: `string`

## `RouteRule.IIF`

Type: `string`

This property defines the matching on incoming interface name.

## `RouteRule.ACTION`

Type: `string`

This property defines the action for the routing policy. The supported values
are: `RouteRule.ACTION_BLACKHOLE`, `RouteRule.ACTION_UNREACHABLE` and
`RouteRule.ACTION_PROHIBIT`

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

## Limitations of static DNS configuration

Currently nmstate only support configuring static DNS on any of these
situations:

 * Has interface configured as static IP address and static default gateway.
 * Has interface configured as dynamic IP with `InterfaceIP.AUTO_DNS: False`.


## `DNS.SEARCH`

Type: list of string

Search list for host-name lookup.

## `DNS.SERVER`

Type: list of string

Name server IP addresses.

Placing IPv4/IPv6 name server in the middle of IPv6/IPv4 name servers is not
supported yet.


# Multipath TCP

Available since 2.2.0.

The `Interface.MPTCP` section of interface holds the Multipath TCP interface
level flags, it will apply flags to all valid IP addresses(both static and
dynamic).  Network backend might has its own rule on excusing special scope of
IP address, e.g. IPv6 multicast address.

Example:

```python
desired_state = {
    Interface.KEY: [
        {
            Interface.NAME: "eth1",
            Interface.TYPE: InterfaceType.ETHERNET,
            Interface.STATE: InterfaceState.UP,
            Interface.MPTCP: {
                Mptcp.ADDRESS_FLAGS: [
                    Mptcp.FLAG_SIGNAL,
                    Mptcp.FLAG_BACKUP,
                ]
            },
        }
    ]
}
libnmstate.apply(desired_state)
```

Supported MPTCP flags are:

 * `Mptcp.FLAG_FULLMESH`
 * `Mptcp.FLAG_SUBFLOW`
 * `Mptcp.FLAG_BACKUP`
 * `Mptcp.FLAG_SIGNAL`

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

[py_example]: ./py_example.md
[rfc-dhcpv4-classless-route]: https://tools.ietf.org/html/rfc3442
[show]: ./api.md#query-network-state
[apply]: ./api.md#apply-network-state
[manual]: ./api.md#manual-transaction-control
[error]: ./api.md#error-handling
[bond-kernel-doc]: https://www.kernel.org/doc/Documentation/networking/bonding.txt
[hairpin]: https://lwn.net/Articles/347344/
[docs_rs_url]: https://docs.rs/nmstate/
[yaml_api_doc]: ./yaml_api.md
