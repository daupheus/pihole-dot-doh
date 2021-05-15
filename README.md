# pihole-dot-doh
Official pihole docker with both DoT (DNS over TLS) and DoH (DNS over HTTPS) clients.

## Usage:
For docker parameters, refer to [official pihole docker readme](https://github.com/pi-hole/pi-hole). Below is an Unraid example.

    docker run -d \
    --name='pihole-dot-doh' \
    --cap-add=NET_ADMIN \
    --restart=unless-stopped \
    --net='bridge' \
	--dns=9.9.9.9 --dns=1.1.1.1 \
    -e TZ="Europe/Paris" \
    -e HOST_OS="PIHOLE" \
    -v '/home/docker/pihole-dot-doh/pihole/':'/etc/pihole/':'rw' \
    -v '/home/docker/pihole-dot-doh/dnsmasq.d/':'/etc/dnsmasq.d/':'rw' \
    -v '/home/docker/pihole-dot-doh/config/':'/config':'rw' \
    -e 'DNS1'='127.1.1.1#5153' \
    -e 'DNS2'='127.2.2.2#5253' \
    -e 'TZ'='Europe/Paris' \
    -e 'WEBPASSWORD'='PASSWORD!' \
    -e 'INTERFACE'='br0' \
    -e 'ServerIP'='10.xxx.xxx.xxx' \
    -e 'ServerIPv6'='' \
    -e 'IPv6'='False' \
    -e 'DNSMASQ_LISTENING'='all' \
    -p '53:53/tcp' \
    -p '53:53/udp' \
    -p '10067:67/udp' \
    -p '10080:80/tcp' \
    'daupheus/pihole-dot-doh:latest'

### Notes:
* Remember to set pihole env DNS1 and DNS2 to use the DoH / DoT IP below. If either DNS1 or DNS2 is NOT set, Pihole will use a non-encrypted service.
  * DoH service (cloudflared) runs at 127.1.1.1#5153. Uses cloudflare (1.1.1.1 / 1.0.0.1) by default
  * DoT service (stubby) runs at 127.2.2.2#5253. Uses quad9 (9.9.9.9) by default
  * To use just DoH or just DoT service, set both DNS1 and DNS2 to the same value. 
* In addition to the 2 official paths, you can also map container /config to expose configuration yml files for cloudflared (cloudflared.yml) and stubby (stubby.yml).
  * Edit these files to add / remove services as you wish. The flexibility is yours.
* Credits:
  * Pihole (buster) base image is the official [pihole/pihole:master-buster](https://hub.docker.com/r/pihole/pihole/tags?page=1&name=master-buster)
  * Cloudflared client was obtained from [official site](https://developers.cloudflare.com/argo-tunnel/downloads)
  * Stubby is a standard debian buster package
