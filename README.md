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

And the _best part about all of this_? **EVERY SINGLE ONE OF THOSE GUIDES INSTALL KUBERNETES IN A DIFFERENT WAY!**

The available docs are in such poor shape, you would think they're _trying to force you to use AWS
or GCP_.

This guide is for people looking for a sane way of installing a simple, production-grade Kubernetes installation without going insane.


## Target Audience

This is for people who are looking to build their own Kubernetes cluster in their private data center
or in a virtualized environment, without being shackled to things like Vagrant and AWS/GCP.

For those who almost gave up, this one's for you.


## Objectives

At the end of this tutorial, you should be able to:

1. Install Kubernetes directly from the [official binaries](https://github.com/kubernetes/kubernetes/releases)
2. Create a Kubernetes cluster with 2 nodes with the following characteristics:
   * Uses SSL for etcd and the kubernetes components.
   * Use most of the security defaults.
   * etcd, API Server, Controller Manager, and Scheduler running as docker containers in the master node
3. Use Weave as the overlay network.
4. Install DNS Addon

I'll do my best to explain almost every part of the process, so you can better customize the cluster
to suit your needs.


## What's not available here

Things that are better documented elsewhere, like how to install your favorite Linux distro and Docker.
In which case, I will try to point you to the corresponding guide on how to install them.


## What about CoreOS?

I don't use it, so I can't write anything about it.

There are plenty of guides for it already. This is for people who want to use their favorite distro
like CentOS, Debian, or Ubuntu.


## Requirements

At the minimum, I will assume your distro uses Systemd, and it uses a x64 kernel that supports
Docker and OverlayFS.

You will need a copy of the [official binaries](https://github.com/kubernetes/kubernetes/releases).
For this guide, we will use version 1.4.0. The steps outlined here should be applicable to later
versions.

If you are using an older release of Kubernetes, you will need to double check the flags used by
the Kubernetes components. Some are considered deprecated or have been removed entirely.


## Table of Contents

* [Cluster Setup](docs/cluster-setup.md)
* Preparation
  * [Download the Official Binary](docs/download-kubernetes.md)
