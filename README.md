# Zecolis — maquettes mobiles et documentation

Maquettes d'interface d'une application de logistique transfrontalière Afrique subsaharienne ↔ Europe
(transport de colis et de voyageurs), et leur documentation technique.

Projet **maquette / prototype statique**. Aucun backend, aucune authentification réelle,
aucun appel réseau : toutes les actions sont simulées et annoncées comme telles dans l'interface.

## Contenu

| Chemin | Rôle |
| --- | --- |
| `ZECOLIS_DOCUMENTATION/SOURCES_HTML_ORIGINALES/` | Les 11 HTML reçus, **jamais modifiés** |
| `ZECOLIS_DOCUMENTATION/VERSIONS_CORRIGEES/` | Les 11 versions mobiles corrigées, avec `img/` |
| `ZECOLIS_DOCUMENTATION/00_INDEX_GENERAL.md` | Index d'ensemble : inventaire, arborescence, graphe de navigation |
| `ZECOLIS_DOCUMENTATION/01_PRESENTATION_PROJET.md` | Présentation du projet |
| `ZECOLIS_DOCUMENTATION/09_POINTS_A_VERIFIER.md` | Registre des écarts, points à arbitrer, validations restantes |
| `ZECOLIS_DOCUMENTATION/0*_*/` | Analyse et corrections, écran par écran |
| `ZECOLIS_DOCUMENTATION/ASSETS/images/` | 8 rendus d'écrans Stitch (`DESIGN.md` + `screen.png` issus de 8 archives) |
| `ZECOLIS_DOCUMENTATION/ASSETS/photos/` | 8 photographies ajoutées le 03/10/2026 |
| `ZECOLIS_DOCUMENTATION/ASSETS/INDEX.html` | Planche de contact des 8 rendus Stitch |
| `ZECOLIS_DOCUMENTATION/VERSIONS_CORRIGEES/img/` | 17 assets locaux utilisés par les versions mobiles |

## Les 11 écrans

Accès : `accueil`, `connexion`, `inscription`, `mot_de_passe_oublie` ·
Onboarding : `onboarding_expediteur`, `onboarding_voyageur`, `onboarding_confiance` ·
Métier : `recherche`, `publier_voyage`, `publier_colis`, `scanner_colis_qr`

## Ouvrir les maquettes

Ouvrir directement `VERSIONS_CORRIGEES/*.html` dans un navigateur — aucune compilation,
aucun serveur. Pour suivre la navigation entre écrans, lancer un petit serveur local :

```bash
cd ZECOLIS_DOCUMENTATION/VERSIONS_CORRIGEES
python -m http.server 8000
# puis http://localhost:8000
```

Les chemins d'images sont relatifs (`img/…`) et fonctionnent aussi en `file://`.

## Corrections appliquées aux versions mobiles

- liens câblés entre les 11 écrans (plus aucune ancre morte) ; les écrans non livrés renvoient un statut explicite ;
- défilement vertical rétabli, hauteur `100dvh`, safe areas réellement appliquées ;
- zoom et pincement réautorisés (`user-scalable=no` retiré), cibles tactiles à 44 px, `touch-action: manipulation` ;
- `prefers-reduced-motion`, retours d'état `role="status"` / `aria-live` / `aria-invalid`, gestion du focus ;
- classes Tailwind inexistantes remplacées ; le zoom automatique iOS évité (champs à 16 px) ;
- fausses réussites supprimées : aucune confirmation de publication, de connexion ni de compte créé ;
- **images localisées** : les 24 images référençaient `lh3.googleusercontent.com`, dont **7 URL sont revenues en HTTP 403** (cassées dans les fichiers d'origine également). Toutes ont été téléchargées dans `img/`.

## État connu

- `09_POINTS_A_VERIFIER.md` est **incomplet** : tronqué lors d'une manipulation de fichier. À régénérer.
- 7 avatars restent des **SVG de substitution** (initiales / icône) : les photos correspondantes sont arrivées dans `ASSETS/photos/` mais ne sont pas encore câblées. Voir `VERSIONS_CORRIGEES/img/LISEZ-MOI.md`.
- `ASSETS/photos/editorial-haut-de-gamme-02-DOUBLON-identique.png` est un doublon octet pour octet, à supprimer.
- Deux systèmes de couleurs coexistent : la palette marine / orange de l'application et la palette Material Polytechnic des rendus Stitch. Arbitrage en attente.

## Licence

Code et documents réservés au projet Zecolis.