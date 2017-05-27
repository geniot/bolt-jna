# How to build bolt-jna?

## Sources

1. Download `bolt-jna` from repository and install sub-modules:

     ```
     git clone git@github.com:protonail/bolt-jna.git
     cd bolt-jna
     git submodule init
     git submodule update
     ```

## Prepare Environment

### Windows Platform

1. Install [MSYS2](http://msys2.github.io/) for correspond architecture.

1. Open MinGW shell correspond to platform architecture (MinGW-w64 Win64 Shell or MinGW-w64 Win32 Shell).

1. Update the system packages:

     ```
     pacman --needed -Sy bash pacman pacman-mirrors msys2-runtime
     ```

1. (!!!) Close MSYS2, run it again from Start menu and update the rest:

     ```
     pacman -Su
     ```

1. Install packages for development:

     ```
    ﻿pacman -S base-devel git
     ```
1. Install Go ( >= 1.8.1) for correspond architecture:

     For Windows x64:

     ```
     pacman -S mingw-w64-x86_64-go
     ```

     For Windows x86:

     ```
     pacman -S mingw-w64-i686-go
     ```

1. Install toolchain for correspond architecture:

     For Windows x64:

     ```
     pacman -S mingw-w64-x86_64-toolchain
     ```

     For Windows x86:

     ```
     pacman -S mingw-w64-i686-toolchain
     ```

### OS X Platform

1. Install packages required for build:

     ```
     brew install go automake libtool pkg-config
     ```

### Linux Platform (i.e. Ubuntu)

1. Install Go (>= 1.8.1) as described [here](https://golang.org/doc/install#tarball).

1. Install packages required for build:

     ```
     apt-get install build-essential
     apt-get install automake libtool pkg-config
     ```

## Build Native Platform Dependent Library

You should repeat these steps for each platform.

1. Open `bolt-jna` root directory and execute build script:

     ```
     bin/build_bolt.sh clean
     ```

     It should build Bolt and copy correspond native library file to `bolt-jna-native/src/main/resources/<JNA platform identificator>/<library name>.{so,dylib,dll}`.

1. Run tests

     ```
     mvn test
     ```

1. Build Java library with native binaries inside and deploy it to remote Maven repository:

     ```
     mvn deploy -DaltSnapshotDeploymentRepository=<altSnapshotDeploymentRepository> -DaltReleaseDeploymentRepository=<altReleaseDeploymentRepository> -pl bolt-jna-native
     ```

     where _altSnapshotDeploymentRepository_ and _altReleaseDeploymentRepository_ is your Maven repositories (i.e. run on Nexus Sonatype). Details is [here](https://maven.apache.org/plugins/maven-deploy-plugin/deploy-mojo.html).

## Build Core Library

1. Build Core Java library and deploy it to remote Maven repository:

     ```
     mvn deploy -DaltSnapshotDeploymentRepository=<altSnapshotDeploymentRepository> -DaltReleaseDeploymentRepository=<altReleaseDeploymentRepository> -pl bolt-jna-core
     ```

     where _altSnapshotDeploymentRepository_ and _altReleaseDeploymentRepository_ is your Maven repositories (i.e. run on Nexus Sonatype). Details is [here](https://maven.apache.org/plugins/maven-deploy-plugin/deploy-mojo.html).

## That's all.
