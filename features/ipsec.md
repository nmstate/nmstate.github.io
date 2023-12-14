<!-- vim-markdown-toc GFM -->

* [IPsec x509/PKI authentication example](#ipsec-x509pki-authentication-example)
* [IPsec RSA authentication example](#ipsec-rsa-authentication-example)
* [IPsec PSK authentication example](#ipsec-psk-authentication-example)

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
