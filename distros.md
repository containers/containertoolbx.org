---
layout: default
---

<picture class="full pixels">
    <source srcset="../assets/distros-dark.gif" media="(prefers-color-scheme: dark)">
    <img src="../assets/distros.gif">
</picture>

## Distro Support

Toolbx supports a number of distributions, allowing you to run development environments totally separate from the host operating system. MS Windows developers will be familiar with the concept from WSL (Windows subsystem for Linux).

By default, Toolbx tries to use an image matching the host operating system distribution for creating containers. If the host is not supported, then it falls back to a Fedora image. Supported host operating systems are:

* **Arch Linux**
* **Fedora**
* **Red Hat Enterprise Linux >= 8.5**
* **Ubuntu**

You can specify a desired distribution when creating the container:

```
toolbox create --distro ubuntu --release 22.04 ubuntu
```

Once you enter the distro environment with `toolbox enter ubuntu` you have access to all the `.deb` packages the distro provide, regardless of the host operating system you use.

![apt-get on Fedora](../assets/apt-get.png){:.full}
