# TP3

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
mysql -u manager -p manager_password  
mysql -u recruiter -p recruiter_password

### Ajouter trois lignes dans la table *games*

INSERT INTO teams.games (match_date, victory, observations) VALUES (NOW(), true, "quelle victoire!");  
INSERT INTO teams.games (match_date, victory, observations) VALUES (NOW(), true, "super match");  
INSERT INTO teams.games (match_date, victory, observations) VALUES (NOW(), false, "ils pensaient vraiment gagner");  
INSERT INTO teams.games (match_date, victory, observations) VALUES (NOW(), false, "très triste défaite");  
INSERT INTO teams.games (match_date, victory, observations) VALUES (NOW(), true, "il a fait un match magnifique");  

#### L'éxecuter avec l'utilisateur adéquat
mysql -u manager -p manager_password
