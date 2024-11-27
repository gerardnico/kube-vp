# One-Clik Self-Hosted Kubernetes on a single VPS





## Steps

### Download requirements
```bash
ansible-galaxy role install -r requirements.yml
```

### Create Inventory

 * Token
```bash
openssl rand -base64 64 | tr '\n' ''
```

### Os

```bash
ansible-playbook ansible/collection/k3s/orchestration/playbooks/site.yml -i conf/inventory.yml
```