I have always assumed that the only way of probing safely if an address was
mapped or not was to setup a signal handler for `SIGSEGV`, attempting an access
and recovering from it, which almost always sounds like more trouble than it is
worth.

As part of the discussions on the `xz` backdoor, folks pointed out that a few
Unix system calls will return `EFAULT` if you provide data that they can not
access, things like `access(path, mode)` can be used to probe the address by
passing the address for `path` to be the area you want to probe.