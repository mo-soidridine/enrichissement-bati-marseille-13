# enrichissement-bati-marseille-13
Workflow FME et Indices Spectraux : Enrichissement de la BD TOPO BATIMENT sur le 13e arrondissement de Marseille

Ce projet détaille la conception et l'exécution d'un flux de travail automatisé de traitement de données spatiales à l'aide de FME (Safe Software). L'objectif est d'enrichir la couche "BATIMENT" de la base de données BD TOPO® de l'IGN, en se restreignant strictement à l'emprise du 13e arrondissement de Marseille (13013).
Cet enrichissement multidimensionnel intègre des données d'élévation issues du LIDAR HD de l'IGN pour calculer la hauteur des bâtiments, des données d'OpenStreetMap (OSM) pour compléter les informations d'usage, ainsi que des indices spectraux issus de l'imagerie satellitaire Landsat 8/9 pour caractériser l'environnement thermique et radiatif de chaque structure. Le résultat final est un fichier Shapefile unique respectant un schéma d'attributs précis, destiné à la modélisation des performances énergétiques urbaines.

Objectifs :

Concevoir un flux de travail d'intégration de données spatiales reproductible dans FME.

Extraire la hauteur des bâtiments à partir d'un nuage de points LiDAR en utilisant des statistiques zonales sur les MNS/MNT.

Calculer la température de surface (LST) et plusieurs indices spectraux (NDVI, NDBI, NDMI, NDWI, BSI, émissivité, albédo) à partir des réflectances de surface Landsat 8/9.

Intégrer les données OSM pour déduire les types de bâtiments et l'utilisation du sol.

Gérer et estimer les valeurs manquantes (nombre d'étages, de logements, occupation) sur la base de statistiques officielles et d'hypothèses de calcul.

Valider, documenter et livrer un fichier Shapefile conforme contenant 120 champs requis.

Données :

BD TOPO® de l'IGN (couche BATIMENT) : base de données topographique de référence.

IGN Lidar HD : nuage de points classifié (utilisé pour dériver le MNS et le MNT).

Imagerie Landsat 8/9 OLI/TIRS

OpenStreetMap (OSM) : données sur l'occupation et l'utilisation des sols.

Méthodologie :

Prétraitement externe (Python)
Calcul des indices spectraux (LST, NDVI, NDBI, etc.) via des scripts Python en amont de FME. Les calculs ont été appliqués sur quatre dates distinctes (23/07/2013, 30/12/2013, 29/11/2025, 19/08/2026).

Découpage et préparation (FME)
Importation de la couche BD TOPO® et découpage (Clipper) strict sur la limite administrative du 13e arrondissement de Marseille.

Extraction altimétrique
Analyse zonale sur le nuage de points LIDAR. La hauteur de chaque bâtiment est extraite en calculant la différence entre l'élévation maximale du toit et l'élévation minimale du sol (Z_DSM - Z_DTM).

Jointure spatiale et statistiques
Extraction pour chaque emprise de bâtiment des valeurs statistiques spectrales (Moyenne, Maximum et Minimum) pour les 8 indices et les 4 dates à partir des rasters générés.

Gestion du schéma d'attributs et des valeurs manquantes
Croisement avec les données OSM pour déduire les usages. Estimation des valeurs manquantes par des formules mathématiques (ex: le nombre d'étages est estimé en divisant la hauteur LiDAR par 3 mètres ; le nombre de logements est déduit de la surface de plancher totale).


Le flux de travail a permis de générer avec succès un fichier Shapefile topologiquement sain, compilant les 120 champs requis pour 29 099 polygones de bâtiments.
L'approche combinant FME et Python s'est avérée efficace pour répondre aux spécifications de la modélisation urbaine. Toutefois, la gestion des valeurs manquantes a soulevé des défis : si l'estimation des étages et des logements s'est appuyée sur la géométrie, l'estimation de l'occupation humaine a produit une surestimation de 18,5 % par rapport aux données de l'INSEE, nécessitant une recalibration. Certains champs, comme les matériaux de construction (absents pour 60 % des bâtiments), ont été documentés et conservés comme valeurs nulles en raison d'un manque de sources fiables.

Outils :

- FME
- Python 

Le rapport PDF de l'étude est disponible dans les fichiers de ce dépôt
Travail universitaire réalisé en groupe dans le cadre du Master 2 - Géomatique et Modélisation Spatiale (GMS), Aix-Marseille Université (Année universitaire 2026-2027).
