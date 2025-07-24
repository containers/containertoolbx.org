---
layout: default
fedora-version: 42
---

<picture class="full pixels">
    <source srcset="../assets/install-dark.png" media="(prefers-color-scheme: dark)">
    <img src="../assets/install.png">
</picture>

## Installation

Toolbx is installed by default on [Fedora Silverblue](https://fedoraproject.org/silverblue/) and [Workstation](https://fedoraproject.org/workstation/). On other operating systems it's a matter of installing the `toolbox` or `podman-toolbox` package.

## Usage

### Create your Toolbx container:
```console
[user@hostname ~]$ toolbox create
Created container: fedora-toolbox-{{ page.fedora-version }}
Enter with: toolbox enter
[user@hostname ~]$
```
On a Fedora {{ page.fedora-version }} host, this will create a container called `fedora-toolbox-{{ page.fedora-version }}`. On an Arch Linux host, it will create one called `arch-linux-latest`, and so on.

### Enter the Toolbx:
```console
[user@hostname ~]$ toolbox enter
⬢[user@toolbox ~]$
```

### Remove a Toolbx container:
```console
[user@hostname ~]$ toolbox rm fedora-toolbox-{{ page.fedora-version }}
[user@hostname ~]$
```

## Dependencies and Building

Toolbx requires at least Podman 1.6.4, and optionally at least Skopeo 1.10.0, to work.

It uses the Meson build system and the following dependencies are required to build it:
- clang or gcc
- go >= 1.20
- go-md2man
- meson >= 0.58.0
- ninja
- shadow >= 4.9
- systemd

If the following dependencies are available when building, then various optional features are enabled:
- bash-completion
- fish

It can be built and installed as any other typical Meson-based project:
```console
[user@hostname toolbox]$ meson setup -Dprofile_dir=/etc/profile.d builddir
[user@hostname toolbox]$ meson compile -C builddir
[user@hostname toolbox]$ sudo meson install -C builddir
```

Toolbx is written in Go. Consult the [src/go.mod](https://github.com/containers/toolbox/blob/main/src/go.mod) file for a full list of all the Go dependencies.

By default, Toolbx uses Go modules and all the required Go packages are automatically downloaded as part of the build. There's no need to worry about the Go dependencies, unless the build environment doesn't have network access or any such peculiarities.
