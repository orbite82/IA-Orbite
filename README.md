# IA-Orbite
IA-Estudos

✅ Arquitetura que vamos usar

🐳 Docker → runtime

☸ Kubernetes via KIND

🏗 Terraform → provisiona o cluster

💻 Linux (Pop!_OS 24.04 no seu caso)

1️⃣ Instalar dependências
Instalar Docker
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER

⚠ Depois disso, faça logout/login novamente.

Instalar kubectl
sudo apt install kubectl -y
Instalar KIND

Projeto oficial: KIND

curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

Testar:

kind --version

2️⃣ Criar projeto Terraform

Crie uma pasta:

mkdir terraform-kind && cd terraform-kind

Crie um arquivo main.tf:

terraform {
  required_providers {
    kind = {
      source = "tehcyx/kind"
      version = "0.0.16"
    }
  }
}

provider "kind" {}

resource "kind_cluster" "default" {
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

3️⃣ Inicializar Terraform
terraform init

4️⃣ Criar o cluster
terraform apply

Digite yes quando pedir confirmação.

5️⃣ Verificar se subiu
kubectl get nodes

Você deve ver:

cluster-local-control-plane
cluster-local-worker
🔥 Para destruir o cluster
terraform destroy