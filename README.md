# Project-2
Installation et configuration de Docker
# docker-setup-guide

## Description
Ce projet fournit un guide étape par étape pour installer et configurer Docker, un outil permettant de créer et gérer des conteneurs. Il inclut un script d'installation, un guide de démarrage rapide, et un exemple d'utilisation avec Docker Compose.

## Contenu du projet
- **README.md** : Guide détaillé pour débuter avec Docker.
- **Script d'installation** : Automatisation du processus d'installation de Docker.
- **Exemple de Docker Compose** : Fichiers pour orchestrer des conteneurs multiples.
- **Ressources** : Documentation supplémentaire.

## README.md

```markdown
# Docker Setup Guide

## Description
Ce guide vous aidera à installer et configurer Docker sur un système Linux. Il inclut également un exemple de fichier Docker Compose pour démarrer rapidement avec des conteneurs.

## Prérequis
- Système d'exploitation basé sur Linux (Ubuntu, Debian, etc.).
- Accès root ou utilisateur avec privilèges sudo.

## Étapes d'installation
1. Téléchargez et exécutez le script d'installation :
   ```bash
   wget https://example.com/install_docker.sh
   chmod +x install_docker.sh
   sudo ./install_docker.sh
   ```

2. Déconnectez-vous et reconnectez-vous pour appliquer les modifications si votre utilisateur a été ajouté au groupe Docker.

3. Vérifiez que Docker est installé correctement :
   ```bash
   docker --version
   docker-compose --version
   ```

## Exemple de Docker Compose
Voici un exemple simple pour exécuter un conteneur Nginx :

```yaml
version: '3.8'
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
```

1. Créez un fichier `docker-compose.yml` avec le contenu ci-dessus.
2. Lancez le conteneur avec :
   ```bash
   docker-compose up -d
   ```
3. Accédez à votre serveur web sur `http://localhost:8080`.

## Ressources supplémentaires
- [Documentation officielle de Docker](https://docs.docker.com/)
- [Référentiel Docker Compose](https://github.com/docker/compose)
```

## Script d'installation (install_docker.sh)
Voici un script Bash pour installer Docker et Docker Compose sur un système Linux :

```bash
#!/bin/bash

# Couleurs pour la sortie
GREEN='\033[0;32m'
RED='\033[0;31m'
NC='\033[0m' # No Color

# Vérifier si l'utilisateur a les droits root
if [ "$EUID" -ne 0 ]; then
  echo -e "${RED}Veuillez exécuter ce script en tant que root ou avec sudo.${NC}"
  exit 1
fi

# Mise à jour des paquets
echo -e "${GREEN}Mise à jour des paquets...${NC}"
apt-get update && apt-get upgrade -y

# Installation des dépendances requises
echo -e "${GREEN}Installation des dépendances...${NC}"
apt-get install -y \ 
    apt-transport-https \ 
    ca-certificates \ 
    curl \ 
    gnupg \ 
    lsb-release

# Ajout de la clé GPG officielle de Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Ajout du dépôt Docker
echo \"deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \$(lsb_release -cs) stable\" > /etc/apt/sources.list.d/docker.list

# Installation de Docker Engine et Docker Compose
echo -e "${GREEN}Installation de Docker...${NC}"
apt-get update
apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Ajout de l'utilisateur actuel au groupe Docker
if [ "$SUDO_USER" ]; then
  usermod -aG docker $SUDO_USER
  echo -e "${GREEN}Ajout de $SUDO_USER au groupe Docker. Déconnectez-vous et reconnectez-vous pour appliquer les changements.${NC}"
fi

# Vérification de l'installation
echo -e "${GREEN}Vérification de l'installation de Docker...${NC}"
docker --version && docker-compose --version

# Message de fin
echo -e "${GREEN}Docker et Docker Compose ont été installés avec succès !${NC}"
```
