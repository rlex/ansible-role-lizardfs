Basically rewrite of https://github.com/stenub/lizardfs_ansible_playbook
Differences:
  * Only one role with role params to select parts of lizardfs
  * Manages only LizardFS and nothing more

Pretty basic overall but easy to expand, i use it for a very simple 2-node setup

Example playbook
```
- name: Install and configure lizardfs
  hosts: all
  roles:
    - { role: lizardfs, tags: lizardfs, when: lizardfs_managed }
```

Master:
```
lizardfs_managed: true
lizardfs_masterserver: true
lizardfs_master_personality: master
lizardfs_masterserver_host: 10.91.91.71
lizardfs_cgiserver: true
lizardfs_exports:
  - '192.168.18.0/24 / rw,alldirs,maproot=0'
```

Shadowmaster and chunkserver:
```
lizardfs_managed: true
lizardfs_masterserver: true
lizardfs_master_personality: master
lizardfs_masterserver_host: 10.91.91.71
lizardfs_chunkserver: true
lizardfs_metalogger: true
lizardfs_disks:
  - /mnt/lizard_vol1
  - /mnt/lizard_vol2
  - /mnt/lizard_vol3
  - /mnt/lizard_vol4
lizardfs_exports:
  - '10.11.11.0/24 / rw,alldirs,maproot=0'
```

Just lizardfs repo & client:
```
lizardfs_managed: true
lizardfs_client: true
```

You can then mount lizardfs volumes with ansible like this:
```
- name: Mount lizardfs volume
  mount:
    path: /mnt/lizard_root
    src: mfsmount
    opts: rw,mfsmaster=10.91.91.71
    fstype: fuse
    state: present
```
