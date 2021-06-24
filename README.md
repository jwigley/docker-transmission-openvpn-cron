# transmission-openvpn-cron

Adds a cron schedule to [docker-transmission-openvpn](https://github.com/haugene/docker-transmission-openvpn/) using 
[torrentcontrol](https://gist.github.com/greenarrow/642874), to provide greater control over scheduled downloads,
 than what transmission provides out of the box.

## Docker Repository

[https://hub.docker.com/r/jwigley/transmission-openvpn-cron/](jwigley/transmission-openvpn-cron)

## Run

See [docker-transmission-openvpn](https://github.com/haugene/docker-transmission-openvpn) for full run instructions.

```bash
docker run \
    --cap-add=NET_ADMIN \
    --device=/dev/net/tun \
    -d \
    -v /docker/transmission/resolv.conf:/etc/resolv.conf \
    -v /docker/transmission/:/data \
    -e "OPENVPN_PROVIDER=xxxx" \
    -e "OPENVPN_CONFIG=xxxx" \
    -e "OPENVPN_USERNAME=xxxx" \
    -e "OPENVPN_PASSWORD=xxxx" \
    -e "LOCAL_NETWORK=192.168.0.0/24" \
    -e "OPENVPN_OPTS=--inactive 3600 --ping 10 --ping-exit 60" \
    -e "PGID=100" \
    -e "PUID=1234" \
    -e "TZ=UTC" \
    -p 9091:9091 \
    --sysctl net.ipv6.conf.all.disable_ipv6=0 \
    --name "transmission-openvpn-cron" \
    jwigley/transmission-openvpn-cron:latest
```

Be sure to set `TZ` to ensure the schedule runs in your timezone.

## Default Torrenting Schedule

By default the cronjob starts torrents at 00:00 (midnight) everyday and stops them at 06:00 in the morning.

### Overriding

The default cronkob can be overidden by mounting your cron job to `/etc/cron.d/torrent-sched-cron`.

```bash
-v $PWD/override-cron:/etc/cron.d/torrent-sched-cron
```
