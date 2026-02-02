# Kubernetes – Deploy de Nginx e Apache com Minikube

Este repositório apresenta uma solução prática de orquestração de containers utilizando Kubernetes. O objetivo é demonstrar a implantação de dois servidores web — Nginx e Apache HTTPD — executando simultaneamente em um cluster local via Minikube, cada um exposto em portas distintas conforme o desafio proposto.

O projeto inclui:

- Criação de imagens personalizadas
- Deployments e Services completos em YAML
- Execução e validação no cluster
- Evidências de funcionamento
- Estrutura final organizada para fácil manutenção

---

## 1. Pré-requisitos

- Docker instalado
- Minikube instalado
- Kubectl configurado
- VS Code (opcional, mas recomendado)

---

## 2. Criação das Imagens Personalizadas

### Estrutura das pastas

apache-custom/
nginx-custom/

Cada pasta contém:

- index.html personalizado
- Dockerfile responsável por copiar o HTML para o servidor

### Dockerfile – Apache

```dockerfile
FROM httpd:latest
COPY index.html /usr/local/apache2/htdocs/index.html
```

### Dockerfile – Nginx

```dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
```

---

## 3. Construção das Imagens e Carregamento no Minikube

```bash
docker build -t apache-custom .
docker build -t nginx-custom .

minikube image load apache-custom
minikube image load nginx-custom
```

---

## 4. Deployments (YAML Completo)

### apache-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: apache-deployment
  labels:
    app: apache
spec:
  replicas: 2
  selector:
    matchLabels:
      app: apache
  template:
    metadata:
      labels:
        app: apache
    spec:
      containers:
        - name: apache
          image: apache-custom:latest
          imagePullPolicy: Never
          ports:
            - containerPort: 80
          resources:
            limits:
              cpu: "500m"
              memory: "256Mi"
            requests:
              cpu: "250m"
              memory: "128Mi"
```

### nginx-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx-custom:latest
          imagePullPolicy: Never
          ports:
            - containerPort: 80
          resources:
            limits:
              cpu: "500m"
              memory: "256Mi"
            requests:
              cpu: "250m"
              memory: "128Mi"
```

---

## 5. Services (YAML Completo)

### apache-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: apache-service
  labels:
    app: apache
spec:
  type: NodePort
  selector:
    app: apache
  ports:
    - name: http
      port: 80
      targetPort: 80
      nodePort: 30081
```

### nginx-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  labels:
    app: nginx
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - name: http
      port: 80
      targetPort: 80
      nodePort: 30080
```

---

## 6. Aplicação dos Arquivos no Cluster

```bash
kubectl apply -f .

kubectl get pods
kubectl get svc
```

---

## 7. Acesso aos Serviços

```bash
minikube service nginx-service
minikube service apache-service
```

---

## 8. Estrutura Final do Repositório

```
kubernetes-projeto/
│
├── apache-custom/
│   ├── Dockerfile
│   └── index.html
│
├── nginx-custom/
│   ├── Dockerfile
│   └── index.html
│
├── apache-deployment.yaml
├── apache-service.yaml
├── nginx-deployment.yaml
├── nginx-service.yaml
│
└── README.md
```

---

## 9. Evidências

- Página Nginx funcionando
- Página Apache funcionando
- Pods listados
- Services listados

---

## 10. Conclusão

O projeto demonstrou a implantação de dois serviços web utilizando Kubernetes e Minikube, reforçando conceitos de conteinerização, criação de imagens personalizadas, definição de Deployments e Services, além da validação prática do funcionamento dos pods e serviços expostos. A estrutura final do repositório segue boas práticas de DevOps, garantindo clareza e fácil manutenção.

---
