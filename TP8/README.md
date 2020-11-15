# TP8

### Fichier docker-compose qui réunit : 

 - Un serveur mariaDB
 - Un serveur prometheus
 - Un serveur mysql-exporter

```
version: '3.7'

services:
  maria-master:
    image: mariadb:10.5
    ports:
      - 3306
    environment:
      MYSQL_ROOT_PASSWORD: password
    volumes:
      - ./mysql:/var/lib/mysql
      - ./backups:/backups
      - ./config/master.cnf:/etc/mysql/mariadb.conf.d/master.cnf
      - ./scripts:/scripts
    networks:
      - mynetwork
  prometheus:
    image: prom/prometheus:latest
    ports:
      - 9090:9090
    environment:
      MYSQL_ROOT_PASSWORD: password
    volumes:
      - ./backups:/backups
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    networks:
      - mynetwork
  mysql-exporter:
    image: prom/mysqld-exporter:latest
    ports:
      - 9104
    environment:
      DATA_SOURCE_NAME: root:password@(maria-master:3306)/db_to_export
    volumes:
      - ./mysql:/var/lib/mysql
      - ./backups:/backups
      - ./config/master.cnf:/etc/mysql/mariadb.conf.d/master.cnf
      - ./scripts:/scripts
    networks:
      - mynetwork

networks:
  mynetwork:
```

Après avoir relié nos serveurs entre eux, Prometheus (`http://localhost:9090`) nous propose des commandes SQL nous donnant la possibilité de récupérer les données de notre serveur de base de données.

Nous pouvons créez un graphique qui comptera le nombre d'occurrence de la commande `SELECT` effectué sur notre serveur grâce à la commande : `mysql_global_status_commands_total{command="select"}`.

![Premier Dashboard](screenshots/graph1.png "Premier Dashboard")

Pour avoir la moyenne des commandes `SELECT` par périodes de 5 minutes, par exemple, nous pouvons utiliser : `(mysql_global_status_commands_total{command="select"}[5m])`

![Deuxième Dashboard](screenshots/graph2.png "Deuxième Dashboard")