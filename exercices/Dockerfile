FROM nginx:alpine

RUN rm -rf /usr/share/nginx/html/*

WORKDIR /usr/share/nginx/html
COPY files_tp_conteneur/3D-Rotate-Tube/ .

EXPOSE 80

CMD ["nginx","-g","daemon off;"]
