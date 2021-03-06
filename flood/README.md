# flood

## Tag available
* latest [(Dockerfile)](https://github.com/xataz/dockerfiles/tree/master/flood/Dockerfile)

## Description
What is [Flood](https://github.com/jfurrow/flood)?
Flood is another web interface for rtorrent. It implements a Node.js server for communicating with the rTorrent API, storing historical data, and serving the web UI.

**This image not contains root process**

## Build Image
```shell
docker build -t xataz/flood github.com/xataz/dockerfiles.git#master:flood
```

## Configuration
### Environments
* UID : Choose uid for launch flood (default : 991)
* GID : Choose gid for launch flood (default : 991)
* RTORRENT_HOST : rtorrent name image (default : torrent)
* RTORRENT_PORT : rtorrent scgi port (default : 5000)
* RTORRENT_SOCKET : enable or not socker for rtorrent (default : false)
* RTORRENT_SOCKET_PATH : path of rtorrent socket if use (default : /tmp/rtorrent.sock)
* FLOOD_SECRET : secret ? (default : flood)
* FLOOD_START : Start flood in develop, production or watch mode (default : production)

### Volumes
* /usr/app/src/server/db

### Ports
* 3000

## Usage
```shell
$ docker run -d --name flood --link torrent:torrent xataz/flood
```

docker-compose.yml example :
```yaml
version: '2'

networks:
  torrent:
    driver: bridge

services:
  rtorrent:
    image: xataz/rtorrent
    environment:
      - UID=1000
      - GID=1000
    tty: true
    networks:
      - torrent

  flood:
    image: xataz/flood
    environment:
      - UID=1000
      - GID=1000
      - RTORRENT_HOST=rtorrent
      - RTORRENT_PORT=5000
      - FLOOD_SECRET=flood
    ports:
      - "3000:3000"
    networks:
      - torrent
```

run with :
```shell
$ docker-compose up -d
```