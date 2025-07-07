## 1. Update existing packages and install necessary tools:

```bash
sudo apt-get update -y
sudo apt-get install -y ca-certificates curl apt-transport-https gnupg
```
## 2. Create the /etc/apt/keyrings directory (if it doesn't exist):
This is where the new GPG key will be stored.

```bash
sudo mkdir -p -m 755 /etc/apt/keyrings
```

## 3. Download the Kubernetes public signing key and save it to the new keyrings directory:
This command fetches the key and stores it securely.

```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

Note: I've kept v1.29 as per your previous command, but ensure this version matches what you intend to install for kubectl (and ideally, what's compatible with your EKS cluster). You can find the latest stable version on https://dl.k8s.io/release/stable.txt.

## 4. Add the Kubernetes apt repository to your sources list:
This tells apt where to find the kubectl packages.

```bash
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

## 5. Update apt package index to include the new repository:

```bash
sudo apt-get update -y
```

## 6. Install kubectl:

```bash
sudo apt-get install -y kubectl
```

## 7. Verify kubectl Installation ✅
After installation, verify that kubectl is installed and accessible by checking its client version:

```bash
kubectl version --client
```
-----

## 8. Install eksctl.

```bash
curl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz" -o /tmp/eksctl.tar.gz
sudo tar -xzf /tmp/eksctl.tar.gz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```


You should now see the client version output, confirming a successful installation.
