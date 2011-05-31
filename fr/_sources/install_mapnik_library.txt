********************************
Installer la librairie Mapnik
********************************

La librairie Mapnik peut être installée de plusieurs manières, choisissez l'une
d'elles:


- Installer depuis votre gestionnaire de paquets
- Installer depuis les binaires (sources déjà compilées pour une plate-forme)
- Installer la version stable de Mapnik depuis les sources
- Installer la dernière version de Mapnik depuis les sources
- tba ?

Est-ce que tout est à jour sur votre système?

::

  sudo aptitude update
  sudo aptitude safe-upgrade


Installer la bibliothèque Mapnik depuis votre gestionnaire de paquets
=====================================================================

Cela ne sera peut-être pas la version la plus récente de Mapnik, selon votre
distribution.

::

  sudo aptitude install mapnik


Installer la bibliothèque Mapnik depuis les binaires
====================================================

Les binaires Windows et MacOSX sont disponibles. Voir les instructions
d'installation pour Windows et MacOSX.

  TODO: Avoir une server de compilation pour créer les binaires .deb et .rpm

  TODO: Ajouter les instructions d'installation pour MacOSX

  TODO: Ajouter les instructions d'installation pour Windows



Installer la dernière version stable de la bibliothèque Mapnik depuis les sources
=================================================================================

::


  cd ~/src
  svn co http://svn.mapnik.org/tags/release-0.7.1/ mapnik
  cd mapnik
  python scons/scons.py configure INPUT_PLUGINS=all OPTIMIZATION=3 SYSTEM_FONTS=/usr/share/fonts/truetype/
  python scons/scons.py
  sudo python scons/scons.py install
  sudo ldconfig

Si Mapnik est compilé sans erreurs, confirmer que python trouve bien Mapnik.

::

  python
  >>> import mapnik
  # if python returns the 3-chevron prompt below without errors mapnik was properly detected
  >>>

