# Proxmox OCI Image LXC Lifecycle Management
Manage OCI image lifecycle during Proxmox 9.1+ tech preview.

Lifecycle management is current a pain and will likely be vastly improved in
future PVE releases. This script automates all of my current OCI container use
cases including automatically migrating mount points to new OCI container
versions.

## Requirements
Configured proxmox cluster. Script should be placed in user path.

## Usage

:warning: Tech Preview
> Container options for **create** and **migrate** should be the same unless
> those changes are intentional. Both options create a new container. Parsing
> existing config deferred as too labor intensive until OCI support is
> officially released.

``` bash
# Basic create usage. Create a seerr container with a static IP, firewall, and
# 1G config disk. Additional mount points can be added after initial creation.
# Environment options for OCI should be added post creation if needed (use
# --no-start).
oci create \
  --enable-ha \
  --oci ghcr.io/seerr-team/seerr \
  --oci-version v3.4.0 \
  --x-net-ip 192.168.10.10/24 \
  --x-net-gw 192.168.10.1 \
  --x-net-firewall \
  --x-net-firewall-rule 5055:tcp::nolog:seerr \
  --x-hostname iseerr \
  --datafs-size 1 \
  --datafs-mount /app/config \
  --datafs-backup

# Basic migrate usage. Migrate an existing OCI container to a new version. All
# mount points are automatically migrated to the new container. Environment
# options for OCI should be added post creation if needed (use --no-start).
oci migrate \
  --enable-ha \
  --oci ghcr.io/seerr-team/seerr \
  --oci-version v3.5.0 \
  --x-net-ip 192.168.10.10/24 \
  --x-net-gw 192.168.10.1 \
  --x-net-firewall \
  --x-net-firewall-rule 5055:tcp::nolog:seerr \
  --x-hostname iseerr

# Most container options are supported. See examples and help.
oci examples
oci -h
```

### Example Migration
``` bash
oci migrate \
  --enable-ha --oci ghcr.io/maintainerr/maintainerr --oci-version 3.31.0 \
  --oci-storage pve --id 120 --rootfs-storage pve --x-net-ip 192.168.1.209/24 \
  --x-net-gw 192.168.1.1 --x-net-firewall \
  --x-net-firewall-rule 6246:tcp:192.168.1.100,192.168.1.150:nolog:maintainerr \
  --x-net-hwaddr 02:A6:4E:E0:74:3A --x-net-vlan 1 --x-hostname imaintainerr \
  --x-onboot --x-tags oci
🛈 Resolving available container ID... 121
🛈 Check OCI image
➔ Pulling OCI image: ghcr.io/maintainerr/maintainerr:3.31.0 ➔ pve
Getting image source signatures
Copying blob e2de96513ba9 done   |
Copying blob 4aaa392ec283 done   |
Copying blob 9350baae2a14 done   |
Copying blob 4112cd220f4f done   |
Copying blob 33417532b6f4 done   |
Copying blob bd3b8fefd597 done   |
Copying blob 7bf4374a0482 done   |
Copying blob 8d362596e868 done   |
Copying blob 7f98db443f48 done   |
Copying blob 834b428f9dc1 done   |
Copying blob 3f65cf94b68e done   |
Copying blob 2dab83e32b98 done   |
Copying blob 3966cda7c0c0 done   |
Copying blob 27abb6fa4234 done   |
Copying blob efc230316587 done   |
Copying blob 5d4bc8b37961 done   |
Copying blob 9cb1a37d2679 done   |
Copying config d5395bbfdd done   |
Writing manifest to image destination
🛈 Migrate 120
➔ Set HA:120 state=stopped
➔ Waiting for container to stop...
➔ Create 121
Formatting '/d/pve/images/121/vm-121-disk-0.raw', fmt=raw size=4294967296 preallocation=off
Creating filesystem with 1048576 4k blocks and 262144 inodes
Filesystem UUID: 91c9c9bc-ef5f-4d4b-8db7-8a5f37aa7b3a
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736
Detected OCI archive
creating base directory for volume at /opt/data
Auto-Enabling host-managed network for network device net0.
Detected container architecture: amd64
➔ Clone 120 ➔ 121
➔ Migrate 120:mp0 ➔ 121:mp0
moving volume 'mp0' from container '120' to '121'
removing volume 'mp0' from container '120' config
target container '121' updated with 'mp0'
➔ Destroy 120
purging CT 120 from related configurations..
🛈 Add firewall rules 121
➔ Port: 6246, Proto: tcp, Source: 192.168.1.100,192.168.1.150, Log: nolog, Comment: maintainerr
🛈 Set HA:121 state=started
➔ Waiting for container to start...
✔ Success!
```

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License][c] | [direct link][f]

## Author Information
PGP: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9][d] | [github gist][e]

[c]: https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0
[d]: https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9
[e]: https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22
[f]: https://github.com/r-pufky/pve_oci_lxc/blob/main/LICENSE
