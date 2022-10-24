## Conditional Compiling

In order to support generating offline configuration on MacOS, we introduced
conditional compiling since nmstate 2.2.0.

By default, nmstate is compiled with all features enabled:
 * `query_apply` -- For querying and applying network state, only for linux
   platform

 * `gen_conf` -- For generating offline configurations. Both Linux and MacOS
   are supported.

To compile only the `gen_conf` feature:

```bash
cd rust && cargo build --no-default-features --features gen_conf
```

You may also download pre-compiled `nmstatectl` binary at
[Nmstate release page][release_url].


[release_url]: https://github.com/nmstate/nmstate/releases
