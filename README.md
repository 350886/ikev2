# notthebee/ikev2
A Docker-ized version of [jawj/IKEV2-setup](https://github.com/jawj/IKEv2-setup), based on [cschlosser/ikev2-docker](https://github.com/cschlosser/ikev2-docker)

This container will set up an IKEv2 VPN server using StrongSwan with a Let's Encrypt certificate and the CNSA/RFC 6379 Suite B cipher.

It will also generate a macOS/iOS .mobileconfig file with optional on-demand connection on specified WiFi networks.

If you're behind a NAT (e.g. a home router), you will need to forward ports 80/tcp, 4500/udp and 500/udp

#### Example docker-compose.yml file
```
version: '3'
services:

  ikev2-vpn:
    build: .
    container_name: ikev2
    image: notthebee/ikev2
    ports:
      - 4500:4500/udp
      - 500:500/udp
      - 80:80/tcp 
      # Port 80 is only needed during the first run for
      # Let's Encrypt certs, feel free to remove it afterwards
    privileged: yes
    volumes:
      - /docker-persistent-data/ikev2/config:/config
      - /docker-persistent-data/ikev2/letsencrypt:/etc/letsencrypt
    environment:
      - DNS_SERVERS="1.1.1.1,1.0.0.1" # Optional, defaults to 1.1.1.1
      - VPNHOST=vpn.example.com
      - EMAILADDR=example@mail.com
      - VPNUSERNAME=example
      - VPNPASSWORD=example
      # Pick one of the following if you want the VPN
      # to connect automatically on certain WiFi networks
      - INCLUDE_SSIDS="SSID1, SSID2"
      # or to connect on all WiFi networks except for these
      - EXCLUDE_SSIDS="SSID1, SSID2"
      # ...or nothing at all
```

All credits go to [jawj](https://github.com/jawj) and [cschlosser](https://github.com/cschlosser)
