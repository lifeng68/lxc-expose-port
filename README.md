# lxc-expose-port

lxc-expose-port is a simple script that sets up port forwarding for LXC
containers. It was influenced by https://github.com/evanhempel/lxc-portforward
but is not a fork of that project.

Unlike lxc-portforward, lxc-expose-port correctly handles connections to the
external port from the host itself and from other containers. Also, it does
not need any extra configuration file.

The script, as it is, works on Ubuntu 14.04 and 15.10 only. To support other
systems, replace this line

 . /etc/default/lxc-net

with a line that sets $LXC_NETWORK to the network used by LXC containers, in
CIDR notation (e.g., to 10.0.3.0/34).

# Basic usage

Copy lxc-expose-port to /etc/lxc. Then, in your container config, add
something like this:

 lxc.network.script.up = /etc/lxc/lxc-expose-port 8080:80 2222:22
 lxc.network.script.down = /etc/lxc/lxc-expose-port

The example above will cause the host's port 8080 to be forwarded to port
80 of the container, and will also forward port 2222 to port 22 of the
container.

# Usage with LXC Web Panel

LXC Web Panel (https://github.com/claudyus/LXC-Web-Panel) is a convenient tool
for managing LXC containers with a web browser. Unfortunately, it does not
allow spaces and colons in lxc.network.script.up. The problem is merely due to
a too-strict validation rule, and can be fixed. Edit lwp/utils.py and change
this line:

 file_match = '^\/\w[\w.\/-]+$'

to this:

 file_match = '^\/\w[\w.\/: -]+$'

and then restart lwp.

# Copyright

The lxc-expose-port script bears the following copyright:

 Copyright (C) 2016 Alexander E. Patrakov <patrakov@gmail.com>
 Sponsored by NobleProg Ltd <http://www.nobleprog.co.uk>
 Distributed under the MIT license, see the LICENSE file.
