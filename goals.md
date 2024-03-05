---
layout: default
---

<picture class="full pixels">
    <source srcset="../assets/goals-dark.gif" media="(prefers-color-scheme: dark)">
    <img src="../assets/goals.gif">
</picture>

## Goals

- Restore the usual Linux command line environment on [OSTree](https://ostreedev.github.io/ostree/) based operating systems for development and troubleshooting the host OS. This is crucial for adoption of these OSes.

- Decouple development environments and troubleshooting tools from the host OS.

- Fit for use as a default command line shell.

  - Reduce the cognitive overhead of using [OCI](https://opencontainers.org/) containers.

  - One shouldn't have to debug why the SSH agent or graphical apps aren’t working.

- Support for multiple [distributions](/distros):

  - Curated Toolbx images.

  - Packages for the `toolbox(1)` command.

Examples of use cases that fit these goals can be found [here](/use).

## Non-goals

- Adding significant features on top of [Podman](https://podman.io/). These should be driven into Podman itself.

- Application distribution format. Use [Flatpak](https://flatpak.org/) for client applications and Podman for server applications.

- Containers that aren't tightly integrated with the host operating system. Extremely sandboxed containers quickly become specific to the user.

- Secure development environment:

  - Security is not testability. Flatpak makes desktop applications more secure, OSTree makes the host OS testable, and Toolbx restores the usual Linux command line environment for those who need it.

  - Toolbx is just as secure as classical package based OSes.

- Supporting multiple container runtimes. Toolbx will exclusively use Podman unless it unlocks significant development and troubleshooting use cases. It will take significant resources to drive changes and test across Docker, Podman, etc..

- Using every single OCI image out there. It's always possible to use `podman run`.
