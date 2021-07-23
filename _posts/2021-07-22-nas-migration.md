---
layout: post
title: "Migrating my NAS from OpenMediaVault to Ubuntu"
comments: false
description: "Diary of how and why I migrate my NAS"
keywords: "NAS, server, docker, container, zfs, vm, linux"
---

Taking advantage of my last week of vacation, I decided to take the task of updating my NAS Server.

# How it was
My NAS server is the most important piece of tech infrastructure in my home, it acts as:
* A NAS, making my HDS redudant using RAID, pooling these HDs, presenting them to me as one big
disk; And sharing my files to other devices through SAMBA.
* A entertaiment server, serving music, movies and series to my other devices using Plex.
* A development server, hosting virtual machines for my projects, allowing me to access them remotely.
* The host of my Home Assist OS, when this machine goes down, my lights won't even turn on!

## OS
I was using a OS called OpenMediaVault, it is designed exactly for my use case, it's based on
debian, support raid using mdadm, but has a bunch of plugins, including docker support, zfs,
snapraid and a virtualbox plugin.

I installed the OS on may of 2018, and the server has been running almost flawlessly since then.

## Storage
* **System Drive**: 256GB nvme SSD using a USB adaptor.

* **ZFS**: 2 HDs in RAID1 (mirror in zfs lang). I use this to keep my personal files, vms,
application data, backups,etc. I have no words for ZFS, if you don't know, I sugest that you look
into it, it's one of the most incredibly pieces of tecnology that I have worked it, and I think no
other file system is close to achive what ZFS does.

* **Snapraid**: 4 4TBs HDs in "RAID5. This software is AWESOME, it acts like a RAID system, but on a
file system level, that way all the hard drives are formated normally in ext, nfst, xfs, whatever
you want, and then you assing one or two hard drives to store parity data (acting like a RAID5
or RAID6 setup), but it do not stripe this data, so the parity drive is separated from the data
drives. This way, in a event of a drive failure, you can restore your data, and if all goes wrong
and the restoration fails, you can scrap the drives for data, you're only going to lose the data on
the drive that failed.

* **mergerFS**: Is a FUSE file system to pool all the drives managed by snapraid, it creates a
single directory with all the files to read and write to, and it will dristribute all the data that
you write to the drives, following the rule of your choise.

## Shares
All of this is shared through the network via SMB, since I have Android, Windows, IOS and Linux
clients, it works really nice. Samba was offered natively by open media vault.

## Containers
All the services on my machine was hosted with docker, qbittorrent, plex, sonarr, radarr etc. One
thing to note is that I have a separate volume to docker on the ZFS pool, this way I can keep all
the container data in a single place, and make snapshots before and update, if all goes south, I
just need to undo the upgrade and restore the snapshot.

## Virtual Machines
Open Media Vault has a nice Virtualbox plugin, I used it to host my Home Assistant, wich I like to
keep on its on OS along with mqtt and network stuff, this way I can migrate the whole machine to
another host easly and keep my home automations working.

This is also where I put my development machines, this way I have access anywhere to work, and keep
host clean.

# What is wrong ?
Fortunally, not much. But some things made me move on.
* Most of the features that I used was through community maintened plugins.
* The version that I was using reached its end of life, and there was no official upgrade path.
* Every version of the OS has major changes, this is very good for someone who like to tinker with
the server, I like to tinker ON my server, and have the minimum amount of maintence possible.
* It's kinda hard to backup, most settings are available only with the gui, I don't know about new
versions, but other OS's let you backup your whole build to a configuration file.

# How I think it should be
## OS
I was looking for a simple general use server distro, most of the NAS distros have tradeoffs or too
much features that I won't use.

This way, I settled with ubuntu server 20.04. Alltough I like OSes
on the Red Hat family, you can't go wrong the Debian way too, and ZFS being a first class citzen
on ubuntu was a major factor, all I have to do is apt install zfsutils-linux, and DONE.

One feature that I like about the dedicated NAS OSes is a GUI to monitor what is happening on the
server. I looked into many software to create a WEB UI for my server, like webmin, cockpit and
container managment like portainer, but it's way much.
Then, I found Glances. It runs inside a container, and just give me a nice screen with all the info
I need, and on top of that, it has a really nice integration with home assitant.

![Glances](/assets/glances_screen.png)

![Glances ingration](/assets/glances_homeassistant.png)

## Storage
Nothing to change here, back then I spent over a week studying and researching about file systems,
backups and redundancy, and I'm really happy with what I chose. BRTFS still don't seems to be a
option, the only other software that I'm keeping an eye is bcacheFS, it appears to fit well my use
case, I'm waiting to it get merged upstream to test it.

## Shares
I'll keep using SAMBA, but I like it to run inside a container to facilitate future migrations.
This image ([dperson/samba](https://github.com/dperson/samba)) worked really well,
all I had to do was add it to my docker compose and disable the samba service on the HOST, all my
clients connected to it without a problem.

## Containers
With docker being phased out on multiple places in favor of podman, I considered migrating to it.
But, podman it's not on Ubuntu's repository, and still lacks a tool like docker-compose. So I'll
keep using docker-ce + docker-compose for now.

## Virtual Machines.
I wanted to drop VirtualBox, altough in the end I was always able to do what I want, managin my VMS,
either through CLI or the Web GUI, was a pain the ass. Would like to migrate my VMS to KVM.

# Migration
## Rehearsal
Created a ubuntu server VM on my machine, compiled snapraid and mergerFS, installed ZFS and docker.
Just to see what I'll be dealing with, no surprises there, was able to create a functional server
in a couple of minutes.

Ok, next step: Backups! Created a snapshot of my docker volume. Copied all my configuration files
(fstab, snapraid, scripts, etc.). Created a snapshot on home assistant OS.

And finally created a image of the whole system disk,

```
dd if=/dev/sdx of=myimage.img bs=1M
```
This way, if something goes wrong, I have the last resource to simple restore my working machine.

## Install
This was the installation of the new OS in a nutshell.
1. Install Ubuntu Server,
2. Install ZFS
3. Compile and install snapraid
4. Compile and install mergerFS
5. Setup users
6. Import my zfs pool
7. Mount disk
8. Run docker-compose
9. Setup scripts
10. Install home assistant OS with KVM (really nice tutorial here:
[Home Assistant Community Tutorial](https://community.home-assistant.io/t/install-home-assistant-os-with-kvm-on-ubuntu-headless-cli-only/254941))

The whole process took about 3 hours, after solving the obligatory permissions issue, everything
just worked.

# Future
I'm pretty happy with the system now, and anything that I would wanna change, its going to be pretty
easy from here. Migrate from dockert to podman, or look into kubernetes, isolate the hard drives
in a machine separete from the containers, move the home assistant to a NUC, so many cool things to
do.

Anyway, Ubuntu support ends in 2030, I have until then to think about that.
