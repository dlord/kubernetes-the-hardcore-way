# Kubernetes the Hardcore Way!

How to install Kubernetes on bare metal. Because AWS and GCP are for pussies!


## Motivation

If you've ever tried installing Kubernetes in an unsupported cloud environment (e.g. bare metal),
you know what I'm talking about.

Ever tried installing Kubernetes on Centos 7? There's [this guide that's totally outdated](http://kubernetes.io/docs/getting-started-guides/centos/centos_manual_config/).

But hey, there's that cool new feature called `kubeadm` right? If you did not scroll to the bottom
of [this quickstart guide](http://kubernetes.io/docs/getting-started-guides/kubeadm/), you would realize
you lose valuable features such as `kubectl logs`.

If that doesn't work, there's always the [official guide to creating a cluster from scratch](http://kubernetes.io/docs/getting-started-guides/scratch/). But when you get to the [Bootstrapping the Cluster](http://kubernetes.io/docs/getting-started-guides/scratch/#bootstrapping-the-cluster)
section, you find out that the setup doesn't work exactly as intended, and you end up with a broken
cluster.

So you give up on the official docs, and go straight to [Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way). The bad thing is that the guide does not explain some of
the most important aspects of the install, such as how to properly setup CNI so you're not limited to
one overlay network solution. What's worse, it doesn't give much explanation about the overlay network
requirements. And if you're like me (I'm not a network guy), you'll be left scratching your head.

And [Weave](https://www.weave.works), my favorite overlay network solution, does not provides a decent
guide on how to properly install and debug if there's a problem!

And the _best part about all of this_? EVERY SINGLE ONE OF THOSE GUIDES INSTALL KUBERNETES IN A DIFFERENT WAY!

The available docs are in such poor shape, you would think they're trying to _force_ you to use AWS or GCP.

This guide is for people looking for a sane way of installing a simple, production-grade Kubernetes installation without going insane.


## Target Audience

This is for people who are looking to build their own Kubernetes cluster in their private data center
or in a virtualized environment, without being shackled to things like Vagrant and AWS/GCP.

For those who almost gave up, but didn't, this one's for you.


## Objectives

At the end of this tutorial, you should be able to:

1. Install Kubernetes directly from the [official binaries](https://github.com/kubernetes/kubernetes/releases)
2. Create a Kubernetes cluster with 2 nodes with the following characteristics:
   * Uses SSL for etcd and the kubernetes components.
   * Use most of the security defaults.
   * etcd, API Server, Controller Manager, and Scheduler running as docker containers in the master node
3. Use Weave as the overlay network.
4. Install DNS Addon