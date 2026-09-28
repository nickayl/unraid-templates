# Quven templates for Unraid

[Quven](https://quven.tv) is a self-hosted media server for films, series and documentaries, with
music and ebooks in preview, and with native clients for Windows, macOS, iPhone, iPad, Android,
Android TV and Fire TV rather than a browser-only interface. This repository holds its Unraid
Community Applications template, and nothing else.

The template runs the published, signed image from GitHub Container Registry, and it isn't a
separate build, since the very same artifact backs the Docker Compose bundle documented on
quven.tv for AMD64 and ARM64 hosts alike.

## Before the first start

The server runs unprivileged, as UID and GID 10001, on a read-only root filesystem, while Unraid
creates the appdata folder as root, so the folder has to change hands before the container can
write its database:

```bash
mkdir -p /mnt/user/appdata/quven && chown -R 10001:10001 /mnt/user/appdata/quven
```

Skip it and the server won't start. It'll say why, though: its log names the directory and prints
that same command.

## What the template sets up

The media share is mounted read-write, because the subtitles Quven generates land beside the film.
If you'd rather the server never wrote there, switch the path to read-only. Scans don't write
anything.

Host networking is used because the native clients discover the server on the LAN, on port 5181.
There's no web page there. So once the container runs, link it to your Quven Account with `docker exec -it Quven /app/Quven.Local.Server account link`, approve the address it
prints, and the apps on your network should find the server by themselves.

Hardware transcoding isn't offered here, because an unprivileged container needs the host's render
and video group IDs as well as the device node, those differ from one machine to the next, and a
template has no way to read them from yours. So the accelerated profiles live on the Compose bundle:
<https://quven.tv/guides/docker-media-server>.

## Support

Something not working? Post in the support thread on the Unraid forum:
<https://forums.unraid.net/topic/200728-support-quven-server/>. A defect in the template itself, such
as a wrong default path, a missing variable or a description that no longer matches the image, can
also be opened here as an issue. Please include the image tag, your Unraid version and the
container log.

## Licence

The MIT licence above covers this repository's contents: the template, its
metadata and this README. It does not cover Quven itself, which is closed-source
freeware distributed under its own terms at <https://quven.tv>.
