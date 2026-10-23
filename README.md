# Network Automation Lab: Python & Ansible

Small lab that automates routine tasks on Cisco IOS devices:

| Task | Ansible | Python |
|---|---|---|
| Back up running configs | `playbooks/backup_configs.yml` | `python/backup_configs.py` |
| Deploy VLANs | `playbooks/harden_and_vlans.yml` | – |
| SSH-only remote access, management ACL, port security | `playbooks/harden_and_vlans.yml` | – |

## Lab setup
Use a virtual lab with real IOS images reachable over SSH (GNS3, EVE-NG, or the free Cisco DevNet Sandbox).
Packet Tracer's IOS is too limited for Ansible modules.

On each device (once, manually): set a hostname and domain name, generate RSA keys (`crypto key generate rsa`),
create a local user, and enable `login local` on the VTY lines so SSH works.

## Install
```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
ansible-galaxy collection install cisco.ios ansible.netcommon
```

## Configure
1. Edit `inventory.yml` with your device IPs.
2. Edit `group_vars/switches.yml` (VLANs, management network, access ports).
3. Export credentials (never commit them):
```bash
export LAB_USER=admin
export LAB_PASS='your-password'
```

## Run
```bash
ansible-playbook playbooks/backup_configs.yml
ansible-playbook playbooks/harden_and_vlans.yml --check --diff   # dry run first
ansible-playbook playbooks/harden_and_vlans.yml
python python/backup_configs.py                                  # Python alternative for backups
```
Backups are written to `backups/`.

## Notes
- Run the hardening playbook with `--check` first. Make sure the management ACL includes your own address, or you can lock yourself out.
- Document your own results here: screenshots of the topology, before/after configs, and what you learned.
