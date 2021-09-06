# notthebee/ikev2
A Docker-ized version of [jawj/IKEV2-setup](https://github.com/jawj/IKEv2-setup), based on [cschlosser/ikev2-docker](https://github.com/cschlosser/ikev2-docker)

This container will set up an IKEv2 VPN server using StrongSwan with a Let's Encrypt certificate and the CNSA/RFC 6379 Suite B cipher.

It will also generate a macOS/iOS .mobileconfig file with on-demand connection enabled by default.

#### Example docker-compose.yml file
```
version: '3'
services:

  ikev2-vpn:
    container_name: ikev2
    image: notthebee/ikev2
    ports:
      - 4500:4500/udp
      - 500:500/udp
      - 80:80/tcp # only needed during the first run for Let's Encrypt certificates, feel free to remove it afterwards
    privileged: yes
    volumes:
      - /docker-persistent-data/ikev2/config:/config
      - /docker-persistent-data/ikev2/letsencrypt:/etc/letsencrypt
    environment:
      - VPNHOST=vpn.example.com
      - EMAILADDR=example@mail.com
      - VPNUSERNAME=example
      - VPNPASSWORD=example
```

All credits go to [jawj](https://github.com/jawj) and [cschlosser](https://github.com/cschlosser)
