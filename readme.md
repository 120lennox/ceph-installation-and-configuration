# Ceph installation and configuration 

This guideline is a summary of Huawei HCIE openEuler lab guide. It contains simplified instructions of how you can get ceph up and running. 

## Chapter I

Key Objectives:
- You'll be learning Ceph architecture
- Performing basic operations like installation
- Managing components
- Installing and deleting hosts

The Preparations section lists 6 key steps:
1. Configure host name resolution
2. Disable firewall and SELinux
3. Configure time synchronization
4. Download and install Cephadm
5. Configure Yum source for Ceph
6. Install the container engine recommended by Ceph

For Step 1 (Host Configuration):
- You need to set up 3 hosts: ceph01, ceph02, and ceph03
- All hosts use the domain .novalocal
- IP addresses are assigned as:
  - ceph01: 192.168.0.1
  - ceph02: 192.168.0.2
  - ceph03: 192.168.0.3

To implement this:

1. First, you'll need to edit the hosts file on each system:
```bash
sudo vi /etc/hosts
```

2. Add these lines to the hosts file:
```
192.168.0.1 ceph01.novalocal ceph01
192.168.0.2 ceph02.novalocal ceph02
192.168.0.3 ceph03.novalocal ceph03
```

3. Test connectivity by pinging each host from ceph01:
```bash
ping ceph02.novalocal
ping ceph03.novalocal
```
