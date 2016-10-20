# Cluster Setup


We will be setting up a 2 node cluster. One master node, and one regular node. For simplicity,
I will use the term _master_ and _slave_.

Our master will act as both the bootstrap node and the main control node. This will not run any
jobs except for certain things like Daemon Sets.

Our slave will run everything else.


## Linux Distro Requirements

Choose a distro that has the following characteristics:

1. Uses Systemd - popular distros like Centos, Debian, and Ubuntu use it.
2. Uses a x64 kernel - while Kubernetes and Docker supports other architectures, let's keep it simple.
3. Uses a kernel version that Docker supports - check with your distro.


## Docker Requirements

Kubernetes requires a fairly recent version of Docker to run. As of this writing, the recommended version
of Docker for Kubernetes 1.4.0 is `1.12.2`.

While some people opt to go with the latest (who doesn't want the latest and greatest?), there are
risks involved. On our production system, we adopt a one-version-behind policy to ensure that we
have enough time to test before updating our cluster. Which is why I will recommend version `1.11.2`
instead.

If you really insist that you want to install the latest, make sure you read the
[Changelog for the docker version you want to use](https://github.com/docker/docker/blob/master/CHANGELOG.md).

### Storage Driver

The default storage driver of Docker varies from Linux distro to distro. If you plan to run your cluster
in a VPS like Linode or Digital Ocean, the storage driver will be dependent on the kernel they provide.

I recommend getting to know more about storage drivers [here](https://docs.docker.com/engine/userguide/storagedriver/selectadriver/).

If you are confused, the most common choices are `devicemapper` or `overlay`. What you use depends
on your needs and your setup.


## Network Requirements

Each node in the cluster must be able to communicate with each other somehow, and it's usually
via a private network.

When running your cluster in a VPS like Linode or Digital Ocean, each node will have 2 IP's:
a public and private one. Make sure to take note of each node's public and private IP.

On a bare metal setup, usually you would only have 1 IP per node. In which case, treat it as both 
the public and private IP.

This will come into play when we start setting up the components of Kubernetes.

You can have multiple clusters spanning multiple data centers that are connected to each other,
but that's beyond the scope of this guide.

### Firewall

Historically, Docker does not play nice with firewalls inside nodes. This has improved over time.
Even [Shorewall has support for Docker](http://shorewall.net/Docker.html).

However, for the purposes of this guide, we will run our nodes without any firewalls. Make sure
to disable all of them. We will be relying on Docker and Kubernetes for managing our iptables.

You may opt to add a firewall once the setup is complete.

### DNS

Kubernetes communicates with each node via the hostname. It is important to have each node accessible
via DNS. Either you setup a DNS server like `dnsmasq`, or have some way to update `/etc/hosts`
for every node.

If this is not properly setup, things like `kubectl logs` won't work properly.

There is a way to override this behavior (via `kubelet`'s `--hostname-override ` flag), but not everyone
likes memorizing IP addresses.
