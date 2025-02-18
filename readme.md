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

Step 2 - Disable Firewall and SELinux:
1. On all hosts, run:
```bash
systemctl disable firewalld
systemctl stop firewalld
```

2. To disable SELinux:
```bash
# Edit the SELinux config file
sudo vi /etc/selinux/config
# Change SELINUX=enforcing to:
SELINUX=disabled
# Then reboot the system
sudo reboot
```
Instructions number 1 and 2 might not work if you're installing ceph on WSL. So you're free to skip them. Also, you might need to use `sudo` to run the commands.

Step 3 - Configure Time Synchronization:
1. Install and enable chrony on all hosts:
```bash
yum install -y chrony
systemctl enable chronyd --now
```

2. Verify time synchronization:
```bash
chronyc sources
```
The output should show time sources similar to the screenshot, with multiple NTP servers and low offset values.

Step 4 - Install Cephadm (on ceph01 only):
1. Clone the Cephadm repository:
```bash
git clone https://gitee.com/yftxa/openeuler-cephadm.git
```
if the repo is not available or you're having problems, you can install cephadm manually as follows 

```bash
yum install -y cephadm 
```
or 
```bash
sudo dnf install -y cephadm
```

The ping tests in the second image confirm successful host name resolution between all nodes, showing:
- ceph01 can reach ceph02 (192.168.0.22)
- ceph01 can reach ceph03 (192.168.0.23)
- All pings are successful with 0% packet loss

