<!-- vim-markdown-toc GFM -->

* [The nmstatectl User Guide](#the-nmstatectl-user-guide)
    * [Query Network State](#query-network-state)
    * [Change Network State](#change-network-state)
    * [Manual Transaction Control](#manual-transaction-control)

<!-- vim-markdown-toc -->

# The nmstatectl User Guide

The nmstatectl is created for users who want to try out nmstate without using
[`libnmstate`][api_doc].

For detail information about network state format and definition, please refer
to the [API document][api_doc].

## Query Network State

To query current network state, please use the following command:

```
nmstatectl show [<interface_name>]
```

[YAML][yaml] is the default output format. Use the `--json` argument to change
the output format to [JSON][json].

To limit the output state to include certain interfaces only, please specify
the interface name as `nmstatectl show <interface_names>`. Please be advised,
global config like DNS will be included.

For multiple interface names, use comma to separate them. You can also use
regular expression patterns for interface names:

```text
*       matches everything
?       matches any single character
[seq]   matches any character in seq
[!seq]  matches any char not in seq
```

For example, to show all interfaces starts with `eth`:

```bash
nmstatectl show eth\*
# The backslash is required to stop shell expanding '*' to file names.
```

## Change Network State

The network state may be changed interactively or by file:

    * Interactive: `nmstatectl edit [<interface_names>]`
      The `nmstatectl` will invoke the text editor defined by environment
      variable `$EDITOR` for editing the network state in [YAML][yaml] format.
      Once the text editor quit, `nmstatectl` will try to apply it using
      `nmstatectl set`.
      If there is any syntax error, you will be asked to edit again.
      Multiple interfaces are supported, check [`nmstatctl show`][ncl_show]
      for detail.

    * File-based: `nmstatectl set <state_file_path>`
      `nmstatectl set` apply the network state from specifed file in YAML
      or JSON format.

By default, if the network state after state applied is not identical to the
desired state, `nmstatectl` rollbacks to the state before `edit`/`set` command.
Use the `--no-verify` argument to skip the verification.

## Manual Transaction Control

The `nmstatectl` supports manual transaction control which allows user to
decide whether rollback to previous(before `nmstatectl set`) state.

This command will create a checkpoint which later could be used for rollback
or commit.

```bash
nmstatectl set --no-commit [--timeout <TIMEOUT>]
```

The checkpoint string will be the last line of `nmstatectl` output, example:
`/org/freedesktop/NetworkManager/Checkpoint/1`.

The network state will be applied to OS but allow user to decide whether
rollback or commit.

To rollback: `nmstatectl rollback <checkpoint_path>`.

To commit: `nmstatectl commit <checkpoint_path>`.

The user must commit the changes within `TIMEOUT`(default is 60 seconds), or
they will be automatically rolled back.

[api_doc]: ./devel/api.md
[yaml]: https://yaml.org/
[json]: https://www.json.org/
[ncl_show]: #query-network-state
