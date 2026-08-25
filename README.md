# Quven templates for Unraid

[Quven](https://quven.tv) is a self-hosted media server for films, TV shows and documentaries, with
native desktop, Android, Apple and television clients rather than a browser-only interface. This
repository holds its Unraid Community Applications template, and nothing else.

The template runs the published image from GitHub Container Registry. It is not a separate build:
the same artifact backs the Docker Compose bundle documented on quven.tv.

## Before the first start

The server runs unprivileged, as UID and GID 10001, on a read-only root filesystem. Unraid creates
the appdata folder as root, so the folder has to change hands before the container can write its
database:

```bash
mkdir -p /mnt/user/appdata/quven && chown -R 10001:10001 /mnt/user/appdata/quven
```

Skip it and the container will start, fail to open its database, and restart. That is the one step
worth doing in the right order.

## What the template sets up

The media share is mounted read-only. Quven never writes to it while scanning, and an ordinary scan
is not a disk-cleaning operation.

Host networking is used because the native clients discover the server on the LAN. The web interface
answers on port 5181.

Hardware transcoding is not offered here. An unprivileged container needs the host's render and video
group IDs as well as the device node, and those are specific to your machine, so the accelerated
profiles live on the Compose bundle instead: <https://quven.tv/docker>.

## Issues

Template problems belong here. Anything about the server itself belongs at <https://quven.tv>.

## Licence

The MIT licence above covers this repository's contents: the template, its
metadata and this README. It does not cover Quven itself, which is closed-source
freeware distributed under its own terms at <https://quven.tv>.
