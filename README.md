# CentOS Vagrant VM with Ansible

This project creates a CentOS VM with Vagrant using the VirtualBox provider and provisions it with `ansible_local`.

## Requirements

- Vagrant
- VirtualBox

## Usage

```powershell
vagrant up --provider=virtualbox
```

Vagrant will:

1. Start a `centos/stream9` VM.
2. Install `ansible-core` inside the guest.
3. Run the main Ansible playbook at `ansible/site.yml`.

To run provisioning again after changes:

```powershell
vagrant provision
```

To validate the hello world file from your host machine:

```powershell
vagrant ssh -c "sudo cat /tmp/hello-world.txt"
```

To run the playbook manually inside the guest:

```powershell
vagrant ssh -c "cd /home/vagrant && ansible-playbook ansible/site.yml -i ansible/inventory.ini -v"
```

You do not need to create an inventory file if you use `vagrant up` or `vagrant provision`; Vagrant handles provisioning for you. You only need an inventory when you run `ansible-playbook` yourself, and this repo now includes `ansible/inventory.ini` for that.

## Ansible Layout

- `ansible/site.yml`: main entry point that imports other playbooks.
- `ansible/inventory.ini`: localhost inventory for manual `ansible-playbook` runs inside the guest.
- `ansible/playbooks/hello_world.yml`: starter playbook that creates `/tmp/hello-world.txt`.
- `ansible/group_vars/all.yml`: shared variables.

## Optional Setup

Coder-specific setup and access instructions live in `CODER.md`.

## Extending the Configuration

Add new playbooks under `ansible/playbooks/` and import them from `ansible/site.yml`, for example:

```yaml
---
- import_playbook: playbooks/hello_world.yml
- import_playbook: playbooks/coder.yml
```
