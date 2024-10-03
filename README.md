## Docker image for Synology's DSM7

Official documentation: [docs.zerotier.com/devices/synology](https://docs.zerotier.com/devices/synology)

# Commands

Start ZeroTier:
```bash
sudo docker run -d --name zt --restart=always --device=/dev/net/tun --net=host --cap-add=NET_ADMIN --cap-add=SYS_ADMIN -v /var/lib/zerotier-one:/var/lib/zerotier-one sandros94/zerotier-synology:latest
```

To update ZeroTier you have to remove the old container first and then pull the latest image:
```bash
sudo docker ps
```
Example output:
```
CONTAINER ID   IMAGE                               COMMAND          CREATED       STATUS      PORTS     NAMES
52c7cb58a1dd   zerotier/zerotier-synology:latest   "zerotier-one"   5 weeks ago   Up 9 days             zt
```

Stop the old container and remove it:
```bash
sudo docker stop 52c7cb58a1dd && docker container rm 52c7cb58a1dd
```

Pull the latest image and then use the command above to start it:
```bash
sudo docker pull sandros94/zerotier-synology:latest
```

## Build

```bash
docker build --no-cache --load --rm -t sandros94/zerotier-synology:34cc261 . --build-arg ZTO_COMMIT=34cc26176c8d1a2c49935928d7297a9859ab2023 --build-arg ZTO_VER=1.14.0
```
