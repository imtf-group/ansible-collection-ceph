# ansible-collection-ceph

## Installation

```
ansible-galaxy collection install git+https://github.com/imtf-group/ansible-collection-ceph
```

## Usage

### Playbook

```
ansible-playbook imtf.ceph.create_cluster -i inventory
```

### Roles

```
- hosts: all
  collections:
    - imtf.ceph
  roles:
    - create_osd
```

## Examples

### All-in-one inventory

```
ceph_mon:
  hosts:
    ceph-1:
      ansible_host: 10.0.0.7
    ceph-2:
      ansible_host: 10.0.0.8
    ceph-3:
      ansible_host: 10.0.0.5
ceph_osd:
  hosts:
    ceph-1:
      ansible_host: 10.0.0.7
    ceph-2:
      ansible_host: 10.0.0.8
    ceph-3:
      ansible_host: 10.0.0.5
```

### Scalable installation inventory

```
ceph_mon:
  hosts:
    ceph-1:
      ansible_host: 10.0.0.7
    ceph-2:
      ansible_host: 10.0.0.8
    ceph-3:
      ansible_host: 10.0.0.5
ceph_osd:
  hosts:
    ceph-4:
      ansible_host: 10.0.0.9
    ceph-5:
      ansible_host: 10.0.0.4
    ceph-6:
      ansible_host: 10.0.0.3
```

## Variables

| Name | Default value | Description |
|------|---------------|-------------|
| all_devices | false | Auto-detect the devices where to create OSD |
| osd_devices | [sdb] | List the devices where to create OSD |
| ceph_mon_host_group | {first group} | Ceph MON ansible host group. To update if a MON hosts belongs to several groups |
| public_network | 10.0.0.0/24 | Ceph network CIDR |
| volume_name | cephfs | CephFS volume name |
| pool_name | cephrbd | CephRBD pool name |
