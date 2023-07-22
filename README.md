# cisco-evpn-vxlan-lab
Cisco N9K EVPN / VXLAN LAB 
<img width="1672" alt="Screenshot 2023-07-21 at 11 49 01 PM" src="https://github.com/satishdotpatel/cisco-evpn-vxlan-lab/assets/10041875/b0a01fe0-0f68-4e29-bf8c-d00196f973f0">

# Linux server bonding command
$ ethtool -s ens2 speed 1000 duplex full
$ ethtool -s ens3 speed 1000 duplex full
$ ip link add bond0 type bond mode 802.3ad
$ ip link set dev ens2 down
$ ip link set dev ens3 down
$ ip link set ens2 master bond0
$ ip link set ens3 master bond0
$ ip link set bond0 up
$ ip addr add 10.0.0.10/24 dev bond0
