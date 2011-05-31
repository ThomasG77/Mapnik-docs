*******************************************************************
Alimenter la base de données PostGIS avec des données géographiques
*******************************************************************

Il y a plusieurs manières d'alimenter la base de données. Choisir
une des façons suivantes:

- Fichier OpenStreetMap "Planet"
  
  Le fichier OpenStreetMap "Planet" est une copie de la base de données mondiale.
  Ce fichier est volumineux et peut par conséquent engendrer quelques problèmes informatiques (également pour les nouveaux utilisateurs).

- Fichier OpenStreetMap "Extract"

  Les fichiers OpenStreetMap "Extract" contiennent les données OpenStreetMap
  pour une région donnée, souvent un pays, un état ou une région. Ils sont
  plsu simple d'utilisation que le fichier "Planet".

- OpenStreetMap API Bounding Box

  De plus petites zones géographiques, à l'échelle d'une ville par exemple, peuvent 
  être obtenues l'API OpenStreetMap.

- Postgresql dump file


Fichier OpenStreetMap "Planet"
==============================

Récupérer la base de données complète OpenStreetMap, appelée également 
"fichier planet" et charger-la dans la base de données PostGIS.

Récupérer le fichier planet
---------------------------

Un nouveau "planet" est publié environ toutes les semaines. Le miroir 
et les archives sont disponibles à l'adresse http://planet.openstreetmap.org/ .
En juillet 2010 le fichier planet pesait environ 10 GB. Si vous n'êtes pas 
intéressé par la base de données complète, vous pouvez choisir de télécharger 
une extraction.

Install the Planet File
-----------------------

Using osm2pgsql or another tool.


OpenStreetMap Extract File
==========================

OpenStreetMap Extract files typically include a single country, state
or province.  They are published periodically by various sites
including

- http://download.geofabrik.de/osm/
- http://download.cloudmade.com/
- others

Aquire the Extract File
-----------------------







