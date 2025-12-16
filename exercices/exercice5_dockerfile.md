# Énoncé — Dockeriser une application Java (multi-stage) et publier l’image sur Docker Hub

## Contexte

Vous disposez d’une application Java (ex. Spring Boot) hébergée sur GitHub. L’objectif est de produire une image Docker **optimisée** via un **Dockerfile multi-stage** (build séparé du runtime), puis de **publier** cette image sur **Docker Hub**.

Si vous n’avez pas de dépôt public sous la main, vous pouvez partir d’un projet Spring Boot “Hello World” de votre choix (ou générer un projet minimal via Spring Initializr), puis le pousser sur GitHub.

---

## Objectifs attendus

1. Construire l’application Java dans une étape dédiée (“builder stage”).
2. Créer une image finale **légère** qui ne contient que le nécessaire pour exécuter l’application.
3. Lancer un conteneur et vérifier que l’application répond correctement.
4. Tagger l’image et la **push** sur Docker Hub.



## Livrables

1. Le fichier `Dockerfile` (multi-stage).
2. Les commandes utilisées pour :
   * build l'image
   * run le conteneur
   * tagger l'image
   * push l'image sur Docker Hub
3. L'URL Docker Hub du repository contenant l'image publiée.

---

## Réponses

### 1. Build de l'image

```bash
docker build -t spring-boot-demo:latest .
```

### 2. Run du conteneur

```bash
docker run -d --name spring-boot-app -p 8082:8080 spring-boot-demo:latest
```

### 3. Vérification de l'application

```bash
curl http://localhost:8082/
```

### 4. Tag de l'image

```bash
docker tag spring-boot-demo:latest dorianobert/spring-boot-demo:1.0.0
```

```bash
docker tag spring-boot-demo:latest dorianobert/spring-boot-demo:latest
```

### 5. Push sur Docker Hub

```bash
docker login
```

```bash
docker push dorianobert/spring-boot-demo:1.0.0
```

```bash
docker push dorianobert/spring-boot-demo:latest
```

### 6. URL Docker Hub

https://hub.docker.com/r/dorianobert/spring-boot-demo
