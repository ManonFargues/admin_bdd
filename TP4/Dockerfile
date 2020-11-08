FROM mysql

ENV MYSQL_ROOT_PASSWORD="password"

RUN apt update -y
RUN apt install curl -y
RUN apt install logrotate -y
RUN apt install cron -y
RUN apt install nano -y
RUN service cron enable