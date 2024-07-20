+++
title = 'Truenas Dragonfish'
date = 2024-06-22T14:32:02+01:00
draft = true
+++

In versions of TrueNAS 24.04.0 and newer you need to remount the virtual drive(s) before
you can use chmod +x

```
mount -o remount,rw 'boot-pool/ROOT/24.04.0/usr'
```

You need to make sure that /usr/local/bin isn't in your PATH too.

```
export PATH=/usr/bin:/usr/sbin
```

and then

```
chmod +x /bin/apt*
chmod +x /usr/bin/dpkg
```