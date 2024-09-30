# CIS Rocky Linux 8 Benchmark Ansible Role

This Ansible role implements the recommendations from the **CIS Rocky Linux 8 Benchmark**, designed to secure Rocky Linux 8 systems. It provides comprehensive coverage of system hardening tasks such as SSH security, file system configurations, kernel module management, service management, network security, auditing, and logging.

## Features
This role ensures compliance with the following CIS Benchmark categories:
- **SSH Hardening**
- **Filesystem Permissions**
- **Authentication Policies (PAM)**
- **Kernel Module Management**
- **Filesystem Partitioning**
- **Service Management**
- **Network Configurations**
- **System Auditing and Logging**
- **Integrity Checking with AIDE**
- **Mandatory Access Control (SELinux)**
- **Crypto Policies**
- **GPG Key Management and Software Updates**
- **Advanced Firewall and nftables Configuration**
- **Remote Logging with journald and rsyslog**
- **Duplicate UIDs and GIDs Audits**
- **Disk Encryption (LUKS)**

## Role Structure

```
ansible_role_cis_rocky8/
├── defaults
│   └── main.yml
├── handlers
│   └── main.yml
├── tasks
│   ├── auth.yml
│   ├── crypto_policies.yml
│   ├── duplicate_uid_gid_audit.yml
│   ├── file_permissions.yml
│   ├── filesystem_partitioning.yml
│   ├── gpg_key_management.yml
│   ├── integrity_checking.yml
│   ├── kernel_modules.yml
│   ├── main.yml
│   ├── mandatory_access_control.yml
│   ├── network_configuration.yml
│   ├── remote_logging.yml
│   ├── service_management.yml
│   ├── ssh.yml
│   ├── system_audit_logging.yml
│   ├── advanced_firewall_nftables.yml
│   ├── disk_encryption.yml
│   └── custom_organization_policies.yml
├── meta
│   └── main.yml
└── README.md
```

## Requirements

This role requires Ansible version 2.9+.

- Ensure you have `sudo` privileges.
- Ensure SELinux is installed and not disabled in the bootloader configuration.

## Role Variables

The following are configurable variables in `defaults/main.yml`:

```yaml
# SSH settings
ssh_port: 22
max_auth_tries: 3

# Password lockout settings
password_lockout_time: 900

# Filesystem partition devices
tmp_partition_device: /dev/sda1
home_partition_device: /dev/sda2
var_partition_device: /dev/sda3
var_tmp_partition_device: /dev/sda4
```

## Usage

1. Clone this repository to your Ansible roles directory:
   ```bash
   git clone https://github.com/diceone/ansible_role_cis_rocky8.git
   ```

2. Include the role in your playbook:
   ```yaml
   ---
   - hosts: all
     become: yes
     roles:
       - ansible_role_cis_rocky8
   ```

3. Run the playbook:
   ```bash
   ansible-playbook your_playbook.yml
   ```

### Example Playbook

```yaml
---
- hosts: all
  become: yes
  vars:
    ssh_port: 2222
    password_lockout_time: 600
    tmp_partition_device: /dev/sdb1
  roles:
    - ansible_role_cis_rocky8
```

## Handlers

This role contains the following handlers:
- **update-grub**: Used to update the GRUB configuration when necessary (e.g., when enabling SELinux).
- **restart auditd**: Restart the auditd service after audit rules are modified.
- **restart rsyslog**: Restart the rsyslog service after logging configurations are changed.
- **apply-crypto-policies**: Apply system-wide cryptographic policies after modifications.

## Tasks Covered

### SSH Hardening
- Disables root login via SSH.
- Configures secure SSH settings (`MaxAuthTries`, `ClientAliveInterval`).
  
### Kernel Module Management
- Disables unnecessary kernel modules (e.g., `cramfs`, `hfs`, `jffs2`, `usb-storage`).

### Filesystem Partitioning
- Ensures `/tmp`, `/var/tmp`, `/home`, `/dev/shm` are on separate partitions with `nodev`, `nosuid`, and `noexec` options.

### Service Management
- Disables unnecessary services (e.g., `autofs`, `avahi-daemon`, `smb`, `telnet`).
  
### Network Configuration
- Disables IPv6, wireless interfaces, and configures secure network kernel parameters.

### System Audit and Logging
- Installs and configures `auditd` with rules for logging critical activities and sets audit log retention policies.
  
### Integrity Checking
- Installs and configures `AIDE` to perform file integrity checks.

### Mandatory Access Control (SELinux)
- Ensures SELinux is installed and enforcing.

### Crypto Policies
- Ensures strong cryptographic algorithms and disables weak ciphers and hashing methods (`SHA1`, `CBC`).

### Remote Logging
- Configures `rsyslog` and `journald` to send logs to a remote server.

### Duplicate UID/GID Audits
- Checks for duplicate UIDs and GIDs and enforces strict user/group management policies.

### Disk Encryption (LUKS)
- Verifies disk encryption using LUKS is enabled.

## License

This role is released under the [MIT License](LICENSE).

## Author

This role was developed by Michael Vogeler. Feel free to contact for any issues or contributions.
```
