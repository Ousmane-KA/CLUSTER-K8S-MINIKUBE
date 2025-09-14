
---

# KUBERNETES

---

# Mise en place d’un Cluster Kubernetes avec Minikube sur Debian 11

Ce projet a pour objectif de mettre en place un cluster Kubernetes local sur une machine virtuelle Debian 11 à l’aide de **Minikube** et **Docker**.

---

## A) Installation de Minikube


---

### 1) Configuration de la VM

- **OS** : Debian 11  
- **CPU** : 2 cœurs  
- **RAM** : 8 Go  
- **Disque** : 50 Go  

---

### 2) Installation de Docker



---


````md


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

<img width="1479" height="244" alt="image" src="https://github.com/user-attachments/assets/6a04e4f0-6141-4066-b028-1e6ab782b70e" />


---

### 4) Démarrer Minikube

```bash
minikube start --driver=docker
```
<img width="1481" height="498" alt="image" src="https://github.com/user-attachments/assets/e496303f-b134-4d56-b090-359337284eb5" />


---

### 5) Vérifier le bon fonctionnement du cluster

```bash
minikube status
```
<img width="1464" height="331" alt="image" src="https://github.com/user-attachments/assets/e358b3ea-c807-4e4d-a2d0-a35859ed3146" />


---

###  6) Accéder au tableau de bord Kubernetes

Lancer le tableau de bord (en mode terminal uniquement) :

```bash
minikube dashboard --url
```
<img width="1486" height="257" alt="image" src="https://github.com/user-attachments/assets/f1eae5c4-184d-4945-bf69-a23f557ba4b8" />


> **Remarque** : Cette URL est accessible uniquement depuis la VM. Pour y accéder depuis votre machine locale, créez un tunnel SSH.

---

### 7) Tunnel SSH depuis votre machine locale

Sur votre PC local (Windows, Mac ou Linux), exécutez :

```bash
ssh -L 39789:127.0.0.1:39789 root@192.168.0.25
```

> Remplacez `192.168.0.25` par l’adresse IP de la VM où tourne Minikube.

<img width="1484" height="357" alt="image" src="https://github.com/user-attachments/assets/cfe18b13-77ff-4a7d-bb32-8d6297474b79" />


---

### 8) Accéder au dashboard dans un navigateur

```txt
http://127.0.0.1:39789/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/
```

<img width="1505" height="782" alt="image" src="https://github.com/user-attachments/assets/1ce67836-0e01-4fc1-9549-dbcdb36fd0f1" />


---

### 9) Commandes utiles

* Afficher les contextes disponibles :

```bash
minikube kubectl config get-contexts
```
<img width="1479" height="252" alt="image" src="https://github.com/user-attachments/assets/7bea6ef6-8056-4738-977d-009846a8c4a2" />



* Obtenir des infos sur le cluster :


```bash
minikube kubectl cluster-info
```
<img width="1457" height="268" alt="image" src="https://github.com/user-attachments/assets/209b1fda-37e0-4dff-a0a6-79beae2379f2" />


---

## B) Phase d’utilisation de MiniKube

### Comprendre les commandes de base

---
* On va créer un déploiement NGINX :

```bash
minikube kubectl -- create deployment nginx-deployment --image=nginx
```

<img width="1486" height="199" alt="image" src="https://github.com/user-attachments/assets/fb8c461c-e2a7-4403-9190-cea4f7c99b24" />

---
* On va exposer le déploiement en NodePort :

```bash
minikube kubectl -- expose deployment nginx-deployment --type=NodePort --port=80
```
<img width="1468" height="189" alt="image" src="https://github.com/user-attachments/assets/16b49c1b-6f72-4829-a952-dbdd1d771e2c" />

---
* On trouver le port NodePort attribué :

```bash
minikube kubectl -- get service nginx-deployment
```
<img width="1491" height="227" alt="image" src="https://github.com/user-attachments/assets/96d88f15-25c1-4986-9dd0-d14bd32c785a" />

---



---

## Conclusion

Ce projet est une base solide pour apprendre à manipuler un cluster Kubernetes localement avec Minikube. Il est idéal pour les environnements de test, d'apprentissage ou de prototypage.

---

