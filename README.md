# Ansible Azure Disk Operations

This project provides a set of Ansible playbooks to automate common Azure VM disk operations, including disk creation, attachment, extension (with LVM), information gathering, and deletion.

## Features

- **Create** managed disks in Azure for VMs
- **Attach** existing disks to Azure VMs
- **Gather** filesystem and LVM info from Linux VMs for extension planning
- **Extend** LVM logical volumes and filesystems with new disks
- **Detach and Delete** managed disks from Azure VMs

## Directory Structure

```
.
├── inventories/                      # Inventory files for target VMs
├── vars/                             # Example variable files for each playbook
├── azure_vm_disk_attach.yml           # Attach a disk to an Azure VM
├── azure_vm_disk_create.yml           # Create a managed disk for an Azure VM
├── azure_vm_disk_delete.yml           # Detach and delete a managed disk from an Azure VM
├── azure_vm_filesystem_extend.yml     # Extend LVM and filesystem with a new disk
├── azure_vm_filesystem_info.yml       # Gather LVM and filesystem info from a VM
```

## Prerequisites

- Ansible 2.9+ with [Azure Collection](https://docs.ansible.com/ansible/latest/collections/azure/azcollection/index.html)
- Azure CLI installed and configured
- Service principal credentials set in environment variables:
  - `AZURE_CLIENT_ID`
  - `AZURE_SECRET`
  - `AZURE_TENANT`
- SSH access to target VMs for filesystem operations

## Usage

### 1. Create a Disk

```sh
ansible-playbook -i inventories/inventory.ini azure_vm_disk_create.yml -e @vars/vars_azure_vm_disk_create.yml
```

### 2. Attach a Disk

```sh
ansible-playbook -i inventories/inventory.ini azure_vm_disk_attach.yml -e @vars/vars_azure_vm_disk_attach.yml
```

### 3. Gather Filesystem & LVM Info

```sh
ansible-playbook -i inventories/inventory.ini azure_vm_filesystem_info.yml -e @vars/vars_azure_vm_filesystem_info.yml
```

### 4. Extend LVM & Filesystem

```sh
ansible-playbook -i inventories/inventory.ini azure_vm_filesystem_extend.yml -e @vars/vars_azure_vm_filesystem_extend.yml
```

### 5. Detach and Delete a Disk

```sh
ansible-playbook -i inventories/inventory.ini azure_vm_disk_delete.yml -e @vars/vars_azure_vm_disk_delete.yml
```

## Example Inventory File

Create an `inventory.ini` file in the `inventories/` directory:

```ini
# inventories/inventory.ini
[azure_vms]
ansible-tester ansible_host=8.8.8.8 ansible_user=azureuser ansible_ssh_private_key_file=~/ansible-secrets/my-vm-key.pem

```

## Example VS Code Settings

Create a `.vscode/settings.json` file for workspace configuration. Use your actual keys, but here are dummy values for illustration:

```json
// .vscode/settings.json
{
  "ansible.python.interpreterPath": "/usr/bin/python3",
  "terminal.integrated.env.linux": {
    "AZURE_CLIENT_ID": "my-client-id",
    "AZURE_SECRET": "my-sp-client-secret",
    "AZURE_TENANT": "my-sp-tenant-id",
    "AZURE_SUBSCRIPTION_ID": "my-sp-subscription-id"
  }
}
```

## Variables

Create files in the vars directory that house the actual values you want to use when running your playbooks.

## Notes

- Playbooks use Azure CLI for some operations; ensure your service principal has sufficient permissions.
- For LVM operations, the target VM must be accessible via SSH and have LVM utilities installed.
- Review each playbook and adjust variables as needed for your environment.

## License

MIT License
