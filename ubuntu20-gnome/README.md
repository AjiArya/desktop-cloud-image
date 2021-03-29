# Ubuntu

# Guide

1. Download the image

    ```bash
    wget \
    https://cloud-images.ubuntu.com/focal/current/focal-server-cloudimg-amd64.img
    ```

2. Backup downloaded image

    ```bash
    cp focal-server-cloudimg-amd64.img focal-server-cloudimg-amd64.img.backup
    ```

3. Resize using `qemu-img`

    ```bash
    qemu-img resize focal-server-cloudimg-amd64.img +5G
    ```

4. Get information of the image `qemu-img` (The virtual size should be larger than original file)

    ```bash
    qemu-img info focal-server-cloudimg-amd64.img
    ```

5. Extend using `virt-customize`

    ```bash
    virt-customize -v -a focal-server-cloudimg-amd64.img \
      --run-command 'growpart /dev/sda 1; resize2fs /dev/sda1' \
      --selinux-relabel
    ```

6. Verify using `virt-filesystems`

    ```bash
    virt-filesystems --all --filesystems --long -h -a focal-server-cloudimg-amd64.img
    ```

# Installing GUI

1. Install GUI using `virt-customize`

```bash
virt-customize -v -a focal-server-cloudimg-amd64.img \
  --run-command "apt update -y; apt upgrade -y" \
  --run-command "apt install -y ubuntu-desktop-minimal gdm3" \
  --selinux-relabel -v
```