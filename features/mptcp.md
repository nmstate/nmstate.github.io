## MultiPath TCP

MultiPath TCP (MPTCP) support is introduced by nmstate 2.2.0 and requires
NetworkManager 1.40 or greater.

By assigning MPTCP flags to IP addresses, MPTCP-enabled TCP connections can
benefit from extra TCP subflows.

Nmstate can query out the MPTCP flags of each IP address found on a specific
interface but only supports assigning the same MPTCP flags to all the IP
addresses of this interface.

To effectively enable MPTCP, besides using nmstate to configure MPTCP flags
on IP addresses, depending on your Linux distribution kernel configuration,
you might also need to perform these actions before using nmstate, and persist
these changes by yourself:

* Enable global MPTCP switch: `sysctl -w net.mptcp.enabled=1`.
  RHEL/CentOS 9 by default is disabling MPTCP. Hence, placing a file in
  `/etc/sysctl.d/` could persist it.

* Increase MPTCP subflow limits: `ip mptcp limits set subflows 2`.
  RHEL/CentOS 9 by default is setting subflow limit to 2. Hence, no action
  is required.

* Might require `sysctl -w net.ipv4.conf.<iface_name>.rp_filter=0`
  Hence, placing a file in `/etc/sysctl.d/` could persist it.

These commands are only for testing purpose, please check with MPTCP documents
and technical support for details.

Example to add 'subflow' and 'signal' MPTCP flags to all IP addresses of eth1.

```yaml
---
interfaces:
  - name: eth1
    type: ethernet
    state: up
    mptcp:
      address-flags:
        - subflow
        - signal
    ipv4:
      enabled: true
      dhcp: false
      address:
        - ip: 192.0.2.2
          prefix-length: 24
```

Example of querying MPTCP of eth1:


```yaml
---
interfaces:
  - name: eth1
    type: ethernet
    state: up
    mptcp:
      address-flags:
        - subflow
        - signal
    ipv4:
      enabled: true
      dhcp: false
      address:
        - ip: 192.0.2.2
          prefix-length: 24
          mptcp-flags:
            - subflow
            - signal
```


### Limitations

* Nmstate can query MPTCP flags per IP address. However, setting MPTCP flags
  on IP addresses is not supported and will be ignored with a warning.

* Nmstate will not apply MPTCP flags to link-local addresses, multicast
  addresses, loopback addresses, and IPv6 unicast local addresses.

### How to tell MPTCP is working.

* Use `mptcpize run` to start the TCP server and client.
* Use `tcpdump, `tshark`, or `wireshark` to capture the network flow.
  Seek for MPTCP subflows.
