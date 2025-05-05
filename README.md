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
| mon_max_pg_per_osd | 300 | Max PG per OSD |
| public_network | 10.0.0.0/24 | Ceph network CIDR |
| volume_name | cephfs | CephFS volume name |
| pool_name | cephrbd | CephRBD pool name |
| pg_num | 8 | pg per pool (see below) |

## PGs per pool calculations

A valid calculation to determine a default PG num per pool is :

```
( ( 100 * osd_total ) / replication_factor ) / pool_count
```
Then, choose the nearest power of 2.

### Example 1

Those roles create:
 - 2 pools for CephFS
 - 1 pool for CephRBD
 - 8 pools for RGW

So if you decide to install one of each on a cluster with 3 OSDs with a replication factor of 3, you would get :

 ```
( (100 * 3 ) / 3 ) / 11 =~ 9
```
The nearest power of 2 is **8**

### Example 2

If you decide to install one RGW and one CephFS in a cluster with 5 OSDs and a replication factor of 2, you would get :

```
( (100 * 5 ) / 2 ) / 9 =~ 27
```
The nearest power of 2 is **32**
