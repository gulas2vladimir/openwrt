# OpenWRT

## tcp tunning and conntrack: generic helper won't handle protocol 47. fix
```sh
opkg install kmod-tcp-bbr kmod-nf-nathelper-extra stubby
```
## enable HTTPS on uhttpd

```sh
opkg update
opkg install luci-lib-px5g px5g-standalone libustream-openssl
/etc/init.d/uhttpd restart
```
## DNS Recursive

```sh
cp stubby /etc/config/
cp stubby.yml /etc/stubby/
/etc/init.d/stubby enable
/etc/init.d/stubby restart
```

## Force dhcp ( TP-Link powerline overwrite default GW on dhcp clients )

```sh
uci set dhcp.lan.force='1'
uci set dhcp.@dnsmasq[0].server='127.0.0.1#5453'
uci add_list dhcp.lan.dhcp_option='3,192.168.0.1'
uci add_list dhcp.lan.dhcp_option='6,192.168.0.1'
uci commit dhcp
/etc/init.d/dnsmasq restart
```

```
3 - gw
6 - DNS
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
