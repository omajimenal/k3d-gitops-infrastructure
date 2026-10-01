# Entorno GitOps k3d - Guía de Operación y Arquitectura

Este repositorio gestiona la infraestructura declarativa y las aplicaciones desplegadas en el clúster de Kubernetes **k3d**, ejecutado sobre WSL2 en el host remoto ASUS y administrado desde una MacBook Pro M5.

---

## 🏗️ Arquitectura de Red y Enrutamiento

* **Dominio Local:** `http://argocd.local`
* **IP Gateway ASUS:** `192.168.1.68`
* **IP WSL2 (Ubuntu):** `172.27.232.30`
* **Estrategia de Exposición:** `argocd-server` expuesto mediante `LoadBalancer` (`NodePort 32285`). Un túnel `socat` en WSL2 redirige el tráfico TCP entrante del puerto `80` al `32285`, haciendo de interfaz transparente hacia `netsh portproxy`.

---

## 📂 Estructura del Repositorio (App of Apps Pattern)

```text
k3d-gitops-infrastructure/
├── bootstrap/
│   └── root-app.yaml              # Aplicación Raíz (App of Apps)
└── apps/
    ├── arc/                       # Controller de GitHub Actions Runners (Futuro)
    ├── keda/                      # Auto-escalado basado en métricas (Futuro)
    └── workloads/                 # Manifiestos y aplicaciones de trabajo
        ├── hello-world.yaml       # Definición de Application para Argo CD
        └── manifests/
            └── pod.yaml           # Manifiestos de Kubernetes (Namespace: dev)