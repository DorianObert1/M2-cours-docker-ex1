# TP conteneur

## Partie 1

- En utilisant votre machine Windows, lancez le service Docker, s'il n'est pas lancé.

   ```bash
   docker ps
   ```

- Créer une image Docker sur votre machine du jeu 2048 (voir screen jeux_2048).

   ```bash
   cd /tmp
   mkdir -p 2048-game
   cd 2048-game
   curl -L https://github.com/gabrielecirulli/2048/archive/master.zip -o 2048.zip
   unzip -q 2048.zip
   mv 2048-master/* .
   rm -rf 2048-master 2048.zip
   ```

- Vérifier que l'image est bien présente sur votre machine.

   ```bash
   docker images nginx
   ```

- Lancer ce jeu sur un port disponible au travers d'un conteneur que vous allez appeler «jeu-votre-nom ». 

   ```bash
   docker run -d --name jeu-dorian -p 8085:80 nginx
   docker cp /tmp/2048-game/. jeu-dorian:/usr/share/nginx/html/
   ```

- Vérifier que le conteneur est bien lancé avec la commande adaptée.

   ```bash
   docker ps
   ```

- Créer un second conteneur qui va lancer le même jeu mais avec un nom différent «jeu2-votre-nom ».

   ```bash
   docker run -d --name jeu2-dorian -p 8086:80 nginx
   docker cp /tmp/2048-game/. jeu2-dorian:/usr/share/nginx/html/
   ```

- Les 2 jeux sont fonctionnels en même temps sur votre machine, effectuez la commande pour vérifier la présence des conteneurs.

   ```bash
   docker ps
   ```

- Ouvrez les 2 jeux sur votre navigateur. 

   ```bash
   open http://localhost:8085
   open http://localhost:8086
   ```

- Stopper les 2 conteneurs et assurez-vous que ces 2 conteneurs sont arrêtés.

   ```bash
   docker stop jeu-dorian jeu2-dorian
   docker ps -a
   ```

- Relancez le conteneur «jeu2-votre-nom » et aller vérifier dans votre navigateur s'il fonctionne bien. Effectuez la commande pour voir s'il a bien été relancé. Puis stopper le. 

   ```bash
   docker start jeu2-dorian
   docker ps
   open http://localhost:8086
   docker stop jeu2-dorian
   ```

- Supprimez l'image du jeu 2048 et les conteneurs associés.

   ```bash
   docker rm jeu-dorian jeu2-dorian
   ```

- Vérifiez que les suppressions ont bien été faite.

   ```bash
   docker ps -a
   ```


## Partie 2

- Récupérer une image docker nginx.

   ```bash
   docker pull nginx
   ```

- Créer un conteneur en vous basant sur cette image en lui attribuant le nom suivant : « nginx-web».

   ```bash
   docker run -d --name nginx-dorian -p 8080:80 nginx
   ```

- Assurez-vous que l'image est bien présente et que le conteneur est bien lancé.

   ```bash
   docker images nginx
   docker ps
   ```

- Ce serveur nginx web (nginx-web) devra être lancé sur un port disponible.

   ```bash
   docker port nginx-dorian
   ```

- Vérifier que le serveur est bien lancé au travers du navigateur.

   ```bash
   open http://localhost:8080
   ```

- Une page web avec «Welcome to nignx » devrait s'afficher (voir nginx.png). 

- Effectuer la commande vous permettant de rentrer à l'intérieur de votre serveur nginx.

   ```bash
   docker exec -it nginx-dorian bash
   ```

- Une fois à l'intérieur, aller modifier la page html par défaut de votre serveur nginx en changeant le titre de la page en :  
Welcome «votre prenom ».

   ```bash
   apt-get update && apt-get install -y nano
   nano /usr/share/nginx/html/index.html
   ```

- Relancez votre serveur et assurez-vous que le changement à bien été pris en compte, en relançant votre navigateur.

   ```bash
   docker restart nginx-dorian
   open http://localhost:8080
   ```

- Refaite la même opération mais en utilisant le serveur web apache et donc il faudra créer un autre conteneur.

   ```bash
   docker pull httpd
   docker run -d --name apache-dorian -p 8081:80 httpd
   docker ps
   docker exec -it apache-dorian bash
   ```

- Il faut supprimer le contenu complet de l'index.html et y mettre : "Je suis heureux et je m'appelle votre prenom".

   ```bash
   docker exec apache-dorian bash -c 'echo "Je suis heureux et je m'\''appelle Dorian" > /usr/local/apache2/htdocs/index.html'
   docker restart apache-dorian
   ```

- Le changement doit appaître dans votre navigateur.

   ```bash
   open http://localhost:8081
   ```

## Partie 3

- Répétez 3 fois la même opération que pour le début de la partie 2, il faudra juste appelez vos conteneurs :

- « nginx-web3 ».

- « nginx-web4 ».

- « nginx-web5 ».

   ```bash
   docker run -d --name nginx-web3 -p 8082:80 nginx
   docker run -d --name nginx-web4 -p 8083:80 nginx
   docker run -d --name nginx-web5 -p 8084:80 nginx
   docker ps
   ```

- Il faudra faire en sorte que les pages html présente dans les fichiers ci-dessous s'affiche dans chacun des navigateurs en lien avec vos conteneurs :

- html5up-editorial-m2i.zip pour nginx-web3

- html5up-massively.zip pour nginx-web4

- html5up-paradigm-shift.zip pour nginx-web5

   ```bash
   cd /Users/dorianobert/IdeaProjects/docker/exercices/files_tp_conteneur
   
   unzip -q html5up-editorial-m2i.zip -d /tmp/site1
   docker cp $(find /tmp/site1 -name "index.html" -exec dirname {} \; | head -1)/. nginx-web3:/usr/share/nginx/html/
   open http://localhost:8082
   
   unzip -q html5up-massively.zip -d /tmp/site2
   docker cp $(find /tmp/site2 -name "index.html" -exec dirname {} \; | head -1)/. nginx-web4:/usr/share/nginx/html/
   open http://localhost:8083
   
   unzip -q html5up-paradigm-shift.zip -d /tmp/site3
   docker cp $(find /tmp/site3 -name "index.html" -exec dirname {} \; | head -1)/. nginx-web5:/usr/share/nginx/html/
   open http://localhost:8084
   ```

- Stopper, ensuite, ces différents conteneurs.

   ```bash
   docker stop nginx-web3 nginx-web4 nginx-web5
   docker ps -a
   ```

