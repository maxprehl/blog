# Teaching a new WSL user

## About WSL

Windows Subsystem for Linux (WSL) is a component of Windows that enables a Linux
or Linux-like "environment". The environment in this case is only terminal (or
command line) based access by default.

The difference between WSL 1 and 2 is the "linux-like" vs "linux" experience.
WSL 1 provides a compatability layer in between the linux session and the NT
kernel (windows os). WSL 2 on the other hand, is an entire virtual machine which
uses the true linux kernel.

You can find out more about
[what WSL is](https://docs.microsoft.com/en-us/windows/wsl/about), read the
[frequently asked questions](https://docs.microsoft.com/en-us/windows/wsl/faq),
see the
[differences between WSL 1 & 2](https://docs.microsoft.com/en-us/windows/wsl/compare-versions),
and more on
[Microsoft's WSL docs site](https://docs.microsoft.com/en-us/windows/wsl/).

### Using Powershell

Powershell is the default "shell" for Windows 10. (CMD is only there for
backwards compatibility.) You can use Powershell to check up on your WSL
instances.

From Powershell the `wsl` command provides most of the utilities you will need.

ex. The command to show all installed WSL distributions, their status, version,
and which one is the default (marked with a \*)

```powershell
wsl --list --all -v
```

ex. To change the default WSL version (used to create new instances)

```powershell
wsl --set-default-version 2
```

## VS Code

> VS Code uses the
> [Remote Development extension pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)
> to allow you to hook VS Code into WSL, a remote machine or even a container!

### Remote - WSL

Here's the
[link](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode) to the
Docs on this.

ex. When in your WSL command line, open the current directory (folder) in VS
Code with:

```bash
code .
```

Instead of the period (`.`) you can put the path to any folder on your system.

ex.

```bash
code /home/username/Repos/my-repo
```

### Remote - Containers

This was designed for Docker, but there are workarounds for Podman:

-   [VS Code docs](https://code.visualstudio.com/docs/remote/containers#_can-i-use-podman-instead-of-docker)
-   [VS Code Release Notes](https://github.com/microsoft/vscode-docs/blob/main/remote-release-notes/v1_45.md#podman-support)
-   [Github Comment](https://github.com/microsoft/vscode-remote-release/issues/116#issuecomment-625772330)
