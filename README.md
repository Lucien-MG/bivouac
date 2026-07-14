# bivouac
A stateless, GitOps-driven mesh infrastructure running rootless AI and productivity containers on an immutable, self-healing OS layer.

---

## 🚀 Bootstrapping a New Node

With an ssh key:

```bash
ansible-playbook -i "192.168.1.X," playbooks/bootstrap.yml -u <default_os_user> --private-key ~/.ssh/id_ed25519 --become
```
ansible-playbook -i "10.142.204.174," playbooks/bootstrap.yml -u lucien --private-key ~/.ssh/id_ed25519 --ask-become-pass

ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i "10.142.204.174," playbooks/bootstrap.yml -u lucien --private-key ~/.ssh/id_ed25519 --become 

With a connection password:

```bash
ansible-playbook -i "192.168.1.X," playbooks/bootstrap.yml -u <default_os_user> -k -K
```

This repository uses a two-phase architecture:
1. **Phase 1 (Push):** A one-time bootstrap script executed from your laptop via SSH to create a secure, unprivileged user (`nomad`) and configure the rootless execution layers.
2. **Phase 2 (Pull):** An autonomous systemd timer on the server that wakes up every 15 minutes to pull updates directly from this repository and apply changes locally via `ansible-pull`.

---

## Synchrinize Manually

```
ansible-playbook -i "10.142.204.174," playbooks/site.yml -u nomad --private-key ~/.ssh/id_ed25519 --become
```

## Prerequisite: Local Setup

Before running the bootstrap command, ensure you have your target node's details and secrets configured on your laptop.

1. **Create your deployment key:**
   ```bash
   ssh-keygen -t ed25519 -f ~/.ssh/nomad_deploy_key -N "" -C "nomad-lab-deploy"