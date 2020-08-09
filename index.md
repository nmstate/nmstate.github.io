# We are NMState!
A declarative network manager API for hosts.

## What is it?
NMState is a library with an accompanying command line tool that manages
host networking settings in a declarative manner.
The networking state is described by a pre-defined schema.
Reporting of current state and changes to it (desired state) both conform to
the schema.

NMState is aimed to satisfy enterprise needs to manage host networking through
a northbound declarative API and multi provider support on the southbound.
NetworkManager acts as the main (and currently the only) provider supported.

Visit the [examples page](./examples.md) to see what it can do.

## Features
- [ethernet](./examples.md#interfaces-ethernet)
- [bond](./examples.md#interfaces-bond)
- [Vlan](./examples.md#interfaces-vlan)
- [Vxlan](./examples.md#interfaces-vxlan)
- [Linux bridge](./examples.md#interface-linux-bridge)
- [ovs-bridge](./examples.md#interfaces-ovs-bridge)
- [dummy](./examples.md#interfaces-dummy)
- [team](./examples.md#interfaces-team)
- [IPv4/IPv6 addresses (static)](./examples.md#interfaces-ethernet)
- [IPv4/IPv6 route](./examples.md#route)
- [IPv4/IPv6 DNS client configuration](./examples.md#dns)
- [LLDP](./features/lldp.md)
- [Open vSwitch Patch Port](./features/ovs_patch.md)
- [Open vSwitch database plugin](./features/ovsdb.md)

## Example output

```json
{
    "interfaces": [
        {
            "description": "Production Network",
            "ethernet": {
                "auto-negotiation": true,
                "duplex": "full",
                "speed": 1000
            },
            "ipv4": {
                "address": [
                    {
                        "ip": "192.0.2.142",
                        "prefix-length": 24
                    }
                ],
                "enabled": true
            },
            "mtu": 1500,
            "name": "eth3",
            "state": "up",
            "type": "ethernet"
        }
    ]
}
```

## Documentation
- [Nmstate Quick Start Guide](./user/quick_guide.md)
- [Nmstate Developer Guide](./devel/dev_guide.md)
- [Design](./devel/design/networking-api.md)
- [API documentation](./devel/api.md)
- [Plugin Design](./devel/plugin.md)

## Contacts
- You can find us on #nmstate IRC channel on freenode. The maintainers are
  Gris, edwardh and ffmancera, please feel free to ask whatever you need!

- Nmstate uses the nmstate-devel@lists.fedorahosted.org for discussions. To
  subscribe you can send an email with 'subscribe' in the subject to
  nmstate-devel-join@lists.fedorahosted.org or visit the [mailing list
page](https://lists.fedorahosted.org/admin/lists/nmstate-devel.lists.fedorahosted.org).

- You can also report any issue on our [official Github repository](https://github.com/nmstate/nmstate)
