<!-- vim-markdown-toc GFM -->

* [Link Layer Discovery Protocol (LLDP)](#link-layer-discovery-protocol-lldp)
    * [Simulate a LLDP package](#simulate-a-lldp-package)
    * [Enable LLDP](#enable-lldp)
* [Query LLDP status](#query-lldp-status)

<!-- vim-markdown-toc -->

## Link Layer Discovery Protocol (LLDP)

LLDP support is introduced by nmstate-0.3.2 using NetworkManager backend.

Once LLDP is enabled, the NetworkManager will listen on LLDP packages, there is
no need to run other LLDP agent daemon like `lldpad`.

### Simulate a LLDP package

We stored a captured LLDP package from real LLDP enabled switch and use
`tcpreply` to sent to veth interface.

```bash
sudo ip link add eth1 type veth peer name eth1peer
sudo ip link set eth1 up
sudo ip link set eth1peer up
sudo nmcli device set eth1 managed yes
cat | sudo nmstatectl apply - << EOL
interfaces:
  - name: eth1
    type: ethernet
    lldp:
      enabled: true
EOL

URL_BASE="https://github.com/nmstate/nmstate"

wget ${URL_BASE}/raw/base/tests/integration/test_captures/lldp.pcap \
    -O /tmp/lldp.pcap
sudo tcpreplay --intf1=eth1peer /tmp/lldp.pcap
```

### Enable LLDP

```bash
cat | sudo nmstatectl apply - << EOL
interfaces:
  - name: eth1
    type: ethernet
    lldp:
      enabled: true
EOL

```

or

```python
import libnmstate
from libnmstate.schmea import Interface
from libnmstate.schmea import InterfaceType
from libnmstate.schmea import LLDP

libnmstate.apply(
    {
        Interface.KEY: [
            {
                Interface.NAME: "eth1",
                Interface.TYPE: InterfaceType.ETHERNET,
                LLDP.CONFIG_SUBTREE: {LLDP.ENABLED: True},
            }
        ]
    }
)
```

## Query LLDP status

Once NetworkManager received LLDP packages, the `libnmstate.show()` or
`nmstatectl.show` can query the LLDP information. Example:

```yml
interfaces:
- name: eth1
  type: ethernet
  state: up
  ipv4:
    enabled: false
    dhcp: false
  ipv6:
    enabled: false
    autoconf: false
    dhcp: false
  lldp:
    enabled: true
    neighbors:
    - - type: 5
        system-name: Summit300-48
      - type: 6
        system-description: Summit300-48 - Version 7.4e.1 (Build 5)
          05/27/05 04:53:11
      - type: 7
        system-capabilities:
        - MAC Bridge component
        - Router
      - type: 1
        _description: MAC address
        chassis-id: 00:01:30:F9:AD:A0
        chassis-id-type: 4
      - type: 2
        _description: Interface name
        port-id: 1/1
        port-id-type: 5
      - type: 127
        ieee-802-1-vlans:
        - name: v2-0488-03-0505
          vid: 488
        oui: 00:80:c2
        subtype: 3
      - type: 127
        ieee-802-3-mac-phy-conf:
          autoneg: true
          operational-mau-type: 16
          pmd-autoneg-cap: 27648
        oui: 00:12:0f
        subtype: 1
      - type: 127
        ieee-802-1-ppvids:
        - 0
        oui: 00:80:c2
        subtype: 2
      - type: 8
        management-addresses:
        - address: 00:01:30:F9:AD:A0
          address-subtype: MAC
          interface-number: 1001
          interface-number-subtype: 2
      - type: 127
        ieee-802-3-max-frame-size: 1522
        oui: 00:12:0f
        subtype: 4
  mac-address: 82:75:BE:6F:8C:7A
  mtu: 1500
```
