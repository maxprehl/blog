# Windows and git, a fantastic history

It all began when I tried to configure GitBash. But what exactly **is** GitBash?
Another terminal emulator? The git source control tool for windows? An entire
development environment?! This is where my story begins.

## MinTTY

To configure my _terminal emulator_ settings, I opened the "options" panel of
GitBash.

"mintty 3.4.0 is available!"

### mintty.github.io

> Mintty is a terminal emulator for Cygwin, MSYS or Msys2, and derived projects,
> and for WSL.

MinTTY is merely the default terminal emulator for the following applications:

-   Cygwin
-   MSYS
-   MSYS2

Based on PuTTY 0.60

## cygwin.com

> This is the home of the Cygwin project
>
> What...
>
> ...is it? Cygwin is:
>
> a large collection of GNU and Open Source tools which provide functionality
> similar to a Linux distribution on Windows. a DLL (cygwin1.dll) which provides
> substantial POSIX API functionality.
>
> ...isn't it? Cygwin is not:
>
> a way to run native Linux apps on Windows. You must rebuild your application
> from source if you want it to run on Windows. a way to magically make native
> Windows apps aware of UNIX® functionality like signals, ptys, etc. Again, you
> need to build your apps from source if you want to take advantage of Cygwin
> functionality.

## MinGW

The Minimal Gnu system for Windows

## mingw.org/wiki/MSYS

> MSYS is a collection of GNU utilities such as bash, make, gawk and grep to
> allow building of applications and programs which depend on traditionally UNIX
> tools to be present.
>
> A common misunderstanding is MSYS is "UNIX on Windows", MSYS by itself does
> not contain a compiler or a C library, therefore does not give the ability to
> magically port UNIX programs over to Windows nor does it provide any UNIX
> specific functionality like case-sensitive filenames. Users looking for such
> functionality should look to Cygwin or Microsoft's Interix instead.

MSYS could be thought of as a minimal version of Cygwin.

### mingw.org/node/30

Interix,

known as Microsoft's **SFU**, Services for Unix, and now it is called **SUA**, a
Subsystem for Unix Application. Similar to **Cygwin**, It is a full-featured
UNIX/POSIX subsystem that runs on the top of the Microsoft Windows NT-based
operating systems.

### wiki.tcl-lang.org/page/Interix

This is part of the original 3 pronged kernel that Microsoft had designed for
NT, with compatibility with win32, POSIX, and OS/2.

Found out this site is actually related to a programming language called
**tcl**.

I ended up following one of the main guys on twitter
([Donal Fellows](twitter.com/donalfellows)), but the other guy,
[Larry W. Virden](https://wiki.tcl-lang.org/page/Larry+Virden) aka LV aka
lvirden, seems to have his twitter hacked by botspam.

## msys2.org

Source at github.com/msys2

> **MSYS2** is a collection of tools and libraries providing you with an
> easy-to-use environment for building, installing and running native Windows
> software.
>
> It consists of a command line terminal called **mintty**, **bash**, version
> control systems like **git** and subversion, tools like tar and awk and even
> build systems like autotools, all based on a modified version of **Cygwin**.
> Despite some of these central parts being based on Cygwin, the main focus of
> MSYS2 is to provide a build environment for native Windows software and the
> Cygwin-using parts are kept at a minimum. MSYS2 provides up-to-date native
> builds for GCC, mingw-w64, CPython, CMake, Meson, OpenSSL, FFmpeg, Rust, Ruby,
> just to name a few.

### [Who is using MSYS2](https://www.msys2.org/docs/who-is-using-msys2/)

> -   Git for Windows is based on MSYS2
> -   The R Project build system rtools40 is based on MSYS2 icw/ a custom
>     repository of mingw packages (all statically linked).

And so it seems we've come full circle... **Git for Windows** is based on MSYS2,
which uses **mintty** as it's terminal emulator. But what is this "Git for
Windows"? That's not the same _name_ as GitBash...

## gitforwindows.org

Boring homepage

### github.com/git-for-windows/git/wiki

Eureka.

>

Here's a
[podcast interview](https://www.allthingsgit.com/episodes/git_for_windows_with_johannes_schindelin.html)
with the maintainer of Git for Windows.
