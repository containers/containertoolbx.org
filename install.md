---
layout: default
---

## Installation

Toolbx is installed by default on Fedora Silverblue. On other operating systems it's just a matter of installing the `toolbox` package.

## Usage

### Create your toolbx container:
```console
[user@hostname ~]$ toolbox create
Created container: fedora-toolbox-36
Enter with: toolbox enter
[user@hostname ~]$
```
This will create a container called `fedora-toolbox-<version-id>`.

### Enter the toolbx:
```console
[user@hostname ~]$ toolbox enter
⬢[user@toolbox ~]$
```

### Remove a toolbx container:
```console
[user@hostname ~]$ toolbox rm fedora-toolbox-36
[user@hostname ~]$
```

## Dependencies and Building

Toolbx requires at least Podman 1.4.0 to work, and uses the Meson build system.

The following dependencies are required to build it:
- clang or gcc
- go >= 1.13
- go-md2man
- meson >= 0.58.0
- ninja
- systemd

The following dependencies enable various optional features:
- bash-completion

It can be built and installed as any other typical Meson-based project:
```console
[user@hostname toolbox]$ meson setup -Dprofile_dir=/etc/profile.d builddir
[user@hostname toolbox]$ meson compile -C builddir
[user@hostname toolbox]$ sudo meson install -C builddir
```

Toolbx is written in Go. Consult the [src/go.mod](https://github.com/containers/toolbox/blob/main/src/go.mod) file for a full list of all the Go dependencies.

By default, Toolbx uses Go modules and all the required Go packages are automatically downloaded as part of the build. There's no need to worry about the Go dependencies, unless the build environment doesn't have network access or any such peculiarities.