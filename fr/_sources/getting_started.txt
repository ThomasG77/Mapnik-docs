*******
Débuter
*******

Après avoir lu ce document, vous devriez être capable de créer un rendu de
carte simple avec Python ou C++.

TODO: Ajouter l'image finale obtenue


De quoi a-t-on besoin pour débuter?
===================================

Premièrement, vous devez installer la bibliothèque Mapnik sur votre système.
Il y a deux manières basiques de le faire, vous pouvez télécharger des binaires
(des exécutables) ou compiler votre librairie vous même depuis les sources.
L'installation depuis les sources est nettement plus comlexe et est documentée
dans Lien vers le document

L'installation dépend de votre système d'exploitation et les sections suivantes
décrivent le processus.

Installation sous Windows
-------------------------

Installer Mapnik sous Windows suit les étapes suivantes:

1. Installer les logiciels prérequis
2. Télécharger et décompresser la bibliothèque Mapnik
3. Configurez votre système et / ou les variables utilisateurs de votre
   environnement

Chacun de ces étapes est définie .. ici ..!

Installation sous Linux
-----------------------

La plupart des distributions modernes de Linux inclut des binaires Mapnik dans
leurs dépôts. La méthode d'installation dépend de votre distribution:

* Debian ou Ubuntu ::

  # aptitude install python-mapnik

* Fedora ::

  # yum install mapnik-python

* Archlinux ::

  # pacman -S mapnik

Installation sous MacOSX
------------------------

Dane Springmeyer héberge et compile généreusement les binaires Mapnik pour
MacOSX, vous pouvez retrouver plus d'informations sur la page de téléchargement
http://dbsgeo.com/downloads/.

Tester l'installation de Mapnik
===============================

La manière la plus simple de faire un test pour vérifier que vous avez installé
et configuré avec succès Mapnik est de démarrer l'interpréteur Python et
d'essayer d'importer la bibliothèque mapnik: ::

   $ python Python 2.6.5 (r265:79063, Apr  1 2010, 05:28:39) [GCC 4.4.3 20100316
   (prerelease)] on linux2 Type "help", "copyright", "credits" or "license" for
   more information.  >>> import mapnik >>>

S'il n'y a pas d'erreurs ou de complaintes, vous êtes sur la bonne voie. Vous
pouvez aussi essayer de trouver la version de Mapnik que vous utilisez
actuellement en exécutant le code suivant: ::

   >>> import mapnik >>> mapnik.mapnik_version() 701 # Human readable version
   >>> mapnik.mapnik_version_string() '0.7.1'

C'est bon, vous êtes prêt à créer votre première carte en utilisant le langage
Python.

