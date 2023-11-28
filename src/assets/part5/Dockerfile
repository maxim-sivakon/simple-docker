FROM nginx:latest

ARG USERNAME=nginx

USER root


COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./server.c /home/$USERNAME/server.c
COPY ./script.sh /home/$USERNAME/script.sh

EXPOSE 81/tcp

RUN apt-get update ; \
    apt-get install -y gcc spawn-fcgi libfcgi-dev ; \
    apt-get clean ; \
    rm -rf /var/lib/apt/lists/*

WORKDIR /home/$USERNAME

RUN usermod -aG adm,root,nginx $USERNAME ; \
    chown -R $USERNAME:$USERNAME /var/cache/nginx/ ; \
    chown -R $USERNAME:$USERNAME /var/run/ ; \
    chown -R $USERNAME:$USERNAME /home/$USERNAME/ ; \
    chmod +x /home/$USERNAME/script.sh

USER $USERNAME

HEALTHCHECK --interval=5s --timeout=10s --retries=3 CMD curl -sS 127.0.0.1 || exit 1

CMD [ "./script.sh" ]