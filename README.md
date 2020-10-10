# Webtin

## Depends

[TinTin++](https://tintin.mudhalla.net)

[ttyd](https://github.com/tsl0922/ttyd)

## Usage

### Quickstart

Install Docker:

* (Debian/Ubuntu): `sudo apt install docker.io`
* (Arch/Manjaro): `sudo pacman -S docker`

Add yourself to the docker group (this is optional, but avoids the need for calling `sudo` a bunch of times):

```
sudo usermod -aG docker ${USER}
```

Logout to take effect. Then:

```
docker run -it -p 3000:3000 -v $(pwd):/data rpolve/webtin
```

Now point your web browser to _localhost:3000_ and it's done.

Tintin will read a _config.tin_ file script from the directory where the container was launched. This can be changed by substituting `$(pwd)` with the absolute path of your choice.

### Port forwarding

`-p <x>:<y>` means matching port \<x> on the host to port \<y> within the container. By construction, Webtin listens to port 3000, but you can change \<x> to whichever port you like, e.g. `-p 80:3000` for default http port.

### Networking from the cointainer perspective

When trying to connect to the Docker host from within the container, you have to look up its IP on the docker network. You can easily do this by running `ip a` on the host to find out the IP address of the `docker0` adapter. In this case __172.17.0.1__:

```
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500
    link/ether 02:42:e8:a9:95:58 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```

An alternative would be to use `--network host` in your `docker run` command. See [here](https://docs.docker.com/network/host/) for more info.

### Important note on security

Add `#CONFIG {CHILD LOCK} ON` to _config.tin_ in order to prevent users from __getting command line access to the insides of the container__.

Although this is not much of a concern in relation to the host machine, given that it still is _just a container_, it isn't suitable when serving mulptiple users, as they could theoretically kill any active session.

### Running in the background

Launch the container in detached mode:

```
docker run -d -p 3000:3000 -v $(pwd):/data rpolve/webtin
```

See `docker run --help` and `docker stop --help`.

### Restarting the container at boot

[Here](https://docs.docker.com/config/containers/start-containers-automatically/) be info.

### Noe0m patch

A patch is made available to strips tintin's `#echo` and `#showme` commands of the `\e[0m` ANSI color code. This caused slight problems on some MUDs with less than usual colouring scheme. The patched container is available for pulling from Docker Hub under the tag `rpolve/webtin:noe0m`. The `\e[0m` which is normally appended to the command echo is also substituted with `\e[0;37;49m`. If the latter is unsatisfactory, you can change the provided _noe0m.patch_ by hand and [build a new container](https://docs.docker.com/engine/reference/commandline/build/) using the _noe0m.Dockerfile_.

### Resource constraints

Docker provides runtime options for limiting CPU and memory usage. [See here](https://docs.docker.com/config/containers/resource_constraints/).

### Raspberry Pi

An Arm compatible image is available by pulling `rpolve/webtin:rpi`.

## About Docker

[Docker intro](https://docs.docker.com/get-started/overview/).

## Acknowledgements

Many thanks to Scandum, Eldakar and Tyrir from tintin discord server for the much appreciated insights.
