# One-Clik Self-Hosted Kubernetes on a single VPS



## Dependencies

* `yq`
* `openssl`
* `ansible`

## Steps

### Download requirements
```bash
ansible-galaxy role install -r requirements.yml
```

### Create Inventory

 * Token
```bash
CLUSTER_API_SERVER_IP=78.46.190.50
CLUSTER_NAME=kube-test
openssl rand -base64 64 | tr '\n' ''
```

### Os

```bash
ansible-playbook -i conf/inventory.yml ansible/collections/ansible_collections/k3s/orchestration/playbooks/site.yml
```

### Copy KUBECONFIG

```bash
ssh-agent add /path/to/your/private/key
scp root@kube-test-server-01.gerardnico.com:/etc/rancher/k3s/k3s.yaml ~/.kube/config
# or 
scp -i /path/to/your/private/key root@kube-test-server-01.gerardnico.com:/etc/rancher/k3s/k3s.yaml ~/.kube/config
# env
export KUBECONFIG=~/.kube/config
# permission
chmod 600 "$KUBECONFIG"
# change server (IP for now because the FQDN should be set before installing kube)
# otherwise `tls: failed to verify certificate: x509: certificate is valid for kube-test-server-01, kubernetes, kubernetes.default, kubernetes.default.svc, kubernetes.default.svc.cluster.local, localhost, not kube-test-server-01.xxx`
yq e ".clusters[0].cluster.server = \"https://$CLUSTER_API_SERVER_IP:6443\"" -i "$KUBECONFIG"
kubectl config rename-context default $CLUSTER_NAME
# test
kubectl cluster-info dump
```

