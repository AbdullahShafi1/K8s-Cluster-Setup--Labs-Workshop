## Step-by-Step Guide

### 1. **Initialize the Master Node**

1. SSH into the master node and initialize the cluster:
   ```bash
   sudo kubeadm init --apiserver-advertise-address=192.168.100.34 --pod-network-cidr=10.244.0.0/16
   ```

2. Save the join command output (e.g., `kubeadm join ...`) for use on worker nodes.

3. Set up `kubectl` for the `ubuntu` user:
   ```bash
   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```

4. Install a network plugin (Flannel):
   ```bash
   kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
   ```

5. Verify the master node is ready:
   ```bash
   kubectl get nodes
   ```

---

### 2. **Add Worker Nodes**

1. SSH into each worker node and run the join command from the master node:
   ```bash
   sudo kubeadm join 192.168.100.34:6443 --token <token> --discovery-token-ca-cert-hash <hash>
   ```

2. Verify that all nodes are connected from the master node:
   ```bash
   kubectl get nodes
   ```

---

### 3. **Deploy Podinfo Application**

1. Create a deployment file `deployment.yaml` on the master node:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: podinfo-deploy
   spec:
     replicas: 1
     selector:
       matchLabels:
         app.kubernetes.io/name: podinfo-deploy
     template:
       metadata:
         labels:
           app.kubernetes.io/name: podinfo-deploy
       spec:
         containers:
           - name: podinfo
             image: stefanprodan/podinfo
             ports:
               - containerPort: 9898
   ```

2. Apply the deployment:
   ```bash
   kubectl apply -f deployment.yaml
   ```

3. Expose the deployment as a NodePort service:
   ```bash
   kubectl expose deployment podinfo-deploy --type=NodePort --name=podinfo-deploy
   ```

4. Verify the service:
   ```bash
   kubectl get svc podinfo-deploy -o wide
   ```

5. Access the application:
   - Use any worker node’s IP and the NodePort assigned to `9898` to access Podinfo in a browser:
     ```
     http://<worker-node-ip>:<node-port>
     ```

---

### 4. **Verify Everything is Working**

- Check the pods:
  ```bash
  kubectl get pods -o wide
  ```

- Check the nodes:
  ```bash
  kubectl get nodes
  ```

---

This concludes the setup of a Kubernetes cluster with a sample application deployment. Feel free to enhance this setup with more features or deployments!