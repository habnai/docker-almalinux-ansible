# Docker Ansible — AlmaLinux 9

Installs and configures Docker CE on AlmaLinux 9.

## Project structure

```
.
├── ansible.cfg
├── requirements.yml
├── site.yml                          # Main playbook entry point
├── inventory/
│   └── hosts.ini                     # Target hosts — edit this
└── ansible/
    ├── vars/
    │   ├── main_vars.yml             # All non-secret variables
    │   └── secret_vars.yml           # Encrypt this with ansible-vault
    ├── tasks/
    │   └── main.yml                  # All installation tasks
    ├── handlers/
    │   └── main.yml                  # Service restart handlers
    └── templates/
        └── dockerEnv.env             # Jinja2 template for .env file
```

## Quick start

### 1. Install required Ansible collection
```bash
ansible-galaxy collection install -r requirements.yml
```

### 2. Edit your inventory
```bash
vi inventory/hosts.ini
```

### 3. Edit variables
```bash
vi ansible/vars/main_vars.yml
```

### 4. (Optional) Encrypt secrets
```bash
ansible-vault encrypt ansible/vars/secret_vars.yml
```

### 5. Run the playbook
```bash
# Without vault
ansible-playbook site.yml

# With vault
ansible-playbook site.yml --ask-vault-pass

# Dry run first
ansible-playbook site.yml --check --diff
```

## Variables reference

| Variable | Default | Description |
|---|---|---|
| `hdd.pool_name` | `DockerPool` | ZFS/storage pool name used to build paths |
| `adminuser` | `user` | Existing OS user to add to the `docker` group |
| `docker.data_dir` | `/DockerPool/docker/data` | Base data directory for Docker volumes |
| `docker.env_dest` | `/DockerPool/docker/.env` | Destination path for the `.env` file |
| `docker.external_network` | `net_proxy` | Name of the shared Docker external network |
| `docker.PUID` | `1000` | UID passed into containers via `.env` |
| `docker.PGID` | `115` | GID passed into containers via `.env` |
