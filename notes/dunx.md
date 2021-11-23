# Dunx is a public facing webserver running Solaris

```console
mwp52@dunx1:~$ uname -a
SunOS dunx1 5.10 Generic_144489-11 i86pc i386 i86pc Solaris
```

http://bhami.com/rosetta.html#software

## TTY/PTY and SSH??

http://www.linusakesson.net/programming/tty/index.php

```console
mwp52@dunx1:~$ tty
/dev/pts/1
mwp52@dunx1:~$ ls -la /dev/pts/1
lrwxrwxrwx 1 root root 28 May  4  2011 /dev/pts/1 -> ../../devices/pseudo/pts@0:1
mwp52@dunx1:~$ ls -la /devices/pseudo/pts@0:1
crw--w---- 1 mwp52 tty 24, 1 Jun  3 03:23 /devices/pseudo/pts@0:1
```

```console
mwp52@dunx1:~$ stty -a
speed 38400 baud; rows 45; columns 120;
intr = ^C; quit = ^\; erase = ^?; kill = ^U; eof = ^D; eol = <undef>; eol2 = <undef>; swtch = <undef>; start = ^Q;
stop = ^S; susp = ^Z; dsusp = ^Y; rprnt = ^R; werase = ^W; lnext = ^V; flush = ^O;
-parenb -parodd cs8 -hupcl -cstopb cread -clocal -crtscts
-ignbrk brkint -ignpar -parmrk -inpck -istrip -inlcr -igncr icrnl ixon -ixoff -iuclc ixany imaxbel
opost -olcuc -ocrnl onlcr -onocr -onlret -ofill -ofdel nl0 cr0 tab3 bs0 vt0 ff0
isig icanon iexten echo echoe echok -echonl -noflsh -xcase -tostop -echoprt echoctl echoke
```


