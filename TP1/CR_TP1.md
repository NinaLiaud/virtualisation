# I. Most simplest LAN  
<br>

🌞 Déterminer l'adresse MAC de vos deux machines  
🌞 Définir une IP statique sur les deux machines  
🌞 Effectuer un ping d'une machine à l'autre  
<br>


### POUR PC1 
```
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:20001
MTU         : 1500
```

```
ip 10.1.1.1/24
Checking for duplicate address...
PC1 : 10.1.1.1 255.255.255.0

PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.1.1/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:20001
MTU         : 1500
```

```
PC1> ping 10.1.1.2

84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=0.074 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=0.162 ms
84 bytes from 10.1.1.2 icmp_seq=3 ttl=64 time=0.117 ms
84 bytes from 10.1.1.2 icmp_seq=4 ttl=64 time=0.146 ms
84 bytes from 10.1.1.2 icmp_seq=5 ttl=64 time=0.177 ms
```
<br>

### POUR PC2 
```
PC2> show ip

NAME        : PC1[2]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20002
RHOST:PORT  : 127.0.0.1:20003
MTU         : 1500
```

```
ip 10.1.1.2/24
Checking for duplicate address...
PC2 : 10.1.1.2 255.255.255.0

PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.1.1.2/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20002
RHOST:PORT  : 127.0.0.1:20003
MTU         : 1500
```

```
PC2> ping 10.1.1.1

84 bytes from 10.1.1.1 icmp_seq=1 ttl=64 time=0.249 ms
84 bytes from 10.1.1.1 icmp_seq=2 ttl=64 time=0.136 ms
84 bytes from 10.1.1.1 icmp_seq=3 ttl=64 time=0.136 ms
84 bytes from 10.1.1.1 icmp_seq=4 ttl=64 time=0.138 ms
84 bytes from 10.1.1.1 icmp_seq=5 ttl=64 time=0.229 ms
```
 
<br>


### 🌞 Wireshark !  
> _voir ping_wireshark.pcapng_

Protocole ping: ICMP  
<br>

### 🌞 ARP
```
PC1> arp

00:50:79:66:68:01  10.1.1.2 expires in 114 seconds
```
<br>
<br>

# II. Ajoutons un switch


### 🌞 Déterminer l'adresse MAC de vos trois machines


```
PC3 > show ip

NAME        : VPCS[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:02
LPORT       : 20004
RHOST:PORT  : 127.0.0.1:20005
MTU         : 1500
```
<br>

### 🌞 Définir une IP statique sur les trois machines  
```
PC3 > ip 10.1.1.3/24
Checking for duplicate address...
PC3 : 10.1.1.3 255.255.255.0

PC3> show ip

NAME        : PC3[1]
IP/MASK     : 10.1.1.3/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:02
LPORT       : 20004
RHOST:PORT  : 127.0.0.1:20005
MTU         : 1500
```
> Pour PC1 et PC2 c'est déjà fait dans la première étape ⬆️  

<br>

### 🌞 Effectuer des ping d'une machine à l'autre
```
PC1> ping 10.1.1.2

84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=0.104 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=0.238 ms
84 bytes from 10.1.1.2 icmp_seq=3 ttl=64 time=0.124 ms
84 bytes from 10.1.1.2 icmp_seq=4 ttl=64 time=0.256 ms
84 bytes from 10.1.1.2 icmp_seq=5 ttl=64 time=0.132 ms
```
```
PC2> ping 10.1.1.3

84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=0.251 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=0.217 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=0.128 ms
84 bytes from 10.1.1.3 icmp_seq=4 ttl=64 time=0.285 ms
84 bytes from 10.1.1.3 icmp_seq=5 ttl=64 time=0.193 ms
```
```
PC1> ping 10.1.1.3

84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=0.132 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=0.148 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=0.154 ms
84 bytes from 10.1.1.3 icmp_seq=4 ttl=64 time=0.131 ms
84 bytes from 10.1.1.3 icmp_seq=5 ttl=64 time=0.128 ms
```
<br>
<br>

# III. Serveur DHCP
<br>

## 1. Legit

### 🌞 Donner un accès Internet à la machine dhcp.tp1.efrei
```
[rocky@dhcp ~]$ ping google.com
PING google.com (172.217.20.174) 56(84) bytes of data.
64 bytes from waw02s07-in-f14.1e100.net (172.217.20.174): icmp_seq=1 ttl=116 time=24.5 ms
64 bytes from waw02s07-in-f174.1e100.net (172.217.20.174): icmp_seq=2 ttl=116 time=21.7 ms
64 bytes from par10s49-in-f14.1e100.net (172.217.20.174): icmp_seq=3 ttl=116 time=26.8 ms
64 bytes from waw02s07-in-f14.1e100.net (172.217.20.174): icmp_seq=4 ttl=116 time=21.7 ms
64 bytes from par10s49-in-f14.1e100.net (172.217.20.174): icmp_seq=5 ttl=116 time=20.7 ms
```
<br>

### 🌞 Installer et configurer un serveur DHCP
```
# adresse reseau & subnetmask
subnet 10.1.1.0 netmask 255.255.255.0 {
    # plage d'adresses attribuables
    range dynamic-bootp 10.1.1.10 10.1.1.50;
    # broadcast
    option broadcast-address 10.1.1.255;
}
```
```
NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.1.1.253
NETMASK=255.255.255.0
```
<br>

### 🌞 Récupérer une IP automatiquement depuis les 3 nodes

####  _avant start serveur_

```
PC1> sh ip

NAME        : PC1[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20009
RHOST:PORT  : 127.0.0.1:20010
MTU         : 1500
```
```
PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.1.1.2/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20005
RHOST:PORT  : 127.0.0.1:20006
MTU         : 1500
```
```
PC3> sh ip

NAME        : PC3[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:02
LPORT       : 20011
RHOST:PORT  : 127.0.0.1:20012
MTU         : 1500
```
<br>

#### _après start serveur DHCP_
```
PC1> dhcp
DDORA
PC1> sh ip IP 10.1.1.42/24

NAME        : PC1[1]
IP/MASK     : 10.1.1.42/24
GATEWAY     : 0.0.0.0
DNS         :
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 43194, 43200/21600/37800
MAC         : 00:50:79:66:68:00
LPORT       : 20009
RHOST:PORT  : 127.0.0.1:20010
MTU         : 1500
```
```
PC2> dhcp
DDORA
PC2> sh ip IP 10.1.1.10/24

NAME        : PC2[1]
IP/MASK     : 10.1.1.10/24
GATEWAY     : 0.0.0.0
DNS         :
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 43196, 43200/21600/37800
MAC         : 00:50:79:66:68:01
LPORT       : 20005
RHOST:PORT  : 127.0.0.1:20006
MTU         : 1500
```
```
PC3> dhcp
DDORA
PC3> sh ip IP 10.1.1.26/24

NAME        : PC3[1]
IP/MASK     : 10.1.1.26/24
GATEWAY     : 0.0.0.0
DNS         :
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 43192, 43200/21600/37800
MAC         : 00:50:79:66:68:02
LPORT       : 20011
RHOST:PORT  : 127.0.0.1:20012
MTU         : 1500
```
<br>

### 🌞 Wireshark !
> _voir dhcp_wireshark.pcapng_

<br>


## 2. DHCP spoofing

### 🌞 Configurez dnsmasq
```
port=0

interface=enp0s3

dhcp-range=10.1.1.210,10.1.1.250,12h
```
<br>

### 🌞 Test !
```
PC3> ip dhcp
DDORA IP 10.1.1.247/24 GW 10.1.1.254
```
<br>

### 🌞 Now race !
```
PC1> ip dhcp
DORA
PC1> sh ip IP 10.1.1.42/24

NAME        : PC1[1]
IP/MASK     : 10.1.1.42/24
GATEWAY     : 0.0.0.0
DNS         :
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 43194, 43200/21600/37800
MAC         : 00:50:79:66:68:00
LPORT       : 10010
RHOST:PORT  : 127.0.0.1:10011
MTU:        : 1500
```

On peut le refaire pour voir si ça change (spoil oui ça peut changer) :
```
PC1> ip dhcp
DORA IP 10.1.1.245/24 GW 10.1.1.254

PC1> sh ip

NAME        : PC1[1]
IP/MASK     : 10.1.1.245/24
GATEWAY     : 10.1.1.254
DNS         :
DHCP SERVER : 10.1.1.254
DHCP LEASE  : 43189, 43200/21600/37800
MAC         : 00:50:79:66:68:00
LPORT       : 10010
RHOST:PORT  : 127.0.0.1:10011
MTU:        : 1500
```
<br>

Exemples avec d'autres VPCS :  
```
PC2> ip dhcp
DORA
PC2> sh ip IP 10.1.1.10/24

NAME        : PC2[1]
IP/MASK     : 10.1.1.10/24
GATEWAY     : 0.0.0.0
DNS         :
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 43191, 43200/21600/37800
MAC         : 00:50:79:66:68:01
LPORT       : 10012
RHOST:PORT  : 127.0.0.1:10013
MTU:        : 1500
```
```
PC3> ip dhcp
DORA
PC3> sh ip IP 10.1.1.26/24

NAME        : PC3[1]
IP/MASK     : 10.1.1.26/24
GATEWAY     : 0.0.0.0
DNS         :
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 43197, 43200/21600/37800
MAC         : 00:50:79:66:68:02
LPORT       : 10015
RHOST:PORT  : 127.0.0.1:10016
MTU:        : 1500
```
```
PC4> ip dhcp
DDORA IP 10.1.1.248/24 GW 10.1.1.254

PC4> sh ip

NAME        : PC4[1]
IP/MASK     : 10.1.1.248/24
GATEWAY     : 10.1.1.254
DNS         :
DHCP SERVER : 10.1.1.254
DHCP LEASE  : 43195, 43200/21600/37800
MAC         : 00:50:79:66:68:03
LPORT       : 10018
RHOST:PORT  : 127.0.0.1:10019
MTU:        : 1500
```
<br>

### 🌞 Wireshark !

> _voir race.pcapng_  

Et c'est le premier qui fait l'offer qui gagne le duel ⚔️

