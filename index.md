---
layout: default
---

<picture class="full pixels">
    <source srcset="assets/toolbx-dark.gif" media="(prefers-color-scheme: dark)">
    <img src="assets/toolbx.gif">
</picture>

## About

**Toolbx** is a tool for Linux, which allows the use of interactive command line environments for software development and troubleshooting the host operating system, without having to install software on the host. It is built on top of [Podman](https://podman.io/) and other standard container technologies from [OCI](https://opencontainers.org/).

Toolbx environments have seamless access to the user's home directory, the Wayland and X11 sockets, networking (including Avahi and CA certificates), removable devices (like USB sticks), systemd journal, SSH agent, D-Bus, ulimits, /dev and the udev database, etc.. The host file system can be accessed at `/run/host`.

Learn more about Toolbx in the [documentation](doc).


## Installation & Use

* [Installation & Getting Started](install)
* [Distro Support](distros)
* [Common Use Cases](use)
* [Project Goals](goals)

## Press
* [Articles & Presentations](articles)
* [Press kit](/assets/logo/toolbx-logo.zip)

Press queries, including requests for comments and interviews can be directed to [press@lists.podman.io](mailto:press@lists.podman.io).


## Get Involved
Toolbx is Free Software and is developed in the open. Code can be found on [GitHub](https://github.com/containers/toolbox).

  * Join us on Matrix at [#toolbx:matrix.org](https://matrix.to/#/#toolbx:matrix.org).
  * Issues are tracked on [GitHub Issues](https://github.com/containers/toolbox/issues).
  * Security issues can be reported at a [private list](mailto:security@lists.podman.io). Here's our [security policy](https://github.com/containers/common/blob/main/SECURITY.md).
  * Contributors are bound to agree to our [Code of Conduct](https://github.com/containers/common/blob/main/CODE-OF-CONDUCT.md).
  * Follow us on [Mastodon/Fediverse](https://floss.social/@containertoolbx){:rel="me"}.

See our [contribution guide](https://github.com/containers/toolbox/blob/main/CONTRIBUTING.md) for further details.
