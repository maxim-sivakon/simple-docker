FROM nginx
WORKDIR /home/
COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./main.c .
COPY ./start.sh .
RUN apt-get update && apt-get install -y gcc spawn-fcgi libfcgi-dev
ENTRYPOINT ["sh", "./start.sh"]