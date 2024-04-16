# Quick start guide

## Install

This guide is helpful to install Nmstate from source, for installing stable
release or other installation methods, pleaser refer to [Nmstate installation
guide](./install.html)

### Dependencies

Nmstate requires NetworkManager 1.26 or later version installed and started.

In order to support specific capabilities, additional packages are required:
- OpenVSwitch support through the NM provider requires `NetworkManager-ovs`.

- Extended ovsdb support requires `openvswitch-2.11` and
`python3-openvswitch2.11` or greater.

- Team interface support through the NM provider requires `NetworkManager-team`

### Install from source

```bash
git clone https://github.com/nmstate/nmstate.git
cd nmstate
PREFIX=/usr make install
```

## View state

There are two ways of using Nmstate. The first one is using nmstatectl command
line tool and the other one is using the libnmstate python library.

### Using nmstatectl

The following command will dump the current networking state in yaml format:

`nmstatectl show`

```yaml
interfaces:
- description: Production Network
  ethernet:
    auto-negotiation: true
    duplex: full
    speed: 1000
  ipv4:
    address:
    - ip: 192.0.2.142
      prefix-length: 24
    enabled: true
  mtu: 1500
  name: eth3
  state: up
  type: ethernet
```

To dump the state in json format, use '--json' flag:

`nmstatectl show --json`

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

### Using libnmstate

The following script prints the current networking state:

```python
import libnmstate
import json

current_state = libnmstate.show()
print(json.dumps(current_state, indent=4))
```

And generates the following output:

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

## Set a state

### Using nmstatectl

- Use `nmstatectl show > desired_state.yaml` to dump the current state to a
file.
- Edit the desired state with the new values.
- Run the set command: `nmstatectl apply desired_state.yaml`

### Using libnmstate

The following script modifies the state of the eth3 interface.

```python
import libnmstate

state = libnmstate.show()
eth3_state = next(ifstate for ifstate in state['interfaces'] if ifstate['name'] == 'eth3')

# take eth3 down
eth3_state['state'] = 'down'
libnmstate.apply(state)
```
