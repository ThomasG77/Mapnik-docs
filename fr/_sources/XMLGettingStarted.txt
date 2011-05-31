***************************************************************
Tutoriel 2  -- 'Hello,world!' en utilisant la configuration XML
***************************************************************

Vue d'ensemble
--------------

Assurez-vous d'avoir Mapnik installé et d'avoir passé avec succès
:doc:`Commencer à utiliser Mapnik avec un exemple simple <GettingStarted-1>`.

* Cette page va vous guider dans l'usage de Mapnik avec Python pour gérer les
  les styles de carte avec un fichier XML séparé.

* C'est une approche pratique pour gérer vos styles/règles pour la carte
  séparément de votre code python, et celà peut être avantageux pour gérer des
  formatages très complexes.
  Par conséquent, c'est un fichier de configuration XML qui est utilisé par le
  projet OpenStreetMap pour faire des filtrages par attributs très importants
  sur des données PostGIS.


Deux exemples vont être montrés:

1) un fichier de configuration XML qui fournit exactement le même résultat que
pour la carte générée dans l'exemple en python pur dans :doc:`Commencer à
utiliser Mapnik avec un exemple simple <GettingStarted-1>`.


* C'est dans le but d'aider les nouveaux utilisateurs à comprendre l'usage
  courant des styles Mapnik et les paramètres des règles.

* Note: Ces exemples sont volontairement limités et ne tirent pas entièrement
  profit des capacités de Mapnik.

  * Voir le script :source:`rundemo.py <trunk/demo/python/rundemo.py>` dans les
    téléchargements pour des usages plus avancés de Mapnik avec Python.

  * Voir :source:`l'exemple du XML OSM
    <trunk/tests/data/good_maps/osm-styles.xml>` dans le répertoire de tests pour
    des usages plus avancés avec le XML.


2) Un XML de configuration qui utilise un jeu de données des contours mondiaux
avec les attributs de population pour créer une carte choroplèthe par taille
de la population. C'est dans le but d'introduire l'utilisation des règles de
filtrage et de gestion des étiquettes avec un XML.

Note: Le code pour l'exemple 1, ainsi que le code de :doc:`Commencer à
utiliser Mapnik avec un exemple simple <GettingStarted-1>` peut être
télécharger depuis un fichier zip à partir du lien ci-dessous. Ces
fichiers contiennent des chemins relatifs à un répertoire de données où vous
devrez placer world_borders.shp (et ses fichiers associés).

Etape 1
-------

**Hello World XML**

En premier, vous aurez toujours besoin d'un script python qui définit les
paramètres basiques de la carte et qui pointe vers le fichier XML de
configuration:

.. sourcecode:: python

    #!/usr/bin/env python
    import mapnik mapfile = 'world_styles.xml'
    map_output = 'hello_world_using_xml_config.png'
    m = mapnik.Map(600, 300)
    mapnik.load_map(m, mapfile)
    bbox = mapnik.Envelope(mapnik.Coord(-180.0, -90.0), mapnik.Coord(180.0, 90.0))
    m.zoom_to_box(bbox)
    mapnik.render_to_file(m, map_output)

* Copier ce code et sauver le fichier sous le nom **world_map.py**

Ensuite, vous allez devoir créer le fichier **world_styles.xml** référencé dans
**world_map.py**

Note: vous devrez spécifier le chemin vers le même `fichier shape des contours
mondiaux <http://mappinghacks.com/data/world_borders.zip>`_ de Mapping Hacks
utilisé dans le tutoriel 1

.. sourcecode:: xml

    <?xml version="1.0" encoding="utf-8"?>
    <!DOCTYPE Map>
    <Map bgcolor="steelblue" srs="+proj=latlong +datum=WGS84">
      <Style name="My Style">
        <Rule>
          <PolygonSymbolizer>
            <CssParameter name="fill">#f2eff9</CssParameter>
          </PolygonSymbolizer>
          <LineSymbolizer>
            <CssParameter name="stroke">rgb(50%,50%,50%)</CssParameter>
            <CssParameter name="stroke-width">0.1</CssParameter>
          </LineSymbolizer>
        </Rule>
      </Style>
      <Layer name="world" srs="+proj=latlong +datum=WGS84">
        <StyleName>My
          Style</StyleName>
        <Datasource>
          <Parameter name="type">shape</Parameter>
          <Parameter name="file">/path/to/your/world_borders</Parameter>
        </Datasource>
      </Layer>
    </Map>


* Copier ce fichier XML et sauver le fichier **world_styles.xml** au même niveau
  que le script **world_map.py**

Maintenant, lancer ce script avec la commande:

.. sourcecode:: bash

    python world_map.py

* Il devrait générer une image png dans le même répertoire un fichier
  équivalent à celui produit dans
  Started Tutorial.



Etape 2
-------

**World Population XML**

Vous trouverez joint ci-dessous et inclus comme code, un exemple de script
python qui accède à un fichier **population.xml pour la configuration de
la carte.**

Note: Vous devrez télécharger :download:`le fichier shp des contours mondiaux
des pays <../_files/world_borders.zip>`.

* Note: Ce fichier est originalement issu de `Thematic Mapping Blog
  <http://thematicmapping.org/downloads/world_borders.php>`_. La version
  jointe est le fichier shape fourni le plus simple comportant quelques
  modifications pour éviter des problèmes quand la carte est générée dans des
  projections telles que le 900913/3785 (ce tutoriel n'utilise pas cette
  projection, donc vous pouvez utiliser les fichiers shape originaux
  également). Voir le `ticket 308 <http://trac.mapnik.org/ticket/308>`_ pour
  des détails.

Ce script devrait générer un résultat graphique comme celui ci-dessous:

.. _worldpop:
.. figure::  ../_images/world_population_minimized.png


.. sourcecode:: python

    #!/usr/bin/env python

    import mapnik mapfile = "population.xml"
    m = mapnik.Map(1400, 600)
    mapnik.load_map(m, mapfile)
    bbox = mapnik.Envelope(mapnik.Coord(-180.0, -75.0), mapnik.Coord(180.0, 90.0))
    m.zoom_to_box(bbox)
    mapnik.render_to_file(m, 'world_population.png', 'png')

Et voici le fichier xml:

.. sourcecode:: xml

    <?xml version="1.0" encoding="utf-8"?>
    <!DOCTYPE Map>
    <!-- Sample Mapnik XML template by Dane Springmeyer -->
    <Map bgcolor="white" srs="+proj=latlong +datum=WGS84">
      <Style name="population">
        <Rule>
          <!-- Built from Seven Class sequential YIGnBu from www.colorbrewer.org -->
          <!-- Quantile breaks originally from QGIS layer classification -->
          <Filter>[POP2005] = 0 </Filter>
          <PolygonSymbolizer>
            <CssParameter name="fill">#ffffcc</CssParameter>
          </PolygonSymbolizer>
          <!-- Outlines for Antarctica look good -->
          <LineSymbolizer>
            <CssParameter name="stroke">black</CssParameter>
            <CssParameter name="stroke-width">.1</CssParameter>
          </LineSymbolizer>
        </Rule>
        <Rule>
          <Filter>[POP2005] &gt; 0 and [POP2005] &lt; 15000</Filter>
          <PolygonSymbolizer>
            <CssParameter name="fill">#c7e9b4</CssParameter>
          </PolygonSymbolizer>
          <!-- Outlines for Antarctica look good -->
          <LineSymbolizer>
            <CssParameter name="stroke">black</CssParameter>
            <CssParameter name="stroke-width">.1</CssParameter>
          </LineSymbolizer>
        </Rule>
        <Rule>
          <Filter>[POP2005] &gt;= 15000 and [POP2005] &lt; 255000</Filter>
          <PolygonSymbolizer>
            <CssParameter name="fill">#7fcdbb</CssParameter>
          </PolygonSymbolizer>
        </Rule>
        <Rule>
          <Filter>[POP2005] &gt;= 255000 and [POP2005] &lt; 1300000</Filter>
          <PolygonSymbolizer>
            <CssParameter name="fill">#1d91c0</CssParameter>
          </PolygonSymbolizer>
        </Rule>
        <Rule>
          <Filter>[POP2005] &gt;= 1300000 and [POP2005] &lt; 4320000</Filter>
          <PolygonSymbolizer>
            <CssParameter name="fill">#41b6c3</CssParameter>
          </PolygonSymbolizer>
        </Rule>
        <Rule>
          <Filter>[POP2005] &gt;= 4320000 and [POP2005] &lt; 9450000</Filter>
          <PolygonSymbolizer>
            <CssParameter name="fill">#225ea8</CssParameter>
          </PolygonSymbolizer>
        </Rule>
        <Rule>
          <Filter>[POP2005] &gt;= 9450000 and [POP2005] &lt; 25650000</Filter>
          <PolygonSymbolizer>
            <CssParameter name="fill">#225ea8</CssParameter>
          </PolygonSymbolizer>
        </Rule>
        <Rule>
          <Filter>[POP2005] &gt;= 25650000 and [POP2005] &lt; 1134000000</Filter>
          <PolygonSymbolizer>
            <CssParameter name="fill">#122F7F</CssParameter>
          </PolygonSymbolizer>
        </Rule>
        <Rule>
          <ElseFilter/>
          <!-- This will catch all other values - in this case just India and China -->
          <!-- A dark red polygon fill and black outline is used here to highlight these two countries -->
          <PolygonSymbolizer>
            <CssParameter name="fill">darkred</CssParameter>
          </PolygonSymbolizer>
          <LineSymbolizer>
            <CssParameter name="stroke">black</CssParameter>
            <CssParameter name="stroke-width">.7</CssParameter>
          </LineSymbolizer>
        </Rule>
      </Style>
      <Style name="countries_label">
        <Rule>
          <!--  Only label those countries with over 9 Million People -->
          <!--  Note: Halo and Fill are reversed to try to make them subtle -->
          <Filter>[POP2005] &gt;= 4320000 and [POP2005] &lt; 9450000</Filter>
          <TextSymbolizer name="NAME" face_name="DejaVu Sans Bold" size="7" fill="black" halo_fill="#DFDBE3" halo_radius="1" wrap_width="20" spacing="5" allow_overlap="false" avoid_edges="false" min_distance="10"/>
        </Rule>
        <Rule>
          <!--  Only label those countries with over 9 Million People -->
          <!--  Note: Halo and Fill are reversed to try to make them subtle -->
          <Filter>[POP2005] &gt;= 9450000 and [POP2005] &lt; 25650000</Filter>
          <TextSymbolizer name="NAME" face_name="DejaVu Sans Book" size="9" fill="black" halo_fill="#DFDBE3" halo_radius="1" wrap_width="20" spacing="5" allow_overlap="false" avoid_edges="false" min_distance="10"/>
        </Rule>
        <Rule>
          <!--  Those with over 25 Million get larger labels -->
          <Filter>[POP2005] &gt;= 25650000 and [POP2005] &lt; 1134000000</Filter>
          <TextSymbolizer name="NAME" face_name="DejaVu Sans Book" size="12" fill="white" halo_fill="#2E2F39" halo_radius="1" wrap_width="20" spacing="5" allow_overlap="false" avoid_edges="true" min_distance="10"/>
        </Rule>
        <Rule>
          <!--  Those with over 25 Million get larger labels -->
          <!--  Note: allow_overlap is true here to allow India to sneak through -->
          <Filter>[POP2005] &gt;= 1134000000</Filter>
          <TextSymbolizer name="NAME" face_name="DejaVu Sans Book" size="15" fill="white" halo_fill="black" halo_radius="1" wrap_width="20" spacing="5" allow_overlap="true" avoid_edges="true" min_distance="10"/>
        </Rule>
      </Style>
      <Layer name="countries" srs="+proj=latlong +datum=WGS84" status="on">
        <!-- Style order determines layering hierarchy -->
        <!-- Labels go on top so they are listed second -->
        <StyleName>population</StyleName>
        <StyleName>countries_label</StyleName>
        <Datasource>
          <Parameter name="type">shape</Parameter>
          <!-- FIXME -->
          <!-- Note:  'TM_WORLD_BORDERS_SIMPL-0.3' is the name of the shapefile (without the .shp file extension) -->
          <Parameter name="file">/PATH/TO/THE/TM_WORLD_BORDERS_SIMPL-0.3</Parameter>
        </Datasource>
      </Layer>
    </Map>

Fichiers joints

:download:`hello_world.zip <../_files/hello_world.zip>` - Tutoriel pour les
exemples Mapnik Hello World utilisant le python pur et le système de fichier
de configuration XML.

:download:`world_population.png <../_images/world_population.png>` Carte
résultante de l'étape 2 en utilisant le fichier XML de configuration pour
afficher une carte chloroplèthe de la population mondiale.

:download:`world_population_minimized.png
<../_images/world_population_minimized.png>` - Version réduite du résultat de
l'étape 2 pour le rendu sous Trac.

:download:`world_borders.zip <../_files/world_borders.zip>`.

