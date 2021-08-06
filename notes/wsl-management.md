# WSL Management

I was under the assumption that you could "give them to your friends like
pokemon"... not necessarily the best analogy here.

## Impetus

I originally started using WSL to keep clutter down on my main Windows desktops.
But now it was getting to the point where these linux systems all had Python,
Java, Racket, and an assortment of other garbage collecting on them.

It was time to do something.

## Docs

Most info available
[here](https://docs.microsoft.com/en-us/windows/wsl/reference#arguments-for-managing-windows-subsystem-for-linux).

Kind of sad it took me this long to find.

## Exporting

Simply `wsl --export` they said. It will be easy they said.

> Note: this command can take a long time.

ex.

```powershell
PS> wsl --export Debian "G:\VMs\wsl\Debian-sid-v2.tar"
```

## Importing

> UPDATE: more docs available
> [here](https://docs.microsoft.com/en-us/windows/wsl/use-custom-distro)

The command `wsl --import` is the name, creating distros is the game.

This is the ONLY way to create multiple distros (of the same base).  
Installing from the store will always just give you ONE and only one instance.

ex.

```powershell
PS> wsl --import Debian-sid-v2 "G:\VMs\wsl\Debian-sid-v2.tar" --version 2
```

You can also use this to create a copy of your distro using a different WSL
version

```powershell
PS> wsl --import Debian-sid-v1 "G:\VMs\wsl\Debian-sid-v1" "G:\VMs\wsl\Debian-sid-v2.tar" --version 1
```

### Using an imported distro

You're default user will be reset to `root`.

In order to remediate this you need to boot into the distro and modify its
`/etc/wsl.conf` file.

```sh
echo -e "[user]\ndefault=max" >> /etc/wsl.conf
```

It would also behoove you to make sure that

-   sudo is installed
-   your user is a sudoer
-   your user is in the `wheel` group
