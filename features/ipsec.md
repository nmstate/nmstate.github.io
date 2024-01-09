<!-- vim-markdown-toc GFM -->

* [IPsec x509/PKI authentication example](#ipsec-x509pki-authentication-example)
* [IPsec RSA authentication example](#ipsec-rsa-authentication-example)
* [IPsec PSK authentication example](#ipsec-psk-authentication-example)
* [IPSec Host-to-Host/P2P tunnel](#ipsec-host-to-hostp2p-tunnel)

<!-- vim-markdown-toc -->

# IPsec x509/PKI authentication example

```yml
---
interfaces:
- name: hosta_conn
  type: ipsec
  ipv4:
    enabled: true
    dhcp: true
  libreswan:
    ipsec-interface: "99"
    left: 192.0.2.251
    leftid: '%fromcert'
    leftcert: hosta.example.org
    right: 192.0.2.151
    rightid: '%fromcert'
    ikev2: insist
    ikelifetime: 24h
    salifetime: 24h
```

The PKI key should be imported by `ipsec import` command or other NSS tools.

# IPsec RSA authentication example


```yml
---
interfaces:
- name: hosta_conn
  type: ipsec
  ipv4:
    enabled: true
    dhcp: true
  libreswan:
    ipsec-interface: "99"
    leftrsasigkey: 0sAwEAAesFfVZqFzRA9F
    left: 192.0.2.250
    leftid: 'hosta-rsa.example.org'
    right: 192.0.2.150
    rightrsasigkey: 0sAwEAAesFfVZqFzRA9E
    rightid: 'hostb-rsa.example.org'
    ikev2: insist
```

The `rightrsasigkey` and `leftrsasigkey` could be retrieved by
`ipsec showhostkey --right --ckaid <CKAID>` command.

# IPsec PSK authentication example

```yml
---
interfaces:
  - name: hosta_conn
    type: ipsec
    ipv4:
      enabled: true
      dhcp: true
    libreswan:
      ipsec-interface: "99"
      right: 192.0.2.153
      rightid: 'hostb-psk.example.org'
      left: 192.0.2.250
      leftid: 'hosta-psk.example.org'
      psk: "JjyNzrnHTnMqzloKaMuq2uCfJvSSUqTYdAXqD2U2OCFyVIJUUEHmXihBbPrUcmik"
      ikev2: insist
```

The PSK method should be only used for test/develop purpose.

# IPSec Host-to-Host/P2P tunnel

By default, NetworkManager libreswan plugin is expecting client-server IPSec
tunnel. In order to get it works for P2P(Host-to-Host) IPSec tunnel, please
explicitly set `rightsubnet` to remote /32 IPv4 address and
`leftmodecfgclient: no`.

For example, assuming remote IPSec host IP is `192.0.2.155` and local IP is
`192.0.2.248`

```yml
interfaces:
- name: hosta_conn
  type: ipsec
  libreswan:
    left: 192.0.2.248
    leftid: 'hosta.example.org'
    leftcert: hosta.example.org
    leftmodecfgclient: no
    right: 192.0.2.155
    rightid: 'hostb.example.org'
    rightsubnet: 192.0.2.155/32
    ikev2: insist
```

This result in P2P policy been created in `ip xfrm`:

```bash
[fge@c9s ~]$ sudo ip xfrm policy
src 192.0.2.248/32 dst 192.0.2.155/32
    dir out priority 1753281 ptype main
    tmpl src 192.0.2.248 dst 192.0.2.155
        proto esp reqid 16389 mode tunnel
src 192.0.2.155/32 dst 192.0.2.248/32
    dir fwd priority 1753281 ptype main
    tmpl src 192.0.2.155 dst 192.0.2.248
        proto esp reqid 16389 mode tunnel
src 192.0.2.155/32 dst 192.0.2.248/32
    dir in priority 1753281 ptype main
    tmpl src 192.0.2.155 dst 192.0.2.248
        proto esp reqid 16389 mode tunnel
```
