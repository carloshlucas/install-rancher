# INSTALLING RANCHER ON KUBERNETES

### MY LAB

* 1x VM HAPROXY Load Balancer
* 3x VMs RANCHER
* 1x DNS/DOMAIN CONTROLLER
* 1x VM CLIENT

2 vCPU | 2GB Memory | 20GB disk | Ubuntu 18.04

## PRE REQUERIMENTS

### INSTALLING HELM (CLIENT)

1. Downloading HELM 3
```
curl -LO https://github.com/helm/helm/releases/download/v3.2.4/helm-v3.2.4-linux-amd64.tar.gz.asc
```
2. Unpack HELM
```
tar -zxvf helm-v3.2.4-linux-amd64.tar.gz.asc
```
3. Move the files
```
mv linux-amd64/helm /usr/local/bin/helm
```
### INSTALLING KUBECTL (CLIENT)

1. Download the latest release
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
```
2. Make the kubectl binary executable.
```
chmod +x ./kubectl
```
3. Move the binary in to your PATH.
```
sudo mv ./kubectl /usr/local/bin/kubectl
```
4. Test to ensure the version you installed is up-to-date:
```
kubectl version --client
```
### INSTALLING RKE (CLIENT)

1. Download RKE
```
curl -LO https://github.com/rancher/rke/releases/download/v1.1.3/rke_linux-amd64
```
2. Rename the package
```
mv rke_linux-amd64 rke
```
3. Make binary RKE executable
```
chmod +x rke
```
4. Move the file to /usr/local/bin/
```
sudo mv ./rke /usr/local/bin/
```
5. Check the version
```
rke --version
```
### GENERATING CERTIFICATES (CLIENT)

1. Type the command below:
```
ssh-keygen
```

2. Generating public/private rsa key pair.
```
Enter file in which to save the key (/home/chl/.ssh/id_rsa): #I kept the same name
Enter passphrase (empty for no passphrase):                  # I didn't configure password 
```
3. Enter same passphrase again:

4. Outcome
```
Your identification has been saved in /home/chl/.ssh/id_rsa
Your public key has been saved in /home/chl/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:6nNI4m5WGt2JOn9hwsHmoWo7EY0XamBFeEl8b5OJmds chl@WINKPN-4JXQ433
The keys randomart image is:

+---[RSA 3072]----+ \
|  *+.            | \
|.o +..           | \
|...+.o= o        | \
|  = o+=*         | \
| . o *+=S.       | \
|  . +.OE=        | \
|   + B.+ .       | \
|  + B.o o        | \
| ..*.oo+         | \
+----[SHA256]-----+
```

### COPY the content from id_rsa.pub (You just created) and copy to Master Node.

1. cat .ssh/id_rsa.pub
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDPTlmolQ6p/6vFJQuXyAKDwC8TiJmPg9WZ0SQKLesXdY3boTIAZqFXXUj3BQWWnsh+1DndqvUK2mbn7UG+w+Ru0J4G6KT4a5veKDH6cVZ00xPfy91zawuknxtEeAR4R++Otm2x7Tq+rvYmb4G3C4VPu+nAgIQGe3y6lzzGihR5qvF3/sMXljuhjB4Tt79iTFIzcOJxpOpFGkkqE6aPA9wPwypiD71LJmFDUOZxuRlFGFRNeFskYeuvXGVWo4iHJhfK7Om4zn0pUDNQAzlbFqk0uoAiowsxef/RhX1cSaxtZouoeFSlIgicJLft9m8VhuSJMQ/Lj0R692004IEKWOnGWP8c5NilIsB7AWLrMOoB3lITXQqE69GTBn3RMFMfXGlm0JkQyRuD7fJuWfiHwBcIKNsQ+qZMfx3F1CQVVpa5o3w9dFKJKVNvuF15T4kiXZbrfjEIqgjpxX8BEKfciTXpblrI5FUqDikn/RWWvi9vaGi6knH2PLhk54x+ZAv9YEc= chl@WINKPN-4JXQ433 
```
2. SSH to ALL Master Nodes and copy the key to .ssh/authorized_keys #(ALL MASTER NODES)

3. vi .ssh/authorized_keys #(CREATE SSH FOLDER IF IT IS REQUIRED)
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDPTlmolQ6p/6vFJQuXyAKDwC8TiJmPg9WZ0SQKLesXdY3boTIAZqFXXUj3BQWWnsh+1DndqvUK2mbn7UG+w+Ru0J4G6KT4a5veKDH6cVZ00xPfy91zawuknxtEeAR4R++Otm2x7Tq+rvYmb4G3C4VPu+nAgIQGe3y6lzzGihR5qvF3/sMXljuhjB4Tt79iTFIzcOJxpOpFGkkqE6aPA9wPwypiD71LJmFDUOZxuRlFGFRNeFskYeuvXGVWo4iHJhfK7Om4zn0pUDNQAzlbFqk0uoAiowsxef/RhX1cSaxtZouoeFSlIgicJLft9m8VhuSJMQ/Lj0R692004IEKWOnGWP8c5NilIsB7AWLrMOoB3lITXQqE69GTBn3RMFMfXGlm0JkQyRuD7fJuWfiHwBcIKNsQ+qZMfx3F1CQVVpa5o3w9dFKJKVNvuF15T4kiXZbrfjEIqgjpxX8BEKfciTXpblrI5FUqDikn/RWWvi9vaGi6knH2PLhk54x+ZAv9YEc= chl@WINKPN-4JXQ433
```
4. Outcome
```
ssh 192.168.15.60
The authenticity of host '192.168.15.60 (192.168.15.60)' cant be established.
ECDSA key fingerprint is SHA256:nGe8DIL4AMjV5bQm4vSOLkrvTbRdS1eXk3AsWwiczWg.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

### INSTALLING KUBECTL (MASTER NODES)

1. Download the latest release
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
```
2. Make the kubectl binary executable.
```
chmod +x ./kubectl
```
3. Move the binary in to your PATH.
```
sudo mv ./kubectl /usr/local/bin/kubectl
```
4. Test to ensure the version you installed is up-to-date:
```
kubectl version --client
```

### INSTALLING DOCKER (ALL MASTER NODES)
```
curl https://releases.rancher.com/install-docker/18.09.sh | sh
```
### INSTALLING KUBERNETES CLUSTER USING RANCHER - RKE (RUN OVER YOUR CLIENT MACHINE)

1. Creating the config file rancher-cluster.yml
```
nodes:
  - address: 192.168.15.60 #IP OF YOUR MASTER NODE
    user: chl
    role: [controlplane, worker, etcd]
  - address: 192.168.15.61 #IP OF YOUR MASTER NODE
    user: chl
    role: [controlplane, worker, etcd]
  - address: 192.168.15.62 #IP OF YOUR MASTER NODE
    user: chl
    role: [controlplane, worker, etcd]

services:
  etcd:
    snapshot: true
    creation: 6h
    retention: 24h

# Required for external TLS termination with
# ingress-nginx v0.22+

ingress:
  provider: nginx
  options:
    use-forwarded-headers: "true"
```
2. Run RKE
```
rke up --config ./rancher-cluster.yml
```
3. Outcome
```
INFO[0141] Finished building Kubernetes cluster successfully

** If you got any error during the process, run the command again.
```
4. Getting the Kubeconfig file
```
cat kube_config_rancher-cluster.yml

apiVersion: v1
kind: Config
clusters:
- cluster:
    api-version: v1
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUN3akNDQWFxZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFTTVJBd0RnWURWUVFERXdkcmRXSmwKTFdOaE1CNFhEVEl3TURZeU1qQTVNVFExTlZvWERUTXdNRFl5TURBNU1UUTFOVm93RWpFUU1BNEdBMVVFQXhNSAphM1ZpWlMxallUQ0NBU0l3RFFZSktvWklodmNOQVFFQkJRQURnZ0VQQURDQ0FRb0NnZ0VCQU9LK3Zvakw3c1FICkQzNldIcmFreVJhL2EzNHArYjc2cFFnOC9EZG9LdUlsc2NRT3ZZSmRzVjVnMnNNcGFCeExzZWRqWFdLSDkvMmgKc1JrYnNnTXhicXdlbU9XaDhTSlVITmFBMkx6RHYzRlkrWWJFZGFyRnJwcXFObjJ4OGluaHNIVlh2Y3JWYTlKTwo4QjdRNEx3N3h5TkxYdUt4RE4zNVRoRmsyMllrMlUzdWF2YnUwRjVHcWxkcXg5VnRJYkRPczlMc3kybVlZcUMvCnF2WXFIR0U2d3FYNmJadzJJei9hekdpdnFQTlFkMjNjVjhUekxRM3hxMDY3N3h0Y04zdWZzYnc0U2k1WU1PZUsKUEtxQnVqdHErek5nS21NNDFrN29NWW9jVVJoOFhaK1BwSDcxRG1lM3lDSHF5dWVMTFl5cHk2NTdiSkk4K3ViVQpZQUhJMVFCUFNORUNBd0VBQWFNak1DRXdEZ1lEVlIwUEFRSC9CQVFEQWdLa01BOEdBMVVkRXdFQi93UUZNQU1CCkFmOHdEUVlKS29aSWh2Y05BUUVMQlFBRGdnRUJBSWdFZEhlRnZPWjU3NVVoR2Z4WEI3V3VCMDRIWVdZYnBDb2EKTThsWHNOOVNQTmVWK1MzYVBFcWJhRHQ1ekViSitjVW5TbGNrbXBQYSs5SFNlQjVPdDJ6YllRMFdXTzBobWtIRAp0N01PUHE5bHM0SituRGRqYnRKWS9EdVRUckF6NjA3ZWRtTnFUSnVsbWZYcDZyb01wLzBaV3VyZ0s2aUJ4UjBrClJtc0ZMVzZKTG5Eb1JiblBQNmhLS1JGc0ZPTHlLZHNieFNMTHF1dlFBM3VDeGhrbGp1SXZGREwyQmRTOCsyalQKTjljbU1DRFZFVnNpZmsyRTJ6bnl4cy9Da2xRYkFwclNaTjhBdTFiSFd4YjJEQTFpcW1QYytLWGo4SHBoTkZxMgpNcEZNQnVOYXdxVmZLeGlWdllETzVpWS81OEU2RWJRUkRKaE9PTnc4NE5aSmY4RzhZZ1U9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    server: "https://192.168.15.60:6443"
  name: "local"
contexts:
- context:
    cluster: "local"
    user: "kube-admin-local"
  name: "local"
current-context: "local"
users:
- name: "kube-admin-local"
  user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM2VENDQWRHZ0F3SUJBZ0lJZUZmNDNuMUFCOE13RFFZSktvWklodmNOQVFFTEJRQXdFakVRTUE0R0ExVUUKQXhNSGEzVmlaUzFqWVRBZUZ3MHlNREEyTWpJd09URTBOVFZhRncwek1EQTJNakF3T1RJMU1UaGFNQzR4RnpBVgpCZ05WQkFvVERuTjVjM1JsYlRwdFlYTjBaWEp6TVJNd0VRWURWUVFERXdwcmRXSmxMV0ZrYldsdU1JSUJJakFOCkJna3Foa2lHOXcwQkFRRUZBQU9DQVE4QU1JSUJDZ0tDQVFFQXJqWjlTVVFwUjdyVjcrWlJwYTVsenZHVndHeUcKOEdqVFdld29YcVRWS2YvN3QxMWxlOU13dlpyajhwNXlPWE1icGdlZ0RtekorbmJ6b1pNbHdRZkp2NzgyYk9zSgp5QnJKRHZFZm5zUEZ1NHJ0NzFRZENCUk5EM0Jub0w5b3lBc0NwR25Xb0xZc2F6bU40bmxhd1JUdnptRk83VFRYClhoeWc3M2puK2dtRmNVSENFRjdGS3BhTmZySGQ0ZDVRMUY3VmxwUFZqTGdsUThWN1U3V2pwYlRWNkdia2ljZjQKSHNldDJjbkdkUnBBbFpIai8rYlRyY1hHVFhDRXlBa1VRUlhuYVZhdFRwNWpYQUR3VHJjZ1dzOHZGOWpZZExibQpVbWhXYlFxRUk4RGVsMU4yeXVrN3U0cXV6b3pxZ1lxWlpxRjcvYmNVZEJIU0RTMllXVkpKeHNaYnlRSURBUUFCCm95Y3dKVEFPQmdOVkhROEJBZjhFQkFNQ0JhQXdFd1lEVlIwbEJBd3dDZ1lJS3dZQkJRVUhBd0l3RFFZSktvWkkKaHZjTkFRRUxCUUFEZ2dFQkFBUEEyOERCRWQrRGlieXdSZ2dwNFU1aDdBWlhKTXh4TmhYNFV3c0pPTEdia1RRQgpQeU1yTUEvekZrb0Q1eDltNTU2VXNYNnQzSVlDQ1Q2T3cweDlwaHlUZWgzMDdJRVJiSUErcC9TU1lpLzBRUEZnClY2NzVLY0NlTnpnOFQwRyt6WTdOMWovNWJpc1RQUkF2azRTdTBDa21CNzRnT0drRmU5T0Vrc0dHM3ZkM25zT2sKZGw0TG1ySzR2MXBqVE93amNVem1iTnRGWWhlTnJ3amhlUktmSk9XMHJkUTh0b1hINGR3NTVYVWZmM1hDTVBTZgpzaU9Xcjlic3lHcXBhOXR5cFZYSlJwNWNUMFpiSm5vQ1pON2IvM1hBK2kyNksreUlWZ3hPRTdrMUFuTVRYOVBDClZYTldsczNwS2t2UnhRRTFuTmVDVTJvRStlZFNxOTdidzFiQWF3Zz0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
    client-key-data: LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFcFFJQkFBS0NBUUVBcmpaOVNVUXBSN3JWNytaUnBhNWx6dkdWd0d5RzhHalRXZXdvWHFUVktmLzd0MTFsCmU5TXd2WnJqOHA1eU9YTWJwZ2VnRG16SituYnpvWk1sd1FmSnY3ODJiT3NKeUJySkR2RWZuc1BGdTRydDcxUWQKQ0JSTkQzQm5vTDlveUFzQ3BHbldvTFlzYXptTjRubGF3UlR2em1GTzdUVFhYaHlnNzNqbitnbUZjVUhDRUY3RgpLcGFOZnJIZDRkNVExRjdWbHBQVmpMZ2xROFY3VTdXanBiVFY2R2JraWNmNEhzZXQyY25HZFJwQWxaSGovK2JUCnJjWEdUWENFeUFrVVFSWG5hVmF0VHA1alhBRHdUcmNnV3M4dkY5allkTGJtVW1oV2JRcUVJOERlbDFOMnl1azcKdTRxdXpvenFnWXFaWnFGNy9iY1VkQkhTRFMyWVdWSkp4c1pieVFJREFRQUJBb0lCQVFDZG4yOTcrcy9ldG8wYQpvYWo5M3V6K2NGYXQ0NE96d1NSNUY5THV6dlNQSzZ6MEd1elhQQjI5YjRjTzNZMDdyWW8yam0raW5ZdWhzajU4CjBrWTJnSFZwcC9mMlZESmxldGF2VGpydHl2UDgyNWFzRTg2d29LMllhbnRZVmRiWUtCNGI1Q2J0QWVidW5RUmoKQkttNFNOa3dRWnJIeENlWVh3NjluUWRHazViSkFOYmZ4amdCTTNEOTV5NWdVeUMvd1MrRmVOMGlrMzVMQnJ6Kwo0TjBwT3RRZXEwcDhOQTNHOGxNcysvYkxPT28xRFdVSUQrb2RRTWFnN091cG55SjAzbUoxTk1CYWdIdndkeWtRCklJQnB3ODVqREI2LzRhQ2FlR1VuQmVlK3dmZEUxWDFTdHZ0WWkwV0JTS0tHMkpISlI1WGQvanZtbDU2ZTgzMzcKSy9NWEZUYUJBb0dCQU9SMk4xSjZDU1JIVWV5dUNqeEY2QVR2K3FlNVlVSlFDaXRSbWtBOU5pNURXbTlEdVpIbApJSzYySEJYUnRES2JKMGQzSnVaVHBacnFKU1FRVDVTTXJGeXI5M21LNWIySXhoUlNDSC9WMms0Zy9XTG05eVpHCmpIWXR4KzFLSlRVTmx2RVFmYVlJL25GUHplOXBoR3NFaFRseGNjMU91eGdscWZlcVRZQkhmT3pMQW9HQkFNTTIKUi9Na0kxNDZLL0Z5UklpR1RmNVo0dkV5Q3A4ZjYxc0pUTjFhMmgvZCtJWTcraStKcEt4alRXRk9nUXBUYkpJMwoxc2hGRkVWOTM3T0RVVWRBVytieE43d3g5djhqUnVnYytBM0JUOUYySHdKSCtyMXdNRzJVM2ptMndGRHpwVTJhCkpJTkJnazB3eEJONjlyMWRjUmNrdE10NkZNM1cyNEhQMjBkN0d6czdBb0dCQUtEUUFhTUJtYnU2cEJVWWNNWDYKWXhaT29iNDhCQlQ4YUJ3VDVhTUFNMG9mU09IMjV3R21LZm93MzlZUDlXcUZ3Z0ZSQXNXdjdXcjlFOFd0NFlacgorc3VFbEw4dUlRYm41cDNaSEhoM1hLSVFJS3ZrYmVXNFZSaStvdk1KZE9WQ09wUlhDa3E4Ry9aQXVYR2pjWDB2CkFtUHIwYkkxa0Fhd1JGSzd3cS9nSWpRL0FvR0JBSWlEd2tjM280QTJ3QnBHWmNUdjMwVWJpUWhSalhvQU1XL2wKamhwU2VhM0ZaaEpCaVZHTE9WelF5UUZlakFFem0vNlpEQnZicXpSNXRXSHRXSDZ5ZEZLSnFBZDFEWjBaQjNJMQpLQWMrZzlvZ2Q4ajVtMUMxVHppaTNkR1FpdWRScSswYithTE13aG5IRzZmZjJ3Tk1nRlRGOTZWQVI2STNPaVNLCnZuYzdrU3lYQW9HQU1SZVNQNFJOOUpMb3lyMGdieWcrQytqb1NvT3B2bmFTRUYwK1VEeEVEZWxWakRvODJ6OXcKeTFQT1cvTmlURWdOMXNVVGt0T29aZGpkejFTbHZQUXJlcG1aUVlMYXhzdWQ1cjdVNlp3QkpmbTR1Y0J2ZUJoLwpkOElVUGoxVDRZTFRic21LSSt3UmdMRCtIYm55Z0E5RVlwRzJnYm40eHlpaCtiMlp1VHpLM2dRPQotLS0tLUVORCBSU0EgUFJJVkFURSBLRVktLS0tLQo=
```
5. Copy the key or file to the master node #First create the folder .kube and then rename the file to config

6. Do the same step on your client, it will avoid you to login to the master nodes everytime you want to create a pod. #We will use our client to run the next step.

### TESTING YOUR CLUSTER

1. Listing Nodes
```
kubectl get nodes

NAME            STATUS   ROLES                      AGE     VERSION
192.168.15.60   Ready    controlplane,etcd,worker   9m39s   v1.17.6
192.168.15.61   Ready    controlplane,etcd,worker   9m38s   v1.17.6
192.168.15.62   Ready    controlplane,etcd,worker   9m39s   v1.17.6
```
2. Listing Pods
```
kubectl get pods -n kube-system

NAME                                      READY   STATUS      RESTARTS   AGE
canal-66j8m                               2/2     Running     0          10m
canal-dqtxk                               2/2     Running     0          10m
canal-qhcv8                               2/2     Running     0          10m
coredns-7c5566588d-cclfx                  1/1     Running     0          7m6s
coredns-7c5566588d-h87d7                  1/1     Running     0          6m52s
coredns-autoscaler-65bfc8d47d-nxjw7       1/1     Running     0          7m4s
metrics-server-6b55c64f86-zhjt6           1/1     Running     0          6m53s
rke-coredns-addon-deploy-job-tshv9        0/1     Completed   0          7m9s
rke-ingress-controller-deploy-job-cv8h5   0/1     Completed   0          6m49s
rke-metrics-addon-deploy-job-4nz4h        0/1     Completed   0          6m59s
rke-network-plugin-deploy-job-x25lb       0/1     Completed   0          10m
```
### EXPLANATION

So far you provisioned the Kubernetes infrastructure using RKE, now we will install Rancher. 

### INSTALLING RANCHER

1. Add the Helm Chart Repository
```
helm repo add rancher-stable https://releases.rancher.com/server-charts/stable
```
2. Create a Namespace for Rancher
```
kubectl create namespace cattle-system
```
3. Install cert-manager
```
kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v0.15.0/cert-manager.crds.yaml
```
4. Create the namespace for cert-manager
```
kubectl create namespace cert-manager
```
5. Add the Jetstack Helm repository
```
helm repo add jetstack https://charts.jetstack.io
```
6. Update your local Helm chart repository cache
```
helm repo update
```
7. Install the cert-manager Helm chart
```
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --version v0.15.0
```
8. Listing the pods 
```
kubectl get pods --namespace cert-manager

NAME                                       READY   STATUS    RESTARTS   AGE
cert-manager-766d5c494b-tq2jv              1/1     Running   0          21s
cert-manager-cainjector-6649bbb695-z4gn4   1/1     Running   0          21s
cert-manager-webhook-68d464c8b-4nd8g       1/1     Running   0          21s
```

### INSTALL RANCHER USING HELM

1. Installing Rancher # YOU HAVE ALREADY DEPLOYED HELM
```
helm install rancher rancher-stable/rancher \
  --namespace cattle-system \
  --set hostname=rancher.chlab.rancher      #SET YOUR LOAD BALANCER DNS NAME #YOU ARE NOT ALLOWED TO ADD AN URL DOMAIN."LOCAL", HENCE I CREATED ".RANCHER"
```
2. Check if the pods were created
```
kubectl -n cattle-system get deploy rancher
```
3. Outcome
```
NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
rancher   3         3         3            3           3m
```
### Finishing Uplink

That’s it. You should have a functional Rancher Server.

Open your web browser, go to the DNS name that forwards traffic to your load balancer. Then you should be greeted by the colorful login page.
```
https://rancher.chlab.rancher # I create this zone in my DNS, Rancher doesn't permit use yourdomain.local. (I have already metioned it)
```
Next step is to set the password and after that declare the URL.

You are done!

#### REFERENCES

https://helm.sh/docs/intro/install/ # INSTALL HELM
https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl #INSTALL KUBECTL
https://serverpilot.io/docs/how-to-use-ssh-public-key-authentication/ #SSH KEY
https://rancher.com/docs/rke/latest/en/installation/ # INSTALL RKE
https://rancher.com/docs/rancher/v2.x/en/installation/k8s-install/kubernetes-rke/ #INSTALL KUBERNETES RANCHER USING RKE
https://rancher.com/docs/rancher/v2.x/en/installation/k8s-install/helm-rancher/ #INSTALL RANCHER USING HELM
