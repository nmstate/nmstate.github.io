## MultiPath TCP

MultiPath TCP(MPTCP support is introduced by nmstate 2.2.0 and requires
NetworkManager 1.40 or greater.

By assigning MPTCP flags to IP address, MPTCP enabled TCP connection could be
benefit from extra TCP subflows.
Nmstate could query out the MPTCP flags of each IP address found on specific
interface, but only support assigning the same MPTCP flags to all the IP
addresses of this interface.

To enable MPTCP use case, beside using nmstate to configure MPTCP flags on IP
address, you might also need(depend on your linux distributions kernel config),
these actions should be done before nmstate and persistent by yourself:

 * Enable global MPTCP switch: `sysctl -w net.mptcp.enabled=1`.
   RHEL/CentOS 9 by default is disabling MPTCP, hence placing a file in
   `/etc/sysctl.d/` could persistent it.

 * Increase MPTCP subflow limits: `ip mptcp limits set subflows 2`.
   RHEL/CentOS 9 by default is setting subflow limit to 2. Hence no action
   required.

 * Might require `sysctl -w net.ipv4.conf.<iface_name>.rp_filter=0`
   File in `/etc/sysctl.d/` could persistent it.

These commands is only for testing purpose, please check with MPTCP documents
and technical support for detail.

Example to add 'subflow' and 'signal' MPTCP flags to all IP of eth1.

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

Example on querying MPTCP of eth1:


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
 * Nmstate can query MPTCP flags per IP address. But setting MPTCP flags on
   IP address is not supported and will be ignored with a warning.

 * Nmstate will not apply MPTCP flags to link-local address, multicast
   address, loopback address, and IPv6 unicast local address.

### How to tell MPTCP is working.

 * Use `mptcpize run` to start the TCP server and client.
 * Use `tcpdump`, `tshark` or `wireshark` to capture the network flow.
   Seek for MPTCP subflows.
