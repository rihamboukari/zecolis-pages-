# Assets images — `VERSIONS_CORRIGEES/img/`

## Pourquoi ce dossier existe

Les maquettes d'origine référenceaient leurs images sur
`https://lh3.googleusercontent.com/aida/...` (export Gemini Canvas).

**7 de ces URL sont revenues en HTTP 403** — Google les a révoquées. Elles sont
mortes **dans les fichiers originaux également** : ce n'est pas une conséquence
des corrections mobiles. Testé le 03/10/2026, sans suffixe de taille, avec
`Referer`, et sur les deux répertoires (`/aida/` et `/aida-public/`) : 403 dans
tous les cas.

Toutes les images ont donc été **téléchargées localement** le 03/10/2026. Les
11 versions mobiles ne dépendent plus d'aucun serveur d'images.

## Fichiers réels (téléchargés depuis l'export, intacts)

| Fichier | Provenance |
| --- | --- |
| `logo.png` | Logo Zecolis (7 URL identiques, dédupliquées) |
| `hero-accueil.jpg` | Illustration d'échange de colis, page d'accueil |
| `avatar-profil-accueil.jpg` | Avatar de l'en-tête de l'accueil |
| `avatar-accueil-1.jpg` `avatar-accueil-2.jpg` `avatar-accueil-3.jpg` | Avatars des 3 cartes de l'accueil |
| `onboarding-1-expediteur.jpg` | Photo plein écran, étape 1 |
| `onboarding-2-voyageur.jpg` | Photo plein écran, étape 2 |
| `onboarding-3-confiance.jpg` | Photo plein écran, étape 3 |
| `scanner-fond.jpg` | Arrière-plan du scanner (flouté à 35 % d'opacité) |

## Fichiers de substitution (SVG) — à remplacer par vos vraies photos

Ces 7 SVG sont des **placeholders honnêtes** : initiales ou icône sur aplat, pas
de fausse photographie. Ils ont été générés parce que les URL d'origine sont
définitivement mortes et que le contenu n'est pas récupérable.

| Fichier | Remplace | Écran |
| --- | --- | --- |
| `avatar-profil.svg` | Avatar « Mon profil » | Recherche |
| `avatar-koffi.svg` | Avatar de Koffi D. | Recherche |
| `avatar-amina.svg` | Avatar d'Amina S. | Recherche |
| `avatar-colis.svg` | Avatar de la carte « Colis en attente » | Recherche |
| `avatar-riham.svg` | Avatar de Riham K. | Publier un voyage |
| `avatar-mariam.svg` | Avatar de Mariam K. | Publier un colis |
| `photo-colis-exemple.svg` | Photo d'exemple du colis | Publier un colis |

**Pour mettre vos vraies photos :** remplacez ces fichiers **en conservant
exactement le même nom** (ou le même format SVG si vous préférez.vectoriser).
Aucun code HTML n'a alors besoin d'être touché.

Si vous préférez fournir des `.jpg`,rezi ze nommez le fichier en `.jpg` et
mettez à jour l'unique attribut `src` concerné : le chemin est relatif au
fichier HTML (`img/...`), donc aucun chemin absolu ne casse le portability.