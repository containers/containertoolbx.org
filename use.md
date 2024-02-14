---
layout: default
fedora-version: 39
---

<picture class="full pixels">
    <source srcset="../assets/builder-dark.gif" media="(prefers-color-scheme: dark)">
    <img src="../assets/builder.gif">
</picture>

## Desktop Apps
**Toolbx isn't your average constrained container tool**. The place where **toolbx** containers shine is the desktop. While modern apps designed for the future are built on [Flatpak](https://flatpak.org), you might need to run an old style application that intertwined with the OS (the old school distro package). Toolbx is an ideal aid in that scenario. You can use a particular distro image and install a plentitude of packages the app might need, without affecting the host OS. 

In fact, the distro in the container can be a different system. Say a 3rd party app is only available as a Debian or Ubuntu package. No problem to run it on your Fedora Silverblue!

## Development
**Building system components, not just apps?** Toolbx has you covered. Developing the system shell for GNOME used to be quite cumbersome and satisfying the dependencies a chore. The shell developers have streamlined their [workflow using toolbox](https://gitlab.gnome.org/GNOME/gnome-shell/-/tree/main/tools/toolbox?ref_type=heads) considerably.

But working with your containers isn't restricted to your commandline. [Builder](https://flathub.org/apps/org.gnome.Builder), the integrated development environment for GNOME has integrated toolbx containers in a way that allows to develop and test applications literally at a push of a button.

![GNOME Shell running inside a toolbx container](../assets/builder-shell-toolbx.webp){: .full}

## Ptyxis Terminal

**Say goodbye to juggling containers and terminals!** [Ptyxis](https://gitlab.gnome.org/chergert/prompt), the game-changing terminal emulator app, seamlessly integrates with Toolbx, making container management a breeze. Whether you're a seasoned developer or just dipping your toes into the container world, Ptyxis streamlines your workflow.

Imagine this: you fire up Ptyxis, select your desired Toolbx container, and boom – you're in! No more complex commands, configuration hassles, or context switching. Ptyxis acts as your direct portal to any container environment, giving you granular control and access to all the tools you need within a familiar terminal interface.

Ptyxis takes advantage of Toolbx's flexibility, allowing you to create custom container configurations tailored to your specific needs. Need a Python development environment? Craft a container with all the necessary libraries and packages. Working on a system component test? Design a container that mirrors your production setup. The possibilities are endless, and Ptyxis empowers you to explore them all with ease. And all those very same containers are accessible from your IDE.

Forget clunky VMs, Toolbx is the container solution for developers who demand control and flexibility.
