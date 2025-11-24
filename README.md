# Link-Minikube-Clusters-from-Windows-and-Linux-using-Config-File-Sharing
This guide provides step-by-step instructions to control a single Minikube Kubernetes cluster from both Windows and Linux. By sharing your kubeconfig file and configuring networking appropriately, you can operate Kubernetes seamlessly across both environments.  

## 1. Install Minikube  
Install Minikube on Windows.  
(Optional) Install Minikube on Linux for local usage.  



### 2. Create Cluster using VMware Driver (Windows)
```
minikube start --driver=vmware  
minikube profile list
```

## 3. Export the Minikube Kubeconfig
```
kubectl config view --minify --flatten --raw --context=minikube > kubeconfig-minikube.yaml
```
The default Minikube kubeconfig on Windows is usually at:  
```
C:\Users\<YourUsername>\.kube\config
```

## 4. Transfer Kubeconfig to Linux
Copy the kubeconfig-minikube.yaml file to your Linux home directory using a file transfer tool (e.g., SCP, WinSCP, Termius):
```
scp C:\Users\<YourUsername>\.kube\kubeconfig-minikube.yaml user@linuxvm:~/kubeconfig-minikube.yaml
```

## 5. Use Windows Kubeconfig on Linux
On Linux:
```
export KUBECONFIG=~/kubeconfig-minikube.yaml  
kubectl get nodes
```
This uses the Windows Minikube cluster for any kubectl commands in the current session.  

## 6. Switch Back to Linux Cluster
To revert to your old context or config, run:  
```
unset KUBECONFIG
```
Or open a new terminal window.  

## 7. (Optional) Permanent Solution (Add to ~/.bashrc)
To avoid exporting KUBECONFIG every time, add this line to your `~/.bashrc`:  
```
echo 'export KUBECONFIG=~/kubeconfig-minikube.yaml' >> ~/.bashrc  
source ~/.bashrc
```
If you face permission issues while editing:  
```
sudo nano ~/.bashrc
```
## 8. (Optional) Merging kubeconfig Files 
You may merge multiple kubeconfig files for multi-cluster usage.

