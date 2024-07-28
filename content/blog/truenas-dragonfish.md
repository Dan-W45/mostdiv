+++
title = 'Truenas Dragonfish'
date = 2024-06-22T14:32:02+01:00
draft = true
+++

## Enabling apt
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
## Showing correct free & used space in File Explorer
SMB shares do not show free/total instead display free/free resulting in a useless
capacity bar in Windows. SMB Auxiliary Parameters are missing from the GUI.
Editing smb4.conf to include `zfs_core:zfs_space_enabled = true` will fix that
however the file will revert after each restart, there is no persistant way to fix this yet.
```
nano /etc/smb4.conf
zfs_core:zfs_space_enabled = true
service smbd restart
```

## Showing correct free & used space in File Explorer Pt.2

After some internet digging new methods have been found to make this presistant after rebooting
by adding `zfs_core:zfs_space_enabled = true` to the auxiliary parameters in the place which it
would be from the GUI in previous TrueNAS versions.

As root user in shell:
```
midclt call sharing.smb.query | jq
midclt call sharing.smb.update <id> '{"auxsmbconf": "zfs_core:zfs_space_enabled = true"}'
```



To view and confirm the changes the file can be viewed in the TrueNAS CLI:
```
cli
sharing smb query (grab the ID of your share)
sharing smb update x (where x = your share ID)
```

