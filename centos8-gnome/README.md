# CentOS

# Guide

1. Download the image

    ```bash
    wget \
    https://cloud.centos.org/centos/8/x86_64/images/CentOS-8-GenericCloud-8.2.2004-20200611.2.x86_64.qcow2 \
    -O template-centos8.qcow2
    ```

2. Backup downloaded image

    ```bash
    cp template-centos8.qcow2 template-centos8.qcow2.backup
    ```

3. Resize using `qemu-img`

    ```bash
    qemu-img resize template-centos8.qcow2 +5G
    ```

4. Get information of the image `qemu-img` (The virtual size should be larger than original file)

    ```bash
    qemu-img info template-centos8.qcow2
    ```

5. Extend using `virt-customize`

    ```bash
    virt-customize -a template-centos8.qcow2 -v \
      --run-command 'growpart /dev/sda 1; xfs_growfs -d /dev/sda1' \
      --selinux-relabel
    ```

6. Verify using `virt-filesystems`

    ```bash
    virt-filesystems --all --filesystems --long -h -a template-centos8.qcow2
    ```

# Installing GUI

1. Install GUI using `virt-customize`

```bash
virt-customize -a template-centos8.qcow2 -v \
  --run-command 'dnf update -y;dnf upgrade -y; dnf groupinstall -y "Server with GUI"' \
  --run-command 'systemctl set-default graphical.target' \
  --selinux-relabel
```