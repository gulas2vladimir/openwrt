# OpenWRT

## tcp tunning and conntrack: generic helper won't handle protocol 47. fix
```sh
opkg install kmod-tcp-bbr kmod-nf-nathelper-extra
```
## enable HTTPS on uhttpd

```sh
opkg update
opkg install luci-lib-px5g px5g-standalone libustream-openssl
/etc/init.d/uhttpd restart
```
## DNS Recursive

```sh
opkg install stubby
cp stubby /etc/config/
cp stubby.yml /etc/stubby/
/etc/init.d/stubby enable
/etc/init.d/stubby restart
```

## Force dhcp ( TP-Link powerline overwrite default GW on dhcp clients )

```sh
uci set dhcp.lan.force='1'
uci set dhcp.@dnsmasq[0].server='127.0.0.1#5353'
uci add_list dhcp.lan.dhcp_option='3,192.168.0.1'
uci add_list dhcp.lan.dhcp_option='6,192.168.0.1'
uci commit dhcp
/etc/init.d/dnsmasq restart
```

```
3 - gw
6 - DNS
```

## all DNS queries to another server like pi-hole
/etc/firewall.user

```sh
iptables -t nat -A POSTROUTING -j MASQUERADE
iptables -t nat -I PREROUTING -i br-lan -p tcp --dport 53 -j DNAT --to 192.168.0.2:53
iptables -t nat -I PREROUTING -i br-lan -p udp --dport 53 -j DNAT --to 192.168.0.2:53
iptables -t nat -I PREROUTING -i br-lan -p tcp -s 192.168.0.2 --dport 53 -j ACCEPT
iptables -t nat -I PREROUTING -i br-lan -p udp -s 192.168.0.2 --dport 53 -j ACCEPT
```
## upgrade all packages each file separately
```sh
#!/bin/sh
#
opkg update

upgrade_list="$(opkg list-upgradable | cut -f 1 -d ' ')"

if [ -n "$upgrade_list" ]; then
  for pkg in $upgrade_list; do
    echo "upgrade $pkg"
    opkg install $pkg
  done
fi
```
