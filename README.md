# P3-Creer-et-utiliser-une-base-de-donnees-immobiliere-avec-SQL

- Mettre à jour un catalogue de données
- Créer des tables dans une base de données
- Effectuer des requêtes SQL pour répondre à une problématique métier
- Créer le schéma d'une base de données
- Charger des données dans une base de données

### *Requete 1. Sélection du nombre total d'appartements vendu pour le 1er semestre 2020*
SELECT COUNT (DISTINCT bien_id) AS "Nombres_appartements"
FROM Vente
JOIN Bien USING (bien_id)
WHERE type_bien = 'Appartement'
AND Date_achat BETWEEN '2020-01-01'AND '2020-06-30';

### *Requête 2. Sélection de la proportion des ventes d'appartements en fonction du nombre de pièces*
SELECT nombre_piece,
ROUND((COUNT (nombre_piece)::numeric)
/
(SELECT COUNT (DISTINCT bien_id) AS "Nombres_appartements"
FROM Vente
JOIN Bien USING (bien_id)
WHERE type_bien = 'Appartement')*100,2) AS "ProporXon"
FROM Bien
WHERE Type_bien = 'Appartement'
GROUP BY nombre_piece ORDER BY nombre_piece;

### *Requête 3. Sélection des 10 départements dont le prix du mètre carré est le plus élevé*
SELECT nombre_piece,
ROUND((COUNT (nombre_piece)::numeric)
/
(SELECT COUNT (DISTINCT bien_id) AS "Nombres_appartements"
FROM Vente
JOIN Bien USING (bien_id)
WHERE type_bien = 'Appartement')*100,2) AS "ProporXon"
FROM Bien
WHERE Type_bien = 'Appartement'
GROUP BY nombre_piece ORDER BY nombre_piece;

### *Requête 4. Calcul du prix moyen du mètre carré d'une maison en Ile-de-France*
SELECT ROUND((avg(valeur_bien /
surf_carrez)::numeric),2) AS "Prix m¬≤"
From Vente
INNER JOIN Bien ON Vente.bien_id = bien.bien_id
INNER JOIN Commune ON Bien.commune_id= commune.commune_id WHERE code_departement in ('75','77','78','91','92','93','94','95')
AND Bien.type_bien = 'Maison';

### *Requête 5. Sélection des 10 appartements les plus chers avec visualisation de leur département et de leur mètre carré*
SELECT
commune.code_departement AS "Departement",
bien.Surf_carrez AS "metre_carre",
vente.Valeur_bien AS "prix"
FROM Vente
INNER JOIN bien
ON vente.bien_id = bien.bien_id
INNER JOIN commune
ON bien.commune_id = commune.commune_id
WHERE bien.type_bien = 'Appartement’
AND valeur_bien != 0 ORDER BY vente.valeur_bien
DESC LIMIT 10;
