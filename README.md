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
# Environment options for OCI should be added post creation if needed -- use
# --no-start.
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
# mount points are automatically migrated to the new container.
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
