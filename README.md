# shadowsocks-client
![](https://img.shields.io/docker/automated/tedostrem/shadowsocks-client.svg)
![](https://img.shields.io/docker/build/tedostrem/shadowsocks-client.svg)

Dockerized Shadowsocks client with HTTP proxy and support for salsa20/chacha20.

Exposes a privoxy HTTP proxy which can be set as system proxy or 
used by other docker containers that need proxying through shaodowsocks.

Also check out the compatible [shadowsocks-server](https://github.com/tedostrem/shadowsocks-server)

## Install helper script
```
$ edit ./proxy              # Insert your server details at the top of the file. 
$ echo `make` >> ~/.bashrc  # Will install script to ~/bin and set your path accordingly.
```
*You probbly want to persist your path in your .bashrc*

## Helper script usage
```
$ eval `proxy on`    # Will start shadowsocks-client container and set http_proxy environment variables.
$ eval `proxy off`   # Removes container and unsets http_proxy environment variables.
```
*The first time the script is run it will take some time to download image from
docker hub. Output is redirected to /dev/null since stdout is used to eval 
environment variables once container is started.*

## Run a proxied docker container
```
$ docker run -it `proxy docker` ubuntu:14.04 bash # Runs a proxied docker container. 
```

## Running manually
```
docker run -d \
	--name shadowsocks-client \
	-p ${HTTP_PROXY_PORT}:8118 
	tedostrem/shadowsocks-client \
		-b 0.0.0.0 \
		-s ${SS_SERVER_ADDRESS} \
		-p ${SS_SERVER_PORT} \
		-l ${SS_SOCKS5_PORT} \
		-k ${SS_PASSWORD} \
		-m ${SS_ENCRYPTION_METHOD}
```

## Manually run a proxied container
```
docker run -it --link shadowsocks-client \
	-e http_proxy=shadowsocks:8118 \
	-e https_proxy=shadowsocks:8118 \
	centos bash
```
