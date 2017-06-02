NFS
===
Configure multiple NFS export and mount points.

The role provides support to both NFS file server and clients. The operating mode is specified by `nfs_mode` (client is the default).

The role supports multiple exports (in server moded) and mounts (in client mode) via `nfs_exports` and `nfs_mounts` lists respectively. Default options for both operating modes are define with variables prefixed `nfs_default_` which can be overrided on each export or mount (see `defaults/main.yml`). Exports on the server are configured using a flexible `/etc/exports.d` directory. The name of this file is set with `nfs_name`. Exported directories are exported/mounted read-only by default. Use `nfs_export_opts` and `nfs_mount_opts` to change that option and others.

For servers it will configure the exports and firewall. For clients it will add the NFS export to fstab and mount it.

The server is configured to support both NFSv3 and NFSv4 (default). On the client side, one can specify which version to use with `nfs_version`.

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
- hosts: fileserver
  roles:
    - role: nfs
      nfs_exports:
        - name: export1
          clients: '{{groups['fileserver-clients']}}'
          export_dir: /dir/to/export1

- hosts: fileserver-clients
  roles:
    - role: nfs
      nfs_mounts:
        - server: fileserver
          export_dir: /dir/to/export1
          mount_point: /mnt/mount1
```

TODO
----
- Configure quotas.
- Configure autofs?
- Export directory permission only works on a single level directory, ie exported directories with subdirs will all get the same permissions, which might be too restrictive.
- Use nfs_version on server.

Licence
-------
Licensed under [CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

Author Information
------------------
Luis Gracia while at [EMBL-EBI](http://www.ebi.ac.uk/):
- luis.gracia [at] ebi.ac.uk
- GitHub at [luisico](https://github.com/luisico)
- Galaxy at [luisico](https://galaxy.ansible.com/luisico)
