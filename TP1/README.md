# TP1

<details>
<summary>Ennoncé du Tp</summary>

```
1. Créez une table clients qui doit pouvoir contenir un nom, un prénom, une date de naissance et un code postal.  
2. Insérez 3 lignes dans cette table  
3. Affichez seulement les prénoms et codes postaux  
```

</details>

### Création de la base de données
CREATE DATABASE tp1;  
USE tp1;

### Création de la table *clients*
#### Contenant un prénom, une date de naissance et un code postal
CREATE TABLE clients (first_name VARCHAR(255), birthday DATE, cedex INT);

### Insérez 3 lignes de données dans la table
INSERT INTO clients (first_name, birthday, cedex) VALUES ("Manon", NOW(), 33700);  
INSERT INTO clients (first_name, birthday, cedex) VALUES ("Maxime", NOW(), 33300);  
INSERT INTO clients (first_name, birthday, cedex) VALUES ("Theo", NOW(), 33000);  

### Affichez seulement les prénoms et codes postaux
SELECT first_name, cedex FROM clients;
