
---

# KUBERNETES

---

````md
# Mise en place d’un Cluster Kubernetes avec Minikube sur Debian 11

Ce projet a pour objectif de mettre en place un cluster Kubernetes local sur une machine virtuelle Debian 11 à l’aide de **Minikube** et **Docker**.

---

## A) Installation de Minikube

### Lien officiel
- [Minikube Documentation](https://minikube.sigs.k8s.io/docs/start/)

---

### 1) Configuration de la VM

- **OS** : Debian 11  
- **CPU** : 2 cœurs  
- **RAM** : 8 Go  
- **Disque** : 50 Go  

---

### 2) Installation de Docker

```bash
#!/bin/bash

# =============================
# INSTALLATION DE DOCKER SUR DEBIAN
# =============================

set -e

echo "Mise à jour du système"
apt update

echo "Installation des dépendances"
apt install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

echo "Ajout de la clé GPG Docker..."
mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | \
    gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo "Ajout du dépôt Docker officiel"
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/debian $(lsb_release -cs) stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null

echo "Mise à jour des dépôts"
apt update

echo "Installation de Docker Engine et plugins"
apt install -y \
    docker-ce \
    docker-ce-cli \
    containerd.io \
    docker-buildx-plugin \
    docker-compose-plugin

echo "Activation de Docker"
systemctl enable docker
systemctl start docker

echo "Docker est installé avec succès !"
````

**Remarque** : Pour exécuter Docker sans `sudo` :

```bash
sudo usermod -aG docker $USER
newgrp docker
```

---

### 3) Installation de Minikube

Télécharger la dernière version de Minikube :

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```

Installer Minikube globalement :

```bash
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

Vérifier la version :

```bash
minikube version
```

---

### 4) Démarrer Minikube

```bash
minikube start --driver=docker
```

---

### 5) Vérifier le bon fonctionnement du cluster

```bash
minikube status
```

---

###  6) Accéder au tableau de bord Kubernetes

Lancer le tableau de bord (en mode terminal uniquement) :

```bash
minikube dashboard --url
```

> **Remarque** : Cette URL est accessible uniquement depuis la VM. Pour y accéder depuis votre machine locale, créez un tunnel SSH.

---

### 7) Tunnel SSH depuis votre machine locale

Sur votre PC local (Windows, Mac ou Linux), exécutez :

```bash
ssh -L 39789:127.0.0.1:39789 root@192.168.0.25
```

> Remplacez `192.168.0.25` par l’adresse IP de la VM où tourne Minikube.

---

### 8) Accéder au dashboard dans un navigateur

```txt
http://127.0.0.1:39789/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/
```

---

### 9) Commandes utiles

* Afficher les contextes disponibles :

```bash
minikube kubectl config get-contexts
```

* Obtenir des infos sur le cluster :

```bash
minikube kubectl cluster-info
```

---

## ⚙B) Phase d’utilisation de MiniKube

### Comprendre les commandes de base



---



---

## Conclusion

Ce projet est une base solide pour apprendre à manipuler un cluster Kubernetes localement avec Minikube. Il est idéal pour les environnements de test, d'apprentissage ou de prototypage.

---

