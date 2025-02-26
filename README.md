# Create VM Ansible Role

This Ansible role is designed to automate the creation of a Linux virtual machine (VM) using **virt-install**. The role supports multiple Linux distributions including **Debian**, **Ubuntu**, **Fedora**, and **Arch Linux**. It also integrates **cloud-init** to automatically configure the VM, including:
- *Add SSH key* 
- *Setting a global password (same password for user and root)*
- *Create new regular user (without sudo rights)*
- *Set vm parameters*
---

## Features

- **Distro Options**: Supports **Debian**, **Ubuntu**, **Fedora**, and **Arch Linux**.
- **Password**: Root password is **not** set during VM creation. The user will set the root password manually by logging via ssh after the VM is created.
- **SSH**: Enables **SSH root login** for remote access.
- **Networking**: Configures DHCP for the VM to automatically obtain an IP address.
- **Cloud-Init**: Uses `cloud-init` to customize the VM (e.g., SSH key setup, user creation, and more).
- **Flexible**: Automatically adapts to the selected Linux distro with appropriate `virt-install` and `cloud-init` configurations.

---

## Requirements

- Ansible (tested on version 2.17.8 but may also work on previous versions )
- `virt-install` and `cloud-init` installed on the host system.
- A Linux hypervisor (KVM, libvirt) with the necessary virtual network setup (`virbr0`).
- Access to the internet for downloading cloud images (Debian, Ubuntu, Fedora, Arch).

---

## Role Variables

- `vm_name`: Name of the virtual machine ( ask for user input.).
- `vm_memory`: Amount of memory to allocate to the VM in MB (ask for user input).
- `vm_vcpus`: Number of virtual CPUs to allocate (ask for user input).
- `vm_disk_size`: Amount of disk space in GB (ask for user input).
- `os_variants`: A dictionary for different supported OS versions (choices: `ubuntu`, `debian`, `fedora`, `arch`).
- `cloud_init_iso`: Path to the cloud-init ISO (default: `/var/lib/libvirt/images/cloud-init.iso`).
- `ssh_public_key`: (Optional) Set your public SSH key to allow SSH login ( ask for user input ).
- `global_password`: Global password valid for new user and root ( ask for user input ).
- `user`: Create a regular user without sudo privileges (ask for user input).

### Default variables 

Default values are located in defaults/main.yml:

- `vm_name`: lab.
- `vm_memory`: 2048 MB.
- `vm_vcpus`: 2.
- `vm_disk_size`: 10 GB.
- `os_variants`: arch.
- `ssh_public_key`: ssh for root will not work if user will not provide a key.
- `global_password`: "password".
- `user`: user.

> [!CAUTION] 
> If no password is set user and root will have the default password but ssh will work **only** for user.

---

## Usage

1. **Clone this repository** to your project:
   ```bash
   git clone <repository-url>

2. **Add the role to your playbook**:

Create a playbook to run the role or use the vm-creator.yml playbook

3. **Run the playbook to create a new VM**:
```bash
ansible-playbook -K vm-creator.yml (from step 2)
```
use -K parameter to run with sudo.

4. **Post-VM Creation**:

Find the VM's IP address:
```bash
virsh net-dhcp-leases default
```

5. **SSH into the VM**:
```bash
ssh root@<VM-IP>
```
### Password for user and root is set during the playbook running ("global_password")


## Customization
    
    Modify the  defaults/main.yml to customize the default vm ram,disk size,cpu etc.

    Modify the  vars/main.yml to add/remove/change the linux distro image of vm you would like to create.  

License

MIT License. See LICENSE for more details.

This role utilizes virt-install and cloud-init to simplify VM creation and configuration on Linux-based systems.


This **README.md** file is structured to provide clear documentation on the role, how to install it, configure it, and how to use it with different Linux distributions. 
Let me know if you want any changes or additions! 🚀

