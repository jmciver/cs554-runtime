# CS554 riscv64 Runtime #

This repository provides a simple riscv64 runtime container,
**jmciver/cs554-runtime**, which can be used to build and run
assignments for UNM CS554.

Basic directions are provided to:
* Install Docker [1]
* Associate QEMU [2][3] for platform emulation
* Run a basic `Hello World!` test using a container instance with bind-mount

## Install

###  Windows 10

Docker when run in Windows 10 can either be installed using the WSL2
or Hyper-V back-ends. For most users it is recommend to use WSL2.

1. To install WSL2 use the following instructions
   [here](https://docs.microsoft.com/en-us/windows/wsl/install).
1. To install Docker with WSL2 support use the following directions
   [here](https://docs.docker.com/desktop/windows/install/). QEMU is
   automatically installed as part of the Docker Windows package.

If using Windows, all further reference to `docker` commands in this
document assume execution in a WSL2 terminal.

### Ubuntu

1. Install Docker using the "Install using the repository"
   [directions](https://docs.docker.com/engine/install/ubuntu/)
1. Your user login will need to be associated to the `docker` group
   unless you wish to privilege escalate using `sudo` on every docker
   command invocation. There are some security risks to be aware of:
   https://docs.docker.com/engine/security/#docker-daemon-attack-surface.
   ```
   # To add your login to the `docker` group replace `LOGIN` with our
   # login name:
   sudo usermod -a -G docker LOGIN
   ```
1. Logout and log back in for group changes to take effect.
1. Install QEMU:
   ```
   sudo apt-get install qemu binfmt-support qemu-user-static
   ```
1. Associate architecture types to a particular emulator (QEMU):
   ```
   docker run \
     --rm \
     --privileged \
     multiarch/qemu-user-static \
     --reset -p yes
   ```

### Fedora

1. Install Docker using the "Install using the repository"
[directions](https://docs.docker.com/engine/install/fedora/)

1. Start and enable the docker service:
   ```
   # Automatically start Docker service on power-on/reboot:
   sudo systemctl enable docker.service

   # Start the service running:
   sudo systemctl start docker.service
   ```
1. Your user login will need to be associated to the `docker` group
   unless you wish to privilege escalate using `sudo` on every docker
   command invocation. There are some security risks to be aware of:
   https://docs.docker.com/engine/security/#docker-daemon-attack-surface.
   ```
   # To add your login to the `docker` group replace `LOGIN` with our
   # login name:
   sudo usermod -a -G docker LOGIN
   ```
1. Logout and log back in for group changes to take effect.
1. Install QEMU:
   ```
   dnf install -y qemu qemu-user-static
   ```
1. Associate architecture types to a particular emulator (QEMU):
   ```
   docker run \
     --rm \
     --privileged \
     multiarch/qemu-user-static \
     --reset -p yes
   ```

## Testing Install

To test configuration run the following command:
```
docker run --rm -t jmciver/cs554-runtime uname -m
```
Which should return `riscv64`.

## Using

A class image is provided that includes: `gcc`, `vi`, `emacs`, `tmux`,
and a small assortment of other utilities.

1. Pull class image:
   ```
   docker image pull jmciver/cs554-runtime:latest
   ```
1. Create a directory, `cd` into it, and create a simple "Hello
   World" C program.
   ```
   mkdir cs554DockerTest && cd cs554DockerTest
   cat << EOF > 'hello.c'
   #include <stdio.h>

   int main()
   {
     printf("Hello World!\n");
     return 0;
   }
   EOF
   ```
1. Run the docker container from the current directory interactively
   (`-it`):
   ```
   # --rm is to remove the instance when we are done.
   # -it provides interactive terminal support.
   # -v is a bind mount in the form of host:container path mapping
   docker run --rm -it -v$PWD:/mnt/build jmciver/cs554-runtime
1. In the contain shell build and run `hello.c`
   ```
   cd /mnt/build && gcc -o hello hello.c && ./hello
   ```

## References

* [1] Docker: https://www.docker.com
* [2] QEMU: https://www.qemu.org
* [3] multiarch/qemu-user-static: https://github.com/multiarch/qemu-user-static
