##################################################################################

The MIT License

Copyright 2018 Dennis Ho

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

###################################################################################

# by Dennis 
# cisco switch general setting
# draft version
# default console 
#terminal 9600n81
# attach console cable to switch end

# check you switch current setting
>enable
>show running-config 
# will display current running config

version 12.2
no service pad
service timestamps debug uptime
service timestamps log uptime
service password-encryption
!
hostname xxxxx
!
enable password 7 ----------------------------------
!
username dennis privilege 15 password 7 ------------------------
no aaa new-model
system mtu routing 1500
ip subnet-zero
!
ip domain-name mylab
ip name-server 1.1.1.1
!
!
crypto pki trustpoint TP-self-signed-217425408
 enrollment selfsigned
 subject-name cn=IOS-Self-Signed-Certificate-217425408
 revocation-check none
 rsakeypair TP-self-signed-217425408
!
!
crypto pki certificate chain TP-self-signed-217425408
 certificate self-signed 01
  308.....
  .....0F5
  quit
!
!
no file verify auto
spanning-tree mode pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface GigabitEthernet0/3
!
interface GigabitEthernet0/4
!
interface GigabitEthernet0/5
!
interface GigabitEthernet0/6
!
interface GigabitEthernet0/7
!
interface GigabitEthernet0/8
!
interface GigabitEthernet0/9
!
interface GigabitEthernet0/10
!
interface GigabitEthernet0/11
!
interface GigabitEthernet0/12
!
interface GigabitEthernet0/13
!
interface GigabitEthernet0/14
!
interface GigabitEthernet0/15
!
interface GigabitEthernet0/16
!
interface GigabitEthernet0/17
!
interface GigabitEthernet0/18
!
interface GigabitEthernet0/19
!
interface GigabitEthernet0/20
!
interface GigabitEthernet0/21
!
interface GigabitEthernet0/22
!
interface GigabitEthernet0/23
!
interface GigabitEthernet0/24
!
interface GigabitEthernet0/25
!
interface GigabitEthernet0/26
!
interface GigabitEthernet0/27
!
interface GigabitEthernet0/28
!
interface GigabitEthernet0/29
!
interface GigabitEthernet0/30
!
interface GigabitEthernet0/31
!
interface GigabitEthernet0/32
!
interface GigabitEthernet0/33
!
interface GigabitEthernet0/34
!
interface GigabitEthernet0/35
!
interface GigabitEthernet0/36
!
interface GigabitEthernet0/37
!
interface GigabitEthernet0/38
!
interface GigabitEthernet0/39
!
interface GigabitEthernet0/40
!
interface GigabitEthernet0/41
!
interface GigabitEthernet0/42
!
interface GigabitEthernet0/43
!
interface GigabitEthernet0/44
!
interface GigabitEthernet0/45
!
interface GigabitEthernet0/46
!
interface GigabitEthernet0/47
!
interface GigabitEthernet0/48
!
interface Vlan1
 ip address 192.168.1.2 255.255.255.0
 no ip route-cache
!
interface Vlan10
 no ip address
 no ip route-cache
!
interface Vlan100
 no ip address
 no ip route-cache
!
ip default-gateway 192.168.1.1
ip http server
ip http secure-server
!
control-plane
!
!
line con 0
line vty 0 4
 login local
 transport input ssh
line vty 5 15
 login
 transport input ssh
!         
ntp clock-period 36030199
ntp server 192.168.1.12 version 2
end

########################################
# to setup a switch 
enable

config terminal
#setup domain name, hostname require for ssh login
ip domain-name <domain.com>
hostname router1
ip system-mtu routing 1500
ip name-server 1.1.1.1
# setup switch ip on default vlan 1
interface vlan 1 
ip address 192.168.1.2 255.255.255.0 
no shutdown
#setup ntp server
ntp server 192.168.1.1 version 2

exit
#exit config
exit 
#
show ip interface vlan 1

config terminal
#setup telnet password
line vty 0 15  
#change password to <password>
password <password>
#accept it
login
exit
#encrypt password
service password-encryption
#write to non-volatile memory
write memory

#setup ssh login
crypto key generate rsa 
>How many bits in the modulus [512]: 2048
#create first user
username user1 privilege 15 password <password>
# console port 
line con 0 
# virtual terminal
line vty 0 15
#login user1
login local
#setup ssh only login
transport input ssh 
exit

# before connect to switch ssh port
# you should have following in you config
Host cisco
  KexAlgorithms +diffie-hellman-group1-sha1
  Ciphers +aes128-cbc
  User user1


#list all users
show users

#show all snmp users
show snmp user

########################################################
# setup new vlan 
config term 
vlan <vlanid 2-4096>
name <vlan-name-text>
# optional ->
#mtu 1460
end
show vlan name <vlan-name-text> 
show vlan id <vlan id>
copy running-config startup-config

#remove a vlan
config term
no vlan <vlanid>
end
show vlan brief
copy running-config startup-config

#repeat this on all ports assign to VLAN
#assign port to VLAN
config term
interface <inferface-id, Gi0/1 to Gi0/48 for GigaEthernet>
switchport mode access
switchport access vlan <vlan-id>
end
show running-config interface <interface-id>


####################################################
#example
config term
interface Gi0/2 
switchport mode access
switchport access vlan 100
#
interface Gi0/3
switchport mode access
switchport access vlan 100
#
interface Gi0/4
switchport mode access
switchport access vlan 100
#
interface Gi0/5
switchport mode access
switchport access vlan 100
#
interface Gi0/6 
switchport mode access
switchport access vlan 100
#
interface Gi0/7
switchport mode access
switchport access vlan 100
#
interface Gi0/8 
switchport mode access
switchport access vlan 100
#
interface Gi0/9
switchport mode access
switchport access vlan 100
#
interface Gi0/10 
switchport mode access
switchport access vlan 100
#
interface Gi0/11
switchport mode access
switchport access vlan 100
#
interface Gi0/12
switchport mode access
switchport access vlan 100
#
interface Gi0/13
switchport mode access
switchport access vlan 100
#
interface Gi0/14
switchport mode access
switchport access vlan 100
#
interface Gi0/15
switchport mode access
switchport access vlan 100
#
interface Gi0/16
switchport mode access
switchport access vlan 100
#
interface Gi0/17
switchport mode access
switchport access vlan 100
#
interface Gi0/18
switchport mode access
switchport access vlan 100
#
interface Gi0/19
switchport mode access
switchport access vlan 100
#
interface Gi0/20
switchport mode access
switchport access vlan 100
#
interface Gi0/21
switchport mode access
switchport access vlan 100
#
interface Gi0/22
switchport mode access
switchport access vlan 100
#
interface Gi0/23
switchport mode access
switchport access vlan 100
#
interface Gi0/24
switchport mode access
switchport access vlan 100
#
interface Gi0/25
switchport mode access
switchport access vlan 100
#
interface Gi0/26
switchport mode access
switchport access vlan 100
#
interface Gi0/27
switchport mode access
switchport access vlan 100
#
interface Gi0/28
switchport mode access
switchport access vlan 100
#
interface Gi0/29
switchport mode access
switchport access vlan 100
#
interface Gi0/30
switchport mode access
switchport access vlan 100
#
interface Gi0/31
switchport mode access
switchport access vlan 100
#
interface Gi0/32
switchport mode access
switchport access vlan 100
#
interface Gi0/33
switchport mode access
switchport access vlan 100
#
interface Gi0/34
switchport mode access
switchport access vlan 100
#
interface Gi0/35
switchport mode access
switchport access vlan 100
#
interface Gi0/36
switchport mode access
switchport access vlan 100
#
interface Gi0/37
switchport mode access
switchport access vlan 100
#
interface Gi0/38
switchport mode access
switchport access vlan 100
#
interface Gi0/39
switchport mode access
switchport access vlan 100
#
interface Gi0/40
switchport mode access
switchport access vlan 100
#
interface Gi0/41
switchport mode access
switchport access vlan 100
#
interface Gi0/42
switchport mode access
switchport access vlan 100
#
interface Gi0/43
switchport mode access
switchport access vlan 100
#
interface Gi0/44
switchport mode access
switchport access vlan 100
#
interface Gi0/45
switchport mode access
switchport access vlan 100
#
interface Gi0/46
switchport mode access
switchport access vlan 100
#
interface Gi0/47
switchport mode access
switchport access vlan 100
#
interface Gi0/48
switchport mode access
switchport access vlan 100
# exit config
end

# to show all vlan config 
show vlan 
show vlan id 100
>VLAN Name                             Status    Ports
>---- -------------------------------- --------- -------------------------------
>100  testvlan100                      active    Gi0/2, Gi0/3, Gi0/4, Gi0/5, Gi0/6, Gi0/7, Gi0/8, Gi0/9, Gi0/10
>                                                Gi0/11, Gi0/12, Gi0/13, Gi0/14, Gi0/15, Gi0/16, Gi0/17, Gi0/18
>                                                Gi0/19, Gi0/20, Gi0/21, Gi0/22, Gi0/23, Gi0/24, Gi0/25, Gi0/26
>                                                Gi0/27, Gi0/28, Gi0/29, Gi0/30, Gi0/31, Gi0/32, Gi0/33, Gi0/34
>                                                Gi0/35, Gi0/36, Gi0/37, Gi0/38, Gi0/39, Gi0/40, Gi0/41, Gi0/42
>                                                Gi0/43, Gi0/44, Gi0/45, Gi0/46, Gi0/47, Gi0/48
>
>VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
>---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
>100  enet  100100     1500  -      -      -        -    -        0      0   
>
>Remote SPAN VLAN
>----------------
>Disabled
>
>Primary Secondary Type              Ports
>------- --------- ----------------- ------------------------------------------


# setup ip address for each vlan
config term
interface vlan 100 
ip address 192.168.100.2 255.255.255.0
end
# check interface info
show interface <interface-id>
# show ip interface <interface-id>
show

copy running-config startup-config

###########################################
