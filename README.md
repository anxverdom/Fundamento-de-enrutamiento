ROUTER 1
en
conf t
hostname R1
interface gig0/1
ip address 192.168.100.1 255.255.255.252
no shutdown
interface gig0/0
no ip address
no shutdown

interface gig0/0.300
encapsulation dot1q 300
ip address 192.168.30.2 255.255.255.0
ip helper-address 192.168.100.10
standby 1 ip 192.168.30.1
standby 1 priority 90
standby 1 preempt


interface Gig0/0.500
encapsulation dot1q 500
ip address 192.168.50.2 255.255.255.0
ip helper-address 192.168.100.10
standby 2 ip 192.168.50.1
standby 2 priority 90
standby 2 preempt

interface gig0/0.999
encapsulation dot1q 999 native
ip address 192.168.99.2 255.255.255.0
ip helper-address 192.168.100.10
standby 3 ip 192.168.99.1
standby 3 priority 90
standby 3 preempt
ip route 0.0.0.0 0.0.0.0 192.168.100.2
ip domain-name campus.local
crypto key generate rsa general-keys
  yes 1024

-------------------------------------------------------------------------------------
username 20252143 privilege 1 secret cisco123
username opelegrino privilege 15 secret cisco123
line vty 0 4
 transport input ssh
 login local
 exit
ip ssh version 2
service password-encryption
enable secret cisco123
line console 0
password cisco123
login
line vty 0 4
password cisco123
login
end
wr
---------------------------------------------------------------------------------------
R4
en
conf t
hostname R4
interface serial0/0/0
ip address 192.168.100.9 255.255.255.252
no shutdown
interface Gig0/1
no ip address
no shutdown

interface Gig0/1.300
encapsulation dot1Q 300
ip address 192.168.30.3 255.255.255.0
ip helper-address 192.168.100.10
standby 1 ip 192.168.30.1
standby 1 priority 110
standby 1 preempt
no shutdown 

interface Gig0/1.500
encapsulation dot1q 500
ip address 192.168.50.3 255.255.255.0
ip helper-address 192.168.100.10
standby 2 ip 192.168.50.1
standby 2 priority 110
standby 2 preempt
no shutdown

interface gig0/1.999
encapsulation dot1q 999 native
ip address 192.168.99.3 255.255.255.0
ip helper-address 192.168.100.10
standby 3 ip 192.168.99.1
standby 3 priority 110
standby 3 preempt
no shutdown

ip route 0.0.0.0 0.0.0.0 192.168.100.10
ip domain-name campus.local
crypto key generate rsa general-keys  
   yes 1024

enable
conf t
ip route 192.168.100.8 255.255.255.252 192.168.100.2
end
write memory

------------------------------------------------------------------------------------------------------
username 20252143 privilege 1 secret cisco123
username opelegrino privilege 15 secret cisco123
line vty 0 4
 transport input ssh
 login local
 exit
ip ssh version 2
service password-encryption
enable secret cisco123
line console 0
password cisco123
login
line vty 0 4
password cisco123
login
end
wr
------------------------------------------------------------------------------------------------------
Copiar R2 para
R2

en
conf t
hostname R2
interface gig0/1
ip address 192.168.100.2 255.255.255.252
no shutdown
interface serial0/0/0
ip address 192.168.100.10 255.255.255.252
clock rate 64000
no shutdown
interface serial0/0/1
ip address 192.168.100.6 255.255.255.252
clock rate 64000
no shutdown
no ip dhcp excluded-address 192.168.32.1
no ip dhcp excluded-address 192.168.32.2
no ip dhcp excluded-address 192.168.32.3
no ip dhcp excluded-address 192.168.32.4
no ip dhcp excluded-address 192.168.32.5
no ip dhcp excluded-address 192.168.32.6 192.168.32.14
ip dhcp excluded-address 192.168.30.1 192.168.30.10
ip dhcp excluded-address 192.168.50.1 192.168.50.10
ip dhcp excluded-address 192.168.99.1 192.168.99.10
ip dhcp pool vlan300
network 192.168.30.0 255.255.255.0
default-router 192.168.30.1
dns-server 192.168.32.5
ip dhcp pool vlan500
network 192.168.50.0 255.255.255.0
default-router 192.168.50.1
dns-server 192.168.32.5
ip dhcp pool vlan999
network 192.168.99.0 255.255.255.0
default-router 192.168.99.1
dns-server 192.168.32.5
ip route 192.168.30.0 255.255.255.0 192.168.100.1
ip route 192.168.30.0 255.255.255.0 192.168.100.9
ip route 192.168.50.0 255.255.255.0 192.168.100.1
ip route 192.168.50.0 255.255.255.0 192.168.100.9
ip route 192.168.99.0 255.255.255.0 192.168.100.1
ip route 192.168.99.0 255.255.255.0 192.168.100.9
ip route 192.168.32.0 255.255.255.0 192.168.100.5
ip domain-name campus.local

crypto key generate rsa general-keys
   yes 1024


username 20252143 privilege 1 secret cisco123
username opelegrino privilege 15 secret cisco123
line vty 0 4
 transport input ssh
 login local
 exit
ip ssh version 2
service password-encryption
enable secret cisco123
line console 0
password cisco123
login
line vty 0 4
password cisco123
login
end
wr
---------------------------------------------------------------------------------------------------------------------------------
Copia router 3 :


R3
en
conf t
hostname R3
interface serial0/0/0
ip address 192.168.100.5 255.255.255.252
no shutdown
interface Gig0/0
ip address 192.168.32.1 255.255.255.0
ip helper-address 192.168.32.5
no shutdown

ip route 0.0.0.0 0.0.0.0 192.168.100.6
ip domain-name campus.local
crypto key generate rsa general-keys
 
 
  yes 1024

username 20252143 privilege 1 secret cisco123
username opelegrino privilege 15 secret cisco123
line vty 0 4
 transport input ssh
 login local
 exit
ip ssh version 2
service password-encryption
enable secret Cisco123
line console 0
password cisco123
login
end
wr

---------------------------------------------------------------------------------------------------------------------------------

SW1

en
conf t
hostname SW1

vlan 300
 name Estudiantes
vlan 500
 name Academicos
vlan 999
 name Administrativa-Nativa
exit

interface range FastEthernet0/23-24
 shutdown
 channel-group 1 mode active
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 300,500,999
 no shutdown
exit

interface Port-channel 1
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 300,500,999
 spanning-tree portfast trunk
 spanning-tree bpduguard enable
 no shutdown
exit

interface FastEthernet0/9
 switchport mode access
 switchport access vlan 500
 spanning-tree portfast
 spanning-tree bpduguard enable
 no shutdown
exit

interface FastEthernet0/2
 switchport mode access
 switchport access vlan 300
 spanning-tree portfast
 spanning-tree bpduguard enable
 no shutdown
exit
interface FastEthernet0/6
 switchport mode access
 switchport access vlan 999
 spanning-tree portfast
 spanning-tree bpduguard enable
 no shutdown
exit

interface Gig0/2
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 300,500,999
 no shutdown
exit

# Si ya existe la llave, Packet Tracer te pedirá confirmar el reemplazo
crypto key generate rsa general-keys
1024

username 20252143 privilege 1 secret cisco123
username opelegrino privilege 15 secret cisco123

line vty 0 4
 transport input ssh
 login local
exit

line console 0
 password cisco123
 login
exit

service password-encryption
enable secret cisco123
end
wr

---------------------------------------------------------------------------------------------------------------------------------

username 20252143 privilege 1 secret cisco123
username opelegrino privilege 15 secret cisco123
line vty 0 4
 transport input ssh
 login local
 exit
ip ssh version 2
service password-encryption
enable secret cisco123
line console 0
password cisco123
login
end
wr

---------------------------------------------------------------------------------------------------------------------------------
Copia SW2
en
conf t
hostname SW2

# 1. Creación de VLANs
vlan 300
 name Estudiantes
vlan 500
 name Academicos
vlan 999
 name Nativa-Administrativa
exit

interface range FastEthernet0/23-24
 shutdown
 channel-group 1 mode active
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 300,500,999
 no shutdown
exit

interface Port-channel 1
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 300,500,999
 spanning-tree portfast trunk
 spanning-tree bpduguard enable
 no shutdown
exit

interface FastEthernet0/5
 switchport mode access
 switchport access vlan 300
 spanning-tree portfast
 spanning-tree bpduguard enable
 no shutdown
exit

interface FastEthernet0/9
 switchport mode access
 switchport access vlan 500
 spanning-tree portfast
 spanning-tree bpduguard enable
 no shutdown
exit

interface FastEthernet0/6
 switchport mode access
 switchport access vlan 999
 spanning-tree portfast
 spanning-tree bpduguard enable
 no shutdown
exit

interface Gig0/2
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 300,500,999
 no shutdown
exit

interface vlan 999
 ip address 192.168.99.11 255.255.255.0
 no shutdown
exit

ip default-gateway 192.168.99.1


ip domain-name campus.local
crypto key generate rsa general-keys
1024

username 20252143 privilege 1 secret cisco123
username opelegrino privilege 15 secret cisco123

line vty 0 4
 transport input ssh
 login local
exit

line console 0
 password cisco123
 login
exit

service password-encryption
enable secret cisco123
end
wr
---------------------------------------------------------------------------------------------------------------------------------
username 20252143 privilege 1 secret cisco123
username opelegrino privilege 15 secret cisco123
line vty 0 4
 transport input ssh
 login local
 exit
ip ssh version 2
service password-encryption
enable secret cisco123
line console 0
password cisco123
login
end
wr
---------------------------------------------------------------------------------------------------------------------------------
SW3

en
conf t
hostname SW3
vlan 300
 name Estudiantes
 exit
vlan 500
 name Academicos
 exit
vlan 999
 name Nativa-Administrativa
exit
interface vlan 999
ip address 192.168.32.2 255.255.255.0
no shutdown
ip default-gateway 192.168.32.1
interface Gig0/1
switchport mode access
switchport access vlan 999
spanning-tree portfast
no shutdown
interface Fa0/3
switchport mode access
switchport access vlan 999
spanning-tree portfast
no shutdown
interface Gig0/2
switchport mode access
switchport access vlan 999
spanning-tree portfast
no shutdown
interface Fa0/4
switchport mode access
switchport access vlan 999
spanning-tree portfast
no shutdown
interface Fa0/1
switchport mode access
switchport access vlan 999
spanning-tree portfast
no shutdown
interface Fa0/2
switchport mode access
switchport access vlan 999
spanning-tree portfast
no shutdown
ip domain-name campus.local
crypto key generate rsa general-keys

yes 1024

---------------------------------------------------------------------------------------------------------------------------------

username 20252143 privilege 1 secret cisco123
username opelegrino privilege 15 secret cisco123
line vty 0 4
 transport input ssh
 login local
 exit
ip ssh version 2
service password-encryption
enable secret Cisco123
line console 0
password cisco123
login
end
wr
==============================================================================================================================================================================================




Luego al servidor, vamos a desktop:config y ponemos la 
192.168.32.5
255.255.255.0
192.168.32.1
192.168.32.5


VAMOS A service:dhcp
encendemos
192.168.32.1
192.168.32.5
192.168.32.3
255.255.255.0

253
0000
192.168.32.4
SAVE



VAMOS A Dns
encendemos
wic
192.168.32.4
add.save


VAMOS A AAA
Encendemos
1812
wic     192.168.32.4

cisco123    add



USUARIOS

Alan   cisco123
Duarte cisco123
Mella  cisco123
Sanchez cisco123
-------------------

WLC conf_manaement
192.168.32.4
255.255.255.0
192.168.32.1
192.168.32.5
---------------------

MELLA PC

Desktop_ip
DHCP
---------------
Conectamos ap-biblioteca con los cables de abajo
luego a config: DHCP

HACEMOS LO MISMO CON AP-LOBY
-----------------------

PC MELLA
Abrimos la web
http//wic: admin-Cisco123 Cisco123

wic

192.168.32.4
255.255.255.0
192.168.32.1
0
next


Alan
wpa2
cisco123
cisco123
next

next

apply
ok

url: https//192.168.32.4: admin-Cisco123


Vamos a WLAN
Luego GO

Sanchez
Sanchez
APPLY

 ACTIVAMOS EL PRIMERO QUE VEMOS Enable

Pasamos a securyti:Layer2

Cambiamos NONE por WPA2

Elejimos WPA2 Encryption

PSK Enable

contraseña: cisco123




VAMOS a advance
Los 2 enable en el medio de la pantalla FLEX CONECT LOCAL

APPLY y luego BACK

y creamos otra en Go 

Mella y duarte 	


A la izquierda dice AP groups CLICK

ADD GROUP

Escribimos AP-BILIOTECA debajo 
           Duarte-Sanchez

Add

y creamos otro grupo arriba a la izquierda

AP-LOBBY
Mella

Add

,,

Click en ap-biblioteca 
wlans
add new (Mover a la derecha)

seleccionamos DUARTE

add


otra vez Wlan
add new
seleccionamos SANCHEZ
add

APPLY
ok
,,,,,,,,,

APs
Hailitamos AP-Bilioteca
Add APs 
ok


BACK

Click en ap-loby
Wlans arriba
add nwe

Mella
add

APs arriba

habilitamos ap-lobby a la izquierda 
add aps
ok

Save configuration arriba del todo luego OKKK

zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz
verificamos dhcp en las pcs de abajo y agregamos a las 
laptops Sanchez-w:
192.168.32.22
255.255.255.0
192.168.32.1
192.168.32.5
LUEGO
Config_wireless0
SSID defaul:Sanchez
Activamos: WPA2-PSK prhase: cisco123

laptops Duarte-w:
192.168.32.20
255.255.255.0
192.168.32.1
192.168.32.5
LUEGO
Config_wireless0
SSID defaul:Duarte
Activamos: WPA2-PSK prhase: cisco123

laptops Mella-w:
192.168.32.21
255.255.255.0
192.168.32.1
192.168.32.5
LUEGO
Config_wireless0
SSID defaul:Sanchez
Activamos: WPA2-PSK prhase: cisco123
