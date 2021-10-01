# CS554 RISCV-64 Runtime #

## Windows 10

Docker when run in Windows 10 can either be installed using the WSL(2)
or Hyper-V back-ends. For most users it is recommend to use WSL2.

1. To install WSL2 use the following instructions
   [here](https://docs.microsoft.com/en-us/windows/wsl/install).

2. To install Docker with WSL2 support use the following directions
   [here](https://docs.docker.com/desktop/windows/install/). QEMU is
   automatically installed as part of the Docker Windows package.

If using Windows, all further reference to `docker` commands in this
document assume execution in a WSL2 terminal.

## Ubuntu

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

## Fedora

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
docker run --rm -t riscv64/ubuntu uname -m
```
Which should return `riscv64`.
