# Scanner Colis & QR — Diagnostic mobile et scroll

| Rubrique | Valeur |
| --- | --- |
| **Fichier analysé** | `SOURCES_HTML_ORIGINALES/scanner_colis_qr.html` (original, non modifié) |
| **Version corrigée produite** | `VERSIONS_CORRIGEES/scanner_colis_qr_mobile.html` |
| **Nature du travail** | Analyse statique du code + corrections de mise en page / défilement / tactile |
| **Limite** | **Aucun navigateur, aucun appareil réel n'a été utilisé.** Les constats sont déduits du code (calcul de hauteurs, `overflow`, unités). Aucune validation visuelle ni test sur téléphone n'a été effectué. |

---

## 1. Verdict

Le fichier **ne défile pas et ne fonctionne pas sur mobile tel quel**. Deux blocages :

1. Le défilement est **explicitement supprimé** (`overflow-hidden` sur `<html>` **et** sur le conteneur), avec une hauteur **figée à 844 px**.
2. Aucun réglage de **zones de sécurité** (`env(safe-area-inset-*)`), alors que le page simule une barre d'état iOS et un indicateur d'accueil.

Conséquence : sur tout écran plus bas que 844 px, le bas de la page est **coupé et inaccessible**.

---

## 2. Blocages de défilement

### 2.1 Hauteur figée `h-[844px]`

**Code :** `<div class="relative w-full max-w-[390px] h-[844px] … overflow-hidden …">`

Le conteneur a une hauteur fixe. Les trois zones (header / main / footer) sont réparties par `flex flex-col justify-between` à l'intérieur de cette hauteur.

**Effet :** la page fait **exactement 844 px de haut, quoi qu'il passe sur l'appareil**. Il n'existe aucun mécanisme d'adaptation.

### 2.2 Défilement neutralisé à deux niveaux

| Élément | Classe | Effet |
| --- | --- | --- |
| `<html>` | `class="h-full"` + `overflow-hidden` (sur `<body>`) | Pas de scroll racine |
| `<body>` | `overflow-hidden` | Aucun défilement |
| Conteneur | `overflow-hidden` | Contenu plus grand que 844 px **coupé, non accessible** |

### 2.3 Hauteur réelle du contenu

Estimation des blocs (d'après les classes du fichier) :

| Zone | Estimation |
| --- | --- |
| En-tête (barre d'état + barre nav + onglets) | ≈ 175 px |
| Zone centrale (instruction + viseur 270 + pastille débordante + actions) | ≈ 415 px |
| Zone basse (carte colis + bouton + ligne confiance + indicateur) | ≈ 200 px |
| **Total** | **≈ 790 à 810 px** |

Le total tient dans 844 px **sur un iPhone 13/14/15 de grande hauteur**. Mais sur les cas suivants, il n'y tient pas :

| Appareil | Hauteur utile (approx.) | Effet sur la page |
| --- | --- | --- |
| iPhone SE (2e/3e gén.) | 667 px | Footer coupé, bouton principal inaccessible |
| iPhone 13 mini | 812 px | Marge nulle, footer partiellement coupé |
| Navigateur mobile (Chrome/Safari) | hauteur réduite de 60 à 130 px par la barre d'adresse | Footer coupé dès la plupart des modèles |
| Mode paysage sur téléphone | 375 à 430 px | Presque tout est coupé |

Le bug le plus probable en usage réel : **ouvrir la page dans un navigateur mobile** réduit la hauteur utile sous 844 px → le bouton « Saisir le code secret / OTP de remise » et le lien « Aide » deviennent inatteignables.

### 2.4 Le `main` ne peut pas déborder proprement

`<main class="relative z-20 flex-1 flex flex-col items-center justify-center px-6">`

`flex-1` sans `min-h-0` : dans un conteneur flex, un enfant peut refuser de descendre sous sa taille de contenu. Combiné au `overflow-hidden` du parent, tout excédent est **rogné sans possibility de défilement**.

---

## 3. Adaptations au téléphone absentes

### 3.1 Zones de sécurité

| Élément | Attendu | Présent |
| --- | --- | --- |
| `<meta viewport>` | `viewport-fit=cover` + `env(safe-area-inset-*)` | `width=device-width, initial-scale=1.0` **seulement** |
| Marge haute | `env(safe-area-inset-top)` (encoche, statut bar) | `pt-3` (12 px fixes) |
| Marge basse | `env(safe-area-inset-bottom)` (barre d gestes iOS, ~34 px) | `pb-6` (24 px fixes) |
| Marges latérales | `env(safe-area-inset-left/right)` en paysage | `px-5` fixes |

Sur un iPhone à encoche, la **barre d'état simulée passe sous l'encoche réelle**, et le bas de page est sous la barre de gestes. Le fond sombre masque visuellement une partie du problème, mais le texte reste trop haut / trop bas.

### 3.2 Barre d'état et indicateur d'accueil simulés

Le fichier dessine une barre d'état iOS (`9:41`, `signal`, `wifi`, batterie) **et** un indicateur d'accueil. Sur un vrai téléphone, ces éléments existent déjà : l'utilisateur voit **deux barres d'état**.

Ce n'est pas une erreur de scroll, mais c'est un défaut mobile net. Le mode le plus simple est de les supprimer ; la version corrigée les masque automatiquement sur écran tactile étroit via une media query, pour préserver l'aperçu sur desktop.

### 3.3 Largeur du viseur non adaptative

**Code :** `w-[270px] h-[270px]` — dimensions fixes.

Disponible sur un écran de 320 px : `320 − 48` (padding `px-6`) = **272 px**. Le viseur de 270 px passe, sans aucune marge.

Sur 280 px (certains appareils Android en sur-small) ou avec une barre de navigateur verticale latérale : **débordement horizontal**, coupe du viseur et des coins de cadrage.

---

## 4. Ergonomie tactile

### 4.1 Zones cliquables trop petites

| Élément | Taille actuelle | Correction mobile usuelle |
| --- | --- | --- |
| Bouton retour | `w-10 h-10` = **40 × 40 px** | 44 × 44 px |
| Bouton torche | `w-10 h-10` = **40 × 40 px** | 44 × 44 px |
| Bouton son | `w-9 h-9` = **36 × 36 px** | 44 × 44 px |
| Onglet « Remise colis » | `py-2` + `text-xs` ≈ **32 px** de haut | 44 px de haut |
| Onglet « QR Voyageur » | idem ≈ **32 px** | 44 px |
| Onglet « Scellé IATA » | idem ≈ **32 px** | 44 px |
| Lien « Aide » | `text-[11px]` sans padding ≈ **15 px** de haut | ≥ 44 px avec zone cliquable élargie |

7 éléments sur 9 sont sous la zone tactile recommandée. Les onglets de mode sont les plus pénalisants : ce sont des cibles principales et les plus proches.

### 4.2 Texte trop petit

| Taille | Occurrences |
| --- | --- |
| `text-[10px]` | 4 (badge `AI Live`, badge `En attente`, `5 kg scellé`) |
| `text-[11px]` | 3 (pastille « Capteur prêt », trajet, ligne de confiance) |

`10 px` sur un téléphone à densité 3× est minuscule. Seuil courant retenu : **12 px minimum** pour du texte d'interface secondaire.

### 4.3 `select-none` global

**Code :** `<body class="… select-none …">` et `user-select: none` appliqué à **tout le document**.

Conséquence concrète : la sélection et le **copier-coller** sont bloqués partout. Or l'action principale de cet écran est la **saisie d'un code secret / OTP** — un champ de saisie à ce stade future devra autoriser la sélection, et le collage du code sera impossible. Bloquer au niveau du `<body>` est trop large.

### 4.4 Pas de `touch-action`

Pas de `touch-action: manipulation` : les boutons conservent un délai de ~300 ms sur certains navigateurs mobiles, et le double-tap peut déclencher un zoom.

---

## 5. Débordements et coupures possibles

| # | Élément | Problème |
| --- | --- | --- |
| 1 | Pastille « Capteur prêt • Autofocus actif » (`whitespace-nowrap`, `text-[11px]`) | `absolute -bottom-4 left-1/2 -translate-x-1/2` + `whitespace-nowrap` : largeur imposée par le texte, **ne se rétrécit jamais**. Sur un écran étroit elle dépasse du viseur et peut sortir du cadre. |
| 2 | Badge titre « Scanner Zecolis » + pastille + « AI Live » | En-tête `justify-between` avec 3 blocs dont un texte `uppercase` non tronqué : sur écran étroit, le badge peut pousser le bouton torche hors du conteneur. |
| 3 | Carte colis (`#COL-7852`, `En attente`) + montant `7 500 F` | `justify-between` sans `min-w-0` : les blocs texte ne peuvent pas rétrécir sous leur taille intrinsèque, risque de chevauchement avec le bloc montant. |
| 4 | Badge `AI Live` en `flex-none` implicite | Aucune contrainte : il ne peut pas céder de place. |
| 5 | Fond `absolute inset-0` | Si la page devient plus haute que le viewport, le fond image **ne couvre plus** la zone supplémentaire et laisse une bande unie. |

---

## 6. Corrections appliquées dans `scanner_colis_qr_mobile.html`

**Périmètre strictement limité à la mise en page, au défilement et au tactile. Aucun texte, aucune donnée, aucun bouton, aucune icône, aucun comportement métier n'a été ajouté, supprimé ou modifié.**

### 6.1 Défilement

| Correction | Avant | Après |
| --- | --- | --- |
| Hauteur du conteneur | `h-[844px]` fixe | `min-height: 100vh` puis `min-height: 100dvh` (`.app-shell`) |
| Verrouillage racine | `overflow-hidden` sur `<body>` | `overflow-x-hidden` — **le scroll vertical est rétabli** |
| Verrouillage conteneur | `overflow-hidden` | supprimé |
| Débordement `main` | `flex-1` seul | `flex-1 min-h-0` — autorise la réduction correcte en flex |
| Repère `100dvh` | — | `dvh` tient compte de la barre d'adresse mobile qui se masque à chaque défilement |

### 6.2 Zones de sécurité

| Correction | Détail |
| --- | --- |
| `<meta viewport>` | ajout de `viewport-fit=cover` |
| Variables CSS | `--sat`, `--sar`, `--sab`, `--sal` = `env(safe-area-inset-*, 0px)` |
| En-tête | `pt-[calc(0.75rem+var(--sat))]` |
| Zone basse | `padding-bottom: calc(1.5rem + var(--sab))` |
| Marges latérales | `padding-left/right: calc(1.25rem + var(--sal/sar))` |

### 6.3 Adaptatif

| Correction | Avant | Après |
| --- | --- | --- |
| Largeur du conteneur | `max-w-[390px]` | `max-w-[430px]` — couvre les grands téléphones tout en gardant un aperçu correct sur desktop |
| Viseur | `w-[270px] h-[270px]` fixe | `width: min(270px, 68vw)` + `aspect-ratio: 1/1` — se réduit proprement, jamais de débordement |
| Silhouette QR dans le viseur | `w-36 h-36` fixe | `w-1/2 h-1/2` — suit la taille du viseur |
| Paysage | aucun traitement | `@media (orientation: landscape) and (max-height: 520px)` → viseur réduit à `min(180px, 34vw)` |
| Fond d'image | `absolute inset-0` | `position: fixed` — couvre toute la hauteur même en cas de scroll |
| Pastille détecteur | `whitespace-nowrap` | `max-width: min(90vw, 330px)`, `text-center`, repli sur 2 lignes |
| Badge titre / montants / trajet | texte non tronqué | `min-w-0` + `truncate` + `flex-none` sur les éléments à ne pas comprimer |
| Barre d'état + indicateur d'accueil | toujours affichés | masqués sur `pointer: coarse` et `max-width: 430px` (vrais téléphones), conservés sur desktop pour l'aperçu |
| `-webkit-text-size-adjust` | — | `100%` ajouté : empêche l'inflation automatique de police sur iOS en rotation |

### 6.4 Tactile

| Correction | Avant | Après |
| --- | --- | --- |
| Retour, torche, son | 40 / 40 / 36 px | **44 / 44 / 44 px** (`.icon-btn`) |
| Onglets de mode | ≈ 32 px | `min-height: 44px` + `gap-1.5` + `px-2` (`.mode-tab`) |
| Bouton principal | `py-3.5` | `min-height: 44px` garanti |
| Lien « Aide » | ≈ 15 px | `min-height: 44px` avec `px-2 -my-2` (zone cliquable élargie sans changer l'alignement) |
| `touch-action` | absent | `.btn { touch-action: manipulation }` sur tous les boutons |
| `select-none` | sur le `<body>` | **retiré du `<body>`**, appliqué uniquement aux éléments d'interface (`.select-none`) — la saisie future du code OTP reste utilisable |
| `-webkit-tap-highlight-color` | sur `body` | sur `*` (tous les éléments) |
| Actions rapides | `flex` non repliable | `flex-wrap` + `justify-center` — passe proprement sur 2 lignes si l'écran est étroit |

### 6.5 Accessibilité

Ajout de `aria-label` sur les 4 boutons sans texte : `Retour`, `Lampe torche`, `Son`. Le bouton « Aide » et « Importer une photo » portent déjà un texte visible. Le bouton torche, seul élément avec comportement, conserve son `id="torchBtn"` et son script **inchangés**.

### 6.6 Classe Tailwind inexistante

| Correction | Avant | Après |
| --- | --- | --- |
| Jauge de batterie du tiroir | `rounded-2xs` — **classe inexistante en Tailwind v3** (nom introduit en v4), donc sans effet : la jauge héritait du rayon du parent | `rounded-sm` |

Portée : correction de rendu uniquement, aucune conséquence sur la mise en page ni sur le comportement. Le même défaut a été traité dans `03_RECHERCHE_CORRECTIONS_MOBILE.md` pour `rounded-2xs` et `shadow-2xs`.

---

## 7. Écarts de design : NON corrigés

Les écarts avec la référence visuelle § 4 (police Plus Jakarta Sans, thème sombre `#050B14`, glassmorphism `.glass-pill`, dégradés orange→vert, statut `En attente` en vert) sont **conservés à l'identique** dans la version corrigée.

Raison : ce sont des **choix de design à arbitrer**, pas des bugs. Les corriger sans décision de votre part reviendrait à modifier la direction visuelle du projet en silence. Voir `04_SCAN_COLIS_QR.md` § 15.

---

## 8. Points restant à traiter (hors périmètre de cette correction)

| # | Point | Nature |
| --- | --- | --- |
| 1 | Les 3 onglets de mode ne changent pas d'état | Fonctionnel — § 10 du document de page |
| 2 | « Importer une photo » sans `input type="file"` | Fonctionnel |
| 3 | Aucune caméra réelle (pas de `getUserMedia`) | Fonctionnel |
| 4 | Aucune saisie OTP possible | Fonctionnel |
| 5 | « Aide » sans gestionnaire | Fonctionnel |
| 6 | Retour `href="#"` | Fonctionnel |
| 7 | Image de fond distante (URL Google, non maîtrisée) | Fragilité réseau — à remplacer par un asset local |
| 8 | `lucide@latest` non épinglé | Fragilité — à passer en version fixe |
| 9 | Aucun état : chargement, erreur, code non lu, permission caméra refusée | Fonctionnel |
| 10 | Mode paysage / tablette / écran pliant non traités au-delà du minimal | À compléter si cibles concernées |

---

## 9. Fichiers

| Fichier | Contenu |
| --- | --- |
| `SOURCES_HTML_ORIGINALES/scanner_colis_qr.html` | **Original, non modifié** |
| `VERSIONS_CORRIGEES/scanner_colis_qr_mobile.html` | Version corrigée — **scroll + mobile uniquement** |

Les deux fichiers sont distincts. Aucun original n'a été écrasé.