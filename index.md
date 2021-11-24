---
layout: default
---

![Toolbx](assets/toolbox.gif){:.full.pixels}

[Toolbx](https://github.com/containers/toolbox) is a tool for Linux operating systems, which allows the use of containerized command line environments. It is built on top of [Podman](https://podman.io/) and other standard container technologies from [OCI](https://opencontainers.org/).

This is particularly useful on [OSTree](https://ostree.readthedocs.io/en/latest/) based operating systems like
[Fedora CoreOS](https://coreos.fedoraproject.org/) and [Silverblue](https://silverblue.fedoraproject.org/). The intention of these systems is to discourage installation of software on the host, and instead install software as (or in) containers — they mostly don't even have package managers like DNF or YUM. This makes it difficult to set up a development environment or install tools for debugging in the usual way.

Toolbx solves this problem by providing a fully mutable container within which one can install their favourite development and debugging tools, editors and SDKs. For example, it's possible to do `yum install ansible` without affecting the base operating system.

However, this tool doesn't *require* using an OSTree based system. It works equally well on Fedora Workstation and Server, and that's a useful way to incrementally adopt containerization.

The toolbx environment is based on an [OCI](https://www.opencontainers.org/) image. On Fedora this is the `fedora-toolbox` image. This image is used to create a toolbx container that seamlessly integrates with the rest of the operating system by providing access to the user's home directory, the Wayland and X11 sockets, networking (including Avahi), removable devices (like USB sticks), systemd journal, SSH agent, D-Bus, ulimits, /dev and the udev database, etc..


## Installation & Use

See our [detailed guide](install).

## Get Involved
Toolbx is Free Software and is developed in the open. Code can be found on [Github](https://github.com/containers/toolbox).

  * Join us on Matrix at [#toolbx:matrix.org](https://matrix.to/#/#toolbx:matrix.org).
  * Issues are tracked on [Github Issues](https://github.com/containers/toolbox/issues).
  * Contributors are bound to agree to our [Code of Conduct](https://github.com/containers/common/blob/main/CODE-OF-CONDUCT.md).
  * Follow us on [Twitter](https://twitter.com/containertoolbx).
