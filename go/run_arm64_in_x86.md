### Setting Up ARM Emulation on x86

We’ll be using QEMU and Docker to set up our emulated environment. QEMU is an open source machine emulator and virtualizer. It allows users to to build ARM CUDA binaries on your x86 machine without needing a cross compiler.

First, let’s see what happens before setting up the emulation when trying to execute a program compiled for a different architecture :

```console
$ uname -m # Display the host architecture
x86_64

$ docker run --rm -t arm64v8/ubuntu uname -m # Run an executable made for aarch64 on x86_64
standard_init_linux.go:211: exec user process caused "exec format error"
```

As expected the instructions are not recognized since the packages are not installed yet. Installing the following packages should allow you to enable support for aarch64 containers on your x86 workstation:

#### 1. Install the qemu and required dependent packages

```console
$ sudo apt-get install qemu binfmt-support qemu-user-static 
```

#### 2. Step to execute the registering scripts
```console
$ docker run --rm --privileged multiarch/qemu-user-static --reset -p yes 
```

#### 3. Testing the emulation environment
```console
$ docker run --rm -t arm64v8/ubuntu uname -m 
aarch64
```
