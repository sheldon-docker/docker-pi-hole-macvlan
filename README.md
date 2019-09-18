# Docker Pi-hole (MacVLAN)

<p align="center">
<a href="https://pi-hole.net"><img src="https://pi-hole.github.io/graphics/Vortex/Vortex_with_text.png" width="150" height="255" alt="Pi-hole"></a><br/>
</p>
<!-- Delete above HTML and insert markdown for dockerhub : ![Pi-hole](https://pi-hole.github.io/graphics/Vortex/Vortex_with_text.png) -->

## Quickstart

To setup your Pi-hole server, use the following instructions:

1. Clone the repository
1. Copy the `.env.sample` file to `.env`
1. Fill in the default environment variables and add any from the list below
1. Start the container with Docker Compose

## Overview

A [Docker](https://www.docker.com/what-docker) project to make a lightweight x86 and ARM container with [Pi-hole](https://pi-hole.net) functionality.

1) Install docker for your [x86-64 system](https://www.docker.com/community-edition) or [ARMv7 system](https://www.raspberrypi.org/blog/docker-comes-to-raspberry-pi/) using those links. [Docker-compose](https://docs.docker.com/compose/install/) is also recommended.
2) Use the above quick start example, customize if desired.
3) Enjoy!

## Running DHCP from Docker Pi-Hole

There are multiple different ways to run DHCP from within your Docker Pi-hole container but it is slightly more advanced and one size does not fit all. DHCP and Docker's multiple network modes are covered in detail on our docs site: [Docker DHCP and Network Modes](https://docs.pi-hole.net/docker/DHCP/)

## Environment Variables

There are other environment variables if you want to customize various things inside the docker container. Add these to your .env file as needed:

| Docker Environment Var. | Description |
| ----------------------- | ----------- |
| `TZ: <Timezone>`<br/> **Recommended** *Default: UTC* | Set your [timezone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) to make sure logs rotate at local midnight instead of at UTC midnight.
| `WEBPASSWORD: <Admin password>`<br/> **Recommended** *Default: random* | http://pi.hole/admin password. Run `docker logs pihole \| grep random` to find your random pass.
| `DNS1: <IP>`<br/> *Optional* *Default: 8.8.8.8* | Primary upstream DNS provider, default is google DNS
| `DNS2: <IP>`<br/> *Optional* *Default: 8.8.4.4* | Secondary upstream DNS provider, default is google DNS, `no` if only one DNS should used
| `DNSSEC: <True\|False>`<br/> *Optional* *Default: false* | Enable DNSSEC support
| `DNS_BOGUS_PRIV: <True\|False>`<br/> *Optional* *Default: true* | Enable forwarding of reverse lookups for private ranges
| `CONDITIONAL_FORWARDING: <True\|False>`<br/> *Optional* *Default: False* | Enable DNS conditional forwarding for device name resolution
| `CONDITIONAL_FORWARDING_IP: <Router's IP>`<br/> *Optional* | If conditional forwarding is enabled, set the IP of the local network router
| `CONDITIONAL_FORWARDING_DOMAIN: <Network Domain>`<br/> *Optional* | If conditional forwarding is enabled, set the domain of the local network router
| `CONDITIONAL_FORWARDING_REVERSE: <Reverse DNS>`<br/> *Optional* | If conditional forwarding is enabled, set the reverse DNS of the local network router (e.g. `0.168.192.in-addr.arpa`)
| `ServerIP: <Host's IP>`<br/> **Recommended** | **--net=host mode requires** Set to your server's LAN IP, used by web block modes and lighttpd bind address
| `ServerIPv6: <Host's IPv6>`<br/> *Required if using IPv6* | **If you have a v6 network** set to your server's LAN IPv6 to block IPv6 ads fully
| `VIRTUAL_HOST: <Custom Hostname>`<br/> *Optional* *Default: $ServerIP*   | What your web server 'virtual host' is, accessing admin through this Hostname/IP allows you to make changes to the whitelist / blacklists in addition to the default 'http://pi.hole/admin/' address
| `IPv6: <True\|False>`<br/> *Optional* *Default: True* | For unraid compatibility, strips out all the IPv6 configuration from DNS/Web services when false.
| `INTERFACE: <NIC>`<br/> *Advanced/Optional* | The default works fine with our basic example docker run commands.  If you're trying to use DHCP with `--net host` mode then you may have to customize this or DNSMASQ_LISTENING.
| `DNSMASQ_LISTENING: <local\|all\|NIC>`<br/> *Advanced/Optional* | `local` listens on all local subnets, `all` permits listening on internet origin subnets in addition to local.
| `WEB_PORT: <PORT>`<br/> *Advanced/Optional* | **This will break the 'webpage blocked' functionality of Pi-hole** however it may help advanced setups like those running synology or `--net=host` docker argument.  This guide explains how to restore webpage blocked functionality using a linux router DNAT rule: [Alternative Synology installation method](https://discourse.pi-hole.net/t/alternative-synology-installation-method/5454?u=diginc)
| `DNSMASQ_USER: <pihole\|root>`<br/> *Experimental Default: root* | Allows running FTLDNS as non-root.

## Docker tags and versioning

The primary docker tags / versions are explained in the following table.  [Click here to see the full list of tags](https://store.docker.com/community/images/pihole/pihole/tags) ([arm tags are here](https://store.docker.com/community/images/pihole/pihole/tags)), I also try to tag with the specific version of Pi-hole Core for version archival purposes, the web version that comes with the core releases should be in the [GitHub Release notes](https://github.com/pi-hole/docker-pi-hole/releases).

| tag                 | architecture | description                                                             | Dockerfile |
| ---                 | ------------ | -----------                                                             | ---------- |
| `latest`            | auto detect  | x86, arm, or arm64 container, docker auto detects your architecture.    | [Dockerfile](https://github.com/pi-hole/docker-pi-hole/blob/master/Dockerfile_amd64) |
| `v4.0.0-1`          | auto detect  | Versioned tags, if you want to pin against a specific version, use one of thesse |  |
| `v4.0.0-1_<arch>`   | based on tag | Specific architectures tags | |
| `dev`       | auto detect  | like latest tag, but for the development branch (pushed occasionally)   | |

### `pihole/pihole:latest` [![](https://images.microbadger.com/badges/image/pihole/pihole:latest.svg)](https://microbadger.com/images/pihole/pihole "Get your own image badge on microbadger.com") [![](https://images.microbadger.com/badges/version/pihole/pihole:latest.svg)](https://microbadger.com/images/pihole/pihole "Get your own version badge on microbadger.com") [![](https://images.microbadger.com/badges/version/pihole/pihole:latest.svg)](https://microbadger.com/images/pihole/pihole "Get your own version badge on microbadger.com")

This version of the docker aims to be as close to a standard Pi-hole installation by using the recommended base OS and the exact configs and scripts (minimally modified to get them working).  This enables fast updating when an update comes from Pi-hole.

https://hub.docker.com/r/pihole/pihole/tags/

## Upgrading, Persistence, and Customizations

The standard Pi-hole customization abilities apply to this docker, but with docker twists such as using docker volume mounts to map host stored file configurations over the container defaults.  Volumes are also important to persist the configuration in case you have removed the Pi-hole container which is a typical docker upgrade pattern.

### Upgrading / Reconfiguring

Do not attempt to upgrade (`pihole -up`) or reconfigure (`pihole -r`).  New images will be released for upgrades, upgrading by replacing your old container with a fresh upgraded image is the 'docker way'.  Long-living docker containers are not the docker way since they aim to be portable and reproducible, why not re-create them often!  Just to prove you can.

0. Read the release notes for both this Docker release and the Pi-hole release
    * This will help you avoid common problems due to any known issues with upgrading or newly required arguments or variables
    * We will try to put common break/fixes at the top of this readme too
1. Download the latest version of the image: `docker pull pihole/pihole`
2. Throw away your container: `docker rm -f pihole`
    * **Warning** When removing your pihole container you may be stuck without DNS until step 3; **docker pull** before **docker rm -f** to avoid DNS inturruption **OR** always have a fallback DNS server configured in DHCP to avoid this problem altogether.
    * If you care about your data (logs/customizations), make sure you have it volume-mapped or it will be deleted in this step.
3. Start your container with the newer base image: `docker run <args> pihole/pihole` (`<args>` being your preferred run volumes and env vars)

Why is this style of upgrading good?  A couple reasons: Everyone is starting from the same base image which has been tested to known it works.  No worrying about upgrading from A to B, B to C, or A to C is required when rolling out updates, it reducing complexity, and simply allows a 'fresh start' every time while preserving customizations with volumes.  Basically I'm encouraging [phoenix server](https://www.google.com/?q=phoenix+servers) principles for your containers.

To reconfigure Pi-hole you'll either need to use an existing container environment variables or if there is no a variable for what you need, use the web UI or CLI commands.

### Pi-hole features

Here are some relevant wiki pages from [Pi-hole's documentation](https://github.com/pi-hole/pi-hole/blob/master/README.md#get-help-or-connect-with-us-on-the-web).  The web interface or command line tools can be used to implement changes to pihole.

We install all pihole utilities so the the built in [pihole commands](https://discourse.pi-hole.net/t/the-pihole-command-with-examples/738) will work via `docker exec <container> <command>` like so:

* `docker exec pihole_container_name pihole updateGravity`
* `docker exec pihole_container_name pihole -w spclient.wg.spotify.com`
* `docker exec pihole_container_name pihole -wild example.com`

### Customizations

The webserver and DNS service inside the container can be customized if necessary.  Any configuration files you volume mount into `/etc/dnsmasq.d/` will be loaded by dnsmasq when the container starts or restarts or if you need to modify the Pi-hole config it is located at `/etc/dnsmasq.d/01-pihole.conf`.  The docker start scripts runs a config test prior to starting so it will tell you about any errors in the docker log.

Similarly for the webserver you can customize configs in /etc/lighttpd

### Systemd init script

As long as your docker system service auto starts on boot and you run your container with `--restart=unless-stopped` your container should always start on boot and restart on crashes.  If you prefer to have your docker container run as a systemd service instead, add the file [pihole.service](https://raw.githubusercontent.com/pi-hole/docker-pi-hole/master/pihole.service) to "/etc/systemd/system"; customize whatever your container name is and remove `--restart=unless-stopped` from your docker run.  Then after you have initially created the docker container using the docker run command above, you can control it with "systemctl start pihole" or "systemctl stop pihole" (instead of `docker start`/`docker stop`).  You can also enable it to auto-start on boot with "systemctl enable pihole" (as opposed to `--restart=unless-stopped` and making sure docker service auto-starts on boot).

NOTE:  After initial run you may need to manually stop the docker container with "docker stop pihole" before the systemctl can start controlling the container.

## Note on Capabilities

DNSMasq / [FTLDNS](https://docs.pi-hole.net/ftldns/in-depth/#linux-capabilities) expects to have the following capabilities available:
- `CAP_NET_BIND_SERVICE`: Allows FTLDNS binding to TCP/UDP sockets below 1024 (specifically DNS service on port 53)
- `CAP_NET_RAW`: use raw and packet sockets (needed for handling DHCPv6 requests, and verifying that an IP is not in use before leasing it)
- `CAP_NET_ADMIN`: modify routing tables and other network-related operations (in particular inserting an entry in the neighbor table to answer DHCP requests using unicast packets)

This image automatically grants those capabilities, if available, to the FTLDNS process, even when run as non-root.\
By default, docker does not include the `NET_ADMIN` capability for non-privileged containers, and it is recommended to explicitly add it to the container using `--cap-add=NET_ADMIN`.\
However, if DHCP and IPv6 Router Advertisements are not in use, it should be safe to skip it. For the most paranoid, it should even be possible to explicitly drop the `NET_RAW` capability to prevent FTLDNS from automatically gaining it.

# User Feedback

Please report issues on the [GitHub project](https://github.com/pi-hole/docker-pi-hole) when you suspect something docker related.  Pi-hole or general docker questions are best answered on our [user forums](https://github.com/pi-hole/pi-hole/blob/master/README.md#get-help-or-connect-with-us-on-the-web).  Ping me (@diginc) on the forums if it's a docker container and you're not sure if it's docker related.
