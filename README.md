# Proxmox OCI Image LXC Lifecycle Management
Manage OCI image lifecycle during Proxmox 9.1+ tech preview.

## Requirements
Configured proxmox cluster. Script should be placed in user's path.

``` bash
# Create a new Seerr OCI container with a 1G config disk.

oci create \
  --oci ghcr.io/seerr-team/seerr \
  --oci-version v3.0.0 \
  --id 700 \
  --enable-ha \
  --node pve2 \
  --rootfs-size 4 \
  --rootfs-opts discard,noatime \
  --rooffs-backup \
  --x-net-name eth0 \
  --x-net-type veth \
  --x-net-bridge vmrb0 \
  --x-net-ip 192.168.10.10/24 \
  --x-net-gw 192.168.10.1 \
  --x-net-firewall \
  --x-net-hwaddr 02:DD:CE:26:9A:1E \
  --x-net-vlan 5 \
  --x-hostname iseerr \
  --x-cores 4 \
  --x-mem 4096 \
  --x-onboot \
  --x-tags oci \
  --x-startup order=2000 \
  --datafs-storage local-lvm \
  --datafs-size 1 \
  --datafs-mount 0 \
  --datafs-opts discard,noatime \
  --datafs-backup

# Migrate Seerr OCI container to new version (container re-created and migrate
# all mp's to new container).

oci migrate \
  --oci ghcr.io/seerr-team/seerr \
  --oci-version v3.5.0 \  # New version to use.
  --id 700 \  # Use existing container ID.
  --enable-ha \
  --node pve2 \
  --rootfs-size 4 \
  --rootfs-opts discard,noatime \
  --rooffs-backup \
  --x-net-name eth0 \
  --x-net-type veth \
  --x-net-bridge vmrb0 \
  --x-net-ip 192.168.10.10/24 \
  --x-net-gw 192.168.10.1 \
  --x-net-firewall \
  --x-net-hwaddr 02:DD:CE:26:9A:1E \
  --x-net-vlan 5 \
  --x-hostname iseerr \
  --x-cores 4 \
  --x-mem 4096 \
  --x-onboot \
  --x-tags oci \
  --x-startup order=2000
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
