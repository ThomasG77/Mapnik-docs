
******
Mapnik
******

Mapnik consiste à faire de belles cartes. Il utilise la `bibliothèque AGG
<http://www.antigrain.com/>`_ ou `la bibliothèque Cairo
<http://www.cairographics.org/>`_ et offre un rendu avec anticrénelage
de première qualité avec une précision inférieure au pixel pour les données
géographiques. Il a développé à partir de zéro en utilisant du C++ moderne
et ne souffre donc pas de choix de conception hérités d'années d'existence.
Quand il s'agit de gérer les tâches communes à la plupart des logiciels comme
la gestion de la mémoire, les accès aux systèmes de fichiers, la gestion des
expressions régulières ou bien d'autres fonctionnalités, Mapnik ne réinvente
pas la roue, mais utilise les meilleures bibliothèques standards de l'industrie
logicielle issues de http://boost.org.


Historique
==========

Le projet Mapnik a été commencé en 2005 par un chaud après-midi de juin, par Artem Pavlenko.
La première version a été mise à disposition ...

La version courante est la 0.7.1 qui est disponible au téléchargement sur
http://mapnik.org/download/.


Communauté
==========

La communauté Mapnik est organisée autour du site principal http://mapnik.org
et du site de développement http://trac.mapnik.org sur lequel vous trouverez
les dernières nouveautés sur Mapnik et son développement. Le site de
développement est organisé comme un wiki qui contient beaucoup d'informations
utiles, et où vous pouvez contribuer librement en vous
`enregistrant <http://trac.mapnik.org/register>`_.

Vous pouvez aussi poser des questions sur les listes de diffusion `mapnik-users
<http://lists.berlios.de/mailman/listinfo/mapnik-users>`_ (mapnik-utilisateurs)
et si vous souhaitez contribuer au développement de Mapnik allez sur
`mapnik-devel
<http://lists.berlios.de/mailman/listinfo/mapnik-devel>`_
(mapnik-développeurs). En suivant ces liens vous trouverez aussi les archives
de ces listes, que vous pourrez consulter avant de poser une question. Vous
pouvez suivre ces listes sur `Nabble
<http://old.nabble.com/Mapnik-f28006.html>`_.

Si vous préférez une communication plus directe, vous pouvez nous trouvez dans
la salle **#mapnik** sur irc.freenode.net, n'hésitez pas à venir pour nous
poser vos questions quand vous voulez.

Plates-formes
=============

Mapnik est une boîte à outils multi-plates-formes qui fonctionne sur Windows,
Mac, et Linux (depuis la version 0.4). Les utilisateurs font fonctionner
généralement Mapnik sur Mac >=10.4.x (avec un processeur Intel comme PPC),
comme sur Debian/Ubuntu, Fedora, Centos, OpenSuse, et FreeBSD.

Formats de données supportés
============================

Mapnik utilise une architecture basée sur les plugins pour lire les différentes
sources de données. Les plugins courants, considérés comme stables et compilés
par défaut, sont :

* *shape* - permet l'accès aux fichiers `Shape ESRI
  <http://en.wikipedia.org/wiki/Shapefile>`_
* *postgis* - permet l'accès aux données géospatiales issues de `PostGIS
  <http://en.wikipedia.org/wiki/PostGIS>`_, l'extension spatiale pour
  `PostgreSQL <http://en.wikipedia.org/wiki/PostgreSQL>`_
* *raster* - permet l'usage d'images raster TIFF simples ou tuilées

Les autres plugins ne sont pas instables mais comme ils ont été rajoutés plus
récemment, ils peuvent encore comporter quelques erreurs:

* *gdal* - permet l'accès aux sources de données `GDAL
  <http://www.gdal.org/formats_list.html>`_
* *ogr* - permet l'accès aux sources de données `OGR
  <http://www.gdal.org/ogr/ogr_formats.html>`_
* *osm* - permet la lecture des fichiers de données XML `OpenStreetMap
  <http://www.openstreetmap.org>`_
* *sqlite* - permet l'accès aux données géospatiales vecteurs '`SQLite
  <http://en.wikipedia.org/wiki/SQLite>`_ / `Spatialite
  <http://www.gaia-gis.it/spatialite>`_
* *occi* - permet l'accès aux données géospatiales vecteurs de `oracle
  spatial <http://en.wikipedia.org/wiki/Oracle_Spatial>`_ 10g (versions
  10.2.0.x)
* *kismet* - permet le support de `kismet <http://www.kismetwireless.net/>`_
  Ce support GPS montre les noeuds WLAN sur la carte
* *rasterlite* - permet l'accès au format raster sqlite avec compression par
  "vaguelettes" `Rasterlite <http://www.gaia-gis.it/spatialite>`_

Plus de plugins d'accès aux données seront disponibles dans le futur. Si vous
ne voulez pas attendre et /ou voulez coder en C++, pourquoi ne pas écrire
votre propre plugin d'accès aux données?

Futur
=====

Un grand nombre de développeurs et de contributeurs développent activement
*mapnik2* qui deviendra la version **0.8.0**. De plus, il est
planifié que la version **0.7.2** sorte prochainement.

