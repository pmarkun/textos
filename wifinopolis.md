# Instalando o Wifinopolis

## Instalando o OpenWRT

Instale o firmware do OpenWRT (stable) no seu router.
Siga as instruções da wiki.
Configure a senha do root para phrack2010.
Conecte o roteador na internet via porta WAN.

## Atualize e instale os pacotes necessários
	opkg update
	opkg install olsrd olsrd-mod-dyn-gw olsrd-mod-txtinfo nano kmod-ipip luci-app-olsr luci-app-olsr-viz

## /etc/config/network

config wifi-device radio0
				option disabled 0

config interface 'mesh'
        option proto 'static'
        option ipaddr '10.0.0.4'
        option netmask '255.0.0.0'
        option dns '208.67.222.222 208.67.220.220'


## /etc/config/wireless

config wifi-iface
        option device   radio0
        option network  lan
        option mode     ap
        option ssid     wifinopolis
        option encryption none

config wifi-iface
        option device radio0
        option network mesh
        option mode adhoc
        option ssid wifinopolis.org
        option hidden 1
        option encryption none

## /etc/config/oslrd

config olsrd
        option IpVersion '4'

config LoadPlugin
        option library 'olsrd_dyn_gw.so.0.5'

config LoadPlugin
        option library 'olsrd_txtinfo.so.0.1'
        option accept '0.0.0.0'

config Interface
        list interface 'mesh'

## /etc/config/firewall

config zone
        option name 'mesh'
        option network 'mesh'
        option input 'ACCEPT'
        option output 'ACCEPT'
        option forward 'ACCEPT'
        option masq '1'

config forwarding
        option dest 'wan'
        option src 'mesh'

config forwarding
        option dest 'mesh'
        option src 'lan'