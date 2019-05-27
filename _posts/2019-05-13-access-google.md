---
title: Access Google
categories: [network, tutorial]
---

Google services are blocked in China which is very inconvenient for researchers. In this post, I will introduce two methods that I used to access Google.

## Access Google with IPv6 and Hosts

The [CERNET](http://www.edu.cn/) in school provides the IPv6 service, which can be used to access most of Google products without any proxy or VPN. In this way, you just need to redirect the HTTP/HTTPS requests to the IPv6 address of the original website with a `hosts` file. Follow is what I do to access Google.

### Update Hosts File

The [`hosts`](https://en.wikipedia.org/wiki/Hosts_(file)) file is used to redirect website access with an IP address. The file location is different between different operating systems. You can copy and paste new hosts file to replace it directly, but I recommend to use an app to manage it, like [SwitchHosts](https://github.com/oldj/SwitchHosts) which provides a user-friendly GUI. Based on your computer's operating system, you can download an install binary in its [here](https://github.com/oldj/SwitchHosts/releases).

After installing the app, you need a new hosts file to update the original one. I use [hosts-ipv6](https://github.com/googlehosts/hosts-ipv6) where the hosts file is [here](https://github.com/googlehosts/hosts-ipv6/blob/master/hosts-files/hosts), but what you need to do is copy the URL of the raw file (`https://raw.githubusercontent.com/googlehosts/hosts-ipv6/master/hosts-files/hosts`) and apply it as a remote hosts, then enable it.

![Switch Hosts](https://raw.githubusercontent.com/lynn9388/lynn9388.github.io/master/assets/img/post/switch-hosts.png)

### Connect to IPv6 Network

As I'm studying in HUST where the network in the lab and dormitory all provide IPv6, it's quite simple to do, just connect to the network and check if computer's network adapter get an IPv6 address in network settings, or visit [test-ipv6](https://test-ipv6.com/) to see if IPv6 is accessible.

![Test IPv6](https://raw.githubusercontent.com/lynn9388/lynn9388.github.io/master/assets/img/post/test-ipv6.png)

### Visit Google

After following the above steps, you should be able to access [Google](https://www.google.com/) now. Next time, just connect to the IPv6 network and you are ready to go.

## Access Google with Shadowsocks and Heroku

The method that I introduced above is quite easy and fast, but it requests IPv6 network access which is inaccessible for me when I am out of school. So I will introduce a more general method to create a private proxy server with [shadowsocks-heroku](https://github.com/onplus/shadowsocks-heroku) here. Actually, you can find an instruction in the project's repository, but you can also follow the steps showed below.

### Deploy Shadowsocks Server

1. Create a free Heroku account [here](https://signup.heroku.com/) if you don't have one.
1. Deploy server [here](https://heroku.com/deploy?template=https://github.com/onplus/shadowsocks-heroku/tree/re), an example is set as below
    - App name: `lynn-example`
      - any valid name is ok
    - Choose a region: `United States`
      - `Europe` is also fine, you can deploy multiple servers with different region then compare the speed
    - KEY: `PASSWORD`
      - set your own private password to access server
    - METHOD: `aes-256-cfb`
      - the default algorithm is ok, and more supported ciphers can be checked [here](https://github.com/mrluanma/shadowsocks-heroku#supported-ciphers)

### Config Local Client

1. Download the local client [here](https://github.com/onplus/shadowsocks-heroku/releases) based on your computers operating system
1. Config `config.json` based on your server's settings, below is a example

    ```json
    {
    "server": "lynn-example.herokuapp.com",
    "local_address": "127.0.0.1",
    "scheme": "ws",
    "local_port": "1080",
    "remote_port": "80",
    "password": "PASSWORD",
    "timeout": 600,
    "method": "aes-256-cfb"
    }
    ```

### Config Proxy

As you can see in `config.json`, the proxy configuration is pretty clear which should be configured in other apps as below

- Protocol: `SOCKS5`
- IP: `127.0.0.1`
  - same as `local_address` in config.json
- Port: `1080`
  - same as `local_port` in config.json

Most time, I only need proxy for chrome browser, and I recommend [SwitchyOmega](https://github.com/FelisCatus/SwitchyOmega) to manage the proxy.

### Launch Local Client

After finishing the above steps, you only need to launch the local client when you want to access Google.