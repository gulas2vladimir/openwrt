# OpenWRT

## tcp tunning
```sh
opkg install kmod-tcp-bbr
```
## enable HTTPS on uhttpd

```sh
opkg update
opkg install luci-lib-px5g px5g-standalone libustream-openssl
/etc/init.d/uhttpd restart
```
## DNS Recursive

```sh
opkg update
opkg install unbound-daemon-heavy luci-app-unbound
uci set unbound.enabled=1
uci set unbound.listen_port=5353
uci set unbound.protocol=ip4_only
uci set unbound.@zone[0].enabled='1'
uci commit unbound
/etc/init.d/unbound restart
```

## Force dhcp ( TP-Link powerline overwrite default GW on dhcp clients )

```sh
uci set dhcp.lan.force='1'
uci set dhcp.lan.dhcp_lists='3,192.168.0.1'
uci set dhcp.lan.dhcp_lists='6,192.168.0.1'
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
