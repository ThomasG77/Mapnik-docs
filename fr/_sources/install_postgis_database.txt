*******************************
Installation de la base PostGIS
*******************************

Installer Postgresql and PostGIS
================================

Est-ce que tout est à jour sur notre système?

::

  sudo aptitude update
  sudo aptitude safe-upgrade

Installer PostgreSQL et l'extension PostGIS pour permettre d'utiliser des
requêtes géographiques.

::

  sudo aptitude install postgresql-8.4-postgis postgresql-contrib-8.4
  sudo aptitude install postgresql-server-dev-8.4
  sudo aptitude install build-essential libxml2-dev
  sudo aptitude install libgeos-dev libpq-dev libbz2-dev proj

Configurer le serveur PostgreSQL
================================

Editer le fichier de configuration situé dans
/etc/postgresql/8.4/main/postgresql.conf pour définir certains des
paramètres par défaut. Ces changements aident lorsque l'on doit gérer des grandes
quantités de données telles qu'on peut les trouver dans certaines bases de
données géographiques.

Ces changements sont à effectuer à quatre endroits dans le fichier de
configuration.

::

  shared_buffers = 128MB # 16384 for 8.1 and earlier
  checkpoint_segments = 20
  maintenance_work_mem = 256MB # 256000 for 8.1 and earlier
  autovacuum = off

Configurer les paramètres du noyau d'exécution
==============================================

Editer /etc/sysctl.conf pour définir les paramètres du noyau après un
redémarrage.

::

  kernel.shmmax=268435456

Ensuite, appliquer ce même paramètre du noyau maintenant mais sans redémarrer.

::

  sudo sysctl kernel.shmmax=268435456

Redémarrer le serveur PostgreSQL
================================

Redémarrer le serveur PostgreSQL pour prendre en compte les modifications de configuration.

::

  sudo /etc/init.d/postgresql-8.4 restart

Le serveur de base de données devrait redémarrer sans messages d'erreurs ou
d'avertissements

   |  * Restarting PostgreSQL 8.4 database server
   |    ...done.

Créer la base de données PostgreSQL
====================================

Créer la base de données nommée "gis". Certains des futurs outils supposerons
que vous utilisez ce nom de base de données. Substituer votre nom d'utilisateur
à la place de "username" dans les deux cas suivants. Ce nom va être celui de
l'utilisateur qui va générer les cartes avec mapnik.

::

  sudo -u postgres -i
  createuser username # answer yes for superuser
  createdb -E UTF8 -O username gis
  createlang plpgsql gis
  exit

Appliquer les extensions PostGIS à la base de données
=====================================================

Appliquer les extensions PostGIS à la base de données.

::

  psql -f /usr/share/postgresql/8.4/contrib/postgis.sql -d gis

La réponse devrait finir avec beaucoup de lignes du type

   |  ...
   |  CREATE FUNCTION
   |  COMMIT

puis lancer

::

  psql -f /usr/share/postgresql/8.4/contrib/spatial_ref_sys.sql -d gis


Substituer votre nom d'utilisateur "username" dans les deux emplacements de la
ligne suivante. C'est cet utilisateur qui doit être utilisé pour générer les
cartes avec mapnik.

::

  echo "ALTER TABLE geometry_columns OWNER TO username; ALTER TABLE
  spatial_ref_sys OWNER TO username;" | psql -d gis

La réponse devrait être du type

   |  ALTER TABLE
   |  ALTER TABLE

Permettre la mise à jour des données OpenStreetMap
====================================================

Cette étape optionnelle est seulement requise si vous prévoyez de faire des
mises à jour régulières de votre donnée OpenStreetMap. Activer pour cela le
module intarray pour pouvoir faire la mise à niveau de la base de manière
journalière, horaire ou par minute en utilisant les fichiers .osc de
OpenStreetMap.

::

  psql -f /usr/share/postgresql/8.4/contrib/_int.sql -d gis

Cela renvoie de multiples lignes du type

   |  ...
   |  CREATE FUNCTION
   |  CREATE OPERATOR CLASS

Définir le SRID pour la base de données PostGIS
===============================================

Cette étape est maintenant inutile normalement, spatial_ref_sys.sql contenant
déjà la référence de la projection. Elle est gardée pour mémoire.

Définir le SRID (Spatial Reference IDentifier) sur la nouvelle base de données.

::

  psql -f ~/bin/osm2pgsql/900913.sql -d gis

Le retour devrait être du type

   |  INSERT 0 1

