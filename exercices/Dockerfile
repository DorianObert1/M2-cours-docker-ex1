FROM nginx:alpine

# Nettoyer la page par défaut
RUN rm -rf /usr/share/nginx/html/*

# Copier le projet web dans le répertoire servi par nginx
WORKDIR /usr/share/nginx/html
COPY files_tp_conteneur/3D-Rotate-Tube/ .

EXPOSE 80

# Lancer nginx en foreground
CMD ["nginx","-g","daemon off;"]
