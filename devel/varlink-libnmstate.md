# Varlink support for libnmstate

## Introduction
This documentation details the varlink protocol support for libnmstate functions. Varlink protocol encodes all the messages as JSON objects and communicates via unix and tcp socket connections. Using the varlink, libnmstate is accessible via varlink stdin/out and varlink client implementation. This documentation uses python
programming language for examples, please refer [language binding document](ttps://varlink.org/Language-Bindings#how-to-test-new-language-bindings) for other existing varlink programming languages binding. Libnmstate service is defined as `io.nmstate` in varlink interface address and functions should be called as specified in varlink interface defination `io.nmstate`.

* [Varlink documentation]()
* [libnmstate API documentation](https://www.nmstate.io/devel/api.html)
* [Github Repository](https://github.com/nmstate/nmstate/tree/master/libnmstate/ifaces)

## Installation of varlink
Pypi:
```
$ pip3 install -u varlink
```
Dnf:
```
$ sudo dnf install python3-varlink
```

## Varlink for service information

To view information about io.nmstate service.

```
$ varlink info unix:@nmstate

Vendor: Red Hat
Product: Nmstate
Version: 0.3.0
URL: https://www.nmstate.io
Interfaces:
  org.varlink.service
  io.nmstate
```
To view the varlink `io.nmstate` interface definition via stdin/out

```
$ varlink help unix:@nmstate/io.nmstate

# Interface definition
interface io.nmstate

# Method definition
method Show(include_status_data: ?bool) -> (state: ?object)

method Apply(
  state: object,
  verify_change: ?bool,
  commit: ?bool,
  rollback_timeout: ?int
) -> (response: ?object)

method Commit(checkpoint: ?string) -> (response: ?object)

method Rollback(checkpoint: ?string) -> (response: ?object)

# Error definition
error NmstateValueError (reason: object)

error NmstatePermissionError (reason: object)

error NmstateConflictError (reason: object)
...
```
## libnmstate basic operations using varlink
The basic functions from libnmstate (show, apply, commit and rollback) are called via varlink interface. Passing arguments to the functions only supports JSON format. Below are the examples for each basic functions using varlink stdin/out and varlink client.

* libnmstate show function (query network state)

Varlink stdin/out:
```varlink_stdin/out
$ varlink call unix:@nmstate/io.nmstate.Show
```
Varlink python client:
```varlink client
import varlink

with varlink.Client("unix:@nmstate")
    .open("io.nmstate", namespaced=False) as con:
    con._call("Show")
```
JSON output:
 libnmstate current network is reported under "state" object.
```
{
    "state":{
        {
            "dns-resolver": {
                "config": {
                    "search": [],
                    "server": []
                },
                "running": {}
            },
            "route-rules": {
                "config": []
            },
            "routes": {
                "config": [],
                "running": []
            },
            "interfaces": [
                {
                    "name": "eth0",
                    "type": "ethernet",
                    "state": "down",
                    "ipv4": {
                        "enabled": false
                    },
                    "ipv6": {
                        "enabled": false
                    }
                },
            ]
        }
    }
}
```

* libnmstate apply function (query network state)

Varlink stdin/out:
```varlink_stdin/out
$ varlink call unix:@nmstate/io.nmstate.Apply '{"state": {"interfaces": [{"name": "foo", "state": "up"}]}}'
```
Varlink python client:
```varlink_client
import varlink

desired_state = {"state": {"interfaces": [{"name": "foo", "state": "up"}]}}
with varlink.Client("unix:@nmstate")
    .open("io.nmstate", namespaced=False) as con:
    con._call("Apply", desired_state)
```
* libnmstate commit function (commit transaction)

Varlink stdin/out:
```varlink_stdin/out
$ varlink call unix:@nmstate/io.nmstate.Commit '{"checkpoint": <checkpoint> }'
```
Varlink python client:
```varlink_client
import varlink

checkpoint = {"checkpoint": <checkpoint>}
with varlink.Client("unix:@nmstate")
    .open("io.nmstate", namespaced=False) as con:
    con._call("Commit", checkpoint)
```
* libnmstate rollback function (rollback transaction)

Varlink stdin/out:
```varlink_stdin/out
$ varlink call unix:@nmstate/io.nmstate.Rollback '{"checkpoint": <checkpoint> }'
```
Varlink python client:
```varlink_client
import varlink

checkpoint = {"checkpoint": <checkpoint>}
with varlink.Client("unix:@nmstate")
    .open("io.nmstate", namespaced=False) as con:
    con._call("Rollback", checkpoint)
```

## Error response
All error nmstate messages are encoded as JSON object format. Errors are identified with specified varlink interface description. Following examples shows the format of the error raised via varlink stdin/out and varlink client.

Varlink stdin/out:
```varlink_stdin/out
Call failed with error: io.nmstate.NmstateValueError
{
  "reason": "No checkpoint specified or found"
}
```
Varlink python client:
```varlink_client
{
    'error': 'io.nmstate.NmstateValueError',
    'parameters': {
        'reason': 'No checkpoint specified or found'
    }
}
```
