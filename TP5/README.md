# TP5
### Importer une base de donée MYSQL vers MariaDB  

#### Création du fichier docker-compose :

```
version:  '3.7'

services:
    mysql:
        image: mysql:5.7
        environment:
            MYSQL_ROOT_PASSWORD: password
        volumes:
            - ./mysql:/var/lib/mysql
            - ./backups:/backups

    maria:
        image: mariadb:10.4
        environment:
            MYSQL_ROOT_PASSWORD: password
        volumes:
            - ./maria:/var/lib/mysql
            - ./backups:/backups
```

#### Lancer un conteneur :
`docker-compose up -d` 

#### Exécuter des commandes à l'intérieur de notre serveur MySQL :
`docker-compose exec mysql sh`

#### Création de la bdd :
`mysql -u root -p`

```
# CREATE DATABASE animals;
# USE animals;
# CREATE TABLE cat (
    cat_name VACHAR(255),
    cat_age INT,
    cat_color VARCHAR(255)
);
# INSERT INTO cat (cat_name, cat_age, cat_color) VALUES ("Romeo", 1, "Roux");
# INSERT INTO cat (cat_name, cat_age, cat_color) VALUES ("Mako", 2, "Noir");
# INSERT INTO cat (cat_name, cat_age, cat_color) VALUES ("Musha", 5, "Noir");

```

#### Dump la base de données :
`mysqldump -u root -p animals cat > backups/database.sql`

#### Se connecter à MariaDB :
```
$ docker-compose exec mariadb bash
# mysql -u root -p < backups/database.sql
```