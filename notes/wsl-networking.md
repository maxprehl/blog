# WSL 2 Networking

Apparently this is like the 10th circle of hell.

## The Impetus

It all started because of ftp.

I was working on some python implementations of ftp, a client and a server.

To develop and test these I needed multiple levels of debugging tools.

The first thing is a working client and server.

## Reference Clients and Servers

In a surprise to absolutely no one, the linux versions of these command line
programs were far better than the built in windows options.

I installed vsFTPd on my Debian bullseye/sid WSL2 instance.

### vsFTPd

This worked fine for just simple interactions using `localhost` from the linux
command line ftp client.

However, when trying to hit it from the Windows built-in ftp client, I always
had a weird issue where the PORT (or EPRT) command would go through correctly,
but it would fail to establish the connection.

This issue was compounded because wireshark (installed on my windows host) could
only see that localhost traffic between windows and linux! With the windows
wireshark instance, I was not able to sniff the localhost traffic inside the WSL
instance (from the linux ftp client to the vsFTPd server).

## The rabbit hole

This led me down a wild goose chase for ways to pick up traffic to/from the WSL
instance, and that led straight into the black hole which is "WSL 2 networking."

## The easy solution

If you want to stop here for the easy solution, it's simple:

Either 1) just use Windows-only programs (a windows ftp server/client) or 2) use
a full linux machine/VM.

I installed wireshark inside my Pop_OS VM and was picking up localhost traffic
lickety-split!

But that didn't stop me from continuing to try to use WSL. (Because whats the
fun in that?)

> Note: Someone remind me to try using VSCode Remote to code inside a Hyper-V VM
> sometime

## Things that didn't work

I had a bunch of things to try that didn't solve the problem right off the bat

1. Using another WSL 2 instance to access the vsFTPd instance
    - traffic is still not visible
    - found out later that all WSL 2 instances share the same IP (and kernel!)
2. Changing the Windows Firewall rules to allow ftp.exe to use public and
   private networks - this literally did nothing
3. Using a VM to access the WSL vsFTPd instance
    - You can't access the WSL instances because they live on the "WSL" virtual
      switch, but this did lead me in the right direction!

## What I ended up doing

I never got to listen to traffic using the Windows wireshark, but since I
already had a linux VM with wireshark and vsFTPd installed...

I basically manually forced my full linux VM onto the "WSL" network.

### Hyper-V Config

In Hyper-V you can see the `Virtual Switch Manager`.

There should be a `Default Switch` and `WSL`. Both of them should be using a
connection type of "Internal network."

> Note: if "WSL" isn't there, try running a wsl command or starting up one of
> your distros. Apparantly it has some
> [randomization](https://github.com/microsoft/WSL/issues/4467) that happens at
> every startup.

After some [hunting](https://stackoverflow.com/a/65960434/12670653) I found out
that these 2 "switches" are basically separate networks, with access to the
"outside" via NAT through the Windows host, but would never have access to each
other.

In the settings for your VM of choice, go to "Network Adapters" and give it the
WSL virtual switch.

Now, when I rebooted the VM, it basically took a dump and could not connect to
any network. That's ok, because the WSL 2 people have been figuring out
[hacky](https://github.com/microsoft/WSL/issues/4467#issuecomment-707932021)
[ways](https://github.com/microsoft/WSL/issues/4150#issuecomment-504209723) to
manually set up the networks ever since WSL 2 launched.

### Windows

Now we need to look at our windows configs:

Go to Powershell and run `ipconfig`. Look for the WSL output:

```txt
Ethernet adapter vEthernet (WSL):

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::e9d7:a9dc:bc4a:2b03%38
   IPv4 Address. . . . . . . . . . . : 172.30.176.1
   Subnet Mask . . . . . . . . . . . : 255.255.240.0
   Default Gateway . . . . . . . . . :
```

You can see my IP for the network was 172.30.176.1 with a /20 subnet.

Using
[ip subnet calculator](https://www.calculator.net/ip-subnet-calculator.html) we
could find out that the usable IP range is from 172.30.176.1 to 172.30.191.254.

Running the commmand `arp -a` we can see all of the network interfaces and the
IPs they have listed. For our adapter we can see:

```txt
Interface: 172.30.176.1 --- 0x26
  Internet Address      Physical Address      Type
  172.30.191.120        00-15-5d-e4-38-83     dynamic
  172.30.191.255        ff-ff-ff-ff-ff-ff     static
  224.0.0.2             01-00-5e-00-00-02     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
```

That one "dynamic" IP is our WSL kernel!

We need to pick an address to assign to our VM. We can choose any address here
that's not taken, but I just chose 172.30.176.5. (In retrospect I probably
should've chose a higher number but oh well.)

### VM ip config

Now to the linux VM.

I followed [this guide](https://stackoverflow.com/a/64217350/12670653) which was
originally written for how to force a WSL 2 instance onto a different network.

Keep tabs on `ip addr` and `ip route` throughout this.

First get rid of any broken config that's leftover there:

```console
$ sudo ip addr flush dev eth0
```

Then we need to give ourselves an address that's in the WSL range but not taken.

```console
$ sudo ip addr add 172.30.176.5/20 dev eth0
```

Then give ourselves a default gateway (through the WSL switch!)

```console
$ sudo ip route add default via 172.30.176.1 dev eth0
```

This should allow us to have LAN access, and hopefully WAN.

DNS might be busted though.

Fix with a new `/etc/resolv.conf`

Including whatever nameserver the WSL instances have, probably the default gate
tho.

```conf
nameserver 172.30.176.1
```

Now we should be able to ping the WSL instance, as well as the internet.

## Using

Using Wireshark inside my VM I could now watch all of the traffic go between my
2 linux instances, including my own code! YAY!
