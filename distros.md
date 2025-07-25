---
layout: default
fedora-version: 42
rhel-version: 10.0
ubuntu-version: 22.04
---

<picture class="full pixels">
    <source srcset="../assets/distros-dark.gif" media="(prefers-color-scheme: dark)">
    <img src="../assets/distros.gif">
</picture>

## Supported Operating System Distributions

Toolbx is regularly tested on the following host operating systems:

* **Arch Linux**
* **Fedora**
* **Red Hat Enterprise Linux >= 8.5**
* **Ubuntu**

By default, Toolbx tries to use an image matching the host operating system distribution for creating containers. If there is no image matching the host, it falls back to a Fedora image. Currently, there are images available for the OSes listed above.

It's possible to create containers for a different distribution through the use of the `distro` and `release` [command line](https://github.com/containers/toolbox/blob/main/doc/toolbox-create.1.md) options, or their counterparts in the [configuration file](https://github.com/containers/toolbox/blob/main/doc/toolbox.conf.5.md). The `distro` option specifies the name of the distribution, and `release` specifies its version.  For example:

```
toolbox create --distro ubuntu --release 22.04
```

Supported combinations are:

|-------+-----------------------------------------------------------------------------------------|
|Distro |Release                                                                                  |
|-------|-----------------------------------------------------------------------------------------|
|arch   |latest or rolling                                                                        |
|-------+-----------------------------------------------------------------------------------------|
|fedora |\<release\> or f\<release\> eg., {{ page.fedora-version }} or f{{ page.fedora-version }} |
|-------+-----------------------------------------------------------------------------------------|
|rhel   |\<major\>.\<minor\> eg., {{ page.rhel-version }}                                         |
|-------+-----------------------------------------------------------------------------------------|
|ubuntu |\<YY\>.\<MM\> eg., {{ page.ubuntu-version }}                                             |
|-------+-----------------------------------------------------------------------------------------|

Once you enter the distro environment with `toolbox enter` you have access to all the `.deb` packages the distro provide, regardless of the host operating system you use.

![apt-get on Fedora](../assets/apt-get.png){:.full}
