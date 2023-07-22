# cisco-evpn-vxlan-lab
Cisco N9K EVPN / VXLAN LAB 
<img width="1672" alt="Screenshot 2023-07-21 at 11 49 01 PM" src="https://github.com/satishdotpatel/cisco-evpn-vxlan-lab/assets/10041875/b0a01fe0-0f68-4e29-bf8c-d00196f973f0">

### Linux server bonding command
```
$ ethtool -s ens2 speed 1000 duplex full
$ ethtool -s ens3 speed 1000 duplex full
$ ip link add bond0 type bond mode 802.3ad
$ ip link set dev ens2 down
$ ip link set dev ens3 down
$ ip link set ens2 master bond0
$ ip link set ens3 master bond0
$ ip link set bond0 up
$ ip addr add 10.0.0.10/24 dev bond0
```

### OSPF neighbore on spine-1
```
spine-1# show ip ospf neighbors
 OSPF Process ID UNDERLAY-NET VRF default
 Total number of neighbors: 6
 Neighbor ID     Pri State            Up Time  Address         Interface
 10.255.1.11       1 FULL/ -          1d01h    10.255.1.11     Eth1/1
 10.255.1.12       1 FULL/ -          1d00h    10.255.1.12     Eth1/2
 10.255.1.21       1 FULL/ -          23:59:42 10.255.1.21     Eth1/3
 10.255.1.22       1 FULL/ -          23:59:32 10.255.1.22     Eth1/4
 10.255.1.1        1 FULL/ -          05:48:34 10.255.1.1      Eth1/5
 10.255.1.2        1 FULL/ -          05:45:41 10.255.1.2      Eth1/6
```

### L2VPN EVPN Routes
```
spine-1# show bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 1669, Local Router ID is 10.255.0.1
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-i
njected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - b
est2

   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.255.1.2:3
* i[5]:[0]:[0]:[0]:[0.0.0.0]/224
                      10.255.255.2                      100          0 99999 888
88 i
*>i                   10.255.255.1                      100          0 99999 888
88 i
* i[5]:[0]:[0]:[23]:[69.25.124.0]/224
                      10.255.255.2                      100          0 i
*>i                   10.255.255.1                      100          0 i

Route Distinguisher: 10.255.1.11:4
*>i[5]:[0]:[0]:[24]:[69.25.124.0]/224
                      10.255.255.10            0        100          0 ?

Route Distinguisher: 10.255.1.11:32827
*>i[2]:[0]:[0]:[48]:[5254.001c.4a81]:[0]:[0.0.0.0]/216
                      10.255.255.10                     100          0 i

Route Distinguisher: 10.255.1.11:32867
*>i[2]:[0]:[0]:[48]:[5254.0008.df8b]:[0]:[0.0.0.0]/216
                      10.255.255.10                     100          0 i
*>i[2]:[0]:[0]:[48]:[5254.0008.df8b]:[32]:[69.25.124.100]/272
                      10.255.255.10                     100          0 i

Route Distinguisher: 10.255.1.12:4
*>i[5]:[0]:[0]:[24]:[69.25.124.0]/224
                      10.255.255.10            0        100          0 ?

Route Distinguisher: 10.255.1.12:32827
*>i[2]:[0]:[0]:[48]:[5254.001c.4a81]:[0]:[0.0.0.0]/216
                      10.255.255.10                     100          0 i

Route Distinguisher: 10.255.1.12:32867
*>i[2]:[0]:[0]:[48]:[5254.0008.df8b]:[0]:[0.0.0.0]/216
                      10.255.255.10                     100          0 i
*>i[2]:[0]:[0]:[48]:[5254.0008.df8b]:[32]:[69.25.124.100]/272
                      10.255.255.10                     100          0 i

Route Distinguisher: 10.255.1.21:4
*>i[5]:[0]:[0]:[24]:[69.25.124.0]/224
                      10.255.255.20            0        100          0 ?

Route Distinguisher: 10.255.1.21:32827
*>i[2]:[0]:[0]:[48]:[5254.0008.0572]:[0]:[0.0.0.0]/216
                      10.255.255.20                     100          0 i

Route Distinguisher: 10.255.1.22:4
*>i[5]:[0]:[0]:[24]:[69.25.124.0]/224
                      10.255.255.20            0        100          0 ?

Route Distinguisher: 10.255.1.22:32827
*>i[2]:[0]:[0]:[48]:[5254.0008.0572]:[0]:[0.0.0.0]/216
                      10.255.255.20                     100          0 i
```

### IP multicast routes 
```
spine-1# show ip mroute
IP Multicast Routing Table for VRF "default"

(*, 232.0.0.0/8), uptime: 3d01h, pim ip
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)


(*, 239.1.1.1/32), uptime: 2d10h, pim ip
  Incoming interface: loopback1, RPF nbr: 10.255.0.123
  Outgoing interface list: (count: 2)
    Ethernet1/2, uptime: 1d00h, pim
    Ethernet1/1, uptime: 1d01h, pim


(10.255.255.10/32, 239.1.1.1/32), uptime: 1d00h, pim mrib ip
  Incoming interface: Ethernet1/1, RPF nbr: 10.255.1.11, internal
  Outgoing interface list: (count: 2)
    Ethernet1/2, uptime: 00:34:22, pim
    Ethernet1/1, uptime: 00:35:12, pim, (RPF)


(10.255.255.20/32, 239.1.1.1/32), uptime: 1d00h, pim mrib ip
  Incoming interface: Ethernet1/4, RPF nbr: 10.255.1.22, internal
  Outgoing interface list: (count: 2)
    Ethernet1/2, uptime: 1d00h, pim
    Ethernet1/1, uptime: 1d00h, pim


(*, 239.1.1.100/32), uptime: 1d01h, pim ip
  Incoming interface: loopback1, RPF nbr: 10.255.0.123
  Outgoing interface list: (count: 3)
    Ethernet1/6, uptime: 05:47:33, pim
    Ethernet1/5, uptime: 05:50:31, pim
    Ethernet1/1, uptime: 1d01h, pim


(10.255.255.1/32, 239.1.1.100/32), uptime: 05:49:17, pim mrib ip
  Incoming interface: Ethernet1/5, RPF nbr: 10.255.1.1, internal
  Outgoing interface list: (count: 3)
    Ethernet1/5, uptime: 01:05:14, pim, (RPF)
    Ethernet1/6, uptime: 05:47:33, pim
    Ethernet1/1, uptime: 05:49:17, pim


(10.255.255.2/32, 239.1.1.100/32), uptime: 05:46:24, pim mrib ip
  Incoming interface: Ethernet1/6, RPF nbr: 10.255.1.2, internal
  Outgoing interface list: (count: 3)
    Ethernet1/6, uptime: 05:44:24, pim, (RPF)
    Ethernet1/5, uptime: 05:46:24, pim
    Ethernet1/1, uptime: 05:46:24, pim


(10.255.255.10/32, 239.1.1.100/32), uptime: 1d00h, pim mrib ip
  Incoming interface: Ethernet1/1, RPF nbr: 10.255.1.11, internal
  Outgoing interface list: (count: 3)
    Ethernet1/1, uptime: 00:49:11, pim, (RPF)
    Ethernet1/6, uptime: 05:43:24, pim
    Ethernet1/5, uptime: 05:50:31, pim


(10.255.255.20/32, 239.1.1.100/32), uptime: 1d00h, pim mrib ip
  Incoming interface: Ethernet1/4, RPF nbr: 10.255.1.22, internal
  Outgoing interface list: (count: 3)
    Ethernet1/6, uptime: 05:47:33, pim
    Ethernet1/5, uptime: 05:50:31, pim
    Ethernet1/1, uptime: 1d00h, pim
```

### BGP neighbors 
```
spine-1# show ip bgp summary

Neighbor        V    AS    MsgRcvd    MsgSent   TblVer  InQ OutQ Up/Down  State/
PfxRcd
10.255.1.1      4 65001        373        395       21    0    0 05:36:16 0

10.255.1.2      4 65001        361        388       21    0    0 05:48:20 0

10.255.1.11     4 65001       3543       3632       21    0    0    1d00h 0

10.255.1.12     4 65001       1601       1779       21    0    0    1d00h 0

10.255.1.21     4 65001       1576       1688       21    0    0    1d00h 0

10.255.1.22     4 65001       1570       1764       21    0    0    1d00h 0
```

### On LEAF switches VxLAN tunnel 
```
leaf-1-a# show nve peers
Interface Peer-IP                                 State LearnType Uptime   Route
r-Mac
--------- --------------------------------------  ----- --------- -------- -----
------------
nve1      10.255.255.1                            Up    CP        05:11:53 5215.
f753.1b08
nve1      10.255.255.20                           Up    CP        1d00h    520c.
b94a.1b08
```

### VNI status 
```
leaf-1-a# show nve vni
Codes: CP - Control Plane        DP - Data Plane
       UC - Unconfigured         SA - Suppress ARP
       S-ND - Suppress ND
       SU - Suppress Unknown Unicast
       Xconn - Crossconnect
       MS-IR - Multisite Ingress Replication
       HYB - Hybrid IRB mode

Interface VNI      Multicast-group   State Mode Type [BD/VRF]      Flags
--------- -------- ----------------- ----- ---- ------------------ -----
nve1      10060    239.1.1.1         Up    CP   L2 [60]
nve1      10061    239.1.1.1         Up    CP   L2 [61]
nve1      10100    239.1.1.100       Up    CP   L2 [100]
nve1      10555    n/a               Up    CP   L3 [ISP]
```


