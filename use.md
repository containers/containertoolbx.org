---
layout: default
fedora-version: 39
---

<picture class="full pixels">
    <source srcset="../assets/builder-dark.gif" media="(prefers-color-scheme: dark)">
    <img src="../assets/builder.gif">
</picture>

**Toolbx** isn't your average constrained container tool. It really shines as an interactive command line environment that can be used just like the usual Linux CLI on the host operating system that everybody is familiar with. Toolbx environments have seamless access to the user's home directory, the Wayland and X11 sockets, networking (including Avahi), removable devices (like USB sticks), systemd journal, SSH agent, D-Bus, ulimits, /dev and the udev database, etc..

This is particularly useful on [OSTree](https://ostreedev.github.io/ostree/) based operating systems like [Fedora CoreOS](https://fedoraproject.org/coreos/) and [Silverblue](https://fedoraproject.org/silverblue/), but also has benefits on classical package based OSes like Fedora Workstation and Server.


* Table of Contents
{:toc}


## Development

One of the primary goals of Toolbx is to help with software development, and it tries to cater to a wide range of use cases. The `toolbox enter` command is designed to be used both standalone, and as the default shell in a terminal emulation application, if so desired.

Forget clunky virtual machines, Toolbx is the container solution for developers who demand control and flexibility.


### Command Line Tools & libraries

**Toolbx** is perfect for hacking on command line tools like compilers and debuggers like [GCC](https://gcc.gnu.org/) and [GDB](https://www.sourceware.org/gdb/), container tools like [Flatpak](https://flatpak.org/) and [Podman](https://podman.io/), and libraries like [GLib](https://docs.gtk.org/glib/) and [GTK](https://gtk.org/).

The development environment can be set up inside the container in the same way as on the host operating system. No longer will project environments clash with the host OS, nor will it be limited to a single OS version. Testing against a different OS user space is only a matter of creating a different Toolbx container.


### Desktop Applications

**Toolbx** really shines on the desktop. The same principles, mentioned above, apply when building desktop applications; both when using classical development environments with [Emacs](https://www.gnu.org/software/emacs/) and [Vim](https://www.vim.org/), or graphical integrated development environments.

[Builder](https://flathub.org/apps/org.gnome.Builder), the IDE for GNOME, has integrated Toolbx in a way that allows developing and testing desktop applications literally at the push of a button. The application is built, run and tested against the development tools and software development kits installed inside a specific Toolbx container.


### Web Applications

Not just desktop applications. Toolbox environments can be used to develop web applications too. This website itself was created using [Toolbx](https://github.com/containers/containertoolbx.org/blob/main/README.md), and it's the recommended way to make changes to it.

The web applications can be developed inside Toolbx containers that closely match the deployment environments used in production, without being constrained by the specifics of the host operating system.


### Operating System Components

Building system components, not just apps? **Toolbx** has it covered. Developing the [shell](https://gitlab.gnome.org/GNOME/gnome-shell) for GNOME used to be quite cumbersome and satisfying the dependencies a chore. The GNOME Shell developers have streamlined their [workflow using Toolbx](https://gitlab.gnome.org/GNOME/gnome-shell/-/tree/main/tools/toolbox?ref_type=heads) considerably.

[Builder](https://flathub.org/apps/org.gnome.Builder), the IDE for GNOME, has integrated Toolbx in a way that allows running GNOME Shell in nested mode as it was an application. This streamlines the *develop > test > debug > submit merge request* workflow considerably.

![GNOME Shell running inside a toolbx container](../assets/builder-shell-toolbx.webp){: .full}


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

Imagine this: you fire up Ptyxis, select your desired Toolbx container, and boom – you're in! No more configuration hassles or context switching. Ptyxis acts as your direct portal to any container environment, giving you granular control and access to all the tools you need within a familiar terminal interface.

Ptyxis takes advantage of Toolbx's flexibility, allowing you to create custom container configurations tailored to your specific needs. Need a Python development environment? Craft a container with all the necessary libraries and packages. Working on a system component test? Design a container that mirrors your production setup. The possibilities are endless, and Ptyxis empowers you to explore them all with ease. And all those very same containers are accessible from your IDE.

