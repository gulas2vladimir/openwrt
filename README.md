# OpenWRT

## enable HTTPS

```bash
opkg update
opkg install luci-lib-px5g px5g-standalone libustream-openssl
/etc/init.d/uhttpd restart
```
## DNS over https

```bash
opkg update
opkg install https-dns-proxy luci-app-https-dns-proxy
```

## Force dhcp ( TP-Link powerline overwrite default GW on dhcp clients )

```bash
uci set dhcp.lan.force='1'
uci set dhcp.lan.dhcp_option='3,192.168.0.1' '6,192.168.0.1'
uci commit dhcp
/etc/init.d/dnsmasq restart
```
