# Coder Setup

This project includes a minimal Coder installation that runs inside the VM and is exposed only on the host loopback interface at `http://localhost:3000`.

## What It Configures

- Imports `ansible/playbooks/coder.yml` from `ansible/site.yml`
- Installs the latest stable Coder release inside the VM
- Installs Docker Engine inside the VM for the default Docker Container template
- Runs Coder as a `systemd` service
- Configures `CODER_ACCESS_URL=http://localhost:3000`
- Forwards guest port `3000` to host `127.0.0.1:3000`

This setup intentionally uses Coder's built-in PostgreSQL database. That is convenient for a local VM, but not for production.

## Usage

If the VM is not running yet:

```powershell
vagrant up --provider=virtualbox
```

If the VM is already running and you added or pulled the Coder networking changes, reload it once so VirtualBox applies the localhost-only port forwarding:

```powershell
vagrant reload
```

To apply or re-apply the Coder configuration:

```powershell
vagrant provision
```

## Accessing Coder

Open this URL from the host machine:

- `http://localhost:3000`

On first login, Coder will prompt you to create the initial admin user.

## Verification

To verify the Coder service inside the VM:

```powershell
vagrant ssh -c "systemctl status coder --no-pager"
```

To inspect the Coder environment file inside the VM:

```powershell
vagrant ssh -c "sudo cat /etc/coder.d/coder.env"
```

To verify that the `coder` service account can reach Docker inside the VM:

```powershell
vagrant ssh -c "sudo -u coder docker info"
```

## Relevant Files

- `Vagrantfile`
- `ansible/site.yml`
- `ansible/playbooks/coder.yml`
- `ansible/group_vars/all.yml`
