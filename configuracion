configuracion del ISP IOU1 

conf t
interface ethernet 0/0
 ip address 200.24.14.1 255.255.255.252
 no shutdown
interface ethernet 0/1
 ip address 200.24.14.5 255.255.255.252
 no shutdown
exit

GUI de Fortigate (ambos) 

config system interface
    edit port1
        set mode dhcp
        set allowaccess http https ssh ping
    next
end

Configuración WAN y Rutas Estáticas
En FortiGate 1 (Lado A):

config system interface
    edit port2
        set alias WAN_ISP
        set ip 200.24.14.2 255.255.255.252
        set allowaccess ping
    next
end

config router static
    edit 1
        set gateway 200.24.14.1
        set device port2
    next
end

En FortiGate 2 (Lado B):

config system interface
    edit port2
        set alias WAN_ISP
        set ip 200.24.14.6 255.255.255.252
        set allowaccess ping
    next
end

config router static
    edit 1
        set gateway 200.24.14.5
        set device port2
    next
end

Configuración de redes LAN
En FortiGate 1 (Lado A):

config system interface
    edit port3
        set alias LAN_OFFICE
        set ip 10.24.14.1 255.255.255.0
        set allowaccess ping
    next
end

En FortiGate 2 (Lado B):

config system interface
    edit port3
        set alias LAN_OFFICE
        set ip 10.14.14.1 255.255.255.0
        set allowaccess ping
    next
end

Túnel VPN (Site-to-Site)
En FortiGate 1 (Lado A)

config vpn ipsec phase1-interface
    edit "VPN_TO_SITEB"
        set interface "port2"
        set peertype any
        set net-device disable
        set proposal aes128-sha256 aes256-sha256
        set remote-gw 200.24.14.6
        set psksecret itla2024
    next
end

config vpn ipsec phase2-interface
    edit "VPN_TO_SITEB_P2"
        set phase1name "VPN_TO_SITEB"
        set proposal aes128-sha1 aes256-sha1
        set src-subnet 10.24.14.0 255.255.255.0
        set dst-subnet 10.14.14.0 255.255.255.0
    next
end

# IMPORTANTÍSIMO: Las políticas para permitir el tráfico
config firewall policy
    edit 1
        set name "LAN_to_VPN"
        set srcintf "port3"
        set dstintf "VPN_TO_SITEB"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
    next
    edit 2
        set name "VPN_to_LAN"
        set srcintf "VPN_TO_SITEB"
        set dstintf "port3"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
    next
end

En FortiGate 2 (Lado B)

config vpn ipsec phase1-interface
    edit "VPN_TO_SITEA"
        set interface "port2"
        set peertype any
        set net-device disable
        set proposal aes128-sha256 aes256-sha256
        set remote-gw 200.24.14.2
        set psksecret itla2024
    next
end

config vpn ipsec phase2-interface
    edit "VPN_TO_SITEA_P2"
        set phase1name "VPN_TO_SITEA"
        set proposal aes128-sha1 aes256-sha1
        set src-subnet 10.14.14.0 255.255.255.0
        set dst-subnet 10.24.14.0 255.255.255.0
    next
end

# Políticas para el Lado B
config firewall policy
    edit 1
        set name "LAN_to_VPN"
        set srcintf "port3"
        set dstintf "VPN_TO_SITEA"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
    next
    edit 2
        set name "VPN_to_LAN"
        set srcintf "VPN_TO_SITEA"
        set dstintf "port3"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
    next
end

FG-1:

config router static
    edit 2
        set dst 10.14.14.0 255.255.255.0
        set device "VPN_TO_SITEB"
    next
end

FG-2:

config router static
    edit 2
        set dst 10.24.14.0 255.255.255.0
        set device "VPN_TO_SITEA"
    next
end

