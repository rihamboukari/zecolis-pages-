# Photos — apport du 03/10/2026, 21 h 07 à 21 h 19

8 fichiers PNG déposés à la racine de `ZECOLIS_DOCUMENTATION/`, renommés et regroupés ici.
Les fichiers d'origine portaient le nom complet du prompt de génération
(`authentic_close_up_documentary_portrait_headshot_of_…`), trop long et directement
dérivé du générateur d'images.

## Fichiers

| Nouveau nom | Origine (nom de prompt) | Dim. | Poids |
| --- | --- | --- | --- |
| `portrait-jeune-ouest-elegant_1024x1024.png` | `…portrait_headshot_of_a_stylish_young_west` | 1024 × 1024 | 1 589 Ko |
| `portrait-professionnel-aimable_1024x1024.png` | `…portrait_headshot_of_a_friendly_professional` | 1024 × 1024 | 1 559 Ko |
| `portrait-smart-casual-aimable_1024x1024.png` | `…portrait_headshot_of_a_friendly_smart_casual` | 1024 × 1024 | 1 514 Ko |
| `portrait-professionnel-ouest-africain-elegant_1024x1024.png` | `…portrait_headshot_of_a_stylish_professional_west_african` | 1024 × 1024 | 1 462 Ko |
| `editorial-haut-de-gamme-01_768x1376.png` | `authentic_high_end_documentary_editorial_photograph…` | 768 × 1376 | 1 441 Ko |
| `editorial-haut-de-gamme-02_768x1376.png` | idem + `(1)` | 768 × 1376 | 1 304 Ko |
| `editorial-haut-de-gamme-02-DOUBLON-identique.png` | idem + `(2)` | 768 × 1376 | 1 304 Ko |
| `plis-textiles-traditionnels-et-modernes_1200x896.png` | `…photograph_of_neatly_folded_traditional_and_modern` | 1200 × 896 | 1 465 Ko |

## Doublon à supprimer

`editorial-haut-de-gamme-02_768x1376.png` et `editorial-haut-de-gamme-02-DOUBLON-identique.png`
sont **strictement identiques** : SHA-256 `FABB7372E594E951…`, 1 335 266 octets chacun.

Le fichier `-DOUBLON-identique` a été conservé plutôt que supprimé, pour ne rien
détruire sans votre accord. **Il peut être effacé sans risque** — cela fera 1,3 Mo de gain.

## Relevé colorimétrique

Mesuré par décodage PNG natif, ~14 400 pixels par image.

| Fichier | Moyenne | % sombre | % clair | Dominante |
| --- | --- | --- | --- | --- |
| `plis-textiles-…` | (126, 111, 94) | 17 % | 7 % | `#604020` |
| `portrait-jeune-ouest-elegant` | (132, 113, 92) | 14 % | 3 % | `#806040` |
| `portrait-professionnel-aimable` | (143, 117, 101) | 21 % | 14 % | `#E0E0E0` |
| `portrait-smart-casual-aimable` | (141, 126, 107) | 12 % | 11 % | `#E0E0E0` |
| `portrait-professionnel-ouest-africain-elegant` | (128, 116, 103) | 22 % | 11 % | `#000000` |
| `editorial-haut-de-gamme-01` | (138, 130, 121) | 17 % | 13 % | `#000000` |
| `editorial-haut-de-gamme-02` | (153, 140, 124) | 23 % | 19 % | `#000000` |

Les 8 images forment un **set cohérent** : photographie documentaire, tons chauds,
**aucune couleur de marque Zecolis** (0 % d'orange `#FF6B00` sur les 8).

## Ce que ces formats permettent

Le point important : les dimensions collent exactement aux emplacements qui manquent.

| Format | Nombre | Emplacements compatibles dans les versions mobiles |
| --- | --- | --- |
| **1024 × 1024** (carré) | 4 portraits | Avatars circulaires ou arrondis — 5 emplacements humains sont concernés |
| **768 × 1376** (9:16) | 2 distinctes | Photos plein écran des 3 onboarding, dont les images actuelles sont en 1080 × 1920 (9:16 également) |
| **1200 × 896** (4:3) | 1 | Photo d'exemple du colis, vignette 96 × 96 en `object-cover` |

## Réserves

**1. Aucun portrait n'est encore attribué à un personnage.** Les 4 portraits peuvent
remplacer les placeholders `avatar-profil.svg`, `avatar-koffi.svg`, `avatar-amina.svg`,
`avatar-riham.svg`, `avatar-mariam.svg` — mais il y a **5 emplacements humains et
4 photos**. Attribuer une photo à un personnage nommé sans les avoir vues risque de
produire une incohérence (genre, âge apparent, origine). **À valider à l'œil.**

**2. Il manque 1 portrait.** Sur les 5 avatars à pourvoir, un restera un placeholder SVG.

**3. Les 3 photos éditoriales ne sont pas attribuées.** Leur format 9:16 correspond aux
onboarding, mais rien ne le prouve : elles pourraient aussi servir d'illustration d'accueil
ou de fond de scanner.

**4. `editorial-haut-de-gamme-02-DOUBLON-identique.png` est à effacer.**

## Prochaine étape

Les fichiers ne sont **pas encore câblés** dans `VERSIONS_CORRIGEES/` : les versions mobiles
continuent d'utiliser les placeholders SVG et les anciennes photos. Dites-moi quelle photo
va où et je mets à jour les 11 fichiers HTML.