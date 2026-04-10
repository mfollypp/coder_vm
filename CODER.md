# Coder Setup

This project includes a minimal Coder installation that runs in Docker Compose inside the VM and is exposed only on the host loopback interface at `http://localhost:3000`.

## What It Configures

- Imports `ansible/playbooks/coder_docker_compose.yml` from `ansible/site.yml`
- Installs Docker Engine inside the VM for the default Docker Container template
- Stages the checked-in compose file from `docker/coder_compose.yml` into `/opt/coder/docker-compose.yml`
- Runs Coder and PostgreSQL with `docker compose`
- Configures `CODER_ACCESS_URL=http://localhost:3000`
- Forwards guest port `3000` to host `127.0.0.1:3000`

This setup intentionally uses the bundled PostgreSQL container from Coder's compose example. That is convenient for a local VM, but not for production.

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

To verify the Coder containers inside the VM:

```powershell
vagrant ssh -c "cd /opt/coder && sudo docker compose ps"
```

To inspect the Docker Compose environment file inside the VM:

```powershell
vagrant ssh -c "sudo cat /opt/coder/.env"
```

To inspect the Coder container logs:

```powershell
vagrant ssh -c "sudo docker logs \$(sudo docker ps --filter name=coder --format '{{.Names}}')"
```

## Relevant Files

- `Vagrantfile`
- `ansible/site.yml`
- `ansible/playbooks/coder_docker_compose.yml`
- `docker/coder_compose.yml`
- `ansible/group_vars/all.yml`
