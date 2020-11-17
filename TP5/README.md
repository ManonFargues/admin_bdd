# TP5
<details>
<summary>Ennoncé du Tp</summary>

```
Utilisez la configuration docker-compose précédente (TP4) afin d'instancier un serveur MySQL et un serveur MariaDB qui partagent un dossier /backups commun.

Connectez vous en premier au serveur MySQL, créez une base de données avec au moins une table qui contient quelques données.

Exportez cette base de données dans le dossier /backups.

Connectez vous au serveur MariaDB et importez la base que vous venez d'exporter.
```

</details>

### Importer une base de donée MYSQL vers MariaDB  

Création du fichier [docker-compose](docker-compose.yaml).

Lancer le conteneur : `docker-compose up -d` 

Exécuter des commandes à l'intérieur de notre serveur MySQL : `docker-compose exec mysql sh`

#### Création de la bdd :
`$ mysql -u root -p`

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
`$ mysqldump -u root -p animals cat > backups/database.sql`

#### Se connecter à MariaDB :
```
$ docker-compose exec mariadb bash
# mysql -u root -p < backups/database.sql
```