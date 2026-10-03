# 02 — Onboarding (écrans 1, 2 et 3)

| Rubrique | Valeur |
| --- | --- |
| **Fichiers analysés** | `SOURCES_HTML_ORIGINALES/onboarding_expediteur_1.html` (11 514 octets, 246 lignes) · `SOURCES_HTML_ORIGINALES/onboarding_voyageur_2.html` (13 024 octets, 238 lignes) · `SOURCES_HTML_ORIGINALES/onboarding_confiance_3.html` (6 728 octets, 99 lignes) — **originaux non modifiés** |
| **Nature du travail** | Analyse statique du code uniquement |
| **Limite** | **Aucun navigateur, aucun appareil réel.** Tous les constats sont déduits du code. |
| **Corrections mobile** | `02_NAVIGATION_ET_ACCUEIL/02_ONBOARDING_CORRECTIONS_MOBILE.md` |

> **Noms de fichiers provisoires.** Le donneur d'ordre n'a communiqué aucun nom d'origine, à l'exception de l'écran 3 (`onboarding_confiance_3.html`). Les deux premières copies sont désignées par leur contenu d'écran.

> **Lot 6 : quatre transmissions pour deux écrans distincts.** L'écran 1 « Expéditeur » a été transmis **deux fois à l'identique** ; un seul original a été archivé.

> **Lot 7 : l'étape 3 a été reçue**, ce qui **comble la séquence 1 → 2 → 3**. L'écran s'intitule « La confiance d'abord. » et ne contient **aucune logique** : 2 boutons, 0 `id`, 0 gestionnaire, 0 lien. Il est analysé ci-dessous en tant qu'**écran 3** ; son nom de fichier d'origine est le seul des trois qui soit connu.

> Le fichier `recherche.html` fait l'objet d'un document séparé : `03_PUBLICATION_ET_ANNONCES/03_RECHERCHE.md`.

---

## 1. Vue d'ensemble

| | Écran 1 — Expéditeur | Écran 2 — Voyageur | Écran 3 — Confiance |
| --- | --- | --- | --- |
| Titre HTML | `ZECOLIS — Onboarding 1/3 · Expéditeur` | `ZECOLIS — Onboarding 2/3 · Voyageur` | `Zecolis - Onboarding Écran 3 (Confiance)` |
| Taille / lignes | 11 514 o / 246 | 13 024 o / 238 | **6 728 o / 99** |
| Message principal | « Envoyez **moins cher.** » | « Voyagez. **Gagnez.** » | « La confiance **d'abord.** » |
| Sous-titre | « Votre colis voyage avec quelqu'un qui part déjà. » | « Votre trajet peut aussi vous rapporter. » | « Des profils vérifiés pour envoyer et voyager sereinement. » |
| Étape affichée | 1 sur 3 | 2 sur 3 | 3 sur 3 |
| Boutons | 2 (Passer, Suivant) | 2 (Passer, Suivant) | 2 (Commencer, Retour) |
| `id` déclarés | **0** | **0** | **0** |
| `addEventListener` | **0** | **0** | **0** |
| Champs de saisie | 0 | 0 | 0 |
| Balises `aria-*` | **0** | 2 (les deux `aria-label` des boutons) | **0** |
| `role` / `tabindex` / `sr-only` | 0 / 0 / 0 | 0 / 0 / 0 | 0 / 0 / 0 |
| Scripts | 2 (CDN Tailwind + `tailwind.config`) | 2 (CDN Tailwind + `tailwind.config`) | 2 (idem) |
| Images | 2 (photo plein cadre + logo) | 1 (photo plein cadre) | 2 (photo plein cadre + logo) |
| SVG | 7 | 8 | 4 |
| `backdrop-filter` / `backdrop-blur` | 4 déclarations CSS (`.glass-pill`, `.glass-pill-dark`) | **5 `backdrop-blur-md`** | 3 `backdrop-blur-md` |
| Dégradés | 3 `bg-gradient-to-*` (2 voiles photo + 1 barre de progression) | 4 (3 voiles photo + 1 bouton) | **2** (les deux voiles photo) |
| Animations permanentes | 1 `animate-pulse` | 1 `animate-pulse` | **0** |
| Polices déclarées | Inter + Plus Jakarta Sans | Plus Jakarta Sans + Inter | Inter |
| Éléments interactifs | **2** | **2** | **2** |
| Éléments Dead (aucun gestionnaire) | **2 sur 2** | **2 sur 2** | **2 sur 2** |

**Constat principal : ces trois écrans ne contiennent aucune logique.** Ce sont des planches de mise en page. Les deux boutons de chaque écran — « Passer » et « Suivant » sur les étapes 1 et 2, « Commencer » et « Retour » sur l'étape 3 — n'ont ni `id`, ni `onclick`, ni gestionnaire : un appui ne produit **aucun effet et aucun message**.

**Sur les trois écrans, 6 éléments interactifs, 6 dead.** L'étape 3 est le plus réduit du lot : 99 lignes, le tiers de la taille des deux autres, et le seul dont le `<body>` porte `overflow-hidden` **sans** contre-balancement `h-[844px]`.

---

## 2. Structure et composition

### 2.1 Écran 1 — Expéditeur

Trois couches empilées dans un conteneur unique :

| Couche | Contenu |
| --- | --- |
| Photo | `absolute inset-0` — image Google Storage, `object-cover` |
| Voiles | 2 dégradés : bandeau haut `#071630/75 → transparent`, puis `#051124/75 → #040d1c` sur les 62 % bas |
| En-tête | barre d'état simulée · pastille logo « ZECOLIS » · bouton « Passer » · barre de progression en 3 segments |
| Pastilles flottantes | trajet `🇧🇯 Cotonou ⇄ Paris 🇫🇷` · `✓ Colis prêt` · `✓ Voyage vérifié` |
| Bas | sur-titre `Envoyer un colis` · `h1` 38–41 px · phrase d'accroche · bouton « Suivant » 56 px · 3 pastilles de pagination · pilule d'accueil `w-32 h-1` |

### 2.2 Écran 2 — Voyageur

Même architecture, contenu différent :

| Couche | Contenu |
| --- | --- |
| Photo | `absolute inset-0` + `overflow-hidden` |
| Voiles | 3 dégradés : bandeau haut, voile latéral gauche→droite, voile bas de 460 px |
| En-tête | barre d'état simulée · logo ZECOLIS en SVG inline · bouton « Passer » · progression 3 segments (1 rempli, 1 actif, 1 vide) |
| Pastilles flottantes | trajet `🇧🇯 Cotonou ⇄ Paris 🇫🇷` · `✓ Paiement reçu` + badge `✓ Instantané` · valise `8 kg disponibles` |
| Bas | sur-titre `Voyageur` · `h1` 34–38 px · accroche · 3 mentions (`Kilos rentabilisés` · `Colis vérifié` · `Paiement garanti`) · bouton « Suivant » 56 px · 3 pastilles de pagination |

### 2.3 Écran 3 — Confiance

**Structure la plus compacte du lot : 99 lignes, tout le HTML est écrit en 4 blocs séparés par des commentaires.**

| Couche | Contenu |
| --- | --- |
| Photo | `absolute inset-0` + `object-cover object-center`, **sans** `overflow-hidden` sur un conteneur de hauteur fixe |
| Voiles | 2 dégradés seulement : `bg-gradient-to-b` (`from-black/40 via-transparent to-[#051329]/95 via-45%`) et `bg-gradient-to-t` (`from-[#051329] via-[#051329]/80 to-transparent via-40%`) |
| En-tête | barre d'état simulée (heure `9:41`, 2 SVG de signal, jauge de batterie dessinée en div) · logo ZECOLIS en pastille `bg-black/35 backdrop-blur-md` · pastille `CONFIANCE` + pagination 3 pastilles (2 grises, 1 orange étirée) |
| Pastilles flottantes | trajet `🇧🇯 Cotonou ⇄ France 🇫🇷` · **`✓ Identité vérifiée`** · **`★ 4,9 Voyageur vérifié`** |
| Bas | `h1` 34 px sur 2 lignes (« La confiance » / « d'abord. » en orange) · une phrase · mention `Identité · Avis · Échanges` · bouton **« Commencer »** 56 px · bouton **« Retour »** · barre d'accueil `w-32 h-1` |

**Différences de structure notables :**

| Point | Écrans 1 et 2 | Écran 3 |
| --- | --- | --- |
| Hauteur | conteneur `h-[844px] max-h-screen` | **aucun** : c'est le `<body>` qui porte `h-full overflow-hidden` |
| Barre de progression | segments `<div>` rectangulaires | pastilles `<span>` circulaires (`w-1.5` / `w-4`) |
| Bouton principal | « Suivant » | **« Commencer »** — l'onboarding se termine ici |
| Bouton secondaire | « Passer » (sortie anticipée) | **« Retour »** (retour à l'étape 2) |
| Pagination basse | 3 pastilles dans le `<footer>` | **doublée** : pastilles dans l'en-tête **et** dans le `<footer>` |

### 2.4 Les trois écrans forment bien une séquence complète

Les indicateurs (« 1/3 », « 2/3 », « CONFIANCE », 3 segments, 3 pastilles) confirment un parcours en **3 étapes**. **Les trois étapes ont désormais été transmises** (étape 3 au lot 7) : la collection forme une série complète et cohérente.

**Ordre et libellés constatés :**

| Étape | Archivo | Intitulé dans l'écran | Bouton de sortie |
| --- | --- | --- | --- |
| 1/3 | `onboarding_expediteur_1.html` | `Expéditeur` | « Suivant » |
| 2/3 | `onboarding_voyageur_2.html` | `Voyageur` | « Suivant » |
| 3/3 | `onboarding_confiance_3.html` | `CONFIANCE` | **« Commencer »** |

---

## 3. Défauts mobile

### 3.1 Hauteur figée et défilement bloqué — le défaut le plus grave

**Écran 1 :**

```html
<div class="relative w-full max-w-[390px] h-[844px] max-h-screen overflow-hidden …">
```

```css
body { … overflow: hidden; user-select: none; }
```

**Écran 2 :**

```html
<body class="… h-full … overflow-hidden">
<main class="relative w-full max-w-[390px] h-[844px] max-h-screen overflow-hidden … select-none">
```

**Écran 3 :**

```html
<body class="bg-black text-white antialiased h-full w-full overflow-hidden flex flex-col justify-between select-none relative">
```

Cas particulier : l'écran 3 **n'a pas** de conteneur `h-[844px]`. Le blocage vient du seul `<body>` (`h-full overflow-hidden`), et le contenu est réparti par `justify-between`. Sur un écran bas ou en paysage, le bloc central (photo) se comprime et le `<footer>` — titre, bouton « Commencer », barre d'accueil — **peut être rogné sans aucun recours au défilement**.

Conséquences :

| Constat | Effet |
| --- | --- |
| `h-[844px]` + `max-h-screen` | sur un écran plus bas que 844 px (petit téléphone, iPhone SE, **paysage**, barre de navigateur visible), `max-h-screen` écrase la hauteur → **contenu coupé** |
| `overflow: hidden` sur le `<body>` | **le défilement vertical est neutralisé** : aucun moyen de faire apparaître ce qui dépasse |
| `justify-between` + `flex-1` non utilisé | la répartition verticale dépend entièrement d'une hauteur fixe |
| Photo en `absolute inset-0` | elle est dimensionnée par le conteneur de 844 px, pas par la fenêtre |

Même famille de défaut que le fichier 1 (Scanner), mais **aggravée** : le scanner n'avait qu'un seul `h-[844px]`, ici le `overflow: hidden` est en plus posé sur le `<body>` des deux fichiers.

### 3.2 Zones de sécurité absentes

| Élément | Attendu | Écran 1 | Écran 2 | Écran 3 |
| --- | --- | --- | --- | --- |
| `viewport-fit=cover` | oui | **absent** | **absent** | **absent** |
| `env(safe-area-inset-top)` | oui | **absent** | **absent** | **absent** |
| `env(safe-area-inset-bottom)` | oui | **absent** | **absent** | **absent** |
| Marges latérales en paysage | oui | **absent** | **absent** | **absent** |
| `100dvh` | oui | **absent** | **absent** | **absent** |

**0 occurrence de `env(safe-area-inset-*)` dans les trois fichiers**, et `100dvh` est absent trois fois.

Les trois fichiers construisent un **faux bandeau d'état iOS** (heure `9:41`, icônes réseau / Wi-Fi / batterie dessinées en SVG) et une **fausse pilule d'accueil** `w-32 h-1`. Sur un appareil réel, l'en-tête passe **sous l'encoche** et la pilule se superpose à la **barre de gestes**.

Détail propre à l'écran 3 : sa jauge de batterie est dessinée avec `rounded-2xs`, **classe inexistante dans Tailwind v3** (§ 8) — le remplissage `bg-white rounded-2xs` n'est donc **pas arrondi**, contrairement à son cadre `rounded-sm`.

### 3.3 Zoom

| Fichier | Viewport | Verdict |
| --- | --- | --- |
| Écran 1 | `width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no` | **zoom et pincement bloqués** — 6ᵉ fichier consécutif |
| Écran 2 | `width=device-width, initial-scale=1.0` | zoom libre, mais **`viewport-fit` manquant** |
| Écran 3 | `width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no` | **zoom et pincement bloqués** — 7ᵉ fichier consécutif |

### 3.4 Cibles tactiles

| Écran | Élément | Taille réelle | Verdict |
| --- | --- | --- | --- |
| 1 | bouton « Passer » | `py-1.5` + `text-[12.5px]` ≈ **26 px** | **sous 44 px** |
| 1 | bouton « Suivant » | `h-[56px]` | conforme |
| 2 | bouton « Passer » | `py-1.5` + `text-xs` ≈ **26 px** | **sous 44 px** |
| 2 | bouton « Suivant » | `h-14` (56 px) | conforme |
| 3 | bouton « Commencer » | `h-14` (56 px) | conforme |
| 3 | bouton « Retour » | `py-1` + `text-[14px]` ≈ **24 px** | **sous 44 px** |

`touch-action` : **0 occurrence** dans les trois fichiers. `cursor-pointer` : **0** dans l'écran 1, **1** (bouton Suivant) dans l'écran 2, **0** dans l'écran 3.

### 3.5 Typographie

| Taille | Écran 1 | Écran 2 | Écran 3 |
| --- | --- | --- | --- |
| `text-[10px]` | 0 | 1 | 0 |
| `text-[10.5px]` | 1 | 0 | 0 |
| `text-[11px]` | 3 | **6** | 1 |
| `text-[11.5px]` | 2 | 0 | 0 |
| `text-[13px]` | 1 (barre d'état factice) | 0 | 0 |
| `text-[12px]` | — | — | **3** (2 pastilles + mention basse) |

Aucun texte sous 10 px : les trois écrans sont **plus accessibles que la moyenne** du dossier. Le `h1` est en revanche très grand (38–41 px sur l'écran 1, 34–38 px sur les écrans 2 et 3).

### 3.6 Accessibilité

| Manque | Écran 1 | Écran 2 | Écran 3 | Impact |
| --- | --- | --- | --- | --- |
| Barre de progression sans `role` ni `aria-current` | oui | oui | oui | étape courante invisible pour un lecteur d'écran |
| Pagination en `<div>`/`<span>` décoratifs | oui | oui | oui (×2 zones) | idem |
| Icônes sans `aria-hidden` | 7 SVG | 8 SVG | 4 SVG | le nom accessible des boutons peut être pollué |
| Aucun `sr-only` | oui | oui | oui | aucun texte alternatif |
| `:focus-visible` | absent | absent | absent | focus clavier non visible |
| `alt` sur les photos | `alt=""` correct | `alt=""` correct | **`alt` descriptif** (2 images) | **point positif** — l'écran 3 est le seul des trois à décrire ses images |

**Point correct à noter :** les écrans 1 et 2 mettent `alt=""` sur les images de fond, ce qui est la bonne pratique (elles ne portent aucune information). L'écran 3 va plus loin avec deux `alt` **descriptifs** : « Rencontre de confiance et remise en main propre d'un colis à l'aéroport » pour la photo, « Logo Zecolis » pour le logo.

**Écart propre à l'écran 3 :** ses 2 boutons n'ont **aucun `aria-label`** — contrairement aux écrans 1 et 2 qui en portent. Ses deux pastilles de progression sont en outre dupliquées (en-tête + pied) sans différenciation : un lecteur d'écran encountera deux fois la même information sans savoir laquelle est la barre d'état.

**Écart relevé sur l'écran 2 :** les deux boutons portent un `aria-label` qui ne contient pas le texte visible — `aria-label="Passer l'onboarding"` pour un bouton dont le texte est « Passer », `aria-label="Écran suivant"` pour « Suivant ». C'est un écart **WCAG 2.5.3 (Label in Name)** : la voix de contrôle « cliquer sur *Passer* » ne trouve pas l'élément.

---

## 4. Code mort et incohérences internes

### 4.1 Classes CSS sans effet

| Fichier | Élément | Constat |
| --- | --- | --- |
| Écran 1 | `.text-wide { letter-spacing: 0.22em }` | **définie, jamais utilisée** |
| Écran 1 | `tailwind.config` : `zecolis.green`, `zecolis.lightBg`, `zecolis.dark`, `zecolis.cardBorder` | **4 jetons déclarés, 0 usage** |
| Écran 2 | `tailwind.config` : **les 8 jetons `brand.*`** (`navy`, `darkNavy`, `deepNight`, `orange`, `orangeHover`, `success`, `cardBg`, `pillBg`) | **0 usage** — toutes les couleurs sont écrites en dur (`bg-[#0B2A5B]`, `bg-[#FF6B00]`…) |
| Écran 2 | SVG d'intensité réseau, ligne 82 | `opacity="0"` → **icône invisible** ; le statut réseau n'est donc jamais affiché |
| Écran 2 | `group-hover:translate-x-1` sur la flèche du bouton « Suivant » | **classe morte** : aucun élément parent ne porte la classe `group` |
| Écran 3 | `tailwind.config` : `brandNavy`, `brandDark`, `brandOrange`, `brandGreen` | **4 jetons déclarés, 0 usage** — cinquième convention de nommage de palette du dossier (voir B21) |
| Écran 3 | `rounded-2xs` sur le remplissage de la jauge de batterie | **classe inexistante** → remplissage non arrondi (§ 8) |
| Écran 3 | `<div class="w-5 h-2.5 … p-0.5 flex items-center"><div class="w-full h-full …">` | la jauge est un **carré de 20 × 10 px** avec un remplissage plein : elle n'indique aucun niveau de batterie et simule un état « chargé » en permanence |

L'écran 2 illustre une **quatrième convention de nommage de palette** (`zecolis.*` dans les fichiers 2 et 3, `brand.*` dans le fichier 4, `zecolis-*` dans le fichier 5, `brand.*` ici) — et, comme les trois précédentes, elle n'est pas utilisée. Voir B21.

### 4.2 Conformité au § 4 (design de référence)

| Point | Écran 1 | Écran 2 | Écran 3 |
| --- | --- | --- | --- |
| Fond clair `#F8FAFC` | **non** (`#030914`, `#040d1c`) | **non** (`#020617`, `#040d1a`) | **non** (`#051329`, noir) |
| Cartes blanches | non (pastilles translucides) | non (pastilles translucides) | non (pastilles translucides) |
| Glassmorphism | `.glass-pill` (blur 12 px) + `.glass-pill-dark` (blur 14 px) | **5 `backdrop-blur-md`** | **3 `backdrop-blur-md`** |
| Dégradés | 3 | 4 | 2 |
| Police Inter | déclarée et chargée | déclarée, **Inter listed en second** | déclarée, **seule police** |
| Palette orange `#FF6B00` | oui | oui | oui |

Les trois écrans sont des **écrans plein cadre photo**, sans rapport avec les écrans applicatifs clairs du § 4. C'est un parti pris graphique cohérent sur les trois, mais divergent du reste du dossier (comme le fichier 1).

**Nuance sur l'écran 3 :** c'est le seul des trois à retrouver une couleur de marque en **fond opaque** — le bouton « Commencer » est en `#0B2A5B` plein, comme les boutons des écrans applicatifs, alors que les écrans 1 et 2 utilisent un dégradé bleu nuit. Le pantalla 3 est donc **plus proche du § 4** que les deux autres.

---

## 5. Données affichées

Toutes les valeurs sont **codées en dur** et **fictives** :

| Écran | Élément | Valeur |
| --- | --- | --- |
| 1 | Trajet | Cotonou ⇄ Paris |
| 1 | Pastilles | `Colis prêt`, `Voyage vérifié` |
| 2 | Trajet | Cotonou ⇄ Paris |
| 2 | Pastilles | `Paiement reçu`, `✓ Instantané`, `8 kg disponibles` |
| 2 | Mentions | `Kilos rentralisés` · `Colis vérifié` · **`Paiement garanti`** |
| 3 | Trajet | Cotonou ⇄ **France** (sans ville) |
| 3 | Pastilles | **`✓ Identité vérifiée`**, **`★ 4,9 Voyageur vérifié`** |
| 3 | Mention basse | `Identité · Avis · Échanges` |
| 1, 2 et 3 | Heure factice | `9:41` |

**Revendications de capacité (§ 3) :**

| # | Affirmation | Problème |
| --- | --- | --- |
| O-1 | `Paiement garanti` (écran 2) | aucun paiement, aucun séquestre, aucun appel réseau dans le fichier |
| O-2 | `✓ Instantané` (écran 2) | aucune temporisation, aucun traitement : le badge est décoratif |
| O-3 | `✓ Colis prêt` / `✓ Voyage vérifié` (écran 1) | aucun code de vérification |
| O-4 | `✓ Identité / paiement validé` implicite via la photo de plein cadre | aucune donnée de profil réelle |
| O-5 | Photo plein cadre de deux personnes et d'un colis | image de banque d'images, présentée comme une scène du produit |

**Revendications propres à l'écran 3 — les plus fortes du lot :**

| # | Affirmation | Problème |
| --- | --- | --- |
| O-6 | **`✓ Identité vérifiée`** | **aucun mécanisme de vérification d'identité dans le projet.** Le § 3 mentionne « voyageur vérifié » comme capacité, mais aucun écran ni aucune règle ne définit ce contrôle. C'est une promesse de conformité presented comme acquise |
| O-7 | **`★ 4,9 Voyageur vérifié`** | note de 4,9 sans source, sans nombre d'avis, sans échelle. Donnée purement décorative |
| O-8 | « Des profils vérifiés pour envoyer et voyager sereinement » | la phrase principale de l'écran repose entièrement sur O-6, qui n'est pas implémenté |
| O-9 | Mention « Identité · Avis · Échanges » | les **avis** n'existent nulle part dans les écrans reçus (ni note, ni commentaire, ni avis sur un profil) |

**Point d'attention pour le lecteur :** l'écran 3 est le dernier de la séquence et il est **presque entièrement bâti sur des non-fonctionnalités**. Il porte la promesse la plus engageante du dossier (« la confiance d'abord ») alors qu'il ne contient aucune logique. C'est l'inverse des écrans 1 et 2, qui promettaient moins et ne comportaient pas non plus de logique.

---

## 6. Fonctionnalités affichées vs implémentées

| Fonctionnalité affichée | Implémentée ? |
| --- | --- |
| « Passer » (saute l'onboarding, écrans 1 et 2) | **Non** — `<button>` sans `id`, sans `onclick`, sans gestionnaire |
| « Suivant » (étape 1 → étape 2) | **Non** — aucun gestionnaire |
| « Suivant » (étape 2 → étape 3) | **Non** — aucun gestionnaire |
| « Commencer » (étape 3 → sortie de l'onboarding) | **Non** — aucun gestionnaire |
| « Retour » (étape 3 → étape 2) | **Non** — aucun gestionnaire |
| Barre de progression | **Non** — segments `<div>` ou pastilles `<span>` sans état ni valeur |
| Pagination | **Non** — décorative, et **dupliquée** sur l'écran 3 |
| Barre d'état / pilule d'accueil | **Non** — dessinées en dur |
| `✓ Identité vérifiée` / `★ 4,9` (écran 3) | **Non** — aucune donnée, aucun calcul |
| Animations d'interface | **Oui** — CSS pur (`animate-pulse` sur les écrans 1 et 2, aucune sur l'écran 3) |
| Icônes SVG | **Oui** — SVG inline (et non Lucide, contrairement aux 5 fichiers précédents) |

**Bilan : 0 action réelle sur 6 éléments interactifs au total.** Le parcours d'onboarding complet (1 → 2 → 3 → accueil) est **impossible à parcourir** dans les originaux : même en ajoutant l'étape 3, la chaîne reste rompue aux six endroits.

---

## 7. Liens avec les autres pages

| Élément | Destination attendue | État |
| --- | --- | --- |
| « Suivant » écran 1 | écran 2 « Voyageur » | **lien logique, absent du code** — les deux écrans reçus sont pourtant la même séquence |
| « Suivant » écran 2 | écran 3 « Confiance » | **lien logique, absent du code** — l'étape 3 **a été reçue** au lot 7, la cible existe |
| « Commencer » écran 3 | sortie de l'onboarding → accueil | **lien logique, absent du code** — `accueil.html` existe (fichier 5) |
| « Retour » écran 3 | écran 2 « Voyageur » | **lien logique, absent du code** |
| « Passer » (écrans 1 et 2) | sortie de l'onboarding | **aucune cible** — l'accueil existe pourtant (`accueil.html`, fichier 5) mais n'est référencé nulle part |

Aucun des trois fichiers ne contient le moindre `<a href>`. **Zéro lien de navigation dans les six boutons de la séquence** : les trois écrans sont des Boards de présentation, pas un parcours.

**Conséquence :** même une fois les trois écrans réunis, l'onboarding reste un ensemble de **trois planches déconnectées**. Le raccordement réalisé dans `onboarding_voyageur_mobile.html` et `onboarding_confiance_mobile.html` (voir `02_ONBOARDING_CORRECTIONS_MOBILE.md`) ne porte que sur les liens, pas sur les actions « Passer » des écrans 1 et 2, qui restent des boutons sans gestionnaire.

---

## 8. Registre des classes Tailwind invalides

Ces classes n'existent pas dans l'échelle Tailwind v3 servie par `cdn.tailwindcss.com` et produisent **aucun effet** :

| Classe | Fichier | Effet réel |
| --- | --- | --- |
| `h-[844px]` | écrans 1 et 2 | valide, mais fige la hauteur (§ 3.1) |
| `to-95%` | écran 1, ligne 93 | **invalide en Tailwind v3** (syntaxe v4) — le second arrêt du voile bas est ignoré |
| `rounded-2xs` | écran 2 | **invalide** — la bordure de la jauge de batterie n'est pas arrondie |
| `rounded-2xs` | **écran 3, ligne 40** | **invalide** — le remplissage de la jauge n'est pas arrondi |
| `via-45%` / `via-40%` | **écran 3, lignes 36-37** | **invalide** (syntaxe v4) — le second arrêt de chacun des deux voiles photo est ignoré, le dégradé s'arrête plus tôt que prévu |
| `text-[12.5px]` / `text-[10.5px]` / `text-[11.5px]` | écran 1 | valides (valeurs arbitraires) |
| `bg-white/90`, `from-[#0d326d]` | écran 2 | valides |

**Bilan du lot onboarding : 6 classes invalides distinctes, 7 occurrences**, réparties sur les trois fichiers. `rounded-2xs` apparaît **deux fois** dans le dossier sur des jauges de batterie dessinées à la main, et `via-XX%` quatre fois au total — un motif qui suggère du code généré par un outil Tailwind v4 sur un CDN v3.

---

## 9. Points à trancher

| # | Question | État |
| --- | --- | --- |
| O-Q1 | Que fait « Passer » : retour vers l'accueil, ou écran d'accueil public pour utilisateur non connecté (cf. B30) ? | **ouvert** — les écrans 1 et 2 conservent un bouton « Passer » sans gestionnaire |
| O-Q2 | Quelle est l'étape 3 de l'onboarding, et quand sera-t-elle transmise ? | **résolu au lot 7** — `onboarding_confiance_3.html` a été reçu et analysé (§ 2.3) |
| O-Q3 | Les écrans plein cadre photo sont-ils voulus, ou s'agit-il de planches de présentation qui doivent devenir des écrans applicatifs clairs (§ 4.2) ? | **ouvert** |
| O-Q4 | `Paiement garanti` et `✓ Instantané` doivent-ils être retirés, ou adossés à un mécanisme réel (B29) ? | **ouvert** |
| O-Q5 | L'onboarding est-il obligatoire au premier lancement, ou optionnel (« Passer » existe pour cela) ? | **ouvert** |
| **O-Q6** | **Sur quoi repose `✓ Identité vérifiée` ?** Quelle procédure, quel prestataire, quel niveau de vérification ? Tant que ce n'est pas défini, la promesse centrale de l'écran 3 ne peut pas être affichée comme acquise | **ouvert — bloquant pour l'écran 3** |
| **O-Q7** | **D'où viennent la note `★ 4,9` et la mention « Avis » ?** Il n'existe aucun écran d'avis ni de notation de profil dans les 11 écrans reçus | **ouvert — bloquant pour l'écran 3** |
| **O-Q8** | « Commencer » (écran 3) doit-il amener à l'accueil, ou à la **connexion** ? Un utilisateur non connecté ne peut pas publier de trajet | **ouvert** |