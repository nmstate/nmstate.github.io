<!-- vim-markdown-toc GFM -->

* [YAML examples for DNS use cases](#yaml-examples-for-dns-use-cases)
    * [Static DNS with auto IP interface](#static-dns-with-auto-ip-interface)
    * [Static DNS with static IP interface](#static-dns-with-static-ip-interface)
    * [Static DNS name servers append to DHCP/autoconf name servers](#static-dns-name-servers-append-to-dhcpautoconf-name-servers)
    * [Static DNS search option with DHCP/autoconf name servers](#static-dns-search-option-with-dhcpautoconf-name-servers)
    * [Static IPv6 link local name server](#static-ipv6-link-local-name-server)

<!-- vim-markdown-toc -->

# YAML examples for DNS use cases

## Static DNS with auto IP interface

Getting IP from DHCP or IPv6 autoconf/RA but use static DNS configurations.

```yml
---
dns-resolver:
  config:
    search:
    - example.com
    - example.org
    server:
    - 2001:db8:f::1
    - 192.0.2.251
interfaces:
  - name: eth1
    type: ethernet
    state: up
    ipv4:
      enabled: true
      dhcp: true
      auto-dns: false
    ipv6:
      enabled: true
      dhcp: true
      autoconf: true
      auto-dns: false
```

## Static DNS with static IP interface

```yml
---
dns-resolver:
  config:
    search:
    - example.com
    - example.org
    server:
    - 2001:4860:4860::8844
    - 192.0.2.251
interfaces:
  - name: eth1
    type: ethernet
    state: up
    ipv4:
      enabled: true
      dhcp: false
      address:
      - ip: 192.0.2.251
        prefix-length: 24
    ipv6:
      enabled: true
      dhcp: false
      autoconf: false
      address:
      - ip: 2001:db8:1::1
        prefix-length: 64
routes:
  config:
  - destination: 0.0.0.0/0
    next-hop-address: 192.0.2.1
    next-hop-interface: eth1
  - destination: ::/0
    next-hop-address: 2001:db8:1::3
    next-hop-interface: eth1
```

## Static DNS name servers append to DHCP/autoconf name servers

When using with dnsmasq or other DNS caching system, you might want `127.0.0.1`
been append to auto DNS learn from DHCP/autoconf.

```yml
---
dns-resolver:
  config:
    search:
    - example.com
    - example.org
    server:
    - 127.0.0.1
interfaces:
  - name: eth1
    type: ethernet
    state: up
    ipv4:
      enabled: true
      dhcp: true
      auto-dns: true
    ipv6:
      enabled: true
      dhcp: true
      autoconf: true
      auto-dns: true
```

## Static DNS search option with DHCP/autoconf name servers

```yml
---
dns-resolver:
  config:
    search:
      - example.com
      - example.org
interfaces:
  - name: eth1
    type: ethernet
    state: up
    ipv4:
      enabled: true
      dhcp: true
    ipv6:
      enabled: true
      dhcp: true
      autoconf: true
```

## Static IPv6 link local name server

```yml
---
dns-resolver:
  config:
    search:
    - example.com
    - example.org
    server:
    - fe80::deef:1%eth1
    - 2001:db8:f::1
    - 192.0.2.251
interfaces:
  - name: eth1
    type: ethernet
    state: up
    ipv4:
      enabled: true
      dhcp: false
      address:
      - ip: 192.0.2.251
        prefix-length: 24
    ipv6:
      enabled: true
      dhcp: false
      autoconf: false
      address:
      - ip: 2001:db8:1::1
        prefix-length: 64
routes:
  config:
  - destination: 0.0.0.0/0
    next-hop-address: 192.0.2.1
    next-hop-interface: eth1
  - destination: ::/0
    next-hop-address: 2001:db8:1::3
    next-hop-interface: eth1
```
