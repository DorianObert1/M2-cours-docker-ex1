# Formation Docker - Exercices et TPs

Ce dépôt contient une série d'exercices et de travaux pratiques pour apprendre Docker.

## Structure du projet

```
docker/
├── README.md
└── exercices/
    ├── exercice1_images.md              # Exercice sur la manipulation des images Docker
    ├── exercice2_conteneur.md           # Exercice sur la manipulation des conteneurs
    ├── exercice3_conteneur.md           # TP complet sur les conteneurs (3 parties)
    ├── exercice3_commandes_rapides.md   # Résumé des commandes de l'exercice 3
    └── files_tp_conteneur/              # Fichiers pour le TP conteneur
        ├── html5up-editorial-m2i.zip
        ├── html5up-massively.zip
        ├── html5up-paradigm-shift.zip
        ├── jeux_2048.png
        └── nginx.png
```

## Contenu des exercices

### Exercice 1 : Images Docker
- Manipulation de base des images Docker
- Commandes : `docker pull`, `docker images`, `docker rmi`, etc.

### Exercice 2 : Conteneurs Docker
- Création et manipulation de conteneurs avec Alpine Linux
- Clonage de dépôts GitHub dans un conteneur
- Modification de fichiers dans un conteneur
- Copie de fichiers entre conteneur et machine locale
- **Concepts clés** : `docker run`, `docker exec`, `docker cp`

### Exercice 3 : TP Complet - Conteneurs
Un TP en 3 parties couvrant :

#### Partie 1 : Jeu 2048
- Création et gestion de conteneurs pour le jeu 2048
- Gestion de multiples conteneurs sur différents ports
- Cycle de vie des conteneurs (start, stop, remove)

#### Partie 2 : Serveurs Web (Nginx et Apache)
- Déploiement de serveurs web Nginx et Apache
- Modification de pages HTML dans les conteneurs
- Accès et modification de fichiers dans les conteneurs

#### Partie 3 : Déploiement de sites HTML
- Création de 3 conteneurs Nginx
- Déploiement de sites HTML5 templates
- Utilisation de `docker cp` pour copier des fichiers

## Comment utiliser ce dépôt

1. **Prérequis** : Assurez-vous d'avoir Docker installé sur votre machine
   ```bash
   docker --version
   ```

2. **Commencer par les bases** : Lisez et complétez les exercices dans l'ordre
   - Exercice 1 : Images
   - Exercice 2 : Conteneurs
   - Exercice 3 : TP Complet

3. **Utiliser le guide rapide** : Pour l'exercice 3, consultez `exercice3_commandes_rapides.md` pour un résumé de toutes les commandes

## Commandes Docker essentielles

```bash
# Images
docker images                  # Lister les images
docker pull <image>           # Télécharger une image
docker rmi <image>            # Supprimer une image

# Conteneurs
docker ps                     # Lister les conteneurs actifs
docker ps -a                  # Lister tous les conteneurs
docker run -d --name <nom> -p <port>:<port> <image>  # Créer et lancer un conteneur
docker start <conteneur>      # Démarrer un conteneur
docker stop <conteneur>       # Arrêter un conteneur
docker rm <conteneur>         # Supprimer un conteneur
docker exec -it <conteneur> bash  # Accéder au shell d'un conteneur

# Copie de fichiers
docker cp <conteneur>:<chemin-source> <chemin-destination>  # Copier depuis le conteneur
docker cp <chemin-source> <conteneur>:<chemin-destination>  # Copier vers le conteneur

# Nettoyage
docker container prune        # Supprimer tous les conteneurs arrêtés
docker image prune            # Supprimer les images inutilisées
```

## Ressources

- [Documentation officielle Docker](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/) - Catalogue d'images Docker

## Notes importantes

- Sur Alpine Linux : utilisez `apk` au lieu de `apt-get`
  - `apk add <package>` pour installer un package
  - `apk update` pour mettre à jour la liste des packages

- Syntaxe de `docker cp` : utilisez `:` (deux-points) entre l'ID du conteneur et le chemin
  - Correct : `docker cp container_id:/path/to/file ./destination`
  - Incorrect : `docker cp container_id/path/to/file ./destination`

---

**Bon apprentissage avec Docker ! 🐳**


