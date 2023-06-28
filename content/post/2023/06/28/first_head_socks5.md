---
title: "First head Socks5"
date: 2023-06-28
tags: ["Socks5", "Proxy"]
draft: false
image: "/post/2023/06/28/socks5_proxy.jpg"
---

![Socks5 Proxy](/post/2023/06/28/socks5_proxy.jpg)

## What is Socks protocol?

SOCKS is an Internet protocol that exchanges network packets between a client and server through a proxy server.

## What is Socks5 protocol?

SOCKS5 optionally provides authentication so only authorized users may access a server. Practically, a SOCKS server proxies TCP connections to an arbitrary IP address, and provides a means for UDP packets to be forwarded.

## How to use Socks5?

### Setup a Socks5 server

I created an Ubuntu 22.04 VM on AWS EC2 and used the `docker` to speed up the setup.

[Socks server docker image](https://hub.docker.com/r/serjs/go-socks5-proxy)


Run commands below to start a Socks server:

```shell
sudo docker run -d --name socks5 -p 1080:1080 -e PROXY_USER=<PROXY_USER> -e PROXY_PASSWORD=<PROXY_PASSWORD> serjs/go-socks5-proxy

```

**Notes:** make sure the firewall is enable to the port you specific.

### Test Socks5 with `curl`

On the client side, run commands below:

```shell
curl --socks5 <Socks Server IP>:1080 https://ifconfig.me/ip -U <PROXY_USER>:<PROXY_PASSWORD>
```

**Notes:** The websites to test your public IP:

- [ifcfg.co](https://ifcfg.co/)
- [ifconfig.me](https://ifconfig.me/)

### Test Socks5 with client

I've written a simple client for Socks5 client. Here is the [demo](https://github.com/HankLi0130/socks5-client-demo).
