# laphost

## Overview

**laphost** is a modular Ansible configuration for provisioning bare-metal laptop hardware as a headless, mobile development host.

## Architecture

The project utilizes a role-based structure to enforce system state, combining an isolated wireless management plane with a bridged Ethernet path dedicated to virtualization:

- **access:** Establishes passwordless `sudo` for the primary user and validates syntax integrity via `visudo`.
- **network:** Deploys a hybrid Netplan configuration. Dynamically detects the active host Wi-Fi SSID, configures wireless for host management (Route Metric: `100`), and provisions a physical Linux Bridge (`br0`) on the Ethernet interface for guest VM networking (Route Metric: `200`).
- **common:** Installs baseline utilities (`curl`, `git`) and provisions `avahi-daemon` for `.local` mDNS name resolution.
- **headless:** Modifies `systemd-logind` configuration to enforce Clamshell Mode (`HandleLidSwitch=ignore`), preventing system sleep on lid closure.
- **docker:** Provisions the official upstream Docker Engine, plugins, and groups, utilizing explicit distribution fact lookups.
- **virt:** Installs the KVM, QEMU, and Libvirt components required for guest virtualization.

## Prerequisites

1. Flash Ubuntu Server to the target hardware.
2. Supply Wi-Fi credentials during OS installation to establish initial wireless connectivity.
3. Verify SSH access to the host interface.

## Execution

### First-Run (Provisioning Access)

Execute this command initially to establish passwordless `sudo` permissions and compile the network bridge. This execution requires the remote user's password for privilege escalation and the active Wi-Fi pre-shared key.

```bash
ansible-playbook site.yml \
  -e "target_address=<IP_OR_HOSTNAME> ansible_user=<USER> wifi_password=<WPA2_KEY>" \
  --ask-become-pass

```

### Standard Execution (State Enforcement)

Execute this command for all subsequent updates once the `access` role has been successfully applied to the filesystem.

```bash
ansible-playbook site.yml \
  -e "target_address=<IP_OR_HOSTNAME> ansible_user=<USER> wifi_password=<WPA2_KEY>"

```