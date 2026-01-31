# Kubernetes – Deploy de Nginx e Apache com Minikube

Este repositório apresenta uma solução prática de orquestração de containers utilizando Kubernetes. O objetivo é demonstrar a implantação de dois servidores web — Nginx e Apache HTTPD — executando simultaneamente em um cluster local via Minikube, cada um exposto em portas distintas conforme o desafio proposto.

---

## Objetivo do Projeto

A solução implementada busca atender aos seguintes pontos:
• Implantar dois servidores web independentes (Nginx e Apache) em um cluster Kubernetes.
• Criar Deployments com múltiplas réplicas para garantir disponibilidade.
• Expor cada aplicação por meio de Services do tipo NodePort.
• Validar o funcionamento do cluster Minikube utilizando o driver Docker.
• Demonstrar o uso de arquivos YAML para definição de recursos Kubernetes.
• Reforçar o uso de ferramentas essenciais no ciclo de vida de aplicações containerizadas.

---

## Arquitetura do Projeto

O projeto contém quatro arquivos YAML responsáveis pela criação dos recursos:

- nginx-deployment.yaml
- nginx-service.yaml
- apache-deployment.yaml
- apache-service.yaml

Cada Deployment cria duas réplicas da aplicação.  
Cada Service utiliza o tipo NodePort para permitir acesso externo.
Portas definidas conforme o desafio:

- Nginx exposto na porta 30080
- Apache exposto na porta 30081

---

## Requisitos do Ambiente

Para executar o projeto, é necessário ter instalado:

- Docker Desktop (Engine Running)
- Minikube
- Kubectl
- WSL 2 atualizado
- Windows 11
- VS Code

---

## Como Executar o Projeto

1. Inicie o Minikube utilizando o driver Docker:

minikube start --driver=docker

2. Aplique todos os arquivos YAML:

kubectl apply -f .

3. Verifique se os pods estão em execução:

kubectl get pods

4. Verifique os serviços criados:

kubectl get svc

---

## Acesso aos Serviços

### Importante sobre o driver Docker

Quando o Minikube utiliza o driver Docker, as portas NodePort não são expostas diretamente no localhost.
Portanto, acessar:

- http://localhost:30080
- http://localhost:30081
  não funciona.

### Forma correta de acessar

Utilize:

minikube service nginx-service
minikube service apache-service

O Minikube abrirá automaticamente o navegador com um endereço no formato:

- 127.0.0.1:xxxxx para Nginx
- 127.0.0.1:xxxxx para Apache

Essas portas são dinâmicas e podem mudar a cada execução.

### Motivo técnico

- O driver Docker não expõe NodePorts diretamente no host.
- O Minikube cria um túnel interno para acesso.
- O comando minikube service identifica e abre a porta correta.
  Esse comportamento é esperado e faz parte do funcionamento do Minikube com o driver Docker.

---

## Estrutura do Repositório

kubernetes-projeto/
│
├── nginx-deployment.yaml
├── nginx-service.yaml
├── apache-deployment.yaml
├── apache-service.yaml
└── README.md

---

## Evidências Recomendadas

Para fins de avaliação, recomenda-se registrar:

- Execução do comando minikube start
- Aplicação dos manifestos com kubectl apply -f .
- Listagem dos pods em execução
- Listagem dos serviços criados
- Acesso às páginas Nginx e Apache via minikube service

---

## Status do Projeto

A solução está concluída e funcional, com ambos os servidores implantados, acessíveis e executando corretamente em um cluster Kubernetes local.
