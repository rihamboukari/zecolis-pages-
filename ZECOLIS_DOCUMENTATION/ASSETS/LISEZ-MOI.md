# ASSETS — ressources visuelles extraites

## Contenu

| Élément | Origine |
| --- | --- |
| `images/stitch-maquette-01…08_*.png` | `screen.png` extrait des 8 archives `stitch_zecolis_mobile_splash_screen*.zip` |
| `systeme-design-stitch.md` | `DESIGN.md`, **identique dans les 8 archives** (une seule copie conservée) |
| `INDEX.html` | Planche de contact : ouvrez-la pour voir les 8 images et les identifier |

## Ce que contenaient les archives

Chacune des 8 archives ne contenait **que 2 entrées** :

| Entrée | Détail |
| --- | --- |
| `DESIGN.md` | 9 947 o — identique dans les 8 (SHA-256 unique) |
| `screen.png` | Rendu d'écran, 782 Ko à 1,6 Mo |

Aucune feuille de style, aucun fichier HTML, aucune photo séparée. **Ces images sont
des rendus d'écrans, pas les photographies utilisées dans le code.** Elles ne peuvent donc
pas servir à remplacer les 7 images mortes du dossier `VERSIONS_CORRIGEES/img/`.

## Pourquoi les fichiers sont numérotés

Les 8 archives portent le même nom de base (`stitch_zecolis_mobile_splash_screen`) et aucune
d'entre elles ne contient d'identifiant d'écran :

- pas de nom d'écran dans le nom de l'archive ;
- `DESIGN.md` ne décrit qu'un **système de design** (palette Material Polytechnic, typographie), pas l'écran ;
- aucun chunk texte `tEXt`/`iTXt` dans les PNG ;
- aucun outil OCR ni vision disponible dans cet environnement pour lire le texte des rendus.

Les noms reprennent donc **l'ordre des archives**, complété des dimensions réelles vérifiées
en lisant l'en-tête `IHDR` de chaque PNG. Pour les renommer en noms d'écrans, il faut ouvrir
`INDEX.html` et me dire à quoi correspond chaque image.

## Correspondance archive → fichier

| # | Archive | Fichier extrait | Dimensions | Poids |
| --- | --- | --- | --- | --- |
| 01 | `stitch_zecolis_mobile_splash_screen (1).zip` | `stitch-maquette-01_1024x1024.png` | 1024 × 1024 | 782 165 o |
| 02 | `stitch_zecolis_mobile_splash_screen (2).zip` | `stitch-maquette-02_1200x896.png` | 1200 × 896 | 1 447 245 o |
| 03 | `stitch_zecolis_mobile_splash_screen (3).zip` | `stitch-maquette-03_1024x1024.png` | 1024 × 1024 | 1 480 877 o |
| 04 | `stitch_zecolis_mobile_splash_screen (4).zip` | `stitch-maquette-04_1200x896.png` | 1200 × 896 | 1 553 954 o |
| 05 | `stitch_zecolis_mobile_splash_screen (5).zip` | `stitch-maquette-05_1024x1024.png` | 1024 × 1024 | 1 520 348 o |
| 06 | `stitch_zecolis_mobile_splash_screen (6).zip` | `stitch-maquette-06_1200x896.png` | 1200 × 896 | 1 655 083 o |
| 07 | `stitch_zecolis_mobile_splash_screen (7).zip` | `stitch-maquette-07_1264x848.png` | 1264 × 848 | 1 508 585 o |
| 08 | `stitch_zecolis_mobile_splash_screen.zip` *(sans numéro)* | `stitch-maquette-08_1024x1024.png` | 1024 × 1024 | 825 946 o |

Les 8 poids correspondent **exactement** aux entrées `screen.png` des archives : l'extraction
est complète et non corrompue.

## Relevé colorimétrique

Mesuré par décodage PNG natif (zlib), sur un échantillon de ~14 400 pixels par image.

| # | Couleur moyenne | % sombre | % clair | % orange `#FF6B00` | Dominante |
| --- | --- | --- | --- | --- | --- |
| 01 | (216, 225, 234) | 2 % | 89 % | 0 % | `#E0E0E0` |
| 02 | (121, 109, 100) | 30 % | 4 % | 0 % | `#000000` |
| 03 | (128, 117, 104) | 25 % | 14 % | 0 % | `#000000` |
| 04 | (130, 112, 95) | 11 % | 5 % | 0 % | `#604020` |
| 05 | (104, 91, 84) | 32 % | 8 % | 0 % | `#000000` |
| 06 | (136, 117, 92) | 22 % | 12 % | 2 % | `#000000` |
| 07 | (153, 153, 145) | 12 % | 21 % | 0 % | `#E0E0E0` |
| 08 | (239, 222, 206) | 0 % | 76 % | 0 % | `#E0E0E0` |

**Repères utiles :** 01 et 08 sont les deux maquettes claires ; 02 à 07 sont à dominante sombre.
La 07 est la seule au format 1264 × 848, les 7 autres sont en 1024 × 1024 ou 1200 × 896.

## Point d'arbitrage ouvert

Ces rendus suivent le système de couleurs fourni dans `DESIGN.md`
(`primary: #000000`, `secondary: #ab3500`, `background: #f8f9ff`), c'est-à-dire une palette
**Material Polytechnic**. Les maquettes applicatives documentées dans
`VERSIONS_CORRIGEES/` utilisent une palette **marine / orange** (`#0B2A5B` / `#FF6B00`).

La présence d'orange Zecolis est quasi nulle dans les 8 rendus (0 %, sauf 2 % sur la 06).
**Ce sont deux systèmes visuels différents.** Il faut trancher lequel fait référence avant
de poursuivre la documentation, sinon les maquettes Stitch et les écrans applicatifs ne
donneront pas la même interface.

## À faire

1. Ouvrir `INDEX.html` et identifier les 8 écrans.
2. Me transmettre la correspondance demandedécran → numéro, pour que je renomme les fichiers.
3. Trancher entre la palette Material Polytechnic et la palette marine / orange.