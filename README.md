NFS
===
Configure nfs share and mount points.

The role provides support to both nfs file server and clients, and can be used in a single play (see example below) or on separate plays for servers and clients.

This role can be used multiple times to share different filesystems, clients or options. The exports on the server are configured using a flexible `/etc/exports.d` directory. The name of this file can be set with `nfs_name`.

For servers it will configure the exports and firewall. For clients it will add the nfs share to fstab and mount it.

The server is configured to support both NFSv3 and NFSv4. On the client side, one can specify which version to use with `nfs_version` (defaults to '4').

Shared directories are exported/mounted read-only by default. Use `nfs_share_opts` and `nfs_mount_opts` to change that option and others.

The `quota` package is also installed on servers and clients, but no configured is done at the moment.

Requirements
------------
See `meta/main.yml`.

Role Variables
--------------
See `defaults/main.yml`.

Dependencies
------------
None.

Example Playbook
----------------
Example:
```
- hosts: fileserver.example.com, fileserver-clients
  roles:
  - role: nfs
    nfs_name: share1
    nfs_server: fileserver.example.com
    nfs_clients: "{{groups['fileserver-clients']}}"
    nfs_share: /dir/to/share
    nfs_mount_point: /mnt/share1
```

TODO
----
- Configure quotas.
- Configure autofs?
- Share directory permission only work on a single level directory, ie share directories with subdirs will all get the same permissions, which might be too restrictive.
- Use array to setup multiple nfs shares instead of playing role multiple times (current setup)?

Licence
-------
Licensed under [CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

Author Information
------------------
Luis Gracia <luis.gracia@ebi.ac.uk>
