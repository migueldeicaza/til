It is possible to reconfigure a running tailscale from the command line
without terminating your connection.  Handy when you are away from home
and can not get to the menu directly.

In my case, I wanted to enable my machine at home to be an exit node,
so I ran:

```
$ Tailscale up  --advertise-exit-node
```