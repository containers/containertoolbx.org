---
layout: default
fedora-version: 42
---

<picture class="full pixels">
    <source srcset="../assets/builder-dark.gif" media="(prefers-color-scheme: dark)">
    <img src="../assets/builder.gif">
</picture>

**Toolbx** isn't your average constrained container tool. It really shines as an interactive command line environment that can be used just like the usual Linux CLI on the host operating system that everybody is familiar with. Toolbx environments have seamless access to the user's home directory, the Wayland and X11 sockets, networking (including Avahi and CA certificates), removable devices (like USB sticks), systemd journal, SSH agent, D-Bus, ulimits, /dev and the udev database, etc.. The host file system can be accessed at `/run/host`.

This is particularly useful on [OSTree](https://ostreedev.github.io/ostree/) based operating systems like [Fedora CoreOS](https://fedoraproject.org/coreos/) and [Silverblue](https://fedoraproject.org/silverblue/), but also has benefits on classical package based OSes like Fedora Workstation and Server. The `toolbox enter` command is designed to be used both standalone, and as the default shell in a terminal emulation application, if so desired.


* Table of Contents
{:toc}


## Development

One of the primary goals of Toolbx is to help with software development, and it tries to cater to a wide range of use cases. Forget clunky virtual machines, Toolbx is the container solution for developers who demand control and flexibility.


### Command Line Tools & Libraries

**Toolbx** is perfect for hacking on command line tools like compilers and debuggers like [GCC](https://gcc.gnu.org/) and [GDB](https://www.sourceware.org/gdb/), container tools like [Flatpak](https://flatpak.org/) and [Podman](https://podman.io/), and libraries like [GStreamer](https://gstreamer.freedesktop.org/) and [GTK](https://gtk.org/).

The development environments can be set up inside Toolbx containers in the same way as on the host operating system. The containers have separate `/usr` file systems from the host, and therefore project environments will no longer clash with the host OS, nor will they be limited to a single OS version. Testing against different OS user spaces is only a matter of creating different containers.


### Desktop Applications

**Toolbx** really shines on the desktop. The same principles, mentioned above, apply when building desktop applications; both when using classical development environments with [Emacs](https://www.gnu.org/software/emacs/) and [Vim](https://www.vim.org/), or graphical integrated development environments.

[Builder](https://flathub.org/apps/org.gnome.Builder), the IDE for GNOME, has integrated Toolbx in a way that allows developing and testing desktop applications literally at the push of a button. The application is built, run and tested against the development tools and software development kits installed inside a specific Toolbx container.

![GNOME Builder building GNOME Maps against a Toolbx container](../assets/builder-desktop-app.png){: .full}


### Web Applications

Not just desktop applications. Toolbx environments can be used to develop web applications too. This website itself was created using [Toolbx](https://github.com/containers/containertoolbx.org/blob/main/README.md), and it's the recommended way to make changes to it.

The web applications can be developed inside Toolbx containers that closely match the deployment environments used in production, without being constrained by the specifics of the host operating system.


### Operating System Components

Building system components like [GNOME Shell](https://gitlab.gnome.org/GNOME/gnome-shell) and [PipeWire](https://pipewire.org/), not just apps? **Toolbx** has it covered.

Developing GNOME Shell used to be quite cumbersome and satisfying the dependencies a chore. The Shell developers have streamlined their [workflow using Toolbx](https://gitlab.gnome.org/GNOME/gnome-shell/-/tree/main/tools/toolbox?ref_type=heads) considerably. [Builder](https://flathub.org/apps/org.gnome.Builder), the IDE for GNOME, has integrated Toolbx in a way that allows running GNOME Shell in nested mode as it was an application. This streamlines the *develop > test > debug > submit merge request* workflow considerably.

![GNOME Builder running Shell from inside a Toolbx container](../assets/builder-shell-toolbx.webp){: .full}


### Wayland Session

Developing operating system components can require running a full [Wayland](https://wayland.freedesktop.org/) session with [systemd-logind](https://www.freedesktop.org/software/systemd/man/latest/systemd-logind.service.html) and [udev](https://www.freedesktop.org/software/systemd/man/latest/udev.html). It's possible to do that from inside a **Toolbx** container.

Here's how a full GNOME session can be run from inside a Toolbx container on a Fedora Silverblue host.

Use `ctrl+alt+f<n>` to switch to a Linux console and log in. Then:
```console
[user@hostname ~]$ export XDG_CURRENT_DESKTOP=GNOME
[user@hostname ~]$ toolbox enter
⬢[user@toolbox ~]$ sudo dnf install flatpak gnome-backgrounds gnome-shell
⬢[user@toolbox ~]$ dbus-run-session gnome-shell --wayland
```

![Full GNOME session running from inside a Toolbx container](../assets/gnome-full-session.png){: .full}


### Boot from Container

Not just that — it's possible to boot from a **Toolbx** container. Here's how Fedora Silverblue can be booted from a Toolbx container.

Create a Toolbx container from the [OCI](https://opencontainers.org/) variant of the Fedora Silverblue image:
```console
[user@hostname ~]$ toolbox create \
                     --image quay.io/fedora-ostree-desktops/silverblue:{{ page.fedora-version }} \
                     silverblue-toolbox-{{ page.fedora-version }}
```

Alter it by installing DNF as an example:
```console
[user@hostname ~]$ toolbox enter silverblue-toolbox-{{ page.fedora-version }}
⬢[user@toolbox ~]$ sudo rpm-ostree install dnf
⬢[user@toolbox ~]$ exit
```

Create an OCI image from the altered Toolbx container:
```console
[user@hostname ~]$ podman commit \
                     --squash \
                     silverblue-toolbox-{{ page.fedora-version }} \
                     localhost/silverblue-toolbox:{{ page.fedora-version }}
```

Expose it to [rpm-ostree](https://coreos.github.io/rpm-ostree/):
```console
[user@hostname ~]$ mkdir /var/tmp/silverblue-toolbox-{{ page.fedora-version }}-00
[user@hostname ~]$ podman save \
                     --format oci-dir \
                     --output /var/tmp/silverblue-toolbox-{{ page.fedora-version }}-00 \
                     localhost/silverblue-toolbox:{{ page.fedora-version }}
[user@hostname ~]$ rpm-ostree rebase \
                     ostree-unverified-image:oci:/var/tmp/silverblue-toolbox-{{ page.fedora-version }}-00
```

Reboot.

![Fedora Silverblue booted from a Toolbx container](../assets/fedora-silverblue-boot-from.png){: .full}


## Troubleshooting

Another important goal of Toolbx is to help troubleshoot the host operating system or network without installing any extra tools on the host. Every additional piece of software that's installed on a production machine can disturb the deployment environments for the workloads and make them vulnerable to bugs in the software.

It's possible to use the `toolbox(1)` command as root on the host OS to create environments for performing privileged operations on the host.


### Network

**Toolbx** is perfect for using [Nmap](https://nmap.org/) to scan IP addresses and ports in a network, including TCP SYN scans with the `-sS` option that requires root access to the host operating system:
```console
[root@hostname ~]# toolbox enter
⬢[root@toolbox ~]# nmap -sS scanme.nmap.org
Starting Nmap 7.93 ( https://nmap.org ) at 2024-03-04 17:09 CET
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.22s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 992 closed tcp ports (reset)
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
135/tcp   filtered msrpc
139/tcp   filtered netbios-ssn
179/tcp   filtered bgp
445/tcp   filtered microsoft-ds
9929/tcp  open     nping-echo
31337/tcp open     Elite

Nmap done: 1 IP address (1 host up) scanned in 6.03 seconds
```


### System Logs

Similarly, **Toolbx** can be used to look at the logs for all processes running on the host with `journalctl(1)`, including those with elevated privileges.


## Ptyxis Terminal

[Ptyxis](https://gitlab.gnome.org/chergert/ptyxis), the game-changing terminal emulator application, seamlessly integrates with Toolbx, making container management a breeze. Whether you're a seasoned developer or just dipping your toes into the container world, Ptyxis streamlines your workflow.

**Say goodbye to juggling containers and terminals!**

![Jekyll in a container](../assets/ptyxis.webp){: .full}

Imagine this: you fire up Ptyxis, select your desired Toolbx container, and boom — you're in! No more configuration hassles or context switching. Ptyxis acts as your direct portal to any container environment, giving you granular control and access to all the tools you need within a familiar terminal interface.

Ptyxis takes advantage of Toolbx's flexibility, allowing you to create custom container configurations tailored to your specific needs. Need a Python development environment? Craft a container with all the necessary libraries and packages. Working on a system component test? Design a container that mirrors your production setup. The possibilities are endless, and Ptyxis empowers you to explore them all with ease. And all those very same containers are accessible from your IDE.
