<!-- vim-markdown-toc GFM -->

* [Nmstate Plugin Interface](#nmstate-plugin-interface)
    * [How to Install and Load the Plugin](#how-to-install-and-load-the-plugin)
    * [Plugin example:](#plugin-example)
    * [Plugin Priority](#plugin-priority)
    * [Plugin Capabilities](#plugin-capabilities)
    * [Limitations](#limitations)

<!-- vim-markdown-toc -->

## Nmstate Plugin Interface

The nmstate-0.3.2 release brings the plugin support which allows user to
managing the network via their own plugin.

Since nmstate-0.4, the nispor project is used for kernel network information
query, plugin should fill the user space informations only and perform.

### How to Install and Load the Plugin

If `NMSTATE_PLUGIN_DIR` is defined as system environment variable in the
runtime of nmstate, nmstate will load plugin form that folder after build-in
plugins been loaded.

If `NMSTATE_PLUGIN_DIR` is not define, nmstate will use the 'plugins' folder
where `libnmstate` python module is installed. For example:
`/usr/lib/python3.6/site-packages/libnmstate/plugins/`

The plugin file should be named with prefix `nmstate_plugin_` and end with
`.py`. For example, the OVS DB plugin ins `nmstate_plugin_ovsdb.py`

If any non-build-in plugin failed to loaded, there will not __no__ `Exception`
raised, but a warning message.

### Plugin example:

You may take the [OVS DB plugin][1] as example or below:

```python
from libnmstate.plugin import NmstatePlugin
from libnmstate.schema import Interface
from libnmstate.schema import InterfaceType
from libnmstate.schema import InterfaceState

class FooPlugin(NmstatePlugin):
    @property
    def name(self):
        return "foo_plugin"

    @property
    def priority(self):
        return NmstatePlugin.DEFAULT_PRIORITY + 1

    @property
    def plugin_capabilities(self):
        return [
            NmstatePlugin.PLUGIN_CAPABILITY_IFACE,
        ]

    def get_interfaces(self):
        return [
            {
                Interface.NAME: "foo1",
                Interface.TYPE: InterfaceType.OTHER,
                Interface.STATE: InterfaceState.UP,
                "foo": {"a": 1, "b": 2},
            }
        ]
    def apply_changes(self, net_state, save_to_disk):
        pass

    def create_checkpoint(self, timeout=60):
        return "string_for_checkpoint"

    def rollback_checkpoint(self, checkpoint=None):
        pass

    def destroy_checkpoint(self, checkpoint=None):
        pass

    @property
    def is_supplemental_only(self):
        return True  # If your plugin does not introduce user space interfaces


NMSTATE_PLUGIN = FooPlugin
```

### Plugin Priority

The bigger number of `NmstatePlugin.priority` property is, the higher priority
the plugin will be.
 * When multiple plugins are providing the information for the same interface,
   their data will be merged, and for data conflict, data from higher priority
   plugin will be used.

 * The apply and checkpoint actions of each loaded plugins are invoked in the
   order of priority from low to high.

 * All routes provided by plugin with
   `NmstatePlugin.PLUGIN_CAPABILITY_ROUTE` capability will be included on
   showing. Any plugin want to handle route modification should do it in
   `apply_changes()` function.

 * Only the highest priority plugin with
   `NmstatePlugin.PLUGIN_CAPABILITY_DNS` capability will be used for
   DNS querying and modifying.

 * Only the highest priority plugin with
   `NmstatePlugin.PLUGIN_CAPABILITY_ROUTE_RULE` capability will be used for
   route rule querying and modifying.

### Plugin Capabilities

The `NmstatePlugin.plugin_capabilities()` function returns a list of constants
indicating the ability of this plugin:
 * `NmstatePlugin.PLUGIN_CAPABILITY_IFACE`
   Implemented `NmstatePlugin.get_interfaces()`

 * `NmstatePlugin.PLUGIN_CAPABILITY_ROUTE`
   Implemented `NmstatePlugin.get_routes()`

 * `NmstatePlugin.PLUGIN_CAPABILITY_ROUTE_RULE`
   Implemented `NmstatePlugin.get_route_rules()`

 * `NmstatePlugin.PLUGIN_CAPABILITY_DNS`
   Implemented `NmstatePlugin.get_dns_client_config()`

### Limitations

 * For any interface type not defined in current schema,
   please use `InterfaceType.OTHER`/`"other"`.

[1]: https://github.com/nmstate/nmstate/blob/base/libnmstate/plugins/nmstate_plugin_ovsdb.py
