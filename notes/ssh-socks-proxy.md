# SSH SOCKS PROXY

To run ssh as a SOCKS server.

-   `-T` - disable pseudo-terminal allocation
-   `-D <port>` - dynamic port forwarding listening on local port
-   `tux` - ssh server destination
-   `&` - run this in the background (optional)

```sh
$ ssh -TD 1080 tux &

# Welcome to tux
```

## Firefox config:

-   SOCKS Host - localhost
-   Port - 1080
-   SOCKSv5 - yes

## References

<https://www.ocf.berkeley.edu/~xuanluo/sshproxywin.html>

<https://support.mozilla.org/en-US/questions/1083282>

## Proxying a RDP connection

-   `-L` - port forward
-   `[bind_address:]port:` - local address/port to listen on
-   `host:hostport` - remote host/port to forward to

```sh
$ ssh -TL 1089:10.246.245.XXX:3389 tux &

# Welcome to tux
```
