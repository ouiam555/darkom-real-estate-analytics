Darkom.ma — Pipeline de données immobilières
Pipeline ETL complet · PostgreSQL · Power BI · Schéma dimensionnel

Architecture du pipeline
CSV source
→
Staging
→
Clean
→
Warehouse
→
Power BI
Exécution complète via python main.py

Structure du projet
main.py
Orchestrateur — lance les 3 étapes en séquence
loads/load_staging.py
Import CSV → table staging.annonces_raw
scripts/clean.py
Nettoyage, feature engineering → clean.annonces
loads/load_warehouse.py
Chargement dimensions + faits → bi_schema
warehouse/warehouse.sql
DDL du schéma étoile (bi_schema)
data/darkom.csv
Fichier source brut des annonces
darkom.pbix
Rapport Power BI (4 dashboards)
staging.log
Logs de chargement staging
Prérequis & configuration
Créer un fichier .env à la racine du projet :

db_host	Hôte PostgreSQL (ex: localhost)
db_port	Port PostgreSQL (ex: 5432)
db_name	Nom de la base (darkom_dwh)
db_user	Utilisateur PostgreSQL
db_password	Mot de passe
pip install -r requirements.txt
Couches de données
staging
Chargement brut du CSV sans transformation. Tous les champs en TEXT. Log automatique dans staging.log.
clean
Nettoyage des doublons, valeurs manquantes, outliers. Feature engineering : prix_m2, age_bien, catégories prix/surface, dimensions temporelles.
warehouse
Schéma étoile dans bi_schema : fact_annonces + 5 tables de dimensions.
Power BI
4 dashboards connectés à bi_schema : Vue globale, Analyse des prix, Géographique, Tendances.
Modèle dimensionnel (bi_schema)
fact_annonces
prix, surface, prix_m2, age_bien + clés FK vers toutes les dimensions
dim_temps
date_publication, annee_pub, mois_pub, trimestre_pub
dim_localisation
ville, quartier
dim_bien
type_bien, categorie_surface, nb_chambres, nb_salles_bain, etage
dim_transaction
Vente / Location
dim_categorie_prix
Économique / Moyen / Haut standing / Luxe
Dashboards Power BI
Vue globale du marché
KPIs, répartition par ville, évolution mensuelle
Analyse des prix
Prix moyen/m², par type de bien, segments de marché
Analyse géographique
Prix par ville et quartier, classements, filtres dynamiques
Analyse des tendances
Évolution temporelle, comparaison par ville, catégories de prix
Stack : Python · pandas · psycopg2 · SQLAlchemy · PostgreSQL · Power BI  |  Données : darkom.ma
