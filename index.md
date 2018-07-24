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

Visit the [demos page](./demos.md) and the [examples page](./examples.md)
to see what it can do.

## Quick Start Guide
### Install
In the first few development cycles, it is recomended to install nmstate from
sources.
```shell
git clone https://github.com/nmstate/nmstate.git
cd nmstate
pip install --user --upgrade .
```

Note: In order to report current state, installing nmstate as a non-root user
should be enough. For change support, install the package as root.

### View state

The following command will dump the current networking state in json format:

`nmstatectl show`

```
{
    "interfaces": [
        {
            "ipv4": {
                "addresses": [
                    {
                        "ip": "192.168.122.1",
                        "prefix-length": 24
                    }
                ],
                "enabled": true
            },
            "name": "eth0",
            "state": "up",
            "type": "ethernet"
        }
    ]
}
```

To dump the state in yaml format, use '--yaml' flag:

`nmstatectl show --yaml`

```
- ipv4:
    addresses:
    - ip: 192.168.122.1
      prefix-length: 24
    enabled: true
  name: eth0
  state: up
  type: ethernet
```

### Change state

- Use `nmstatectl show > desired_state.json` to dump the current state to a
file.
- Edit the desired state with the new values.
- Run the set command: `nmstatectl set desired_state.json`

## Demos
Want to see nmstate in action? Check out the [demos](./demos.md).

## Supported Interfaces:
- bond
- dummy
- ethernet

## Stack support:
- IPv4 addresses

