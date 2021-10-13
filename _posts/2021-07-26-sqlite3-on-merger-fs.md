---
layout: post
title: "Sqlite3 error on MergerFS"
comments: false
description: "How fix sqlite error mergerfs"
keywords: "NAS, linux, filesystem, mergerfs, debug"
---

Today when setting up an application that used an sqlite database, I received this error:

```
sqlite3.OperationalError: disk I/O error
```

In the past I had these errors, but when writing to a sqlite db using a samba share, due to locking
issues, but this time the application was on a container in the same machine.

After a couple hours of researching and debugging, I managed to isolate the bug to the filesystem
that I was trying to run originally, a MergerFS mount.

Turns out, it's a cache issue. MergerFS being a FUSE fs, living in the userspace, the kernel will
double cache the files, once on the underlying fs, and again on the FUSE fs.

The issue is well documented on the [github](https://github.com/trapexit/mergerfs) page, setting
the cache to *partial*, enabling the option *dropcacheonclose* was enough to solve the problem.

This is how the fstab entry looks like now:

```
fuse.mergerfs defaults,allow_other,cache.files=partial,dropcacheonclose=true,use_ino,category.create=mfs,minfreespace=4G 0 0
```
