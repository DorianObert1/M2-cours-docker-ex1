FROM alpine/git AS fetcher
ARG REPO_URL=https://github.com/withaarzoo/3D-Rotate-Tube
RUN git clone --depth 1 "$REPO_URL" /src

FROM nginx:alpine
RUN rm -rf /usr/share/nginx/html/*
WORKDIR /usr/share/nginx/html
COPY --from=fetcher /src/ .
EXPOSE 80
CMD ["nginx","-g","daemon off;"]
