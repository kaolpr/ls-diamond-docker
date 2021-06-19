# Docker image for Lattice Semiconductor Diamond

If you happen to need LS Diamond on non-RHEL system, this
image may be your chance. It is based on an article:
https://section5.ch/index.php/2017/01/20/669/ with some 
updates:

* `license.dat` is copied to the container upon build,
* added 32b requirements for running ModelSim,
* added simple script for running container with current
  working directory mounted.

**WARNING:** This *simple* script assumes your uid is the
same as `diamond` user created in the base image (see
`docker/Dockerfile`), i.e. `1000`. This is often the case
if you are the only one user of the PC (or the first 
normal user created). If you're operating multi-user 
environment you'll need to adjust `Dockerfile` to your 
needs otherwise every time you run dockerized Diamond 
you'll experience permission problems.

## How to build?

These instructions are for version 3.12
(`diamond_3_12-base-240-2-x86_64-linux.rpm`). If you have
other version, it still may work, just adjust paths in 
`docker/env-setup.sh`.

1. Download RPM from LS site and copy it to the repo root.

2. Obtain your license file. It will probably be linked to 
   your MAC. Choose wisely not to use USB network card or
   some not-always available device. Alternatively, you can
   choose arbitrary valid MAC, as we can set one for Docker
   container.

3. Set `LS_DIAMOND_ETHADDR` to MAC address of your choice.
   It may be handy to put it in your `~/.bashrc` as it will
   be necessary every time container is started.

4. Enter `docker` directory and build base image:

```bash
docker build -t diamond:base .
```

5. Enter repo root and run `start_setup_shell`. This will 
   start container with current directory mounted at `/mnt`.

6. In the opened shell install Diamond:

```bash
rpm -i /mnt/diamond_3_12-base-240-2-x86_64-linux.rpm
```

7. Test installation:

```bash
su -l diamond
diamond
```

8. Commit changes to the container. Note container ID that is 
   in your prompt (`diamond@<CONTAINER_ID>`), then exit `su` and 
   container shell. Commit changes with:

```bash
docker commit -m "Installed LS Diamond" <CONTAINER_ID> diamond:v3.12
```

## Running Diamond in Docker

To run container you can use `run_diamond` script. This will
(at first run) create `~/.lsdiamond` directory and mount it as 
container's `diamond` user home. Current directory will be mounted
as `~/workspace`. This script can be executed in any location, so
it may be handy to put it in your `PATH`.





