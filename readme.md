# Cluster Kubernetes com Rancher


## Objetivo do playbook
Provisionar de forma rápida e padronizada um cluster Kubernets gerenciado pelo Rancher.

Para a criação do cluster Kubernetes foi utilizado [RKE (Rancher Kubernetes Engine)](https://rancher.com/products/rke/) e [Traefik](https://traefik.io/) como ingress controller.

Há configuração do Rancher foi realizada utilizando a [Rancher 2.x API](https://rancher.com/docs/rancher/v2.x/en/api/), alguns ajustes na configuração precisam ser realizados conforme a necessidade, para isso consulte as [configurações](#configuração)

Caso a execução do playbook seja realizado em VM's ou computadores que estejam na rede local informe o **IP dos hosts** no inventario, caso contrario pode apresentar erro de DNS.

---
## Pré-requisitos Rancher

- 1 Hosts
- 4GB de memória RAM
- 2 CPU'S
- 8GB de disco
- OS: CentOS 7.8

## Pré-requisitos Cluster Kubernetes

- 3 Hosts
- 2GB de memória RAM
- 2 CPU'S
- 8GB de disco
- OS: CentOS 7.8

---

## Configuração

Existe algumas variáveis no arquivo de [inventario](inventory/hosts) que devem ser alteradas conforme suas necessidades.

```sh
# Senha para acesso ao dashboard Rancher utilizando o usuário: admin
RANCHER_ADMIN_PASSWORD=admin

# Nome do cluster que será criado no Rancher
CLUSTER_NAME=ansiblek8s

# Usuario que terá as permissões para executar comando kubectl
USER_KB8S=user_rke

# Dominio do cluster
CLUSTER_DOMAIN=k8scluster.net

# Endereço do loadbalancer
TRAEFIK_ADDRESS=traefik.k8scluster.net
```

No grupo de hosts **k8s-cluster** existe a variável role que definimos a responsabilidade de cada nó do cluster (**controlplane, etcd, worker**).

Também devemos escolher um dos nós e definir a variável **primary como true**, pois esse nós será responsável pela execução de todos os comandos para criação do cluster Kubernetes.

```sh
0.0.0.1 roles="controlplane, etcd" primary=true
0.0.0.2 roles="worker"
0.0.0.3 roles="worker"
```

---

## Execução do playbook

Todos os testes realizados foram feitos utilizando o **Ansible 2.10.5** 

```sh
ansible-playbook -i inventory/hosts play-cluster.yml
```
