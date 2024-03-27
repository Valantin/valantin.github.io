---
layout: post
title:  "Damn macos!"
date:   2024-03-27 15:04:27 +0100
categories: puppet beaker vagrant libvirt
---
I've start to do things in the Voxpupuli community, because I need feature for my production
environment and don't won't to continue to maintain a local module fork.

The true motivation was 'I need fun in my daily work'.

So I've approached the #voxpupuli channel on IRC and with a lot of new stuff to learn I set up my environment to contribute in the (I hope) correct way.

But damn mac have applesilicon and I have to run things on x86, so I started using my workstation with a remote vscode.
It's boring, but it works.

At some point my podman configuration doesn't work like the docker based workflow, and for bug I need to test functionality on vm.

So someone, probably @ekohl, suggest me to try on vagrant-libvirt, damn, another things I've never used before, but it just work on my Debian.

So I start to contrib.

At some point I get bored to connect to my workstation for run test, so my mind said "you can connect to a remote libvirt".
I need to configure vagrant-libvirt to use remote host, It can do, but I have to do it on macos.

Lickily it's not so complicated, set it up and beaker can create the vm on my workstation, but I can't connect to the remote private network.

My mind starts to overthink "we need a tunnel to route the connection". So I start looking a solution with tecnology I use daily, but I'm not on Linux, so I ask my colleague and it suggests using a simple VPN, but I've allready skip that solution, at some point it suggest me sshuttle and it work.
But meantime I realized that vagrant use standard ssh and I know how to get around the ssh problem of log into a not directly accessible server.

The simple solution was ProxyJump or, for the limitation of vagrant, ProxyCommand.

`.vagrant.d/Vagrantfile`

{% highlight bash %}
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.provider :libvirt do |libvirt|
    libvirt.uri = "qemu+ssh://server/system"
  end
  config.ssh.proxy_command = "ssh -W %h:%p user@server"
end
{% endhighlight %}
