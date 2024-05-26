# shadowsocks_linux

* This demo record how to set shadowsocks on the vultr server and local machine

* Need: vultr server

1. First: install and set shadowsocks on vultr

1.1 run the command
`sudo apt update && sudo apt upgrade -y`
`sudo apt install shadowsocks-libev -y`

1.2 Configure the json file
`vi /etc/shadowsocks-libev/config.json`

an example:

```
{
    "server":"0.0.0.0",
    "server_port":8388,
    "password":"your_password",
    "timeout":300,
    "method":"aes-256-cfb"
}
```

1.3 start the service
`sudo systemctl start shadowsocks-libev`
`sudo systemctl enable shadowsocks-libev`

2. Configure on the local linux machine

2.1 download the shadowsocks
`sudo apt install shadowsocks-libev -y`

2.2 edit the config
`vi ~/shadowsocks-client.json`

an example for local setting:
```
{
    "server":"your_vultr_server_ip",
    "server_port":8388,
    "local_port":1080,
    "password":"your_password",
    "timeout":300,
    "method":"aes-256-cfb"
}
```

2.3 start the service on local machine
`ss-local -c ~/shadowsocks-client.json`

3. test whether it success

`curl -x socks5h://127.0.0.1:1080 https://www.google.com -I`

4. Note
If fails, maybe it is due to the port is not open on firmwall

on the server:
`sudo ufw allow 8388/tcp`
`sudo ufw allow 8388/udp`

5. add socks5 to github
```
git config --global http.proxy socks5h://127.0.0.1:1080
git config --global https.proxy socks5h://127.0.0.1:1080
```

delete proxy:
git config --global --unset http.proxy
git config --global --unset https.proxy
