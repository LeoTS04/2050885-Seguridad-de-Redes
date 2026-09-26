https://itlaedudo-my.sharepoint.com/:v:/g/personal/20250885_itla_edu_do/IQBAX9BwL8YLQY336T9_7xiaAWozJkIidy9HGIPkULZSILA?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0NvcHkifX0&e=MuLcZM



Documentacion

Estudiante: Emir Trinidad
Asignatura: Seguridad de redes
Fecha: 25/9/2026

En este laboratorio se implementó una infraestructura de red virtual utilizando GNS3, FortiGate, máquinas virtuales Ubuntu y un switch. El objetivo principal es configurar una red segmentada mediante VLAN, proporcionar conectividad entre usuarios y servidores, y aplicar diferentes mecanismos de seguridad mediante el firewall FortiGate.

La infraestructura cuenta con una red para usuarios, una red destinada a servidores WEB y DB, servicios DHCP, NAT, políticas de firewall, inspección de tráfico, protección contra ataques y mecanismos de registro de eventos

Diseño de la Topologia:

<img width="462" height="553" alt="image" src="https://github.com/user-attachments/assets/625a70dd-74f3-486d-bfc1-dacc96413399" />

La topología esta formada por una nube simulando un ISP, un fortigate, un switch cisco, una maquina virtual simulando un usuario y dos maquinas virutales simulando servidores de web y base de datos

Plan de direccionamiento

Ruta estatica: 0.0.0.0/0.0.0.0 192.168.85.30

Fortigate:

port1: 192.168.85.129/24
    
port2:

- Vlan 10 (usuarios): 10.8.85.1/25

- vlan 20 (servidores): 85.85.85.1/28

Switch cisco(SW1): 
             vlan 10: 10.8.85.10/25

Usuario:
    DHCP: 10.8.85.2/25

Web-Server:
    Manual: 85.85.85.3/28

DB-Server:
    Manual: 85.85.85.4/28

<img width="1663" height="261" alt="image" src="https://github.com/user-attachments/assets/4eb85a31-9cbf-41f5-a18b-9f720bfbc3bf" />

Politicas de firewall:

Se crearon 5 politicas:

1. Permitir la vlan usuarios conectarse al servidor web
2. Bloquear a los usuarios conectarse al servidor DB
3. Permitir el servidor web conexión al DB por el puerto 3306
4. Bloquear que el Web server conecte por otro puerto al DB
5. Politica de NAT

<img width="1666" height="380" alt="image" src="https://github.com/user-attachments/assets/fdfdf244-25ed-4b0c-8622-71a836e5159d" />

Se creo un perfil de seguridad IPS que bloquea los ataques de SQL injection y coloca al atacante en cuarentena

<img width="1001" height="568" alt="image" src="https://github.com/user-attachments/assets/a05ed93f-a2e5-4385-806a-24360be54ab1" />

Se creo un perfil de seguridad de filtración de archivos que bloquea las descargas de archivos .exe desde el servidor web

<img width="993" height="557" alt="image" src="https://github.com/user-attachments/assets/e758e483-1fdd-4a8a-9638-4f884114a2c6" />

Y se crearon politicas IPv4 que bloquean los ataques DoS

<img width="981" height="832" alt="image" src="https://github.com/user-attachments/assets/e2edf493-f705-4859-a80b-65be0a0b7202" />

Show Running config SW1:

SW1#show run
Building configuration...

Current configuration : 3792 bytes
!
! Last configuration change at 22:45:30 UTC Fri Sep 25 2026
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
service compress-config
!
hostname SW1
!
boot-start-marker
boot-end-marker
!
!
!
username admin secret 5 $1$uFow$rLo5ZGUruMoMUOSh.0DC81
no aaa new-model
!
!
!
!
!
!
!
!
no ip domain-lookup
ip domain-name empresa.local
ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
interface GigabitEthernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/1
 switchport access vlan 10
 switchport mode access
 negotiation auto
!
interface GigabitEthernet0/2
 switchport access vlan 20
 switchport mode access
 negotiation auto
!
interface GigabitEthernet0/3
 switchport access vlan 20
 switchport mode access
 negotiation auto
!
interface GigabitEthernet1/0
 negotiation auto
!
interface GigabitEthernet1/1
 negotiation auto
!
interface GigabitEthernet1/2
 negotiation auto
!
interface GigabitEthernet1/3
 negotiation auto
!
interface GigabitEthernet2/0
 switchport access vlan 20
 switchport mode access
 negotiation auto
!
interface GigabitEthernet2/1
 negotiation auto
!
interface GigabitEthernet2/2
 negotiation auto
!
interface GigabitEthernet2/3
 negotiation auto
!
interface GigabitEthernet3/0
 negotiation auto
!
interface GigabitEthernet3/1
 negotiation auto
!
interface GigabitEthernet3/2
 negotiation auto
!
interface GigabitEthernet3/3
 negotiation auto
!
interface Vlan10
 ip address 10.8.85.10 255.255.255.128
!
ip forward-protocol nd
!
ip http server
ip http secure-server
!
ip ssh version 2
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
!
!
!
!
control-plane
!
banner exec ^C
IOSv - Cisco Systems Confidential -

Supplemental End User License Restrictions

This IOSv software is provided AS-IS without warranty of any kind. Under no circumstances may this software be used separate from the Cisco Modeling Labs Software that this software was provided with, or deployed or used as part of a production environment.

By using the software, you agree to abide by the terms and conditions of the Cisco End User License Agreement at http://www.cisco.com/go/eula. Unauthorized use or distribution of this software is expressly prohibited.
^C
banner incoming ^C
IOSv - Cisco Systems Confidential -

Supplemental End User License Restrictions

This IOSv software is provided AS-IS without warranty of any kind. Under no circumstances may this software be used separate from the Cisco Modeling Labs Software that this software was provided with, or deployed or used as part of a production environment.

By using the software, you agree to abide by the terms and conditions of the Cisco End User License Agreement at http://www.cisco.com/go/eula. Unauthorized use or distribution of this software is expressly prohibited.
^C
banner login ^C
IOSv - Cisco Systems Confidential -

Supplemental End User License Restrictions

This IOSv software is provided AS-IS without warranty of any kind. Under no circumstances may this software be used separate from the Cisco Modeling Labs Software that this software was provided with, or deployed or used as part of a production environment.

By using the software, you agree to abide by the terms and conditions of the Cisco End User License Agreement at http://www.cisco.com/go/eula. Unauthorized use or distribution of this software is expressly prohibited.
^C
banner motd ^C########## Solo personal AUTORIZADO ##############^C
!
line con 0
 exec-timeout 5 0
 login local
line aux 0
line vty 0 4
 exec-timeout 5 0
 login local
 transport input ssh
!
!
end





