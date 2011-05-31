*******************************************************************************
Tutoriel 1 -- 'Hello,world!' en Python
*******************************************************************************

Etape 1
-------

Assurez- vous d'avoir mapnik installé. En fonction de votre chemin
d'installation, vous aurez peut-être besoin de modifier le chemin python
PYTHONPATH, /etc/ld.so.conf, d'exporter LD_LIBRARY_PATH ou de faire n'importe
quelle modification nécessaire au fonctionnement du système.

La manière la plus simple de faire cette vérification est de démarrer
l'interpréteur python à partir de la ligne de commande en tapant python
et ensuite de taper simplement:

.. sourcecode:: python

    >>> import mapnik


et si vous n'avez pas de messages d'erreur, vous êtes sur le bon chemin. Dans
le cas contraire, vous allez devoir encore vérifier votre installation.

.. note:: Si vous avez compilé mapnik en mode debug, vous devriez voir
   apparaître le listing des sources de données utilisables, incluant:

.. sourcecode:: python

    >>> import mapnik
    registered datasource : gdal
    registered datasource : postgis
    registered datasource : raster
    registered datasource : shape

Pour obtenir ce listing, vous avez aussi la possibilité de faire

.. sourcecode:: python

    python -c "from mapnik import DatasourceCache as c;print ','.join(c.plugin_names())"


Etape 2
-------

Le code ci-dessous peut être coller dans votre interpréteur python. Idéalement,
copier ligne par ligne le code pour confirmer que chaque étape fonctionne. Les
lignes commentées (#) devrait pouvoir être collées sans problème mais la
configuration de votre interpréteur pourra dans certains cas causer des erreurs.

 * Voir le bout de code (snippet) à l'étape 3 pour un code sans commentaires.

Importer la librairie mapnik et définir des paramètres basiques pour la carte

.. sourcecode:: python

    import mapnik

    # Instantiate a map object with given width, height and spatial reference
    # system
    m = mapnik.Map(600,300,"+proj=latlong +datum=WGS84")
    # Set background colour to 'steelblue'.
    # You can use 'named' colours, #rrggbb, #rgb or rgb(r%,g%,b%) format
    m.background = mapnik.Color('steelblue')



.. note:: Mapnik accepte toutes les projections que supporte PROJ4. Allez sur
   http://spatialreference.org pour obtenir les chaînes de caractères PROJ4
   pour les différentes projections. Par exemple,
   http://spatialreference.org/ref/epsg/4326/mapnikpython/ vous donnera les
   paramètres exacts pour la projection EPSG 4326 (utilisée dans les GPS par
   exemple).

Créer les styles et les règles pour la symbologie de la carte

.. sourcecode:: python

    # Now lets create a style and add it to the Map.
    s = mapnik.Style()
    # A Style can have one or more rules. A rule consists of a filter, min/max
    # scale demoninators and 1..N Symbolizers. If you don't specify filter and
    # scale denominators you get default values :
    #   Filter =  'ALL' filter (meaning symbolizer(s) will be applied to all
    #   features)
    #   MinScaleDenominator = 0
    #   MaxScaleDenominator = INF
    # Lets keep things simple and use default value, but to create a map we
    # we still must provide at least one Symbolizer. Here we  want to fill
    # countries polygons with greyish colour and draw outlines with a bit
    # darker stroke.

    r=mapnik.Rule()
    r.symbols.append(mapnik.PolygonSymbolizer(mapnik.Color('#f2eff9')))
    r.symbols.append(mapnik.LineSymbolizer(mapnik.Color('rgb(50%,50%,50%)'),0.1))
    s.rules.append(r)


Faire le lien entre les informations de style et celles de votre carte et de
votre donnée

.. sourcecode:: python

    # Here we have to add our style to the Map, giving it a name.
    m.append_style('My Style',s)

    # Here we instantiate our data layer, first by giving it a name and srs
    # (proj4 projections string), and then by giving it a datasource.
    lyr = mapnik.Layer('world',"+proj=latlong +datum=WGS84")
    # Then provide the full filesystem path to a shapefile in WGS84 or
    # EPSG 4326 projection without the .shp extension
    # A sample shapefile can be downloaded from
    # http://mapnik-utils.googlecode.com/svn/data/world_borders.zip
    lyr.datasource = mapnik.Shapefile(file='/Users/path/to/your/data/world_borders')
    lyr.styles.append('My Style')


Enfin, ajouter les couches à la carte et zoomer sur l'étendue maximale de la couche.

.. sourcecode:: python

    m.layers.append(lyr)
    m.zoom_to_box(lyr.envelope())

Finir en générant l'image de la carte du monde

.. sourcecode:: python

    #!python
    # Write the data to a png image called world.png in the base directory of your user
    mapnik.render_to_file(m,'world.png', 'png')

    # Exit the python interpreter
    exit()


Ensuite revenir dans votre terminal:

.. sourcecode:: bash

    # On a mac
    open world.png
    # On windows
    start world.png


Ou naviguer dans votre répertoire et ouvrir world.png avec un résultat qui
devrait ressembler à celui-ci-dessous:

.. _world:
.. figure::  ../_images/world.png


Etape 3
-------

L'étape suivante la plus logique est le lancement de ce même code depuis un
script python lancé via votre terminal. De cette manière, vous serez capable
de modifier et d'expérimenter des changements de paramètres.

Cette étape peut être réalisée en ajoutant la ligne suivante au début du script:

.. sourcecode:: python

    #!/usr/bin/env python

Copier tout le texte ci-dessous et l'enregistrer dans un fichier portant le
nom world.py.

.. sourcecode:: python

    #!/usr/bin/env python

    import mapnik
    m = mapnik.Map(600,300,"+proj=latlong +datum=WGS84")
    m.background = mapnik.Color('steelblue')
    s = mapnik.Style()
    r=mapnik.Rule()
    r.symbols.append(mapnik.PolygonSymbolizer(mapnik.Color('#f2eff9')))
    r.symbols.append(mapnik.LineSymbolizer(mapnik.Color('rgb(50%,50%,50%)'),0.1))
    s.rules.append(r)
    m.append_style('My Style',s)
    lyr = mapnik.Layer('world',"+proj=latlong +datum=WGS84")
    lyr.datasource = mapnik.Shapefile(file='/Users/path/to/your/data/world_borders')
    lyr.styles.append('My Style')
    m.layers.append(lyr)
    m.zoom_to_box(lyr.envelope())
    mapnik.render_to_file(m,'world.png', 'png')

.. note:: N'oubliez pas de changer le chemin pour qu'il pointe vers votre
   fichier shp world_borders.

   * Mapnik supporte aussi bien les chemins absolus que relatifs vers vos
     données.
   * Il en est de même pour le chemin de votre fichier.

Ensuite rendre le script exécutable. Sur Mac ou Linux, vous ferez cela avec la
commande:

.. sourcecode:: bash

    chmod +x world.py


Lancer le script avec la commande:

.. sourcecode:: bash

    # You must be in the same directory as you saved the script
    ./world.py
    # Add a second optional command to open the resulting file with one
    # keystroke
    # On a mac
    ./world.py; open world.png
    # On windows
    start world.py && start world.png


* Lancer de cette manière le script permettra d'écraser le fichier et d'ouvrir
  la carte world.png.
* Maintenant, vous pouvez facilement ouvrir le script dans un éditeur de texte
  et essayer de changer les dimensions, les couleurs ou la source de donnée
  (en n'oubliant pas de choisir la bonne projection).

Pour générer la même carte avec XML, lisez le
:doc:`Commencez avec mapnik et le XML (tutoriel 2) <XMLGettingStarted>` qui
montre l'usage du XML pour la configuration de la carte.

Pour télécharger, ce script ainsi avec d'autres scripts de tutoriels,
voir http://code.google.com/p/mapnik-utils/


Pièce jointe :download:`world.png <../_images/world.png>`.

