# Activity 3: Perform the activity2 on redhat(centos)
* I need to setup a redhat(centos) vm with devops user
* manual steps:

```
sudo yum install httpd -y
sudo yum install php php-cli php-common php-gd php-mysqlnd php-pdo -y
```
* Lets create a file ``` /var/www/html/info.php ``` with the following content

```
<?php phpinfo(); ?>
```
* Playbook & inventory file provided below

```yml
---
- name: activity 3 install apache and php
  become: yes
  hosts: all
  tasks:
    - name: install apache
      ansible.builtin.yum:
        name:
          - httpd
          - php
          - php-cli 
          - php-common
        state: present
    - name: create info.php
      ansible.builtin.copy:
        content: '<?php phpinfo(); ?>'
        dest: /var/www/html/info.php
```
* hosts

```
172.31.23.151
```
* Execute ansible playbook after syntax check and check

```
ansible-playbook -i hosts php.yaml
```
* We need to start the service check below for the changes done to start and enable the service

```yml
---
- name: activity 3 install apache and php
  become: yes
  hosts: all
  tasks:
    - name: install apache
      ansible.builtin.yum:
        name:
          - httpd
          - php
          - php-cli 
          - php-common
        state: present
    - name: create info.php
      ansible.builtin.copy:
        content: '<?php phpinfo(); ?>'
        dest: /var/www/html/info.php
    - name: ensure httpd is running
      ansible.builtin.service:
        name: httpd
        enabled: yes
        state: started
```
# Activity 4: Use the same playbook for ubuntu and redhat machines
* To do this activity, the two topics which we need to understand are
    * inventory groups
    * facts

# Inventories in Ansible
* Inventory is collection of nodes.
* Inventory is of two types
    * Static
    * Dynamic

* Inventory can be written in two formats
    * ini format
    * yaml format

* ini format

```
ipaddress/name

[group1]
ipaddress/name
ipaddress/name

[group2]
ipaddress/name
ipaddress/name
```
* example1

```
172.31.30.90
172.31.17.52
172.31.23.151
```

* example2

```
[local]
172.31.30.90

[nodes]
172.31.17.52
172.31.23.151
```
* [Refer Here](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#defining-variables-in-ini-format) for inventory documentation

![Preview](./Images/ansible18.png)

# fact collection
* [Refer Here](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_vars_facts.html#ansible-facts) for ansible facts
* To see the facts we have a module called as setup [Refer Here](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/setup_module.html)

* facts provided below

```yml
{
    "ansible_facts": {
        "ansible_all_ipv4_addresses": [
            "172.31.30.90"
        ],
        "ansible_all_ipv6_addresses": [
            "fe80::2a:e5ff:fec3:da99"
        ],
        "ansible_apparmor": {
            "status": "enabled"
        },
        "ansible_architecture": "x86_64",
        "ansible_bios_date": "08/24/2006",
        "ansible_bios_vendor": "Xen",
        "ansible_bios_version": "4.11.amazon",
        "ansible_board_asset_tag": "NA",
        "ansible_board_name": "NA",
        "ansible_board_serial": "NA",
        "ansible_board_vendor": "NA",
        "ansible_board_version": "NA",
        "ansible_chassis_asset_tag": "NA",
        "ansible_chassis_serial": "NA",
        "ansible_chassis_vendor": "Xen",
        "ansible_chassis_version": "NA",
        "ansible_cmdline": {
            "BOOT_IMAGE": "/boot/vmlinuz-6.2.0-1013-aws",
            "console": "ttyS0",
            "nvme_core.io_timeout": "4294967295",
            "panic": "-1",
            "ro": true,
            "root": "PARTUUID=0f687d73-f2aa-4143-b9be-b065fa2d72de"
        },
        "ansible_date_time": {
            "date": "2023-10-08",
            "day": "08",
            "epoch": "1696772979",
            "epoch_int": "1696772979",
            "hour": "13",
            "iso8601": "2023-10-08T13:49:39Z",
            "iso8601_basic": "20231008T134939257990",
            "iso8601_basic_short": "20231008T134939",
            "iso8601_micro": "2023-10-08T13:49:39.257990Z",
            "minute": "49",
            "month": "10",
            "second": "39",
            "time": "13:49:39",
            "tz": "UTC",
            "tz_dst": "UTC",
            "tz_offset": "+0000",
            "weekday": "Sunday",
            "weekday_number": "0",
            "weeknumber": "40",
            "year": "2023"
        },
        "ansible_default_ipv4": {
            "address": "172.31.30.90",
            "alias": "eth0",
            "broadcast": "",
            "gateway": "172.31.16.1",
            "interface": "eth0",
            "macaddress": "02:2a:e5:c3:da:99",
            "mtu": 9001,
            "netmask": "255.255.240.0",
            "network": "172.31.16.0",
            "prefix": "20",
            "type": "ether"
        },
        "ansible_default_ipv6": {},
        "ansible_device_links": {
            "ids": {},
            "labels": {
                "xvda1": [
                    "cloudimg-rootfs"
                ],
                "xvda15": [
                    "UEFI"
                ]
            },
            "masters": {},
            "uuids": {
                "xvda1": [
                    "4513eb34-58e6-408e-8ed7-3d487fe6b35b"
                ],
                "xvda15": [
                    "6192-5E23"
                ]
            }
        },
        "ansible_devices": {
            "loop0": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "0",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "49944",
                "sectorsize": "512",
                "size": "24.39 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop1": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "0",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "51000",
                "sectorsize": "512",
                "size": "24.90 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop2": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "0",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "113944",
                "sectorsize": "512",
                "size": "55.64 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop3": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "0",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "113992",
                "sectorsize": "512",
                "size": "55.66 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop4": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "0",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "129712",
                "sectorsize": "512",
                "size": "63.34 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop5": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "0",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "129976",
                "sectorsize": "512",
                "size": "63.46 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop6": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "0",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "229272",
                "sectorsize": "512",
                "size": "111.95 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop7": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "0",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "109032",
                "sectorsize": "512",
                "size": "53.24 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop8": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "0",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "83648",
                "sectorsize": "512",
                "size": "40.84 MB",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "loop9": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {},
                "removable": "0",
                "rotational": "0",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "none",
                "sectors": "0",
                "sectorsize": "512",
                "size": "0.00 Bytes",
                "support_discard": "4096",
                "vendor": null,
                "virtual": 1
            },
            "xvda": {
                "holders": [],
                "host": "",
                "links": {
                    "ids": [],
                    "labels": [],
                    "masters": [],
                    "uuids": []
                },
                "model": null,
                "partitions": {
                    "xvda1": {
                        "holders": [],
                        "links": {
                            "ids": [],
                            "labels": [
                                "cloudimg-rootfs"
                            ],
                            "masters": [],
                            "uuids": [
                                "4513eb34-58e6-408e-8ed7-3d487fe6b35b"
                            ]
                        },
                        "sectors": "16549855",
                        "sectorsize": 512,
                        "size": "7.89 GB",
                        "start": "227328",
                        "uuid": "4513eb34-58e6-408e-8ed7-3d487fe6b35b"
                    },
                    "xvda14": {
                        "holders": [],
                        "links": {
                            "ids": [],
                            "labels": [],
                            "masters": [],
                            "uuids": []
                        },
                        "sectors": "8192",
                        "sectorsize": 512,
                        "size": "4.00 MB",
                        "start": "2048",
                        "uuid": null
                    },
                    "xvda15": {
                        "holders": [],
                        "links": {
                            "ids": [],
                            "labels": [
                                "UEFI"
                            ],
                            "masters": [],
                            "uuids": [
                                "6192-5E23"
                            ]
                        },
                        "sectors": "217088",
                        "sectorsize": 512,
                        "size": "106.00 MB",
                        "start": "10240",
                        "uuid": "6192-5E23"
                    }
                },
                "removable": "0",
                "rotational": "0",
                "sas_address": null,
                "sas_device_handle": null,
                "scheduler_mode": "mq-deadline",
                "sectors": "16777216",
                "sectorsize": "512",
                "size": "8.00 GB",
                "support_discard": "0",
                "vendor": null,
                "virtual": 1
            }
        },
        "ansible_distribution": "Ubuntu",
        "ansible_distribution_file_parsed": true,
        "ansible_distribution_file_path": "/etc/os-release",
        "ansible_distribution_file_variety": "Debian",
        "ansible_distribution_major_version": "22",
        "ansible_distribution_release": "jammy",
        "ansible_distribution_version": "22.04",
        "ansible_dns": {
            "nameservers": [
                "127.0.0.53"
            ],
            "options": {
                "edns0": true,
                "trust-ad": true
            },
            "search": [
                "us-west-2.compute.internal"
            ]
        },
        "ansible_domain": "us-west-2.compute.internal",
        "ansible_effective_group_id": 1001,
        "ansible_effective_user_id": 1001,
        "ansible_env": {
            "DBUS_SESSION_BUS_ADDRESS": "unix:path=/run/user/1001/bus",
            "HOME": "/home/devops",
            "LANG": "C.UTF-8",
            "LOGNAME": "devops",
            "MOTD_SHOWN": "pam",
            "PATH": "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin",
            "PWD": "/home/devops",
            "SHELL": "/bin/bash",
            "SHLVL": "0",
            "SSH_CLIENT": "172.31.30.90 36782 22",
            "SSH_CONNECTION": "172.31.30.90 36782 172.31.30.90 22",
            "SSH_TTY": "/dev/pts/2",
            "TERM": "xterm-256color",
            "USER": "devops",
            "XDG_RUNTIME_DIR": "/run/user/1001",
            "XDG_SESSION_CLASS": "user",
            "XDG_SESSION_ID": "3",
            "XDG_SESSION_TYPE": "tty",
            "_": "/bin/sh"
        },
        "ansible_eth0": {
            "active": true,
            "device": "eth0",
            "features": {
                "esp_hw_offload": "off [fixed]",
                "esp_tx_csum_hw_offload": "off [fixed]",
                "fcoe_mtu": "off [fixed]",
                "generic_receive_offload": "on",
                "generic_segmentation_offload": "on",
                "highdma": "off [fixed]",
                "hsr_dup_offload": "off [fixed]",
                "hsr_fwd_offload": "off [fixed]",
                "hsr_tag_ins_offload": "off [fixed]",
                "hsr_tag_rm_offload": "off [fixed]",
                "hw_tc_offload": "off [fixed]",
                "l2_fwd_offload": "off [fixed]",
                "large_receive_offload": "off [fixed]",
                "loopback": "off [fixed]",
                "macsec_hw_offload": "off [fixed]",
                "netns_local": "off [fixed]",
                "ntuple_filters": "off [fixed]",
                "receive_hashing": "off [fixed]",
                "rx_all": "off [fixed]",
                "rx_checksumming": "on [fixed]",
                "rx_fcs": "off [fixed]",
                "rx_gro_hw": "off [fixed]",
                "rx_gro_list": "off",
                "rx_udp_gro_forwarding": "off",
                "rx_udp_tunnel_port_offload": "off [fixed]",
                "rx_vlan_filter": "off [fixed]",
                "rx_vlan_offload": "off [fixed]",
                "rx_vlan_stag_filter": "off [fixed]",
                "rx_vlan_stag_hw_parse": "off [fixed]",
                "scatter_gather": "on",
                "tcp_segmentation_offload": "on",
                "tls_hw_record": "off [fixed]",
                "tls_hw_rx_offload": "off [fixed]",
                "tls_hw_tx_offload": "off [fixed]",
                "tx_checksum_fcoe_crc": "off [fixed]",
                "tx_checksum_ip_generic": "off [fixed]",
                "tx_checksum_ipv4": "on [fixed]",
                "tx_checksum_ipv6": "on",
                "tx_checksum_sctp": "off [fixed]",
                "tx_checksumming": "on",
                "tx_esp_segmentation": "off [fixed]",
                "tx_fcoe_segmentation": "off [fixed]",
                "tx_gre_csum_segmentation": "off [fixed]",
                "tx_gre_segmentation": "off [fixed]",
                "tx_gso_list": "off [fixed]",
                "tx_gso_partial": "off [fixed]",
                "tx_gso_robust": "on [fixed]",
                "tx_ipxip4_segmentation": "off [fixed]",
                "tx_ipxip6_segmentation": "off [fixed]",
                "tx_lockless": "off [fixed]",
                "tx_nocache_copy": "off",
                "tx_scatter_gather": "on",
                "tx_scatter_gather_fraglist": "off [fixed]",
                "tx_sctp_segmentation": "off [fixed]",
                "tx_tcp6_segmentation": "on",
                "tx_tcp_ecn_segmentation": "off [fixed]",
                "tx_tcp_mangleid_segmentation": "off",
                "tx_tcp_segmentation": "on",
                "tx_tunnel_remcsum_segmentation": "off [fixed]",
                "tx_udp_segmentation": "off [fixed]",
                "tx_udp_tnl_csum_segmentation": "off [fixed]",
                "tx_udp_tnl_segmentation": "off [fixed]",
                "tx_vlan_offload": "off [fixed]",
                "tx_vlan_stag_hw_insert": "off [fixed]",
                "vlan_challenged": "off [fixed]"
            },
            "hw_timestamp_filters": [],
            "ipv4": {
                "address": "172.31.30.90",
                "broadcast": "",
                "netmask": "255.255.240.0",
                "network": "172.31.16.0",
                "prefix": "20"
            },
            "ipv6": [
                {
                    "address": "fe80::2a:e5ff:fec3:da99",
                    "prefix": "64",
                    "scope": "link"
                }
            ],
            "macaddress": "02:2a:e5:c3:da:99",
            "mtu": 9001,
            "pciid": "vif-0",
            "promisc": false,
            "timestamping": [],
            "type": "ether"
        },
        "ansible_fibre_channel_wwn": [],
        "ansible_fips": false,
        "ansible_form_factor": "Other",
        "ansible_fqdn": "ip-172-31-30-90.us-west-2.compute.internal",
        "ansible_hostname": "ip-172-31-30-90",
        "ansible_hostnqn": "",
        "ansible_interfaces": [
            "lo",
            "eth0"
        ],
        "ansible_is_chroot": false,
        "ansible_iscsi_iqn": "",
        "ansible_kernel": "6.2.0-1013-aws",
        "ansible_kernel_version": "#13~22.04.1-Ubuntu SMP Fri Sep  8 17:29:56 UTC 2023",
        "ansible_lo": {
            "active": true,
            "device": "lo",
            "features": {
                "esp_hw_offload": "off [fixed]",
                "esp_tx_csum_hw_offload": "off [fixed]",
                "fcoe_mtu": "off [fixed]",
                "generic_receive_offload": "on",
                "generic_segmentation_offload": "on",
                "highdma": "on [fixed]",
                "hsr_dup_offload": "off [fixed]",
                "hsr_fwd_offload": "off [fixed]",
                "hsr_tag_ins_offload": "off [fixed]",
                "hsr_tag_rm_offload": "off [fixed]",
                "hw_tc_offload": "off [fixed]",
                "l2_fwd_offload": "off [fixed]",
                "large_receive_offload": "off [fixed]",
                "loopback": "on [fixed]",
                "macsec_hw_offload": "off [fixed]",
                "netns_local": "on [fixed]",
                "ntuple_filters": "off [fixed]",
                "receive_hashing": "off [fixed]",
                "rx_all": "off [fixed]",
                "rx_checksumming": "on [fixed]",
                "rx_fcs": "off [fixed]",
                "rx_gro_hw": "off [fixed]",
                "rx_gro_list": "off",
                "rx_udp_gro_forwarding": "off",
                "rx_udp_tunnel_port_offload": "off [fixed]",
                "rx_vlan_filter": "off [fixed]",
                "rx_vlan_offload": "off [fixed]",
                "rx_vlan_stag_filter": "off [fixed]",
                "rx_vlan_stag_hw_parse": "off [fixed]",
                "scatter_gather": "on",
                "tcp_segmentation_offload": "on",
                "tls_hw_record": "off [fixed]",
                "tls_hw_rx_offload": "off [fixed]",
                "tls_hw_tx_offload": "off [fixed]",
                "tx_checksum_fcoe_crc": "off [fixed]",
                "tx_checksum_ip_generic": "on [fixed]",
                "tx_checksum_ipv4": "off [fixed]",
                "tx_checksum_ipv6": "off [fixed]",
                "tx_checksum_sctp": "on [fixed]",
                "tx_checksumming": "on",
                "tx_esp_segmentation": "off [fixed]",
                "tx_fcoe_segmentation": "off [fixed]",
                "tx_gre_csum_segmentation": "off [fixed]",
                "tx_gre_segmentation": "off [fixed]",
                "tx_gso_list": "on",
                "tx_gso_partial": "off [fixed]",
                "tx_gso_robust": "off [fixed]",
                "tx_ipxip4_segmentation": "off [fixed]",
                "tx_ipxip6_segmentation": "off [fixed]",
                "tx_lockless": "on [fixed]",
                "tx_nocache_copy": "off [fixed]",
                "tx_scatter_gather": "on [fixed]",
                "tx_scatter_gather_fraglist": "on [fixed]",
                "tx_sctp_segmentation": "on",
                "tx_tcp6_segmentation": "on",
                "tx_tcp_ecn_segmentation": "on",
                "tx_tcp_mangleid_segmentation": "on",
                "tx_tcp_segmentation": "on",
                "tx_tunnel_remcsum_segmentation": "off [fixed]",
                "tx_udp_segmentation": "on",
                "tx_udp_tnl_csum_segmentation": "off [fixed]",
                "tx_udp_tnl_segmentation": "off [fixed]",
                "tx_vlan_offload": "off [fixed]",
                "tx_vlan_stag_hw_insert": "off [fixed]",
                "vlan_challenged": "on [fixed]"
            },
            "hw_timestamp_filters": [],
            "ipv4": {
                "address": "127.0.0.1",
                "broadcast": "",
                "netmask": "255.0.0.0",
                "network": "127.0.0.0",
                "prefix": "8"
            },
            "ipv6": [
                {
                    "address": "::1",
                    "prefix": "128",
                    "scope": "host"
                }
            ],
            "mtu": 65536,
            "promisc": false,
            "timestamping": [],
            "type": "loopback"
        },
        "ansible_loadavg": {
            "15m": 0.00537109375,
            "1m": 0.080078125,
            "5m": 0.0166015625
        },
        "ansible_local": {},
        "ansible_locally_reachable_ips": {
            "ipv4": [
                "127.0.0.0/8",
                "127.0.0.1",
                "172.31.30.90"
            ],
            "ipv6": [
                "::1",
                "fe80::2a:e5ff:fec3:da99"
            ]
        },
        "ansible_lsb": {
            "codename": "jammy",
            "description": "Ubuntu 22.04.2 LTS",
            "id": "Ubuntu",
            "major_release": "22",
            "release": "22.04"
        },
        "ansible_lvm": "N/A",
        "ansible_machine": "x86_64",
        "ansible_machine_id": "94bb51c7ba414fff868a3870c33c6ece",
        "ansible_memfree_mb": 273,
        "ansible_memory_mb": {
            "nocache": {
                "free": 670,
                "used": 279
            },
            "real": {
                "free": 273,
                "total": 949,
                "used": 676
            },
            "swap": {
                "cached": 0,
                "free": 0,
                "total": 0,
                "used": 0
            }
        },
        "ansible_memtotal_mb": 949,
        "ansible_mounts": [
            {
                "block_available": 1241694,
                "block_size": 4096,
                "block_total": 1985394,
                "block_used": 743700,
                "device": "/dev/root",
                "fstype": "ext4",
                "inode_available": 889680,
                "inode_total": 1032192,
                "inode_used": 142512,
                "mount": "/",
                "options": "rw,relatime,discard,errors=remount-ro",
                "size_available": 5085978624,
                "size_total": 8132173824,
                "uuid": "4513eb34-58e6-408e-8ed7-3d487fe6b35b"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 196,
                "block_used": 196,
                "device": "/dev/loop0",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 16,
                "inode_used": 16,
                "mount": "/snap/amazon-ssm-agent/6312",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 25690112,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 200,
                "block_used": 200,
                "device": "/dev/loop1",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 16,
                "inode_used": 16,
                "mount": "/snap/amazon-ssm-agent/7628",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 26214400,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 446,
                "block_used": 446,
                "device": "/dev/loop2",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 10905,
                "inode_used": 10905,
                "mount": "/snap/core18/2745",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 58458112,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 446,
                "block_used": 446,
                "device": "/dev/loop3",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 10944,
                "inode_used": 10944,
                "mount": "/snap/core18/2790",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 58458112,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 507,
                "block_used": 507,
                "device": "/dev/loop4",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 11954,
                "inode_used": 11954,
                "mount": "/snap/core20/1879",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 66453504,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 508,
                "block_used": 508,
                "device": "/dev/loop5",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 11991,
                "inode_used": 11991,
                "mount": "/snap/core20/2015",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 66584576,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 896,
                "block_used": 896,
                "device": "/dev/loop6",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 873,
                "inode_used": 873,
                "mount": "/snap/lxd/24322",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 117440512,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 426,
                "block_used": 426,
                "device": "/dev/loop7",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 658,
                "inode_used": 658,
                "mount": "/snap/snapd/19122",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 55836672,
                "uuid": "N/A"
            },
            {
                "block_available": 0,
                "block_size": 131072,
                "block_total": 327,
                "block_used": 327,
                "device": "/dev/loop8",
                "fstype": "squashfs",
                "inode_available": 0,
                "inode_total": 658,
                "inode_used": 658,
                "mount": "/snap/snapd/20092",
                "options": "ro,nodev,relatime,errors=continue,threads=single",
                "size_available": 0,
                "size_total": 42860544,
                "uuid": "N/A"
            },
            {
                "block_available": 201345,
                "block_size": 512,
                "block_total": 213716,
                "block_used": 12371,
                "device": "/dev/xvda15",
                "fstype": "vfat",
                "inode_available": 0,
                "inode_total": 0,
                "inode_used": 0,
                "mount": "/boot/efi",
                "options": "rw,relatime,fmask=0077,dmask=0077,codepage=437,iocharset=iso8859-1,shortname=mixed,errors=remount-ro",
                "size_available": 103088640,
                "size_total": 109422592,
                "uuid": "6192-5E23"
            }
        ],
        "ansible_nodename": "ip-172-31-30-90",
        "ansible_os_family": "Debian",
        "ansible_pkg_mgr": "apt",
        "ansible_proc_cmdline": {
            "BOOT_IMAGE": "/boot/vmlinuz-6.2.0-1013-aws",
            "console": [
                "tty1",
                "ttyS0"
            ],
            "nvme_core.io_timeout": "4294967295",
            "panic": "-1",
            "ro": true,
            "root": "PARTUUID=0f687d73-f2aa-4143-b9be-b065fa2d72de"
        },
        "ansible_processor": [
            "0",
            "GenuineIntel",
            "Intel(R) Xeon(R) CPU E5-2676 v3 @ 2.40GHz"
        ],
        "ansible_processor_cores": 1,
        "ansible_processor_count": 1,
        "ansible_processor_nproc": 1,
        "ansible_processor_threads_per_core": 1,
        "ansible_processor_vcpus": 1,
        "ansible_product_name": "HVM domU",
        "ansible_product_serial": "NA",
        "ansible_product_uuid": "NA",
        "ansible_product_version": "4.11.amazon",
        "ansible_python": {
            "executable": "/usr/bin/python3",
            "has_sslcontext": true,
            "type": "cpython",
            "version": {
                "major": 3,
                "micro": 12,
                "minor": 10,
                "releaselevel": "final",
                "serial": 0
            },
            "version_info": [
                3,
                10,
                12,
                "final",
                0
            ]
        },
        "ansible_python_version": "3.10.12",
        "ansible_real_group_id": 1001,
        "ansible_real_user_id": 1001,
        "ansible_selinux": {
            "status": "disabled"
        },
        "ansible_selinux_python_present": true,
        "ansible_service_mgr": "systemd",
        "ansible_ssh_host_key_dsa_public": "AAAAB3NzaC1kc3MAAACBAN0iD2c454KWhefc94n9pAvRIagCTUyooL69az8NiCcPJF5q8SATx8WHIxENUeaRHZsb7I0wZgscw+gEyuOIVazwjk7XEVXBfOZnwLBeovdv6UXkBiX92EfxqnJxCRpfk4obTfB4/U6hFV2rbFQaSUTY1hUn9q5Jd2QuD0Zcr8QZAAAAFQClCDYE/QmA1iK6/tLiCgdcq0OCkwAAAIEA2OkkyU51GH3PO+c05E9VtjkABlVhhRkYISbcRwbLLWJTSTB6UmCvZe72ZiQdKlDwkLFsjnz3BDc2YdTPt3tkXp4Ob3zVeh+3VEJLE9cziS91aF2XfgBlm1dXGQgEamdpaxGJWPBJqeg35Eoszxt/KNXvhS2ksufY4SxFbXHXy/cAAACALso4iTixb6RxjsSCMnDQAqqiefcgPKgpkTb4Gz386+WAJKL9yDqvzTxaIasbpcRQI2ZLFtJpejY0Ab8/kjrs7WzVSb/wBHExZtzG68hbGYcgChmHmkRd9qteIJOXNNTlRuAZqB3+kkOdLRFrJwRGyqs6haiLYH1t9y/ZT8+YG4I=",
        "ansible_ssh_host_key_dsa_public_keytype": "ssh-dss",
        "ansible_ssh_host_key_ecdsa_public": "AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNlU1cau65e6fjmVJlL78taEk6MAOgjq8eTYNLzTLQC92/sWeuXJQRV39pceadnAYQfR4CWn9CMtIZxJC01BDHI=",
        "ansible_ssh_host_key_ecdsa_public_keytype": "ecdsa-sha2-nistp256",
        "ansible_ssh_host_key_ed25519_public": "AAAAC3NzaC1lZDI1NTE5AAAAIFVCbd9YSk0XbKw+6a9PBCfSG7y5hOzidx6GNV7dikG1",
        "ansible_ssh_host_key_ed25519_public_keytype": "ssh-ed25519",
        "ansible_ssh_host_key_rsa_public": "AAAAB3NzaC1yc2EAAAADAQABAAABgQC2QIA6Hx6ydgwfaRGR9IgAQ6QSqG1kqejVIV6e4sA8iE8wVNAqOXO2+dZ5D9gjrdwWeUo+gRr1Mc8Xmu0cc9roxpH3Iizki5+fT8WBVwAO0C1kcE3RM4ThYQpk3b1+BIzTDGsSM5A7R1a3vjuX6GdbMW+kmhpb1TV4qudIS//o+XIxFQQL50iMHZFSPrCL46mgBnZIiLb3mEx9lp2kmfIM8xu6eDZD7FAXPJR7fdDDX3itulSgHCzatZ6lMrPM4QDiIDXA7xarPp7fqlbOvlBrDKO6OucALzL+L+M8S+yPfgRCxsIWnsi4h3J9I0itDJaKDw9uBIuRoiQh0FcV7e55YHZPr37h4FBl3Fo3ERmPZzC4lALMnxthYwdCNEuu6xGxocGksGffnGdHRdq0d0Cy0cunUc4UQkV7Jxg9hsTmnkVp5EmqSyCr5Q+w+nPoneHf16tX3BeaED7eUXd/jiBxPs/WsZvzgakeswWKtOLWj8L/j4TGi4yhs/sKD9cgtp0=",
        "ansible_ssh_host_key_rsa_public_keytype": "ssh-rsa",
        "ansible_swapfree_mb": 0,
        "ansible_swaptotal_mb": 0,
        "ansible_system": "Linux",
        "ansible_system_capabilities": [
            ""
        ],
        "ansible_system_capabilities_enforced": "True",
        "ansible_system_vendor": "Xen",
        "ansible_uptime_seconds": 1181,
        "ansible_user_dir": "/home/devops",
        "ansible_user_gecos": ",,,",
        "ansible_user_gid": 1001,
        "ansible_user_id": "devops",
        "ansible_user_shell": "/bin/bash",
        "ansible_user_uid": 1001,
        "ansible_userspace_architecture": "x86_64",
        "ansible_userspace_bits": "64",
        "ansible_virtualization_role": "guest",
        "ansible_virtualization_tech_guest": [
            "xen"
        ],
        "ansible_virtualization_tech_host": [],
        "ansible_virtualization_type": "xen",
        "discovered_interpreter_python": "/usr/bin/python3",
        "gather_subset": [
            "all"
        ],
        "module_setup": true
    },
    "changed": false
}
```
# Activity 4: Lets write one playbook for installing php in both redhat and ubuntu
* What is needed?
   * inventory
   * facts
   * using facts with conditionals [Refer Here](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_conditionals.html)

* Playbook provided below

```yml
---
- name: activity 2 - install php
  become: yes
  hosts: webservers
  tasks:
    - name: install apache and php package
      ansible.builtin.apt:
        name:
          - apache2
          - php 
          - libapache2-mod-php 
          - php-mysql
        update_cache: yes
        state: present
      when: ansible_facts["os_family"] == "Debian"
    - name: install apache
      ansible.builtin.dnf:
        name:
          - httpd
          - php
          - php-cli 
          - php-common
        state: present
      when: ansible_facts["os_family"] == "RedHat"
    - name: create info.php
      ansible.builtin.copy:
        content: '<?php phpinfo(); ?>'
        dest: /var/www/html/info.php
    - name: ensure httpd is running
      ansible.builtin.service:
        name: httpd
        enabled: yes
        state: started
      when: ansible_facts["os_family"] == "RedHat"
```

* Add a failback message for unsupported operating systems [Refer Here](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/fail_module.html) for module. Provided below for the changes done to the playbook

```yml
---
- name: activity 2 - install php
  become: yes
  hosts: webservers
  tasks:
    - name: fail on unsupported operating systems
      ansible.builtin.fail:
        msg: "This playbook will work on Debian and RedHat Only"
      when: ansible_facts["os_family"] != "Debian" and ansible_facts["os_family"] != "RedHat"

    - name: install apache and php package
      ansible.builtin.apt:
        name:
          - apache2
          - php 
          - libapache2-mod-php 
          - php-mysql
        update_cache: yes
        state: present
      when: ansible_facts["os_family"] == "Debian"
    - name: install apache
      ansible.builtin.dnf:
        name:
          - httpd
          - php
          - php-cli 
          - php-common
        state: present
      when: ansible_facts["os_family"] == "RedHat"
    - name: create info.php
      ansible.builtin.copy:
        content: '<?php phpinfo(); ?>'
        dest: /var/www/html/info.php
    - name: ensure httpd is running
      ansible.builtin.service:
        name: httpd
        enabled: yes
        state: started
      when: ansible_facts["os_family"] == "RedHat"







---
- name: activity 2 - install php
  become: yes
  hosts: webservers
  tasks:
    - name: fail on unsupported operating systems
      ansible.builtin.fail:
        msg: "This playbook will work on Debian and RedHat Only"
      when: ansible_facts["os_family"] != "Debian" and ansible_facts["os_family"] != "RedHat"
      
    - name: install apache and php package
      ansible.builtin.apt:
        name: "{{ all_packages }}"
        update_cache: yes
        state: present
      when: ansible_facts["os_family"] == "Debian"
    - name: install apache
      ansible.builtin.dnf:
        name: "{{ all_packages }}"
        state: present
      when: ansible_facts["os_family"] == "RedHat"
    - name: create info.php
      ansible.builtin.copy:
        content: '<?php phpinfo(); ?>'
        dest: /var/www/html/info.php
    - name: ensure httpd is running
      ansible.builtin.service:
        name: "{{ package_name }}"
        enabled: yes
        state: started
      when: ansible_facts["os_family"] == "RedHat"
```

