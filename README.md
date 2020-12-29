# SETUP KUBERNETES CLUSTER
### STEP
1. Ansible-playbook pre_install.sh (on all node)
2. Ansible-playbook ansible_LB1.sh (on first master)
3. Ansible-playbook ansible_LB2.sh (on other masters)
4. Init first master
5. Apply CNI
6. Join node

### INITIATE FIRST MASTER NODE
````````
sudo kubeadm init --control-plane-endpoint “172.16.5.234:16443” --upload-certs
````````
### APPLY KUBERNETES CNI (CONTAINER NETWORK INTERFACE)
```````
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

```````
### JOIN OTHER MASTER NODES TO CLUSTER
```
kubeadm join 172.16.5.234:16443 --token 9do5ly.ian85n78k6spgo8w \
    --discovery-token-ca-cert-hash sha256:55eb9f6da688f51a55a9fec2a393d6c357cd251983aecc4f206b7ca1cb077d63 \
    --control-plane --certificate-key b694cccdf88b840190140c95d11c894b1c76a994c8091a77f0d0ceadac792c67
```
### JOIN WORKER NODES TO CLUSTER
```
kubeadm join 172.16.5.234:16443 --token 9do5ly.ian85n78k6spgo8w \
    --discovery-token-ca-cert-hash sha256:55eb9f6da688f51a55a9fec2a393d6c357cd251983aecc4f206b7ca1cb077d63
```
**Label worker node**

Run this command on Control Plane Node
```
kubectl label node worker1 node-role.kubernetes.io/worker=worker

```
