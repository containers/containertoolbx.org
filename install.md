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

Toolbx requires at least Podman 1.6.4, and optionally at least Skopeo 1.10.0, to work.

It uses the Meson build system and the following dependencies are required to build it:
- clang or gcc
- go >= 1.13
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

## Custom Images
There are two different ways of creating a custom image. The first one consists on starting from scratch by creating a container file or using an existing one. In the following link, we can see an example of a [Containerfile](/example-container-file). The other possibility consists on customizing an existing image.

The following steps can be followed in order to create a custom image:

    1. Create a toolbox - in this case I'm using a custom image as base

        ```
        $ toolbox create -i docker.io/akdev1l/ubuntu-toolbox:22.04
        Created container: ubuntu-toolbox-22.04
        Enter with: toolbox enter ubuntu-toolbox-22.04
        ```

    2. Enter the toolbox and do modifications

        ```
        $ toolbox enter ubuntu-toolbox-22.04
        ⬢$ sudo apt install -y neofetch
        sudo: unable to resolve host toolbox: No address associated with hostname
        ```

    3. commit the changes:

        ```
        $ podman commit ubuntu-toolbox-22.04 my-custom-image
        ```

    4. You can now create new custom toolboxes by doing: 

        ```
        $ toolbox create -i my-custom-image
        ```
