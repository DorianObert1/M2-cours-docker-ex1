# Exercice — Portabilité d’un site web avec Docker et volumes nommés

**Changement de serveur web sans perte de données**

---

## Mise en situation

Vous intervenez sur un projet interne qui consiste à déployer un **site web statique** à l’aide de Docker.
Le site est actuellement servi par **Nginx**, mais des décisions techniques successives vont imposer des changements de serveur web.

La contrainte métier est claire :
**le site ne doit jamais être perdu ni recopié manuellement**, quel que soit le serveur utilisé.
Les données doivent survivre à la suppression et à la recréation des conteneurs.

---

## Site web de départ

Vous utiliserez un site statique existant, librement réutilisable :

* **Start Bootstrap – Clean Blog** (licence MIT)

  * Lien : [https://github.com/StartBootstrap/startbootstrap-clean-blog](https://github.com/StartBootstrap/startbootstrap-clean-blog)

Vous pouvez également utiliser **votre propre site statique personnel** (HTML / CSS / JS uniquement), si vous en disposez déjà.

---

## Étape 1 — Dockerisation initiale avec Nginx

### Contexte

Le site doit être déployé dans un conteneur Docker à l’aide du serveur **Nginx**.

### Travail demandé

* Télécharger (ou utiliser) le site statique.
* Déployer le site dans un conteneur Docker basé sur Nginx.
* Vérifier que le site est accessible depuis un navigateur.

### Contraintes

* Le site doit être stocké dans un **volume nommé Docker**.
* L’utilisation de **bind mounts est interdite**.
* Les données du site doivent être indépendantes du conteneur.
* Vous êtes libres d’utiliser ou non un `Dockerfile` pour cette étape.

### Livrables attendus

* Toutes les **commandes Docker** exécutées.
* Une preuve que le site est bien accessible via Nginx.

---

## Étape 2 — Migration vers Apache HTTP Server

### Mise en situation

Une nouvelle politique de sécurité est appliquée dans l’entreprise.
Une **faille critique** a été identifiée sur la version de Nginx utilisée, et son usage est désormais interdit.

### Travail demandé

* Supprimer le conteneur Nginx.
* Redéployer le site à l’aide d’**Apache HTTP Server**.
* Réutiliser **le même volume nommé** contenant le site web.

### Contraintes

* Le volume ne doit pas être supprimé.
* Le contenu du site ne doit pas être modifié.
* Aucune copie manuelle du site ne doit être effectuée.

### Validation

* Le site doit être accessible via Apache.
* Le contenu doit être strictement identique à celui servi précédemment par Nginx.

---

## Étape 3 — Migration vers Caddy

### Mise en situation

Une phase de tests de performance et de simplicité d’exploitation est lancée.
L’équipe souhaite évaluer **Caddy**, réputé pour sa configuration simplifiée et ses performances.

### Travail demandé

* Supprimer le conteneur Apache.
* Déployer le site avec **Caddy**.
* Réutiliser **le même volume nommé** que précédemment.

### Validation

* Le site doit être accessible via Caddy.
* Les données doivent toujours provenir du même volume.
* Le contenu du site doit rester inchangé.

---

## Attendus globaux

Vous devez fournir :

* l'ensemble des **commandes Docker** exécutées pour les trois étapes ;

---

## Réponses

### Préparation - Création du volume et du site web

```bash
docker volume create site-web-volume
```

```bash
docker run --rm -v site-web-volume:/data alpine sh -c 'echo "<!DOCTYPE html>
<html lang=\"fr\">
<head>
    <meta charset=\"UTF-8\">
    <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">
    <title>Site Portable Docker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 50px auto;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }
        .container {
            background: rgba(255,255,255,0.1);
            padding: 30px;
            border-radius: 10px;
            backdrop-filter: blur(10px);
        }
        h1 { font-size: 2.5em; margin-bottom: 20px; }
        p { font-size: 1.2em; line-height: 1.6; }
        .server-info {
            background: rgba(0,0,0,0.2);
            padding: 15px;
            border-radius: 5px;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class=\"container\">
        <h1>Site Web Portable avec Docker</h1>
        <p>Ce site démontre la portabilité des données avec Docker et les volumes nommés.</p>
        <p>Le même contenu peut être servi par différents serveurs web :</p>
        <ul>
            <li>Nginx</li>
            <li>Apache HTTP Server</li>
            <li>Caddy</li>
        </ul>
        <div class=\"server-info\">
            <strong>Volume Docker :</strong> site-web-volume<br>
            <strong>Exercice :</strong> Portabilité des données
        </div>
    </div>
</body>
</html>" > /data/index.html'
```

### Étape 1 - Nginx

```bash
docker run -d --name web-nginx -p 8085:80 -v site-web-volume:/usr/share/nginx/html:ro nginx:alpine
```

```bash
curl http://localhost:8085/
```

### Étape 2 - Apache HTTP Server

```bash
docker rm -f web-nginx
```

```bash
docker run -d --name web-apache -p 8085:80 -v site-web-volume:/usr/local/apache2/htdocs:ro httpd:alpine
```

```bash
curl http://localhost:8085/
```

### Étape 3 - Caddy

```bash
docker rm -f web-apache
```

```bash
docker run -d --name web-caddy -p 8085:80 -v site-web-volume:/usr/share/caddy:ro caddy:alpine
```

```bash
curl http://localhost:8085/
```

### Vérification du volume

```bash
docker volume ls
```

```bash
docker volume inspect site-web-volume
```


