## VRP is Huawei’s network operating system that runs on network devices such as routers and switches. Knowledge of VRP is essential to understanding Huawei products and technologies, and many of the configuration examples provided in this book are based on VRP.


# Basic Configuration
## Hostname
```
sysname [HOSTNAME]
```

## Clock Time
```
clock timezone WIB add 07:00:00
```

## Username and Password
```
security password
```

## AAA
```
aaa
 user-password password-force-change disable
 local-user [USERNAME] password irreversible-cipher [PASSWORD]
 local-user [USERNAME] service-type ftp terminal telnet ssh
```

## Line VTY
```
user-interface vty 0 4
authentication-mode aaa
user privilege level 3
idle-timeout 5 0
protocol inbound telnet ssh
protocol outbound telnet ssh
```

## Telnet
```
stelnet server enable
telnet server-source all-interface
undo telnet ipv6 server enable
undo telnet ipv6 server-source all-interface
```

## SSH
```
ssh server-source all-interface
ssh ipv6 server-source all-interface
ssh authorization-type default aaa
ssh user [USERNAME]
ssh user [USERNAME] authentication-type all
ssh user [USERNAME] service-type all
ssh server-source -i [INTERFACE]
ssh server-source -i [LOOPBACK]
```

## FTP
```
sftp server enable
undo FTP server-source all-interface
undo FTP ipv6 server-source all-interface
```

## NTP
```
ntp-service server disable
ntp-service unicast-server [IP_NTP-SERVER]
ntp-service unicast-server [IP_NTP-SERVER]
ntp-service source-interface LoopBack0

ntp-service ipv6 server disable
ntp-service server source-interface all disable
ntp-service ipv6 server source-interface all disable
```

## LLDP
```
lldp enable
```

## Disable Web-Server
```
undo web-auth-server source-ip all
undo web-auth-server source-ipv6 all
```

## Disable DHCP
```
undo dhcp enable
```

## ACL (Access Control List)
```
acl number [NUMBER-ID]
 rule 5 permit source [IP_ADDRESS_NETMASK]
 rule 6 permit source [IP_ADDRESS_NETMASK]
 rule 7 permit source [IP_ADDRESS_NETMASK]
```

# Interface
## Loopback
```
interface LoopBack0
 ip address [IP_ADDRESS] [NETMASK]
```

## Port Interface
```
interface GigabitEthernet0/3/0
 mtu 9100
 description [DESCRIPTION]
 undo shutdown
 ip address [IP_ADDRESS] [NETMASK]
 undo dcn
```
OSPF OPTIONAL
```
interface GigabitEthernet0/3/0
 ospf authentication-mode md5 1 cipher [PASSWORD]
 ospf network-type p2p
 ospf timer hello 5
 ospf timer dead 15
```
MPLS OPTIONAL
```
interface GigabitEthernet0/3/0
 mpls
 mpls te
 mpls rsvp-te
 mpls rsvp-te hello
 mpls ldp
```

## VLAN
VLAN Configuration
```
vlan [VLAN-ID]
vlan name [NAME]
```
Interface VLAN Configuration
```
interface vlan [VLAN-ID]
ip address [IP_ADDRESS] [NETMASK]
```

# Routing OSPF
## OSPF
```
ospf 100 router-id [LOOPBACK]
opaque-capability enable
area 0.0.0.0 <- Area ID
network [POINT_TO_POINT] 0.0.0.0
network [LOOPBACK] 0.0.0.0
```
OSPF OPTIONAL
```
bfd all-interfaces enable
bandwidth-reference [BANDWIDTH]
silent-interface all
undo silent-interface [PORT]
undo silent-interface [LOOPBACK]
frr
loop-free-alternate
mpls-te enable
```

# MPLS, LDP, BFD, RSVP
## MPLS
```
mpls lsr-id [LOOPBACK]
mpls
 mpls te
 mpls rsvp-te
 mpls rsvp-te bfd all-interfaces enable
 mpls rsvp-te hello
 mpls rsvp-te srefresh
 mpls te cspf
 mpls bfd enable
```

## LDP
```
mpls ldp
graceful-restart
md5-password cipher [LOOPBACK] [PASSWORD]
md5-password cipher [LOOPBACK] [PASSWORD]
```
## BFD
```
bfd
mpls
mpls bfd enable
```



