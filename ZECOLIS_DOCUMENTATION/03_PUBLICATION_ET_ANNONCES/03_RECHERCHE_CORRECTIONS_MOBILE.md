# Recherche — Diagnostic mobile et version corrigée

| Rubrique | Valeur |
| --- | --- |
| **Fichier analysé** | `SOURCES_HTML_ORIGINALES/recherche.html` (28 862 octets, 551 lignes) — **original non modifié** |
| **Version corrigée produite** | `VERSIONS_CORRIGEES/recherche_mobile.html` (43 361 octets) |
| **Nature du travail** | Analyse statique du code + corrections mobile / accessibilité / comportements front |
| **Limite** | **Aucun navigateur, aucun appareil réel n'a été utilisé.** Les constats sont déduits du code. |
| **Analyse détaillée** | `03_PUBLICATION_ET_ANNONCES/03_RECHERCHE.md` |

---

## 1. Verdict

L'écran **s'affiche et deux fonctionnalités marchent réellement** (bascule de mode, inversion de trajet) — c'est le meilleur score de fonctionnalité depuis l'accueil. Mais **la recherche n'est pas une recherche** : aucun champ de saisie, aucun filtre appliqué, aucun appel serveur, et 15 cibles tactiles sur 18 sous 44 px.

| Gravité | Constat |
| --- | --- |
| Bloquant | Zoom et pincement **interdits** (`maximum-scale=1.0, user-scalable=no`) |
| Bloquant | **0 champ de saisie** : départ et arrivée sont des `<div>` statiques (0 `<input>` dans tout le fichier) |
| Bloquant | **11 actions sans aucun gestionnaire**, dont le CTA « Rechercher », les 3 `Voir l'annonce` / `Voir le colis` et les 5 entrées de la barre basse |
| Bloquant | **0 `<a href>`** dans tout le fichier : la navigation est entièrement morte |
| Majeur | Inversion départ/arrivée : **les pays et les drapeaux restent attachés à la mauvaise ville** |
| Majeur | Les 4 puces de filtre **ne filtrent rien** et ne synchronisent pas leur état |
| Majeur | Le mode Voyageur annonce « 28 colis en attente » **sans changer les 3 cartes** |
| Majeur | Zones de sécurité déclarées (`viewport-fit=cover`) mais **`env(safe-area-inset-*)` utilisé 0 fois** |
| Majeur | **15 cibles tactiles sur 18 sous 44 px** (la plus petite : 24 px) |
| Majeur | Barre basse `fixed` sur un conteneur `min-h-[844px]` : sur écran plus bas, **la barre flotte au milieu de l'écran** |
| Mineur | 4 classes Tailwind invalides distinctes, 10 occurrences (`h-13` ×2, `w-13` ×1, `rounded-2xs` ×1, `shadow-2xs` ×6) |
| Mineur | `user-select: none` sur le `<body>` : prix, dates et villes non copiables |

---

## 2. Corrections appliquées dans `recherche_mobile.html`

**Périmètre : mise en page mobile, accessibilité, et comportements front. Aucun choix de design n'a été modifié : palette, cartes, rayons, hiérarchie typographique, dégradés de boutons et barre basse sont conservés à l'identique.**

### 2.1 Mobile

| Correction | Avant | Après |
| --- | --- | --- |
| Viewport | `maximum-scale=1.0, user-scalable=no` | `width=device-width, initial-scale=1.0, viewport-fit=cover` — zoom et pincement réautorisés |
| Zones de sécurité | `viewport-fit` déclaré, **`env()` jamais utilisé** | variables `--sat/--sar/--sab/--sal` = `env(safe-area-inset-*, 0px)` : en-tête `pt-[calc(0.5rem+var(--sat))]`, barre basse `padding-bottom: calc(0.5rem + var(--sab))`, marges latérales sur le conteneur et sur les deux barres |
| Hauteur | `min-h-[844px]` | `.app-shell { min-height: 100vh; min-height: 100dvh }` |
| Réserve basse du contenu | `pb-28` (112 px, figé) | `.main-pad { padding-bottom: calc(6.5rem + var(--sab)) }` — suit la hauteur réelle de la barre + zone de geste |
| Barre basse | `pb-5` fixe + pilule d'accueil dessinée | `padding-bottom` calculé, **pilule masquée sur mobile tactile** (elle est conservée en aperçu sur grand écran) |
| Barre d'état simulée | toujours visible | masquée sur `(pointer: coarse)` ou largeur ≤ 430 px |
| Jauge de batterie | `rounded-2xs` **invalide** | `rounded-sm` |
| Ombres de pastilles | `shadow-2xs` **invalide** (×6) | `shadow-sm` |
| Bouton `Publier` | `w-13 h-13 w-12 h-12` | `w-12 h-12` — les classes invalides supprimées |
| CTA recherche | `h-13 py-3.5` (**`h-13` invalide**) | `h-14 py-3.5` — 56 px garantis |
| Barre de puces | défilement horizontal avec marges nulles | `overflow-x-auto` + `-mx-1 px-1` : le focus clavier ne rogne plus la première puce |
| Paysage court | — | media query `(orientation: landscape) and (max-height: 560px)` sur le titre éditorial |
| Sélection de texte | `user-select: none` sur le `<body>` | `.no-select` appliqué aux seuls éléments d'interface — **prix, dates, villes et noms deviennent copiables** |
| Tap highlight / focus | partiels | appliqués sur `*` ; `:focus-visible` avec contour orange 2 px |
| Animations permanentes | 2 en boucle | conservées, neutralisées sous `prefers-reduced-motion` |
| Images sous la ligne de flottaison | `loading` par défaut | `loading="lazy"`, `decoding="async"`, `width`/`height` sur les 5 images |
| Cibles tactiles | **15 sous 44 px** | `.tap { min-height: 44px; min-width: 44px; touch-action: manipulation }` sur les 24 éléments interactifs |

### 2.2 Accessibilité

| Correction | Détail |
| --- | --- |
| Sélecteur de mode | `role="tablist"` + `role="tab"` + `aria-selected` **synchronisé** au clic (absent de l'original) |
| Inverseur départ/arrivée | cible portée à 44 px, `aria-describedby` vers la zone de statut, résultat **annoncé vocalement** |
| Champs de lieu | `<div>` sans nom → `role="group"` + `tabindex="0"` + `aria-label="Départ : Cotonou, Bénin"` ; traitement `Enter`/`Espace` |
| Blocs Période et Capacité | `<div cursor-pointer>` → **vrais `<button>`** avec libellé complet |
| Puces de filtre | `aria-pressed="true|false"` + `role="group"` + libellé `aria-label="Filtres rapides"` |
| `Réinitialiser` | `<span>` → **vrai `<button>`** |
| Barre basse | `nav` + `aria-label="Navigation principale"` ; onglet courant `aria-current="page"` ; bouton central `aria-label="Publier une annonce"` |
| Cloche | `aria-label="Notifications : 2 non lus"` + cible 44 px |
| Avatar | `<div>` sans rôle → **vrai `<button>`** avec `aria-label="Mon profil"` |
| Retours | `#appStatus` en `role="status"` `aria-live="polite"` + toast visuel (non bloquant, 4 s) |
| Bloc pour lecteur d'écran | `<h2>` du fil, `h3` par carte conservés ; `<main>` + `<header>` + `<nav>` structurés |
| Données fictives | mention visible « Maquette : les profils, les prix et les disponibilités sont fictifs » + « 3 entrées figées, aucun appel serveur » |

### 2.3 Bugs fonctionnels corrigés

| Bug d'origine | Correction |
| --- | --- |
| Inversion : villes échangées mais **pastilles de pays statiques** → `DÉPART France 🇧🇯` | `#departure-country` et `#arrival-country` sont échangés avec les villes ; résultat annoncé vocalement |
| Mode Voyageur : titre « 28 colis en attente » mais **fil inchangé** | le basculement **annonce explicitement** que le fil affiché reste le jeu de démonstration (2 voyageurs + 1 colis) |
| Puces : `active` jamais posé, contours alternés, **aucun filtrage** | état unique `aria-pressed` + classe `active` + contour, résumé « n filtres actifs » sous la barre de puces, et message indiquant qu'**aucun filtrage serveur n'est appliqué** |
| `Réinitialiser` inerte | remise à l'état initial (1re puce active), résumé et annonce mis à jour |
| CTA `Rechercher` sans gestionnaire | retour honnête : **« Recherche simulée sur 3 profils de démonstration : aucun appel serveur, aucune réservation possible »** |
| 11 actions sans effet | toutes converties en `<button>` avec `data-demo` : un bandeau explique **pourquoi** l'action est impossible (écran non reçu, non branché) |

### 2.4 Ce qui n'a **pas** été corrigé (et pourquoi)

| Point | Décision |
| --- | --- |
| 35 textes entre 9 et 11 px | **conservés** : les changer tous modifierait la composition visuelle d'ensemble. Chantier de design à arbitrer |
| Plus Jakarta Sans | **conservée** : c'est la police de l'écran ; § 4.4 fixe Inter, divergence déjà enregistrée (DV-1) |
| Barre basse à 5 entrées | conservée telle quelle : c'est la seule représentation complète de la navigation du § 4.5 reçue à ce jour (cf. R-Q6) |
| Données des 3 cartes | conservées (mêmes profils, prix, dates) mais **explicitement étiquetées comme démonstration** |
| `En direct` + `animate-ping` | conservés visuellement, mais **la nature statique du fil est annoncée** à l'utilisateur |
| Images distantes Google | conservées (5 URLs), comme sur les 7 autres fichiers |

---

## 3. Points restant à traiter

| # | Point | Nature |
| --- | --- | --- |
| 1 | **Aucun champ de saisie** : la recherche n'est pas saisissable | Fonctionnel — nécessite une source de lieux (cf. B3) |
| 2 | **Écrans non reçus** : détail d'annonce ×2, détail de colis, publication, messagerie, profil, notifications | Écrans manquants |
| 3 | Les 4 filtres ne filtrent rien | Fonctionnel — critères à définir (B1, R-Q2) |
| 4 | Mode Expéditeur / Voyageur sans changement de contenu | Fonctionnel — à trancher (R-Q3) |
| 5 | Plages de dates contradictoires (« 18–24 » / « 18–25 ») et prix du filtre (≤ 10 €/kg) contre le colis affiché (12 €/kg) | Cohérence de la maquette |
| 6 | « Air Parcel Hub » : marque absente du cahier des charges | Arbitrage (R-Q4) |
| 7 | 35 textes sous 12 px, dont les prix et les dates | Design — à arbitrer |
| 8 | Safe areas, barre basse en paysage, clavier virtuel | Non testable ici (G11, G12) |
| 9 | 5 images distantes non versionnées | Fragilité réseau |

---

## 4. Contrôles statiques effectués

| Contrôle | Résultat |
| --- | --- |
| Encodage UTF-8 sans BOM | ✔ |
| Caractères CJK ou de remplacement | aucun |
| Viewport sans `user-scalable` ni `maximum-scale` | ✔ |
| `viewport-fit=cover` + 4 variables `env(safe-area-inset-*)` | ✔ |
| `100dvh` présent | ✔ (2 occurrences : repli `100vh` + `100dvh`) |
| `id` déclarés / en double | 18 / **aucun** |
| `getElementById` sans élément correspondant | **aucun** (18 appels, tous résolus) |
| Sélecteurs `querySelectorAll` | `.filter-chip` → 4 éléments ✔ · `.filter-chip[aria-pressed="true"]` → dynamique ✔ · `[data-demo]` → 13 éléments |
| Liens internes `href` → fichier existant | ✔ (`accueil_mobile.html`) |
| Équilibrage des balises | équilibré (aucune anomalie) |
| Classes Tailwind invalides (`h-13`, `w-13`, `rounded-2xs`, `shadow-2xs`) | **aucune dans le code** — les 3 occurrences restantes de `h-13`/`w-13` sont uniquement dans les commentaires de correction ; `rounded-2xs` et `shadow-2xs` ont totalement disparu |
| Taille de l'original | **inchangée** — 28 862 octets |

---

## 5. Fichiers

| Fichier | Contenu |
| --- | --- |
| `SOURCES_HTML_ORIGINALES/recherche.html` | **Original, non modifié** |
| `VERSIONS_CORRIGEES/recherche_mobile.html` | Version corrigée — mobile + accessibilité + 3 bugs fonctionnels corrigés + retours honnêtes |

Les deux fichiers sont distincts. Aucun original n'a été écrasé.