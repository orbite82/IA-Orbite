<div align="center">
<img src="https://user-images.githubusercontent.com/47891196/139104117-aa9c2943-37da-4534-a584-e4e5ff5bf69a.png" width="350px" />
</div>

# IA-Orbite

IA-Estudos

✅ Arquitetura que vamos usar

🐳 Docker → runtime

☸ Kubernetes via KIND

🏗 Terraform → provisiona o cluster

💻 Linux (Pop!_OS 24.04 no seu caso)

```
---

1️⃣ Instalar dependências

Instalar Docker

```
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER
```
⚠ Depois disso, faça logout/login novamente.

✅ Instalar kubectl no Pop!_OS 24.04

1️⃣ Atualizar dependências

```
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl
```

2️⃣ Adicionar chave GPG oficial

```
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

3️⃣ Adicionar repositório oficial

```
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

5️⃣ Verificar instalação

```
kubectl version --client

```
---

# Instalar KIND

Projeto oficial: KIND

```
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

Testar:

```
kind --version
```
2️⃣ Criar projeto Terraform

Crie uma pasta:

```
mkdir terraform-kind && cd terraform-kind
```

Crie um arquivo main.tf:

```
terraform {
  required_providers {
    kind = {
      source = "tehcyx/kind"
      version = "0.0.16"
    }
  }
}

provider "kind" {}

resource "kind_cluster" "IA" {
  name = "cluster-local"

  kind_config {
    kind = "Cluster"
    api_version = "kind.x-k8s.io/v1alpha4"

    node {
      role = "control-plane"
    }

    node {
      role = "worker"
    }
  }
}
```

3️⃣ Inicializar Terraform

```
terraform init
```
5️⃣ Verificar se subiu

```
kubectl get nodes
```

Você deve ver:

```
cluster-local-control-plane
cluster-local-worker
```

🔥 Para destruir o cluster

```
terraform destroy
```
---