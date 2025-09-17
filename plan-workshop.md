# Plan

## 1. Introduction: Cells vs. Filaments (5 min) (Volker)

- Une cellule est un objet compact contrairement aux filaments.
- Quelques pixels qui manquent ou se rajoutent sur la segmentation d'une cellule ne sont pas dramatiques.
  Sur un filament, ça peut changer la morphologie en cassant ou créant une branche.
- Les métriques pour évaluer la segmentation de cellules varie de celles pour des filaments.
- On ne veut pas extraire les mêmes mesures de ces objets, qu'on peut voir comme des graphes ou arbres.
- Quelles sont les spécificités d'une image qui représente des filaments ?
    - Peu d'objet par rapport au fond
    - Potentiellement une perte de signal

## 2. Filaments segmentation (10 min)

- Quelles métriques utiliser pour savoir si on a une bonne segmentation pour des filaments ? (Clément)
    - F1 score, precision, and recall
    - clDice + Soft clDice
    - Skeleton Recall Loss
    - Morpho Cross Entropy Loss
    - BCE
    - Distance de Fréchet
    - RMSE
- *Quelle est la limite pour considérer qu'on est sur un filament? (ex: myotubes).*
- Différentes méthodes de segmentation (Volker)
    - SNT (semi-manuel)
    - Filtres + threshold
    - *ASB / SOAC / FIESTA*
    - Pixel classifier
    - Voir les conséquences du bruit, des débris
    - La présence ou non d'un soma/fusion qui est plus épais.
- Segmentation en utilisant un UNet. (essayer un multi-channels UNet ?) (Clément)
    - Qu'est-ce qu'un UNet
    - Différentes ground-truth utilisables
        - Masque de skeleton
        - Distance map
        - Distance géodésique depuis le soma
    - Les loss à utiliser selon la GT utilisée
    - Data augmentation + gestion des pertes de signal le long des filaments.
        - Merge by distance (postprocess -> faux positifs)
        - Ajout de trous dans la GT
- Juste mentionner que ça peut arriver: Comment gérer les zones où les filaments s'entremêlent. (Clément)
    - Sur une image labélisée, on ne peut pas simplement représenter un chevauchement.
    - Utilisation de B-splines
    - Différencier branche et chevauchement

## 3. Exercices segmentation (35 min)

- Comment calculer les métriques pour un lot de masque: donner un notebook qui les calcule pour un dossier
- Méthode en filter + threshold (Volker)
- Essayer en utilisant LabKit/QuPath/un plugin Napari qui interface skimage + calculer les métriques (Volker)
- Plugin Napari pour tune le UNet? (Clément)
- Train un UNet + calculer les métriques
    - Hyper-paramètres à modifier:
        - Attention gates ou non
        - Profondeur du réseau
        - Loss utilisée
    - Data augmentation pour boucher les trous
- Comparer les métriques obtenues pour les 3 méthodes (Volker)

## 4. Postprocessing & measurements (10 min)

- Comment reconstruire les skeletons depuis la distance map (Volker)
    - Brightest path tracing
        - A*
        - Minimisation des erreurs
- Différentes méthodes de skeletonization (à comparer)? (Volker)
    - 2D/3D thinning algorithm from Lee
    - Zhang
    - HJS
- Mesures à extraire d'un graphe non-orienté vs. arbre (Clément)
    - Longueur totale
    - Nombre de edges
    - Sholl
    - Profondeur maximale (arbre)
    - Plus long chemin
    - Nombre de vertices & leaves
    - Comptage/mesures/traitement des loops
- Mesure de l'épaisseur des filaments (Volker)

## 5. Exercices postprocessing (35 min)

- Utiliser le résultat de distance map de l'exo d'avant (Volker)
    - Semi-assisted tracing dans Napari
    - Reconstruire les objets
- Comparer métriques finales entre skeletons obtenus par distance map vs. mask (Clément)
- Mesure des épaisseurs (sur d'autres images) (Volker)
    - Tester en 2D/3D

## 6. Summary & outlook

- Évoquer les cas complexes (e.g. neurones encheuvétrés) (à voir)
