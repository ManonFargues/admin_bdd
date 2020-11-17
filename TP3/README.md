# TP3
<details>
<summary>Ennoncé du Tp</summary>

```
1. Écrivez un script pour :
    1. Créer une base de données nommée teams
    2. Cette base contient une table games avec les colonnes match_date, victory, observations avec les types adaptées
    3. Cette base contient une table players avec les colonnes firstname, lastname, start_date avec les types adaptées
    4. Donner tous les droits sur la table games à un nouvel utilisateur manager avec le mot de passe manager_password
    5. Donner les droits d'écriture et de lecture sur la table players à un nouvel utilisateur recruiter avec le mot de passe recruiter_password
    6. Valider ces privilèges
2. Exécutez ce script pour l'utilisateur adéquat
3. Écrivez un script pour ajouter au moins trois lignes dans la table games et exécutez le pour l'utilisateur adéquat
```

</details>

### Création et utilisation de la base de données
CREATE DATABASE IF NOT EXISTS teams;  
USE teams;

### Création des tables
#### Table *games* avec match_date, victory et observations
CREATE TABLE IF NOT EXISTS games (match_date DATE, victory BOOLEAN, observations TEXT);

#### Table *players* avec firstname, lastname, start_date
CREATE TABLE IF NOT EXISTS players (firstname VARCHAR(255), lastname VARCHAR(255), start_date DATE);

#### Donner tous les droits sur la table *games* au user *manager*
GRANT ALL PRIVILEGES ON games TO 'manager'@'localhost'
IDENTIFIED BY 'manager_password';

#### Donner droits d'écriture et de lecture sur la table players au *user recruiter*
GRANT INSERT, SELECT ON players TO 'recruiter'@'localhost'
IDENTIFIED BY 'recruiter_password';

#### Valider les privilèges
FLUSH PRIVILEGES;

### Exécuter le script pour l'utilisateur adéquat
mysql -u manager -p   
mysql -u recruiter -p 

### Script pour ajouter au moins trois lignes dans la table *games*

INSERT INTO teams.games (match_date, victory, observations) VALUES (NOW(), true, "quelle victoire!");  
INSERT INTO teams.games (match_date, victory, observations) VALUES (NOW(), true, "super match");  
INSERT INTO teams.games (match_date, victory, observations) VALUES (NOW(), false, "ils pensaient vraiment gagner");  

#### L'éxecuter avec l'utilisateur adéquat
mysql -u manager -p 
