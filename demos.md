# Demos
At the end of each development sprint, a demo is presented describing
new capabilities of nmstate.

## Demo-1: Link State
Date: 23 May 2018

List all links on the node and control the links (admin) state.

[![asciicast](https://asciinema.org/a/KglHfhCVn1ISWBON5PetD9BAm.png)](https://asciinema.org/a/KglHfhCVn1ISWBON5PetD9BAm)


## Demo-2: Bonds and Static IPv4
Date: 13 Jun 2018

Create a bond over several ports and define an IPv4 static address.

[![asciicast](https://asciinema.org/a/xZNpB6Pl7Vo0yyDlSpAduYEuE.png)](https://asciinema.org/a/xZNpB6Pl7Vo0yyDlSpAduYEuE)


Create a bond interface and change their members with Ansible.

[![asciicast](https://asciinema.org/a/Ny2rkdpHJaZGnBWK8o3og5sGX.png)](https://asciinema.org/a/Ny2rkdpHJaZGnBWK8o3og5sGX)


## Demo-3: OVS Bridge
Date: 5 Jul 2018

Create an OVS bridge and connect a system interface to it.

[![asciicast](https://asciinema.org/a/bv2g1RHADTjokXH4uYFljpNC6.png)](https://asciinema.org/a/bv2g1RHADTjokXH4uYFljpNC6)

## Demo-4: Revert when state change fails
Date: 6 September 2018

NMState now supports checking whether changing the Network Manager config
actually results in the state that the user specified. To demonstrate this, a
property of virtual network interfaces is used. They silently ignore changes to
their speed settings. So when a speed is set that the card does not accept, the
Network Manager configuration is changed for a short time. When the
verification of the state fails, NMState reverts the configuration back to its
previous state.

[![asciicast](https://asciinema.org/a/4HojPs4JcvsFExKXuX5zQzm9I.png)](https://asciinema.org/a/4HojPs4JcvsFExKXuX5zQzm9I)
