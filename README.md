shadowsocks-openwrt
===================

A package of shadowsocks for OpenWrt

### latest version: shadowsocks-libev-1.0

## Background
This is a OpenWrt's package description for [shadowsocks-libev](https://github.com/madeye/shadowsocks-libev)

## Build from source
Download OpenWrt source from [dev][] or SDK from [downloads][]. And go to the root of the SDK or source. e.g.:

    [OpenWrt-SDK]$ ls -l
    total 76
    -rw-r--r-- 1 haohaolee users    32 Aug 16  2011 Config.in
    drwxr-xr-x 2 haohaolee users  4096 Dec 30 03:16 dl
    drwxr-xr-x 2 haohaolee users  4096 Nov 26 11:41 docs
    -rw-r--r-- 1 haohaolee users   567 Nov 26 19:03 feeds.conf.default
    drwxr-xr-x 3 haohaolee users  4096 Nov 26 19:03 include
    -rw-r--r-- 1 haohaolee users 17992 Aug 16  2011 LICENSE
    -rw-r--r-- 1 haohaolee users  1161 Aug 16  2011 Makefile
    drwxr-xr-x 4 haohaolee users  4096 Dec 28 18:12 package
    -rw-r--r-- 1 haohaolee users   337 Aug 16  2011 README.SDK
    -rw-r--r-- 1 haohaolee users  9563 Nov 26 11:41 rules.mk
    drwxr-xr-x 4 haohaolee users  4096 Nov 26 11:41 scripts
    drwxr-xr-x 5 haohaolee users  4096 Nov 26 19:03 staging_dir
    drwxr-xr-x 3 haohaolee users  4096 Nov 26 19:03 target
    
    [OpenWrt-SDK]$ git clone https://github.com/madeye/shadowsocks-openwrt.git package/shadowsocks-openwrt
    ...
    [OpenWrt-SDK]$ make package/shadowsocks-openwrt/shadowsocks-libev/compile
    ...
    
Finally find your package in dir bin

## Prebuilt ipk

You can download the latest prebuilt packages from http://buildbot.sinaapp.com. Currently, we only provide prebuilt packages for ar71xx and bcm47xx platforms.

## Basic usage

Log into OpenWrt via SSH and edit the config file `/etc/config/shadowsocks.json`. Then start the service like this:

    root@Wrt:~# /etc/init.d/shadowsocks start # start the daemon
    root@Wrt:~# /etc/init.d/shadowsocks enable # enable startup at boot
    
## Advance usage

The latest shadowsocks-libev has provided a transparent mode. You can configure your router with IPTABLES to proxy all tcp traffic transparently.

    # Create new chain
    root@Wrt:~# iptables -t nat -N SHADOWSOCKS
    
    # Ignore your shadowsocks server's addresses
    # It's very IMPORTANT, just be careful.
    root@Wrt:~# iptables -t nat -A SHADOWSOCKS -d 123.123.123.123 -j RETURN

    # Ignore LANs and any other addresses you'd like to bypass the proxy
    # See Wikipedia and RFC5735 for full list of reserved networks.
    # See ashi009/bestroutetb for a highly optimized CHN route list.
    root@Wrt:~# iptables -t nat -A SHADOWSOCKS -d 0.0.0.0/8 -j RETURN
    root@Wrt:~# iptables -t nat -A SHADOWSOCKS -d 10.0.0.0/8 -j RETURN
    root@Wrt:~# iptables -t nat -A SHADOWSOCKS -d 127.0.0.0/8 -j RETURN
    root@Wrt:~# iptables -t nat -A SHADOWSOCKS -d 169.254.0.0/16 -j RETURN
    root@Wrt:~# iptables -t nat -A SHADOWSOCKS -d 172.16.0.0/12 -j RETURN
    root@Wrt:~# iptables -t nat -A SHADOWSOCKS -d 192.168.0.0/16 -j RETURN
    root@Wrt:~# iptables -t nat -A SHADOWSOCKS -d 224.0.0.0/4 -j RETURN
    root@Wrt:~# iptables -t nat -A SHADOWSOCKS -d 240.0.0.0/4 -j RETURN

    # Anything else should be redirected to port 12345
    root@Wrt:~# iptables -t nat -A SHADOWSOCKS -p tcp -j REDIRECT --to-ports 12345
    
    # Apply the rules
    root@Wrt:~# iptables -t nat -A OUTPUT -p tcp -j REDSOCKS
    
    # Start the shadowsocks-redir
    root@Wrt:~# ss-redir -c /etc/config/shadowsocks.json -f /var/run/shadowsocks.pid

[dev]: https://dev.openwrt.org
[downloads]: http://downloads.openwrt.org/
