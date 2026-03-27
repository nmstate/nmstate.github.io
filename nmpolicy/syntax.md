# Nmpolicy Syntax

The nmpolicy contains the following fields:
* `capture`: Contains some simple expressions
   to evaluate the current network configuration and store the result at
   variables.
* `desired`: You also use `desiredState`. Contains network state yaml with
  references to `capture` entries.

Example on changing MTU of gateway interface to 1280.
```yaml
capture:
  gw: routes.running.destination=="0.0.0.0/0"
  gw-iface: interfaces.name==capture.gw.routes.running.0.next-hop-interface
desired:
  interfaces:
  - name: "{{ capture.gw-iface.interfaces.0.name }}"
    type: ethernet
    state: up
    mtu: 1280
```

## Capture syntax

The capture is a map of capture entries with an identifier as the key that can
be referenced with `capture.[id]`

The map value is a capture entry expression that evaluates the current
network state and stores the result.

The following is a semi-formal definition of the capture entry expression:

```
<letter> ::= "A" | "B" | "C" | "D" | "E" | "F" | "G"
       | "H" | "I" | "J" | "K" | "L" | "M" | "N"
       | "O" | "P" | "Q" | "R" | "S" | "T" | "U"
       | "V" | "W" | "X" | "Y" | "Z" | "a" | "b"
       | "c" | "d" | "e" | "f" | "g" | "h" | "i"
       | "j" | "k" | "l" | "m" | "n" | "o" | "p"
       | "q" | "r" | "s" | "t" | "u" | "v" | "w"
       | "x" | "y" | "z"
<digit> ::= [0-9]
<number> ::= <digit>+
<identity> ::= <letter> ( <digit> | "-" | <letter> )*
<boolean> ::= "true" | "false"
<dot> ::= "."
<path> ::= <identity> ( <dot> ( <identity> | <number> ))*
<string> ::= \" (<all characters>)* \"

<captureid> ::= <identity>
<capturepath> ::= "capture" <dot> <captureid> <path>
<eqoperator> ::= "=="
<eqexpression> ::= <path> <eqoperator> (<string> | <number> | <boolean> | <capturepath>)
<replaceoperator> ::= ":="
<replaceexpression> ::= <path> <replaceoperator> (<string> | <number> | <boolean> | <capturepath>)
<pathexpression> ::= <path>
<expression> ::= <pathexpression> | <eqexpression> | <replaceexpression>
<pipe> ::= "|"
<pipedexpression> ::= <capturepath> <pipe> <expression>
```

### Path `<path>`
The path expression contains different "steps" separated by dots, each "step"
can be a key from a map or the index starting with 0 from a list.

```
interfaces.name
routes.running.0.next-hop-interface
```

To reference a capture entry from a path the reserved word `capture` has to be
used followed by a dot and the capture entry name:

```
capture.default-gw.routes.running.0
capture.primary-nic.interfaces.0.name
```

### Equality filter `<eqexpression>`
Filter the current state based on specific state values.
The filter follows a simple syntax, similar to jsonpath.
Values may be explicit or appear as references to other expressions.
The resulting output is a full NMState state containing only the filtered
values.

```
interfaces.name == "eth1"
routes.destination == "0.0.0.0/0"
dns.server == "192.168.1.1"
interfaces.name == capture.default-gw.interfaces.0.name
```

### Path filter `<pathexpression>`
Filter out current state to include only the data matching the ```<path>```

Following is a path filter to filter out everything except the running DNS
configuration:
```
dns-resolver.running
```

This will create a capture entry with the the following Nmstate
```yaml
dns-resolver:
  running:
    search:
    - redhat.com
    server:
    - 8.8.8.8
```

This can be referened later on with:

```
"{{ capture.[capture name].dns-resolver.running }}"
```

### Replace `<replaceexpression>`
These commands can replace values from the specified fields
at the input NMState and they can reference other capture entries.

```
routes.running.next-hop-interface := "br1"
```

### Pipe `<pipexpression>`
When expressions are piped the output from the left expression is passed
to the input of the right command.

```
capture.base-iface-routes | routes.running.next-hop-interface := "br1"
```

## Desired state syntax

The state follows [NMState](https://nmstate.io/examples.html) syntax and will
include optionally references to capture entries so they can be expanded
in-place.
capture references have to be enclosed between `"{{` and `}}"` expressions, the
`desired` field can be expressed using JSON or YAML.

The only supported expressions are capture entry reference path like the
following

```
capture.base-iface.interfaces.0.mac-address
capture.base-iface.interfaces.0.name
```

For example to override the routes config from a capture `new-routes`
the following can be specified

```yaml
routes:
  config: "{{ capture.new-routes.running }}"
```

Or clone the mac-address from a capture entry `primary-nic`
```yaml
interfaces:
- name: br1
  type: linux-bridge
  state: up
  mac-address: "{{ capture.primary-nic.interfaces.0.mac-address }}"
```
