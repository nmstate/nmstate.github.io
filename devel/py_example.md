<!-- vim-markdown-toc GFM -->

* [Python code examples](#python-code-examples)
    * [Interface common tasks](#interface-common-tasks)
        * [Remove interface configuration](#remove-interface-configuration)
        * [Set static IP address](#set-static-ip-address)
        * [DHCP/Autoconf](#dhcpautoconf)
            * [Enable DHCP/Autoconf](#enable-dhcpautoconf)
            * [Enable DHCP/Autoconf without DNS configuration](#enable-dhcpautoconf-without-dns-configuration)
            * [Enable DHCP/Autoconf without gateway configuration](#enable-dhcpautoconf-without-gateway-configuration)
            * [Enable DHCP/Autoconf without route configuration](#enable-dhcpautoconf-without-route-configuration)
    * [Ethernet](#ethernet)
        * [Change Ethernet MTU](#change-ethernet-mtu)
    * [DNS](#dns)
    * [Route configuration](#route-configuration)
        * [Add static route entry](#add-static-route-entry)
        * [Change default gateway](#change-default-gateway)
        * [Remove all routes next hop to eth1](#remove-all-routes-next-hop-to-eth1)
    * [Bond](#bond)
        * [Create Bond](#create-bond)
        * [Add Interface to Bond](#add-interface-to-bond)
        * [Delete Bond](#delete-bond)
    * [VLAN](#vlan)
        * [Create VLAN](#create-vlan)
        * [Change VLAN ID](#change-vlan-id)
        * [Delete VLAN](#delete-vlan)
    * [Linux Bridge](#linux-bridge)
        * [Create Linux Bridge](#create-linux-bridge)
        * [Add Interface to Linux Bridge](#add-interface-to-linux-bridge)
        * [Delete Linux Bridge](#delete-linux-bridge)
    * [Open vSwitch Bridge](#open-vswitch-bridge)
        * [Create Open vSwitch Bridge](#create-open-vswitch-bridge)
        * [Delete Open vSwitch Bridge](#delete-open-vswitch-bridge)

<!-- vim-markdown-toc -->

# Python code examples

## Interface common tasks
### Remove interface configuration

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceState

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'eth1',
            Interface.STATE: InterfaceState.ABSENT
        }
    ]
})
```

### Set static IP address

Define an ethernet interface with two static IPv4 addresses and two static IPv6
addresses.

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceIPv4
from libnmstate.schema import InterfaceIPv6

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'eth1',
            Interface.STATE: InterfaceState.UP
            Interface.IPV4: {
                InterfaceIPv4.ENABLED: True,
                InterfaceIPv4.ADDRESS: [
                    {
                        InterfaceIPv4.ADDRESS_IP: '192.168.0.1',
                        InterfaceIPv4.ADDRESS_PREFIX_LENGTH: 24,
                    },
                    {
                        InterfaceIPv4.ADDRESS_IP: '192.168.1.1',
                        InterfaceIPv4.ADDRESS_PREFIX_LENGTH: 24,
                    },
                ],
                InterfaceIPv4.DHCP: False,
            },
            Interface.IPV6: {
                InterfaceIPV6.ENABLED: True,
                InterfaceIPv6.ADDRESS: [
                    {
                        InterfaceIPv6.ADDRESS_IP: '2001:db8:1::1',
                        InterfaceIPv6.ADDRESS_PREFIX_LENGTH: 64,
                    },
                    {
                        InterfaceIPv6.ADDRESS_IP: '2001:db8:2::1',
                        InterfaceIPv6.ADDRESS_PREFIX_LENGTH: 64,
                    },
                ],
                InterfaceIPV6.DHCP: False,
                InterfaceIPV6.AUTOCONF: False,
            },
        }
    ]
})
```

### DHCP/Autoconf
#### Enable DHCP/Autoconf

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceIPv4
from libnmstate.schema import InterfaceIPv6

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'eth1',
            Interface.STATE: InterfaceState.UP
            Interface.IPV4: {
                InterfaceIPv4.ENABLED: True,
                InterfaceIPv4.DHCP: True,
            },
            Interface.IPV6: {
                InterfaceIPV6.ENABLED: True,
                InterfaceIPV6.DHCP: True,
                InterfaceIPV6.AUTOCONF: True,
            },
        }
    ]
})
```

#### Enable DHCP/Autoconf without DNS configuration

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceIPv4
from libnmstate.schema import InterfaceIPv6

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'eth1',
            Interface.STATE: InterfaceState.UP
            Interface.IPV4: {
                InterfaceIPv4.ENABLED: True,
                InterfaceIPv4.DHCP: True,
                InterfaceIPv4.AUTO_DNS: False
            },
            Interface.IPV6: {
                InterfaceIPV6.ENABLED: True,
                InterfaceIPV6.DHCP: True,
                InterfaceIPV6.AUTOCONF: True,
                InterfaceIPv6.AUTO_DNS: False
            },
        }
    ]
})
```

#### Enable DHCP/Autoconf without gateway configuration

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceIPv4
from libnmstate.schema import InterfaceIPv6

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'eth1',
            Interface.STATE: InterfaceState.UP
            Interface.IPV4: {
                InterfaceIPv4.ENABLED: True,
                InterfaceIPv4.DHCP: True,
                InterfaceIPv4.AUTO_GATEWAY: False
            },
            Interface.IPV6: {
                InterfaceIPV6.ENABLED: True,
                InterfaceIPV6.DHCP: True,
                InterfaceIPV6.AUTOCONF: True,
                InterfaceIPv6.AUTO_GATEWAY: False
            },
        }
    ]
})
```

#### Enable DHCP/Autoconf without route configuration

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceIPv4
from libnmstate.schema import InterfaceIPv6

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'eth1',
            Interface.STATE: InterfaceState.UP
            Interface.IPV4: {
                InterfaceIPv4.ENABLED: True,
                InterfaceIPv4.DHCP: True,
                InterfaceIPv4.AUTO_ROUTES: False
            },
            Interface.IPV6: {
                InterfaceIPV6.ENABLED: True,
                InterfaceIPV6.DHCP: True,
                InterfaceIPV6.AUTOCONF: True,
                InterfaceIPv6.AUTO_ROUTES: False
            },
        }
    ]
})
```

## Ethernet

### Change Ethernet MTU

```python
import libnmstate
from libnmstate.schema import Interface

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'eth1',
            Interface.STATE: InterfaceState.UP
            Interface.MTU: 1460
        }
    ]
})
```

## DNS

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import DNS

libnmstate.apply(
{
    Interface.KEY: []
    DNS.KEY: {
        DNS.CONFIG: {
            DNS.SEARCH: [
                'example.org'
            ],
            DNS.SERVER: [
                '192.0.2.251'
            ]
        }
    }
})
```

## Route configuration

### Add static route entry

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import Route

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'eth1'
        }
    ]
    Route.KEY: {
        Route.CONFIG: [
            {
                Route.DESTINATION: "198.51.100.0/24",
                Route.NEXT_HOP_INTERFACE: "eth1",
                Route.NEXT_HOP_ADDRESS: "192.0.2.1",
            }
        ]
    }
})
```

### Change default gateway

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import Route

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'eth1'
        }
    ]
    Route.KEY: {
        Route.CONFIG: [
            {
                Route.STATE: Route.STATE_ABSENT,
                Route.DESTINATION: "0.0.0.0/0",
            }
        ]
        Route.CONFIG: [
            {
                Route.DESTINATION: "0.0.0.0/0",
                Route.NEXT_HOP_INTERFACE: "eth1",
                Route.NEXT_HOP_ADDRESS: "192.0.2.1",
            }
        ]
    }
})
```

### Remove all routes next hop to eth1

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import Route

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'eth1'
        }
    ]
    Route.KEY: {
        Route.CONFIG: [
            {
                Route.STATE: Route.STATE_ABSENT,
                Route.NEXT_HOP_INTERFACE: "eth1",
            }
        ]
    }
})
```

## Bond

### Create Bond

Create bond with two interfaces.

```python
import libnmstate
from libnmstate.schema import Bond
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceState
from libnmstate.schema import InterfaceType

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'eth1',
            Interface.STATE: InterfaceState.UP,
        },
        {
            Interface.NAME: 'eth2',
            Interface.STATE: InterfaceState.UP,
        },
        {
            Interface.NAME: 'bond99',
            Interface.TYPE: InterfaceType.BOND
            Interface.STATE: InterfaceState.UP,
        }
    ]
})
```

### Add Interface to Bond

Add an interface to existing bond.

```python
import libnmstate
from libnmstate.schema import Bond
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceState

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'eth3',
            Interface.STATE: InterfaceState.UP,
        },
        {
            Interface.NAME: 'bond99',
            Interface.TYPE: InterfaceType.BOND
            Interface.STATE: InterfaceState.UP,
            Bond.CONFIG_SUBTREE: {
                Bond.SLAVES: ['eth1', 'eth2', 'eth3']
            }
        }
    ]
})
```

### Delete Bond

Delete bond.

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceState

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'bond99',
            Interface.STATE: InterfaceState.ABSENT,
        }
    ]
})
```

## VLAN

### Create VLAN

Create VLAN for VLAN ID 101 of interface 'eth1'.

```python
import libnmstate
from libnmstate.schema import VLAN
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceState

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'eth1',
            Interface.STATE: InterfaceState.UP,
        },
        {
            Interface.NAME: 'eth1.101',
            Interface.TYPE: InterfaceType.VLAN,
            Interface.STATE: InterfaceState.UP,
            VLAN.CONFIG_SUBTREE: {
                VLAN.ID: 101,
                VLAN.BASE_IFACE: 'eth1'
            }
        }
    ]
})
```

### Change VLAN ID

TBD

### Delete VLAN

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceState

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'eth1.101',
            Interface.STATE: InterfaceState.ABSENT
        }
    ]
})
```

## Linux Bridge

### Create Linux Bridge

Create a linux bridge with two interfaces.

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceType
from libnmstate.schema import InterfaceState
from libnmstate.schema import LinuxBridge

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'br0',
            Interface.STATE: InterfaceState.UP,
            Interface.TYPE: LinuxBridge.TYPE,
            LinuxBridge.CONFIG_SUBTREE: {
                LinuxBridge.PORT_SUBTREE: [
                    {
                        LinuxBridge.PORT_NAME: 'eth1'
                    },
                    {
                        LinuxBridge.PORT_NAME: 'eth2'
                    }
                ]
            }
        }
    ]
})
```

### Add Interface to Linux Bridge

Add `eth3` to existing linux bridge.

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import LinuxBridge

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'br0',
            LinuxBridge.CONFIG_SUBTREE: {
                LinuxBridge.PORT_SUBTREE: [
                    {
                        LinuxBridge.PORT_NAME: 'eth1'
                    },
                    {
                        LinuxBridge.PORT_NAME: 'eth2'
                    },
                    {
                        LinuxBridge.PORT_NAME: 'eth3'
                    }
                ]
            }
        }
    ]
})
```

### Delete Linux Bridge

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceState

libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'br0',
            Interface.STATE: InterfaceState.ABSENT
        }
    ]
})
```

## Open vSwitch Bridge

### Create Open vSwitch Bridge

Create Open vSwitch bridge with two interfaces and one internal interface.

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceType
from libnmstate.schema import InterfaceState
from libnmstate.schema import OVSBridge
from libnmstate.schema import OVSBridgePortType


libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'ovs0-br0',
            Interface.STATE: InterfaceState.UP,
            Interface.TYPE: OVSBridge.TYPE,
            OVSBridge.CONFIG_SUBTREE: {
                OVSBridge.PORT_SUBTREE: [
                    {
                        OVSBridge.PORT_NAME: 'eth1',
                        OVSBridge.PORT_TYPE: OVSBridgePortType.SYSTEM,
                    },
                    {
                        OVSBridge.PORT_NAME: 'ovs0',
                        OVSBridge.PORT_TYPE: OVSBridgePortType.INTERNAL,
                    }
                ]
            }
        },
    ]
})
```

### Delete Open vSwitch Bridge

```python
import libnmstate
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceState


libnmstate.apply(
{
    Interface.KEY: [
        {
            Interface.NAME: 'ovs0-br0',
            Interface.STATE: InterfaceState.ABSENT
        }
    ]
})
```
