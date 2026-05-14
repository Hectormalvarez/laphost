# laphost

## Overview
**laphost** is a minimal Ansible configuration for provisioning bare-metal laptop hardware as a headless, mobile testing and development host. 

## Scope
*   **Hardware:** Enforce systemd Clamshell Mode (`HandleLidSwitch=ignore`).
*   **Access:** Configure passwordless `sudo` and ingest GitHub SSH keys for the primary user. Disable root login and password authentication.
*   **Runtimes:** Install Docker Engine, QEMU, KVM, and Libvirt.
*   **Permissions:** Append primary user to `docker`, `kvm`, and `libvirt` groups.

## Execution
1. Flash Ubuntu Server to target hardware.
2. Supply Wi-Fi credentials during OS installation.
3. Identify target DHCP IP address.
4. Execute state enforcement:
   ```bash
   ansible-playbook playbook.yml -i <TARGET_IP>, -u <USER> --ask-become-pass
   ```