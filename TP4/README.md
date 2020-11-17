# TP4
<details>
<summary>Ennoncé du Tp</summary>

```
Première partie :

Créez une image Docker qui contient tous les outils nécessaires pour mettre en place un système de backup automatique.

Deuxième partie :

Mettez en place une stratégie de backups grâce à cron qui génère un dump de la base de données tous les lundis à 17h et génère un fichier compressé en format gzip contenant la date de création.
```

</details>

### Création de l'image

Dockerfile :

```
FROM mysql                           # Import de l'image

ENV MYSQL_ROOT_PASSWORD="password"   # Définition de la variable d'environnement

RUN apt update -y                    # Update et installation de curl 
RUN apt install curl -y              # pour pouvoir installer les paquets 
RUN apt install logrotate -y         # nécessaires
RUN apt install cron -y
RUN apt install nano -y
RUN service cron enable              # On active cron de façon permanente
```

### On rentre dans notre service
`$ docker exec -it bash`

Puis on configure le crontab pour obtenir une backup tous les lundis à 17h.

`17 0 * * 1 tar -zcf /var/backups/"$(date +%Y-%m-%d)".gz /home/backups`

Puis on configure logrotate pour un dump journalier en gardant les 5 derniers dumps au format bz2

```
/backups/"$(date +%Y-%m-%d)".gz {
        rotate 5
        daily
        postrotate
                /home/backups/"$(date +%Y-%m-%d)"
        compresscmd /bin/bzip2
        compressext .bz2
        endscript
}
```