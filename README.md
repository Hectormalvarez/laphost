# laphost

## Overview

**laphost** is a modular Ansible configuration for provisioning bare-metal laptop hardware as a headless, mobile development host.

## Architecture

The project utilizes a role-based structure to enforce system state:

- **access:** Establishes passwordless `sudo` for the primary user.
- **common:** Installs baseline utilities and configures mDNS (`avahi-daemon`) for `.local` resolution.
- **headless:** Enforce systemd Clamshell Mode (`HandleLidSwitch=ignore`) to prevent sleep on lid closure.

## Prerequisites

1. Flash Ubuntu Server to target hardware.
2. Supply Wi-Fi credentials during OS installation.
3. Establish initial SSH connectivity.

## Execution

### First-Run (Provisioning Access)

Execute this command initially to establish passwordless `sudo` permissions. This requires the remote user's password for privilege escalation.

```bash
ansible-playbook site.yml -e "target_address=<IP_OR_HOSTNAME> ansible_user=<USER>" --ask-become-pass

```

### Standard Execution (State Enforcement)

Execute this command for all subsequent updates once the `access` role has been successfully applied.

```bash
ansible-playbook site.yml -e "target_address=<IP_OR_HOSTNAME> ansible_user=<USER>"

```
