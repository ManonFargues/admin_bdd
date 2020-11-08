# TP2

### Création de la base de données
CREATE DATABASE events;  
USE events;

### Création des tables
CREATE TABLE public_events (event_date DATE, event_name VARCHAR(255), event_age_requirement TINYINT);  
CREATE TABLE private_events (event_date DATE, event_name VARCHAR(255), event_age_requirement TINYINT);

### Création de l'utilisateur *event_manager* et lui donner toutes les permissions sur la base de données *events*
GRANT ALL PRIVILEGES ON events TO 'event_manager'@'localhost'
IDENTIFIED BY 'password';

### Création de l'utilisateur *event_supervisor* et lui donner le droit de visualiser le contenu de la table public_events
GRANT SELECT ON public_events TO 'event_supervisor'@'localhost'
IDENTIFIED BY 'password';

#### Valider les droits
FLUSH PRIVILEGES;

### Connexion à l'utilisateur event_manager
mysql -u event_manager -p  

#### Insérez 3 lignes de données dans les tables
INSERT INTO public_events (event_date, event_name, event_age_requirement) VALUES (NOW(), "Manon", 18);  
INSERT INTO public_events (event_date, event_name, event_age_requirement) VALUES (NOW(), "Leo", 33);  
INSERT INTO public_events (event_date, event_name, event_age_requirement) VALUES (NOW(), "Lisa", 96);  

INSERT INTO private_events (event_date, event_name, event_age_requirement) VALUES (NOW(), "Roméo", 12);  
INSERT INTO private_events (event_date, event_name, event_age_requirement) VALUES (NOW(), "Lauryne", 19);  
INSERT INTO private_events (event_date, event_name, event_age_requirement) VALUES (NOW(), "Lise", 5);  

### Connexion à l'utilisateur event_supervisor
mysql -u event_supervisor -p  

#### Lire le contenu de la table public_events
SELECT * from public_events  
+---------------+------------------+---------------+  
| event_date | event_name | event_age |  
+---------------+------------------+---------------+  
| 2020-09-28 | Manon   -------    |        18 -----------|  
| 2020-09-28 | Leo -----------     |        33 -----------|  
| 2020-09-28 | Lisa -----------   |        96 -----------|  
+---------------+------------------+---------------+  
3 rows in set (0.001 sec)

SELECT * FROM private_events;  
ERROR 1142 (42000): SELECT command denied to user 'event_supervisor'@'localhost' for table 'private_events'

### Reconnexion en root
mysql -u root -p  

#### Suppression de l'utilisateur event_supervisor
DROP USER 'event_supervisor'@'localhost'
