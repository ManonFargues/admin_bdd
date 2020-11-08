# TP6

## Script de création des deux instances MariaDB
### docker-compose :
```
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
  maria-slave:
    image: mariadb:10.5
    environment:
      MYSQL_ROOT_PASSWORD: password
    volumes:
      - ./maria:/var/lib/mysql
      - ./backups:/backups
      - ./scripts:/scripts
    networks:
      - mynetwork
networks:
  mynetwork:
```
## Configuration des deux serveurs : 
### master.cnf : 
```
[mariadb]
log-bin
server_id=1
log-basename=master1
binlog-format=mixed
```

### slave.cnf :
```
[mariadb]
log-bin
server_id=2
```

## Donner les droits
Il faut maintenant donner les droit de réplication à l'utilisateur de *master*.
```
CREATE USER IF NOT EXISTS 'replicant'@'%' identified by 'password';

grant replication SLAVE on *.* to replicant;

flush privileges;
```
## Réplication & export de bdd
On se connecte au serveur master : `$ docker-compose exec maria-master bash`.  
On lance le script (replication) : `# mysql -u root -p < scripts/replication`.  
Puis on exporte notre base de données dans un script : `# mysql -u root -p --databases tp6 > backups/database_tp.sql`.  

## Serveur slave

En étant connecté au serveur slave, on peut récupérer le script de création de base de données exporter grâce au master.
On lui dit à quel moment il doit reprendre la replication et où trouver le master.

```
CHANGE MASTER TO MASTER_HOST='maria',
MASTER_USER='replicant',
MASTER_PASSWORD='replicant_password',
MASTER_PORT=3306,
MASTER_LOG_FILE='master1-bin.000005',
MASTER_LOG_POS=800,
MASTER_CONNECT_RETRY=10;
```

On peut donc lancer la replication sur le slave et vérifier son état :
```
START SLAVE;
SHOW SLAVE STATUS;

Slave_IO_Running: Yes
Slave_SQL_Running: Yes
```

La replication fonctionne bien sur le slave et les modifications faites par le master seront directement envoyées au slave.
