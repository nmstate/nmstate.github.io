# Varlink support for libnmstate

## Introduction
This documentation details the varlink protocol support for libnmstate functions. Varlink protocol encodes all the messages as JSON objects and communicates via unix and tcp socket connections. Using the varlink, libnmstate is accessible via varlink stdin/out and varlink client implementation. This documentation uses python
programming language for examples, please refer [language binding document](ttps://varlink.org/Language-Bindings#how-to-test-new-language-bindings) for other existing varlink programming languages binding. Libnmstate service is defined as `io.nmstate` in varlink interface.

* [Varlink documentation]()
* [libnmstate API documentation](https://www.nmstate.io/devel/api.html)
* [Github Repository](https://github.com/nmstate/nmstate/tree/master/libnmstate/ifaces)

## Installation of varlink
Pypi:
```bash
$ pip3 install -u "varlink>=29.0.0"
```
Dnf:
```bash
$ sudo dnf install python3-varlink
```

## Varlink for service information
To view information about io.nmstate service.

```bash
$ varlink info unix:/run/nmstate.so

Vendor: Red Hat
Product: Nmstate
Version: 0.3.0
URL: https://www.nmstate.io
Interfaces:
  org.varlink.service
  io.nmstate
```
To view the varlink `io.nmstate` interface definition via stdin/out

```bash
$ varlink help unix:/run/nmstate.so/io.nmstate

# Interface definition
interface io.nmstate

# Types definition
type Logs (
  time: string,
  level: string,
  message: string
)

# Method definition
method Show(arguments: [string]object) -> (state: ?object, log: []Logs)

method Apply(arguments: [string]object) -> (log: []Logs)

method Commit(arguments: [string]object) -> (log: []Logs)

method Rollback(arguments: [string]object) -> (log: []Logs)

# Errors definition
error NmstateValueError (error_message: string, log: []Logs)

error NmstatePermissionError (error_message: string, log: []Logs)
...
```

## libnmstate basic operations using varlink
The basic functions from libnmstate (show, apply, commit and rollback) are called via varlink interface. Passing arguments to the functions only supports JSON format and parameters should be passed json object under ```arguments``` key. Below are the examples for each basic functions using varlink stdin/out and varlink client.

* libnmstate show function (query network state)

Varlink stdin/out:
```bash
$ varlink call unix:/run/nmstate.so/io.nmstate.Show
```

Varlink python client:
```python
import varlink

with varlink.Client("unix:/run/nmstate.so").open("io.nmstate", namespaced=False) as con:
    con.Show()
```

JSON output:
 libnmstate current network is reported under "state" object.
```json
{
  "log": [
    {
      "level": "DEBUG",
      "message": "Async action: Retrieve applied config: foo started",
      "time": "2020-08-05 10:22:19"
    },
    {
      "level": "DEBUG",
      "message": "Async action: Retrieve applied config: foo finished",
      "time": "2020-08-05 10:22:19"
    }
  ],
  "state": {
    "dns-resolver": {
      "config": {
        "search": [],
        "server": []
      },
      "running": {}
    },
    "interfaces": [
      {
        "ipv4": {
          "enabled": false
        },
        "ipv6": {
          "enabled": false
        },
        "lldp": {
          "enabled": false
        },
        "mac-address": "36:66:98:1D:6A:C8",
        "mtu": 1500,
        "name": "eth0",
        "state": "down",
        "type": "ethernet"
      },
    ],
    "route-rules": {
      "config": []
    },
    "routes": {
      "config": [],
      "running": []
    }
  }
}```

* libnmstate apply function (query network state)

Varlink stdin/out:
```bash
$ varlink call unix:/run/nmstate.so/io.nmstate.Apply '{"arguments": {"desired_state": {"interfaces": [{"name": "foo", "type": "dummy", "state": "up", "ipv4": {"enabled": false}, "ipv6": {"enabled": false}}]} } }'
```

* When using varlink client it is not required specify hte "argument" parameter.
Varlink python client:
```python
import varlink

state = {'desired_state': {'interfaces': [{'name': 'foo', 'type': 'dummy', 'state': 'up', 'ipv4': {'enabled': False}, 'ipv6': {'enabled': False}}]} }

with varlink.Client("unix:/run/nmstate.so").open("io.nmstate") as nmstate:
    nmstate.Apply(state)
```

* libnmstate commit function (commit transaction)

Varlink stdin/out:
```bash
$ varlink call unix:@nmstate/io.nmstate.Commit '{"checkpoint": <checkpoint> }'
```

Varlink python client:
```python
import varlink

checkpoint = {"checkpoint": "<checkpoint_path>"}
with varlink.Client("unix:/run/nmstate.so").open("io.nmstate", namespaced=False) as con:
    con.Commit(checkpoint)
```

* libnmstate rollback function (rollback transaction)

Varlink stdin/out:
```bash
$ varlink call unix:@nmstate/io.nmstate.Rollback '{"checkpoint": "<checkpoint_path>" }'
```

Varlink python client:
```python
import varlink

checkpoint = {"checkpoint": "<checkpoint_path>" }
with varlink.Client("unix:/run/nmstate.so").open("io.nmstate", namespaced=False) as con:
    con.Rollback(checkpoint_path)
```

## Error response
All error nmstate messages are encoded as JSON object format. Errors are identified with specified varlink interface and include the error message and log. Following examples shows the format of the error raised via varlink stdin/out and varlink client.

Varlink stdin/out:
```bash
Call failed with error: NmstateValueError
{
  "error_message": "No checkpoint specified or found",
  "log": [
    {
      "level": "ERROR",
      "message": "No checkpoint specified or found",
      "name": "root",
      "time": "2020-07-31 15:16:22"
    }
  ]
}
```

Varlink python client:
```python
varlink.error.VarlinkError: {
    'error': 'NmstateValueError',
    'parameters': {
        'error_message': 'No checkpoint specified or found',
        'log': [
            {
                'time': '2020-07-31 15:18:06',
                'level': 'ERROR',
                'message': 'No checkpoint specified or found'
            }
        ]
    }
}
```