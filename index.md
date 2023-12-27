---
layout: default
---

![Toolbx](assets/toolbox.gif){:.full.pixels}

[Toolbox](https://containertoolbx.org/) is a tool for Linux, which allows the use of interactive command line environments for development and troubleshooting the host operating system, without having to install software on the host. It is built on top of [Podman](https://podman.io/) and other standard container technologies from [OCI](https://opencontainers.org/).

Toolbox environments have seamless access to the user's home directory, the Wayland and X11 sockets, networking (including Avahi), removable devices (like USB sticks), systemd journal, SSH agent, D-Bus, ulimits, /dev and the udev database, etc..

This is particularly useful on [OSTree](https://ostreedev.github.io/ostree/) based operating systems like
[Fedora CoreOS](https://fedoraproject.org/coreos/) and [Silverblue](https://fedoraproject.org/silverblue/). The intention of these systems is to discourage installation of software on the host, and instead install software as (or in) containers — they mostly don't even have package managers like DNF or YUM. This makes it difficult to set up a development environment or troubleshoot the operating system in the usual way.

Toolbx solves this problem by providing a fully mutable container within which one can install their favourite development and troubleshooting tools, editors and SDKs. For example, it's possible to do `yum install ansible` without affecting the base operating system.

However, this tool doesn't *require* using an OSTree based system. It works equally well on Fedora Workstation and Server, and that's a useful way to incrementally adopt containerization.

The toolbx environment is based on an [OCI](https://www.opencontainers.org/) image. On Fedora this is the `fedora-toolbox` image. This image is used to create a toolbox container that offers the interactive command line environment.

Note that Toolbox makes no promise about security beyond what's already available in the usual command line environment on the host that everybody is familiar with.


## Installation & Use

See our guides on [installing & getting started](install) with Toolbx and [Linux distro support](distros).

## Press Information
We are very happy to answer questions from journalists and tech writers. The toolbox logo is licensed [CC-BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/) and can be [downloaded here](/assets/logo/toolbx-logo.zip).

Press queries, including requests for comments and interviews can be directed to [press@lists.podman.io](mailto:press@lists.podman.io).


## Get Involved
Toolbx is Free Software and is developed in the open. Code can be found on [GitHub](https://github.com/containers/toolbox).

  * Join us on Matrix at [#toolbx:matrix.org](https://matrix.to/#/#toolbx:matrix.org).
  * Issues are tracked on [GitHub Issues](https://github.com/containers/toolbox/issues).
  * Security issues can be reported at a [private list](mailto:security@lists.podman.io). Here's our [security policy](https://github.com/containers/common/blob/main/SECURITY.md).
  * Contributors are bound to agree to our [Code of Conduct](https://github.com/containers/common/blob/main/CODE-OF-CONDUCT.md).
  * Follow us on [Twitter](https://twitter.com/containertoolbx) or stay updated with our latest [happenings](articles).

See our [contribution guide](https://github.com/containers/toolbox/blob/main/CONTRIBUTING.md) for further details.

[![Zuul](https://zuul-ci.org/gated.svg)](https://softwarefactory-project.io/zuul/t/local/builds?project=containers/toolbox) [![Daily Pipeline](https://softwarefactory-project.io/zuul/api/tenant/local/badge?project=containers/toolbox&pipeline=periodic)](https://softwarefactory-project.io/zuul/t/local/builds?project=containers%2Ftoolbox&pipeline=periodic) [![Arch Linux package](https://img.shields.io/archlinux/v/community/x86_64/toolbox)](https://www.archlinux.org/packages/community/x86_64/toolbox/) [![Fedora package](https://img.shields.io/fedora/v/toolbox/rawhide)](https://src.fedoraproject.org/rpms/toolbox/)
