# Accueil

| Rubrique | Valeur |
| --- | --- |
| **Nom de la page** | Accueil (titre du document : « Zecolis - Accueil ») |
| **Fichier source** | Nom de fichier d'origine **non communiqué**. Copie conservée sous `SOURCES_HTML_ORIGINALES/accueil.html` (nom de travail). |
| **Catégorie retenue** | `02_NAVIGATION_ET_ACCUEIL` — écran d'accueil + barre de navigation principale. |
| **Statut d'analyse** | Analysé (fichier unique, autonome) |
| **Nature** | Maquette HTML autonome, **la plus riche des cinq reçues** : 8 sections, 2 carrousels, barre de navigation à 5 entrées, bouton flottant, modale scanner. Toutes les données sont **en dur** et toutes les actions sont **mortes ou simulées**. |

---

## 1. Objectif de la page

Servir d'écran d'accueil de l'application : saluer l'utilisateur connecté, permettre une recherche d'itinéraire, proposer les deux intentions principales (expédier / voyager), montrer des opportunités de voyage et des colis à transporter, rappeler la progression du profil, et donner accès à la barre de navigation principale.

C'est le seul fichier reçu qui contienne la **navigation principale** de l'application (§ 4.5 du cahier des charges) et le seul qui présente une offre de marché.

## 2. Utilisateur concerné et rôle métier

Utilisateur **connecté** — le fichier suppose une session active :

| Indice | Élément |
| --- | --- |
| Nom personnalisé | `Bonjour, Riham 👋` |
| Avatar + badge vérifié | image de profil + coche verte |
| Notifications | cloche + point orange |
| Activité en cours | `1 envoi en cours #COL-7852` · `2 colis • 1 voyage réservé` |
| Progression profil | `Profil complété à 70%` · `Étape 2/3` |

**Aucun rôle n'est demandé ni affiché** : ni « expéditeur », ni « voyageur ». Les deux intentions sont presented côte à côte (carte « Envoyer un colis » / carte « Voyager avec un colis »), ce qui est **conforme à § 2.1** (un compte unique, deux usages).

**Aucun mécanisme de session** : rien dans le fichier ne permet de savoir qui est « Riham », ni d'afficher un écran de connexion pour un visiteur non connecté. Le fichier 2 (Connexion) n'affiche aucun lien vers cet écran.

## 3. Point d'entrée dans la navigation

C'est l'écran **hub** de l'application. Il n'a pas de lien entrant réel : c'est le fichier 2 qui devrait y mener, et il ne le fait pas.

| Élément | Cible | État |
| --- | --- | --- |
| Onglet `Accueil` | `href="#accueil"` | **Inopérant** — devrait être la racine de l'application |
| Onglet `Recherche` | `href="#recherche"` | **Inopérant** |
| Onglet `Messages` | `href="#messages"` | **Inopérant** |
| Onglet `Profil` | `href="#profil"` | **Inopérant** |
| Bouton central `Publier` | `<button type="button">` sans gestionnaire | **Inopérant** |
| Carte « Envoyer un colis » | `href="#envoyer"` | **Inopérant** |
| Carte « Voyager avec un colis » | `href="#voyager"` | **Inopérant** |
| Lien « Suivi » | `href="#suivi"` | **Inopérant** |
| Lien « Tout voir » | `href="#marketplace"` | **Inopérant** |
| Lien « Compléter » | `href="#profile-edit"` | **Inopérant** |
| 3 liens « en savoir plus » | `#garantie`, `#paiement-protection`, `#guide-douane` | **Inopérants** |
| Avatar | `<div class="... cursor-pointer">` sans gestionnaire | **Inopérant** |
| Bouton cloche (notifications) | `<button type="button">` sans gestionnaire | **Inopérant** |

**12 ancres mortes, 3 boutons sans gestionnaire, 1 div cliquable sans effet.** Aucun des 5 ongllets de la barre de navigation ne mène nulle part — **c'est le point le plus bloquant du fichier** : la navigation principale de § 4.5 existe visuellement et est entièrement non fonctionnelle.

Seule action réellement câblée : le bouton flottant Scanner (§ 8).

## 4. Structure de l'écran (ordre de lecture)

```
<main class="... max-w-[390px] min-h-screen pb-28">
├── header  (sticky, blur)          Logo + corridor · cloche + point · avatar vérifié · Bonjour, Riham · Réseau Cotonou actif
├── section Hero                     Photo 270 px + dégradé · badges FRANCE ↔ BÉNIN / Départs quotidiens · titre
├── section Recherche                Carte blanche superposée au hero (-mt-7) : Départ · Arrivée · bouton Rechercher
├── section Intentions              2 cartes : Envoyer un colis / Voyager avec un colis
├── section Activité                 1 envoi en cours #COL-7852 · 2 colis • 1 voyage réservé · Suivi
├── section Voyages (carrousel)      3 cartes voyageur (285 px) + « Tout voir »
├── section Colis (carrousel)        3 cartes colis (280 px)
├── section Engagements (carrousel)  3 cartes Réassurance (310 px) + points de pagination
├── section Profil                   Profil complété à 70% · Étape 2/3 · barre de progression · Compléter
└── div FAB Scanner (fixed)         Bouton flottant « Scanner Live · Colis & QR » + pastille pulsée
    + <div id="qr-scanner-modal">    Modale scanner (cachée)
<nav> (fixed)  Barre flottante : Accueil · Recherche · [Publier] · Messages · Profil
```

Le `pb-28` (112 px) du `<main>` est le mécanisme qui empêche le contenu de passer sous la barre de navigation fixe. **Correct dans l'intention**, mais calculé sans safe area (§ 16.2).

## 5. Sections et composants visuels

### 5.1 Configuration Tailwind — **quatrième convention**

| Jeton | Valeur | § 4.2 |
| --- | --- | --- |
| `zecolis-navy-deep` | `#1D1D4B` | ✔ |
| `zecolis-navy` | `#0B2A5B` | ✔ |
| `zecolis-orange` | `#FF6B00` | ✔ |
| `zecolis-orange-light` | `#FFF7ED` | ✘ hors palette, **déclaré et inutilisé** |
| `zecolis-green` | `#22A55A` | ✔ |
| `zecolis-surface` | `#F8FAFC` | ✔ **déclaré et inutilisé** (le fond est écrit `#F8FAFC` en dur) |
| `shadow-dock` | `0 10px 25px -5px rgba(11,42,91,.12), 0 8px 10px -6px rgba(11,42,91,.08)` | Double ombre, douce en intensité mais **20 px + 25 px de diffusion** |

Nommage à **prefixe plat** (`zecolis-navy`, pas `zecolis.navy`), contrairement aux fichiers 2 et 3 (`zecolis.navy`). Et comme eux, **toutes les couleurs sont en dur dans les classes** : la configuration ne sert à rien. Quatre fichiers, trois conventions de nommage, zéro configuration réellement partagée.

### 5.2 Police — **déclaration sans chargement**

```js
fontFamily: { sans: ['Inter', 'Plus Jakarta Sans', 'system-ui', 'sans-serif'] }
```

**Aucun appel à `fonts.googleapis.com` dans tout le fichier** (vérifié : 0 occurrence).

Conséquence : la famille `Inter` est déclarée mais **jamais chargée**. Le navigateur retombe sur les polices système disponibles. Le rendu réel n'est donc **ni Inter, ni Plus Jakarta Sans** — c'est une pile système non maîtrisée.

> **Défaut propre à ce fichier** : les fichiers 1 et 4 chargent Plus Jakarta Sans, les fichiers 2 et 3 chargent Inter, et ce fichier **n'en charge aucune**. Aucune page de l'application n'a donc une typographie fiable.

### 5.3 En-tête (`sticky top-0 z-40`)

| Élément | Détail |
| --- | --- |
| Logo | `<img>` Googleusercontent, `h-8`, `alt="Logo officiel Zecolis"` — **URL différente des trois autres fichiers** |
| Sur-titre | `Corridor Fiable` — `text-[10px] font-bold uppercase tracking-widest text-[#FF6B00]` |
| Sous-titre | `France ↔ Bénin` — `text-xs font-semibold` |
| Cloche | `w-9 h-9` (36 px) + point orange `bg-[#FF6B00]` `absolute top-2 right-2` |
| Avatar | `w-9 h-9` + **badge de vérification vert** (`check` sur cercle `#22A55A`) |
| Salutation | `Bonjour, Riham 👋` en `font-semibold text-sm` |
| État réseau | point vert `animate-pulse` + `Réseau Cotonou actif` en `text-[11px]` |

Le libellé **« Corridor Fiable »** n'existe nulle part dans le cahier des charges : c'est un **argumentaire marketing inventé** (§ 10.4).

L'en-tête est `bg-white/95 backdrop-blur-md border-b border-slate-100` : c'est un **voile flouté sur en-tête sticky**, à savoir discussion avec l'interdiction du § 4.4 (§ 15).

### 5.4 Section Hero

| Élément | Détail |
| --- | --- |
| Bloc | `mx-4 mt-3 rounded-3xl overflow-hidden h-[270px] bg-slate-900` |
| Image | Photo « Voyageur et expéditrice échangeant un colis à l'aéroport de Cotonou Cadjehoun » — image **distante Googleusercontent**, `alt` très détaillé |
| Voile | `bg-gradient-to-t from-[#0B2A5B]/95 via-[#0B2A5B]/40 to-transparent` — **dégradé à 3 arrêts** |
| Badge 1 | `FRANCE ↔ BÉNIN` — orange, `text-[10px]` |
| Badge 2 | `Départs quotidiens` — `bg-black/40 backdrop-blur-sm` |
| `h1` | « Votre colis peut déjà **voyager.** » — `text-2xl font-bold text-white`, « voyager » en orange |
| Sous-titre | « Trouvez quelqu'un qui part déjà. » |

**` Départs quotidiens`** est une **promesse de service** (départ quotidien sur le corridor) qui n'est adossée à aucun code ni à aucune donnée du cahier des charges (§ 10.4). **À arbitrer.**

### 5.5 Carte de recherche d'itinéraire

Carte blanche `bg-white rounded-2xl shadow-lg border border-slate-100 p-4`, **superposée au hero** (`-mt-7`) — un choix de composition réussi visuellement.

| Élément | Détail |
| --- | --- |
| Titre | « Vous allez où ? » `text-[11px] font-bold text-slate-400 uppercase` |
| Champ Départ | `Cotonou (COO)` — **`<span>`, pas un `<input>`** |
| Champ Arrivée | `France (Paris, Lyon...)` — **`<span>`, pas un `<input>`** |
| Bouton inversion | `arrow-up-down` — `w-8 h-8` (32 px), `absolute right-3 top-1/2` |
| Bouton Rechercher | `bg-[#FF6B00] py-3.5 rounded-xl font-semibold text-sm` — **orange**, et non navy |

**Les deux champs sont du texte statique : l'utilisateur ne peut pas saisir de ville.** La recherche d'itinéraire — cœur du § 2.3 du cahier des charges — est donc **purement décorative**. Le bouton d'inversion n'a aucun gestionnaire : il ne fait rien.

Le bouton `Rechercher` est le **seul bouton principal orange de l'application**. Les fichiers 2, 3 et 4 utilisent `bg-[#0B2A5B]` (navy) pour leur bouton principal : **contradiction directe** avec ce fichier (voir registre D).

### 5.6 Cartes d'intention (2 cartes)

Grille `grid-cols-2 gap-3`, deux cartes `h-36` avec image de fond, voile `bg-gradient-to-t from-[#0B2A5B]/90 ...`, badge, titre, sous-titre et bouton-puce.

| Carte | Badge | Titre | Sous-titre | Action |
| --- | --- | --- | --- | --- |
| `href="#envoyer"` | `EXPÉDITION` (orange) | Envoyer un colis | Je cherche un voyageur disponible | `Continuer` → |
| `href="#voyager"` | `VOYAGEUR` (`bg-white/30 backdrop-blur-md`) | Voyager avec un colis | Je propose de l'espace valise | `Publier` → |

**Deux mentions de modèle économique non documentées :**
- « **Je propose de l'espace valise** » : la capacité transportée est **la valise du voyageur**, pas un volume négocié. Cela structure tout le modèle de tarification au kg affiché partout dans la page.
- Le badge `VOYAGEUR` est le seul élément de la page à poser un `backdrop-blur-md` **directement sur du texte** : rendu correct dans l'esprit, mais hors § 4.4.

**Ces deux cartes sont les deux seuls points d'entrée métier de l'application** — et ce sont deux ancres mortes.

### 5.7 Bandeau d'activité

`1 envoi en cours` + `#COL-7852` (gris) et `2 colis • 1 voyage réservé`, point vert pulsé, lien `Suivi` orange.

Données **entièrement fictives** et **incohérentes entre elles** : « 1 envoi en cours » mais « 2 colis ». Le § 3 interdit d'inventer des données ; le § 9.1 interdit d'inventer un processus de paiement ou un statut.

### 5.8 Carrousel « Des opportunités près de vous » (voyageurs)

Conteneur `flex gap-3.5 overflow-x-auto snap-x no-scrollbar pb-2 -mx-4 px-4` (débordement horizontal latéral de 16 px pour que les cartes « respirent » jusqu'aux bords). **3 cartes** de `w-[285px]`.

| # | Voyageur | Badge | Date | Trajet | Capacité | Prix | Note |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | **KPO Ivy** · Vérifié | `VOYAGE CONFIRMÉ` (`bg-blue-50`) | 14 mai 2025 | Cotonou → France | `9 kg dispos` | **9 € / kg** | ★ 4,9 (34 avis vérifiés) |
| 2 | **Koffi D.** · Vérifié | `VOYAGE CONFIRMÉ` (`bg-emerald-50`) | 18 mai 2025 | Paris (CDG) → Cotonou | `14 kg dispos` | **10 € / kg** | ★ 5,0 (18 avis) |
| 3 | **Amina S.** · Vérifiée | `VOYAGE CONFIRMÉ` (`bg-blue-50`) | 22 mai 2025 | Cotonou → Lyon | `6 kg dispos` | **8 € / kg** | ★ 4,9 (42 avis) |

Chaque carte : bouton `Voir le voyage` (`bg-[#0B2A5B] py-2.5`) — **6 boutons dead**, aucun gestionnaire.

**Incohérences à relever :**

| Constat | Détail |
| --- | --- |
| Statut `VOYAGE CONFIRMÉ` sur les 3 cartes | Le même statut est répété 3 fois, avec **2 couleurs de badge différentes** (`blue-50` et `emerald-50`) pour un statut identique. Incohérence visuelle. |
| 3 profils « Vérifié » sans aucun mécanisme | § 6.6 prévoit un processus de vérification ; rien ne le rattache à ces badges |
| 3 notes et 94 avis au total | **Aucune source**, aucune trace de calcul, aucun avis individuel affiché |
| Prix au kg | Le § 9.1 interdit d'inventer un processus de paiement : ici un modèle de tarification au kilogramme est **affirmé sur 6 cartes** sans aucune trace de son origine |
| Capacité en kg avec décimales | `9 kg`, `14 kg`, `6 kg` — pourquoi pas 9,0 ? La capacité est-elle un entier ou un décimal ? **À préciser.** |
| Dates 2025 | Les trois dates sont en **mai 2025** : des données figées, périmées ou à risque selon la date de lecture |

### 5.9 Carrousel « Des colis cherchent un voyageur » (colis)

Même mécanisme, **3 cartes** de `w-[280px]`, photo + étiquette + statut vert + infos + bouton `Voir le colis` (`external-link`).

| # | Colis | Étiquette | Statut | Trajet | Poids | Prix |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Carton compact scellé | `TECH & DOCUMENTS` | `Colis prêt` | Cotonou (COO) → Paris (CDG) | 5 kg | 12 €/kg |
| 2 | Appareil photo & accessoires | `TECH & ACCESSOIRES` | `Colis prêt` | Paris (ORY) → Cotonou | 3,2 kg | 15 €/kg |
| 3 | Dossier notarié sous pli scellé | `DOCUMENTS OFFICIELS` | **`Pli vérifié`** | Cotonou → France (Toulouse) | 1,2 kg | **20 € forfait** |

**Constats :**

- Le **3ᵉ colis mélange deux modèles de prix** : les deux premiers sont au kg, le troisième est au forfait — sans explication. Règle de tarification à clarifier.
- **`Pli vérifié`** est un statut **distinct** de `Colis prêt` : il introduit une notion de **vérification de contenu** pour un pli notarié. Le § 6.8 mentionne un « scellé » ; il ne parle pas de vérification de pli notarié. **Concept à valider.**
- `DOCUMENTS OFFICIELS` : la nature des documents transportés n'est pas collectée dans le modèle ; le § 7 évoque les documents légaux du produit, pas le contenu des colis. **À clarifier.**
- Aucun poids maximal, aucune dimension, aucune restriction déclarative.
- « Paris (ORY) » vs « Paris (CDG) » dans la même page : **deux aéroports pour une même ville** sans règle de choix.

### 5.10 Carrousel « Engagements & Sécurité » (3 cartes réassurance)

`min-w-[310px] max-w-[310px]` — la carte la plus large de la page (sur 390 px de viewport, moins 32 px de marge : il reste ~48 px de la carte suivante visible, ce qui est un **bon indicateur de défilement**).

| Carte | Fond (dégradé) | Titre | Argument | Pastilles |
| --- | --- | --- | --- | --- |
| 1 | `from-[#ECFDF5] via-[#E6F8F0] to-[#D1FAE5]` | **GARANTIE ZECOLIS** | Une communauté qui voyage en confiance. | Identité vérifiée · Avis certifiés · Échanges garantis |
| 2 | `from-[#FFF7ED] via-[#FFEDD5] to-[#FED7AA]/60` | **PAIEMENT SÉCURISÉ & SÉQUESTRE** | Vos fonds protégés jusqu'à la remise. | Séquestre 100% garanti · Code OTP unique · Remboursement garanti |
| 3 | `from-[#F0F9FF] via-[#E0F2FE] to-[#BAE6FD]/60` | **CONTRÔLE & DOUANE** | Chaque colis inspecté et scellé. | Inspection conjointe · Scellé sécurisé · Assistance 24/7 |

Plus, dans chaque carte, une **tache décorative** : `absolute w-24 h-24 rounded-full blur-xl` en `bg-[#22A55A]/10`, `#FF6B00/10` ou `#0B2A5B/10`.

**C'est le bloc le plus risqué du fichier au regard du § 3** (« ne pas inventer de fonctionnalités ») :

| Revendication | Conséquence |
| --- | --- |
| « **Séquestre 100% garanti** » | Mécanisme financier fort, adossé à un paiement et à une libération conditionnelle. Aucun code, aucun prestataire nommé. |
| « **Code OTP unique** » | Notamment présent aussi dans le fichier 1 (onglet « Scellé IATA »), qui invente un mode « scellé IATA » absent du cahier des charges. **Deux fichiers inventent chacun un mécanisme OTP différent.** |
| « **Remboursement garanti** » | Politique de remboursement non définie dans les documents reçus |
| « **Identité vérifiée** », « **Avis certifiés** », « **Échanges garantis** » | Les trois font référence à des fonctions d'arbitrage et de modération non documentées |
| « **Inspection conjointe** », « **Scellé sécurisé** » | Inspection douanière conjointe : un **contrôle physique** par deux parties. Aucun workflow, aucun rôle douanier dans § 2.1 |
| « **Assistance 24/7** » | Engagement de niveau de service, non sourcé |
| « **Douane** », « Guide douane & règles bagages » | Le § 7 ne mentionne pas de guide douane |

Points de pagination **entièrement statiques** : `w-5 h-1 bg-[#FF6B00]` + 2 × `w-1.5 h-1 bg-slate-300`. Aucun `scroll-snap` callback, aucun indicateur suivant la position réelle du carrousel. **L'utilisateur ne sait pas qu'il y a 3 cartes** sauf en formatant.

### 5.11 Progression du profil

`Profil complété à 70%` + `Étape 2/3` + barre `bg-[#FF6A6B]00` `w-[70%]` + bouton `Compléter` (`bg-slate-100`).

**Aucune trace des 3 étapes.** Le § 6.6 et le § 6.7 prévoient la vérification et les informations personnelles, mais **la composition de « Étape 2/3 » n'est documentée nulle part**. Barres de progression statiques : c'est exactement le type d'« étape » que § 3 interdit de présenter comme franchie sans confirmation système.

### 5.12 Bouton flottant Scanner (FAB)

```html
<div class="fixed bottom-24 right-3.5 z-40 ...">
  <button onclick="document.getElementById('qr-scanner-modal')?.classList.remove('hidden')" ...>
```

`bottom-24` = 96 px, donc **posé au-dessus de la barre de navigation** (qui mesure ≈ 76 px). Gradient `from-[#0B2A5B] via-[#0D346E] to-[#071c3d]`, `shadow-2xl`, `backdrop-blur-md`, icône scanner, libellé `Scanner`, badge `LIVE` en `text-[8px]`, sous-titre `Colis & QR`, et une **pastille verte `animate-ping`** permanente.

**`LIVE` est une fausse indication d'état** : rien n'est « live », il n'y a ni flux ni caméra (§ 10.4). Même nature que le badge `AI Live` du fichier 1.

### 5.13 Modale scanner

`#qr-scanner-modal` — `fixed inset-0 z-50 bg-slate-900/90 backdrop-blur-md hidden flex flex-col justify-between p-4 max-w-[390px] mx-auto`.

| Zone | Contenu |
| --- | --- |
| Barre haute | Bouton fermer (⭠) + titre `Scanner Zecolis` + bouton flash |
| Centre | Badge `DÉTECTION EN DIRECT` + viseur `w-[280px] h-[270px] rounded-3xl` avec 4 coins (2 orange, 2 vert) + ligne de balayage `animate-pulse` + icône `w-16 h-16` + « Alignez le QR code du colis ou du voyageur » / « Lecture automatique sans contact » |
| Pastille | `Protégé par Zecolis Shield` en vert |
| Actions | `Importer une image` (`alert(...)`) · `Saisir le code PIN / OTP manuel` · `Annuler et retourner à l'accueil` |

**Cette modale est un doublon du fichier 1** (`scanner_colis_qr.html`), qui présente le même viseur, la même ligne de balayage, les mêmes coins orange/vert et les mêmes boutons. **Deux implémentations du même écran dans deux fichiers, avec des modes différents** : la modale n'a ni onglets de mode, ni import de photo réel, ni réglage de son, ni champ de saisie OTP — mais affiche « Saisir le code PIN / OTP manuel » **sans ouvrir aucun champ**.

Le § 3 impose de ne pas présenter une vérification comme réussie : « Lecture automatique sans contact », « Détection en direct » et « Protégé par Zecolis Shield » sont **trois affirmations de capacité de scan** sans une seule ligne de décodage.

## 6. Textes visibles (intégralité)

| Zone | Textes |
| --- | --- |
| Titre onglet | `Zecolis - Accueil` |
| En-tête | `Corridor Fiable` · `France ↔ Bénin` · `Bonjour, Riham 👋` · `Réseau Cotonou actif` |
| Hero | `FRANCE ↔ BÉNIN` · `Départs quotidiens` · `Votre colis peut voyager.` · `Trouvez quelqu'un qui part déjà.` |
| Recherche | `Vous allez où ?` · `Départ` · `Cotonou (COO)` · `Arrivée` · `France (Paris, Lyon...)` · `Rechercher` |
| Intentions | `Que voulez-vous faire ?` · `Expédition` / `Envoyer un colis` / `Je cherche un voyageur disponible` / `Continuer` · `Voyageur` / `Voyager avec un colis` / `Je propose de l'espace valise` / `Publier` |
| Activité | `1 envoi en cours` · `#COL-7852` · `2 colis • 1 voyage réservé` · `Suivi` |
| Voyages | `Des opportunités près de vous` · `Des trajets disponibles prochainement` · `Tout voir` · `VOYAGE CONFIRMÉ` · `Trajet direct` · `Vérifié` · `Voir le voyage` (×3) |
| Colis | `Des colis cherchent un voyageur` · `Prêts à être transportés dans votre valise` · `Colis prêt` · `Pli vérifié` · `Voir le colis` (×3) |
| Engagements | `Engagements & Sécurité` · `Faites glisser →` · `En savoir plus sur la garantie Zecolis →` · `Découvrir la protection des paiements →` · `Guide douane & règles bagages →` |
| Profil | `Profil complété à 70%` · `Étape 2/3` · `Compléter` |
| FAB | `Scanner` · `LIVE` · `Colis & QR` |
| Modale | `Scanner Zecolis` · `DÉTECTION EN DIRECT` · `Alignez le QR code du colis ou du voyageur` · `Lecture automatique sans contact` · `Protégé par Zecolis Shield` · `Importer une image` · `Saisir le code PIN / OTP manuel` · `Annuler et retourner à l'accueil` |
| Navigation | `Accueil` · `Recherche` · `Publier` · `Messages` (badge `2`) · `Profil` |

## 7. Champs et données

**Aucun champ de saisie dans tout le fichier.** Les deux champs d'itinéraire sont des `<span>`.

| Donnée affichée | Origine | Code correspondant |
| --- | --- | --- |
| Nom `Riham` | En dur | aucun |
| Avatar + badge vérifié | URL d'image statique | aucun |
| `#COL-7852` | En dur | aucun |
| `2 colis • 1 voyage réservé` | En dur | aucun |
| `Réseau Cotonou actif` | En dur | aucun |
| `Profil complété à 70%` / `Étape 2/3` | En dur | aucun |
| 3 voyages (dates, kg, €/kg, notes, avis) | En dur | aucun |
| 3 colis (poids, prix, statut, catégorie) | En dur | aucun |
| Badge `LIVE` | En dur | aucun |
| Badge de notification (cloche) | En dur | aucun |
| Badge `2` sur Messages | En dur | aucun |

**Aucun appel réseau : ni `fetch`, ni XHR, ni Firebase, ni fonction de données.** Tout le contenu est du HTML statique. C'est le seul fichier reçu dans ce cas — les fichiers 2, 3 et 4 contiennent au moins une logique de formulaire.

## 8. Boutons et actions

| # | Élément | Type | Comportement réel |
| --- | --- | --- | --- |
| 1 | Bouton flottant Scanner | `<button>` + `onclick` | **Réel** — affiche `#qr-scanner-modal` |
| 2 | Fermer modale | `<button>` + `onclick` | **Réel** — masque la modale |
| 3 | Bouton flash | `<button>` + `onclick` | **Simulé** — bascule `bg-amber-500` / `text-slate-900`, **n'accède pas au flash de l'appareil** |
| 4 | Annuler modale | `<button>` + `onclick` | **Réel** — masque la modale |
| 5 | Importer une image | `<button>` + `alert()` | **Factice** — un `alert()` bloquant ; **aucun `input type="file"`** |
| 6 | Saisir le code PIN / OTP | `<button>` + `onclick` | **Inopérant** — ferme la modale au lieu d'ouvrir un champ |
| 7 | Inverser le trajet | `<button>` | **Inopérant** |
| 8 | Rechercher | `<button>` | **Inopérant** |
| 9 | `Publier` (centre de la nav) | `<button>` | **Inopérant** |
| 10 | Cloche notifications | `<button>` | **Inopérant** |
| 11 | Avatar | `<div>` cliquable | **Inopérant** |
| 12 | 12 ancres `#...` | `<a>` | **Inopérantes** |
| 13 | 6 boutons `Voir le voyage` / `Voir le colis` | `<button>` | **Inopérants** |
| 14 | Carousels horizontaux | `overflow-x-auto` | **Seul comportement réel** — le défilement tactile fonctionne, `snap-x` compris |

Sur 20 actions : **6 réelles** (dont 2 se contentent d'ouvrir/fermer une modale vide, 1 affiche une `alert`, 1 ne fait qu'un changement de couleur), **14 inopérantes**.

## 9. Interactions et comportements observés

### 9.1 JavaScript

**Un seul bloc de script de 7 lignes** dans tout le fichier :

```js
document.addEventListener('DOMContentLoaded', () => {
  if (window.lucide) { window.lucide.createIcons(); }
});
```

C'est le fichier le plus **pauvre en logique** des cinq reçus, malgré être le plus long (683 lignes) et le plus riche visuellement. Toutes les interactions sont en attributs `onclick` d'une ligne.

Bonne pratique isolée : le garde `if (window.lucide)` **teste l'existence de la bibliothèque** avant de l'appeler — seul fichier des cinq à le faire.

### 9.2 Lucide et SVG pré-rendus

Les icônes des carrousels, de l'en-tête et de la barre de navigation sont des **SVG bruts pré-rendus** (`class="lucide lucide-home ..."`, `width="24" height="24"`, `<path>` explicites) : trace d'un `createIcons()` executed puis collé dans le fichier. Les icônes du bouton flottant et de la modale sont en revanche des **SVG écrits à la main**, sans `data-lucide`.

Conséquence : `lucide.createIcons()` au `DOMContentLoaded` régénère les icônes qui portent `data-lucide`, et **laisse intactes** les autres. Deux modes de représentation cohabitent dans la même page.

### 9.3 Carrousels

Trois conteneurs `overflow-x-auto snap-x no-scrollbar`. Le défilement tactile fonctionne nativement. **Mais :**

- aucun indicateur de position réel (les 3 points de pagination sont **codés en dur** et ne bougent jamais) ;
- aucun bouton précédent/suivant ;
- aucune gestion du clavier ;
- aucun `scroll-snap-align` autre que `snap-start` ;
- `.no-scrollbar` masque la barre de défilement sur Chrome/Edge et `scrollbar-width: none` sur Firefox — **l'utilisateur n'a aucun indice visuel** qu'il peut faire défiler horizontalement, hormis le débordement partiel de la carte suivante.

C'est le point le plus important en accessibilité de ces carrousels : **le contenu horizontal est invisible quant à son étendue**.

### 9.4 Comportements d'interface notables

| Comportement | Observation |
| --- | --- |
| Barre de navigation fixe | `fixed bottom-0 ... pb-2` + `pointer-events-none` sur le `<nav>` et `pointer-events-auto` sur le conteneur : **bonne pratique**, la zone autour du dock ne bloque pas les clics |
| Marqueur d'onglet actif | Point orange `w-1 h-1` sous `Accueil` ; les autres onglets ont `bg-transparent` — l'onglet actif est signalé **à la fois** par la couleur orange du texte et par le point |
| `hover:` sur mobile | `group-hover:scale-105` sur les boutons-puces, `hover:underline` sur plusieurs liens : **effets sans effet** sur tactile |
| `shadow-dock` sur la barre | Double ombre douce — cohérente avec « ombres légères » |
| Zone de safe area | **Absente** : ni `viewport-fit=cover`, ni `env(safe-area-inset-*)`. Le `<div class="h-4 w-full">` dans le `<nav>` est un **espaceur factice** de 16 px, censé remplacer l'indicateur d'accueil que l'appareil dessine déjà |

## 10. États possibles

| État | Implémenté | Observation |
| --- | --- | --- |
| Accueil, utilisateur connecté | **Oui** | Rendu statique |
| Accueil, utilisateur non connecté | **Non** | Aucun état invited, aucune redirection : un visiteur non connecté verrait « Bonjour, Riham » |
| Page de résultats de recherche | **Non** | Le bouton Rechercher ne fait rien ; aucun état vide, aucun état « aucun résultat » |
| Modale scanner ouverte / fermée | **Oui** | `hidden` basculé — **seul état réellement interactif de la page** |
| Modale scanner — code en cours | **Non** | Le bouton « Saisir le code PIN / OTP » ferme la modale au lieu d'ouvrir un clavier |
| Modale scanner — import de photo | **Non** | `alert()` bloquant |
| Modale scanner — flash actif | **Oui (visuel)** | Simple bascule de couleur |
| Carrousel — position | **Non** | Pagination codée en dur |
| Chargement / squelette | **Non** | Aucun état de chargement |
| Erreur réseau | **Non** | Aucun |
| Session expirée | **Non** | Aucun |
| Notifications non lues | **Non** | Pastille orange codée en dur, badge `2` codé en dur |

**Sur 12 états applicatifs plausibles, 2 sont atteignables.**

## 11. Navigation vers les autres écrans

**C'est le point le plus critique du fichier.** Le § 4.5 du cahier des charges définit une navigation principale ; elle est **présente visuellement et non fonctionnelle**.

| Écran cible | Lien | État |
| --- | --- | --- |
| Recherche | `Recherche` → `#recherche` | **Inopérant** |
| Messages | `Messages` → `#messages` | **Inopérant** |
| Profil | `Profil` → `#profil` | **Inopérant** |
| Accueil | `Accueil` → `#accueil` | **Inopérant** — devrait être la racine |
| Publication d'annonce | `Publier` (bouton) | **Inopérant** — § 3 du cahier des charges |
| Publication de voyage | `#voyager` | **Inopérant** |
| Publication de colis | `#envoyer` | **Inopérant** |
| Suivi de colis | `#suivi` | **Inopérant** |
| Marketplace | `#marketplace` | **Inopérant** |
| Complétion du profil | `#profile-edit` | **Inopérant** |
| Scanner | bouton flottant | **Ouverture de modale ✔** |
| Notifications, avatar | clic | **Inopérant** |
| Connexion | — | **Absent** (et non nécessaire si l'on est connecté) |

**Aucun des fichiers reçus n'est relié à cet écran**, et cet écran ne mène nulle part. Le schéma `Accueil ⇄ Connexion → Inscription → Mot de passe oublié` est **entièrement rompu** : les quatre fichiers sont quatre îlots.

## 12. Règles métier visibles dans le code

| Règle | Implémentation |
| --- | --- |
| Tarification voyageur | **Au kilogramme** (8, 9, 10 € / kg), capacité disponible affichée en kg |
| Tarification colis | **Au kilogramme** (12, 15 € / kg) **ou au forfait** (20 €) — **sans règle de choix** |
| Modèle de capacité | **Espace valise du voyageur** (« Je propose de l'espace valise ») |
| Statuts d'annonce | `VOYAGE CONFIRMÉ` (×3), `Colis prêt` (×2), `Pli vérifié` (×1) — **jamais définis** |
| Critère « trajet direct » | Mentionné 3 fois, **aucun trajet indirect dans l'application** |
| Recherche par itinéraire | Champs **non éditables** : aucune règle applicable |
| Détection de correspondance colis ↔ voyage | **Absente** |
| Alertes de réception (avant arrivée) | **Absentes** — pourtant c'est le cœur d'un transport par Collegiate : § 6 mentionne des notifications |
| Annulation / remboursement | Mentionnée (« Remboursement garanti ») **sans règle** |
| Vérification d'identité | Mentionnée (« Identité vérifiée ») **sans règle** |

**Aucune règle n'est implémentée : toutes sont affirmées par le texte.**

Point notable : **le § 3 « Correspondance colis ↔ voyage » n'a aucune représentation dans l'application** — ni sur l'accueil, ni ailleurs. Le cœur fonctionnel du produit est absent de tous les fichiers reçus.

## 13. Dépendances techniques

| Type | Valeur | Remarque |
| --- | --- | --- |
| Tailwind | `cdn.tailwindcss.com?plugins=forms,container-queries` | Idem fichier 4 : plugins chargés, donc **rendu des champs différent** des fichiers 2 et 3 |
| Police | **aucune** — `fontFamily` déclare `['Inter','Plus Jakarta Sans','system-ui']` sans charger aucune | ✘ rendu réel non garanti |
| Icônes | `unpkg.com/lucide@latest` | Non épinglé ; ~20 SVG pré-rendus + ~8 SVG manuels |
| Images | **11 URLs** `lh3.googleusercontent.com` (logo, avatar, hero, 2 cartes d'intention, 3 portraits, 3 photos de colis) | Toutes distantes, non versionnées, **dépendantes d'un compte Google** |
| CSS | 1 bloc : `*`, `body`, `.no-scrollbar` (2 règles) | 16 lignes |
| JS | 1 bloc | **7 lignes** — le plus petit script des cinq fichiers reçus |
| Build | Aucun | Ni Vite, ni React, ni TypeScript, ni Firebase, ni Capacitor |
| Encodage | UTF-8 sans BOM, `<meta charset="utf-8">` | ✔ |

**Aucune dépendance applicative.** Le fichier est un document statique de 683 lignes dont **13 ressources réseau distinctes** : Tailwind (CDN), Lucide (CDN `unpkg`) et 11 images Google.

## 14. Éléments provisoires ou fictifs

### Fictif — données

| # | Élément |
| --- | --- |
| 1 | Utilisateur `Riham` + avatar + badge vérifié |
| 2 | Point de notification sur la cloche |
| 3 | Badge `2` sur l'onglet Messages |
| 4 | `Réseau Cotonou actif` + point vert pulsé |
| 5 | `1 envoi en cours #COL-7852` |
| 6 | `2 colis • 1 voyage réservé` |
| 7 | `Profil complété à 70%` / `Étape 2/3` |
| 8 | 3 voyageurs nommés : KPO Ivy, Koffi D., Amina S. |
| 9 | 3 badges `Vérifié` / `Vérifiée` |
| 10 | 3 notes : ★ 4,9 (34 avis vérifiés) · ★ 5,0 (18 avis) · ★ 4,9 (42 avis) |
| 11 | 3 dates : 14, 18, 22 mai 2025 |
| 12 | 6 prix (8, 9, 10, 12, 15 €/kg + 20 € forfait) |
| 13 | 3 capacités (9, 14, 6 kg) et 3 poids (5, 3,2, 1,2 kg) |
| 14 | 3 colis décrits (carton scellé, appareil photo, dossier notarié) |

### Fictif — affirmations de capacité

| # | Affirmation | Problème |
| --- | --- | --- |
| 15 | `Départs quotidiens` | Promesse de service non sourcée |
| 16 | `Corridor Fiable` | Argumentaire marketing absent du cahier des charges |
| 17 | Badge `LIVE` sur le bouton Scanner | Rien n'est « live » : ni caméra, ni décodage |
| 18 | `DÉTECTION EN DIRECT` (modale) | Aucun traitement d'image |
| 19 | `Lecture automatique sans contact` | Aucun décodage |
| 20 | `Protégé par Zecolis Shield` | « Shield » n'existe nulle part ailleurs dans les documents |
| 21 | `Séquestre 100% garanti` | Aucun paiement, aucun prestataire |
| 22 | `Code OTP unique` | Aucun champ, aucun clavier, aucun serveur |
| 23 | `Remboursement garanti` | Aucune politique |
| 24 | `Identité vérifiée` / `Avis certifiés` / `Échanges garantis` | Aucun dispositif |
| 25 | `Inspection conjointe` / `Scellé sécurisé` | Aucun workflow, aucun tiers |
| 26 | `Assistance 24/7` | Engagement de service non sourcé |
| 27 | `VOYAGE CONFIRMÉ` (×3) | Aucun statut défini |
| 28 | `Pli vérifié` | Concept absent du cahier des charges |
| 29 | `Profil complété à 70%` / `Étape 2/3` | Étapes non documentées |

### Fictif — mise en scène

| # | Élément | Observation |
| --- | --- | --- |
| 30 | 11 images distantes Googleusercontent | Toutes non versionnées et dépendantes d'un compte tiers ; **le logo est différent sur chaque fichier reçu** |
| 31 | Photos « aéroport de Cotonou Cadjehoun », « Authentic photograph of a safely packaged… », « Dossier sous pli scellé avec cachet de cire… » | Images de banque d'images présentées comme des scènes du produit, avec des `alt` rédigés au présent |
| 32 | `w-32 h-1` dans le `<nav>` | **Pas l'indicateur d'accueil** cette fois : c'est un simple espacement de 16 px sous le dock |
| 33 | Pastille `animate-ping` permanente sur le FAB | Simule une activité permanente |

### Non inventé — à noter positivement

Les images utilisent des `alt` **détaillés et honnêtes** (« Colis soigneusement emballé prêt pour le départ à Cotonou avec valise et passeport », « Dossier sous pli scellé avec cachet de cire pour expédition officielle »). C'est une **bonne pratique d'accessibilité**, rare dans les maquettes. De même, `selection:bg-[#FF6B00]/20 selection:text-[#FF6B00]` sur le `<body>` autorise la sélection de texte — **contrairement aux quatre autres fichiers** (`select-none` ou `user-select: none`) : seul fichier des cinq sans blocage de sélection.

## 15. Écarts avec l'identité visuelle de référence

| Point | Référence § 4 | Ce fichier | Écart |
| --- | --- | --- | --- |
| **Police** | Inter | déclarée mais **non chargée** | ✘ rendu non garanti |
| Fond général | `#F8FAFC` | `#F8FAFC` | ✔ |
| Cartes | `#FFFFFF` | `bg-white` | ✔ |
| Bordures | `#F1F5F9` | `border-slate-100`, `border-slate-200`, `border-slate-200/80`, `border-slate-200/90` | ✔ (équivalents) |
| Texte | `#0F172A` / `#64748B` | `text-[#0F172A]`, `text-slate-500` / `text-slate-900` | ✔ |
| Navy / orange / vert | `#1D1D4B` / `#0B2A5B` / `#FF6B00` / `#22A55A` | ✔ + variantes | ✔ |
| **Dégradés** | **interdits** | **au moins 9** : hero `from/via/to`, 2 cartes d'intention, 3 cartes réassurance, 3 taches `blur-xl`, badge `bg-white/30` | ✘ **Divergence majeure** |
| **Glassmorphism** | **interdit** | **12 occurrences de `backdrop-blur`** : en-tête `backdrop-blur-md` sur `bg-white/95`, dock `backdrop-blur-lg` sur `bg-white/95`, badge `Voyageur` `backdrop-blur-md`, badge `Départs quotidiens` `backdrop-blur-sm`, FAB `backdrop-blur-md`, modale `backdrop-blur-md`, plus les 2 pastilles `backdrop-blur-md` des colis | ✘ **Divergence majeure** |
| **Couleurs hors palette** | § 4.2 | `slate-50/100/200/300/400/500/600/700/900`, `blue-50`, `blue-100`, `emerald-50/100`, `amber-300/400/500`, `black/40`, `white/10/30/80`, `ECFDF5`, `E6F8F0`, `D1FAE5`, `FFF7ED`, `FFEDD5`, `FED7AA`, `F0F9FF`, `E0F2FE`, `BAE6FD`, `147A3f`, `071c3d`, `0D346E`, `e05e00`, `CBD5E1`(non utilisé) | ✘ **Divergence majeure** : toute la palette Tailwind standard est utilisée |
| **Ombres** | « légères », éviter les « épaisses » | `shadow-2xl` (FAB), `shadow-lg` (carte de recherche, boutons), `shadow-2xl shadow-[#0B2A5B]/40`, `shadow-lg shadow-[#FF6B00]/40`, `shadow-md shadow-[#FF6B00]/25`, `shadow-dock` (25 px), `shadow-lg shadow-[#FF6B00]/30`, `shadow-2xl` (viseur) | ✘ **8 ombres marquées** |
| **Rayons** | cartes ~16 px, boutons ~12 px | `rounded-3xl` (24 px) sur 3 éléments : le hero, le viseur de la modale et le bloc blanc de la modale ; `rounded-2xl` (16 px) sur les cartes | ✘ à arbitrer |
| Bloc de statut en vert | vert réservé aux états positifs | `bg-emerald-50 text-[#22A55A]` sur `VOYAGE CONFIRMÉ` **et** `bg-blue-50 text-[#0B2A5B]` sur le **même statut** | ✘ incohérence interne |
| Bandeau de bas de page | non spécifié | **absent** | ✔ |
| Icônes | Lucide | Lucide (+ SVG manuels) | ✔ |

**Bilan : c'est le fichier le moins conforme à § 4 des cinq reçus**, sur les points exacts que § 4.3 et § 4.4 interdisent explicitement : **dégradés** et **glassmorphism**. Les valeurs de palette Zecolis sont bien présentes, mais noyées dans une utilisation massive de la palette Tailwind standard.

## 16. Problèmes mobiles et accessibilité

*(Analyse statique du code — aucun navigateur, aucun appareil réel utilisé. Aucun correctif n'a été appliqué à ce fichier.)*

### 16.1 Défilement vertical — correct

| Élément | Valeur | Effet |
| --- | --- | --- |
| `<main>` | `min-h-screen` | croît avec le contenu ✔ |
| `<body>` | `min-h-full`, `overflow-x-hidden` | **aucun `overflow-hidden`** |
| Marge basse | `pb-28` (112 px) | compense la barre de navigation fixe ✔ |

**Le défilement vertical fonctionne.** Contrairement au fichier 1, cette page longue est défilable. Le `overflow-x-hidden` du `<body>` est nécessaire et sans danger : il empêche le débordement des carrousels `-mx-4 px-4` de créer un défilement horizontal de page.

Point d'attention : `touch-action: manipulation` sur `body` et `overscroll-behavior-y: none` — le pull-to-refresh est supprimé, ce qui est discutable sur un écran d'accueil (on s'attend au pull-to-refresh pour actualiser un fil d'actualité).

### 16.2 Zones de sécurité — manquantes, et deux éléments fixes

C'est le fichier le plus exposé : il cumule **barre de navigation fixe en bas** et **bouton flottant fixe**.

| Élément | Présent | Impact |
| --- | --- | --- |
| `viewport-fit=cover` | **Non** | — |
| `env(safe-area-inset-top)` | **Non** — `pt-3` (12 px) | L'en-tête `sticky` démarre **sous l'encoche** : logo et libellé « Corridor Fiable » sont à 12 px du haut de l'écran |
| `env(safe-area-inset-bottom)` | **Non** — `pb-2` (8 px) + `h-4` factice (16 px) | La barre de navigation se place **sur la barre de gestes iOS** (≈ 34 px) ; les onglets `Messages` et `Profil` deviennent difficiles à atteindre, voire cliquables au mauvais endroit |
| Bouton flottant | `fixed bottom-24` (96 px) | Au-dessus de la barre, mais **le calcul ignore la safe area** ; à 96 px du bord bas, il reste 96 px, ce qui tient sur un iPhone à barre d'accueil, mais pas si la barre monte |
| Marge du `<main>` | `pb-28` (112 px) | Compense le dock (≈ 76 px) mais **pas** la safe area |

> **Correction prioritaire :** sur un iPhone à encoche, les 4 onglets inférieurs sont dans la zone du geste « swipe vers le haut ». C'est le défaut mobile le plus concret de ce fichier, devant le FAB.

### 16.3 Zones tactiles trop petites

Recensement exhaustif des **28 éléments interactifs** du fichier (16 `<button>` + 12 `<a href="#…">`) :

| Élément | Taille estimée | Verdict |
| --- | --- | --- |
| Bouton central `Publier` | `w-12 h-12` = **48 px**, surelevé de `-mt-6` | ✔ (cible confortable) |
| FAB scanner | `py-3.5` + icône, ≈ **56 px** de haut | ✔ |
| Bouton `Rechercher` | `py-3.5 text-sm` ≈ **49 px** | ✔ |
| Bouton `Importer une image` | `py-3.5 text-sm` ≈ **49 px** | ✔ |
| Onglets de la barre de navigation (×4) | `w-14` = 56 px de large ; hauteur `py-1` + icône 24 + `mt-0.5` + texte 15 + point 4 ≈ **55 px** | ✔ **conforme** |
| Cartes d'intention `#envoyer` / `#voyager` | `h-36` = **144 px** | ✔ |
| **Cloche** | `w-9 h-9` = **36 px** | ✘ |
| **Avatar** (`<div cursor-pointer>`) | `w-9 h-9` = **36 px** | ✘ (aucune zone élargie) |
| **Bouton inversion du trajet** | `w-8 h-8` = **32 px** | ✘ |
| **Bouton fermer la modale** | `w-10 h-10` = **40 px** | ✘ |
| **Bouton flash de la modale** | `w-10 h-10` = **40 px** | ✘ |
| **6 boutons `Voir le voyage` / `Voir le colis`** | `py-2.5 text-xs` ≈ **38 px** | ✘ |
| **Bouton `Saisir le code PIN / OTP manuel`** | `py-2.5 text-xs` ≈ **38 px** | ✘ |
| **Bouton `Annuler et retourner à l'accueil`** | `py-1.5 text-xs` ≈ **30 px** | ✘ |
| **Lien `Compléter`** | `py-1.5 text-xs` ≈ **30 px** | ✘ |
| **3 liens « en savoir plus → »** | `text-[11px] inline-block` ≈ **17 px** | ✘ (le pire cas du fichier) |
| **Liens `Suivi` / `Tout voir`** | `text-xs` sans marge ≈ **18 px** | ✘ |
| « Faites glisser → » | `text-[10px]`, non cliquable | — |
| Points de pagination | `h-1`, non cliquables | — |

**Bilan : 18 éléments interactifs sous le minimum de 44 × 44 px** (12 boutons + 6 liens), plus l'avatar en 36 px. **La barre de navigation principale — les 4 onglets — est en revanche conforme** (≈ 55 px) : c'est le seul point d'entrée majeur bien dimensionné, avec le bouton `Publier`.

Les plus critiques sont les **3 liens « en savoir plus » (17 px)**, qui portent les engagements de confiance de la page, et les **6 CTA de carte (38 px)**, seules actions métier de l'écran.

### 16.4 Barre de navigation — points positifs et négatifs

| Point | Observation |
| --- | --- |
| `pointer-events-none` sur le `<nav>` + `pointer-events-auto` sur le conteneur | ✔ **Bonne pratique** : la zone autour du dock ne bloque pas les clics sur le contenu |
| `max-w-[390px]` + centrage | ✔ le dock ne s'étire pas sur tablette |
| Zone tactile de l'onglet actif | le point `w-1 h-1` n'est pas cliquable et sert uniquement d'indicateur |
| Accessibilité de l'onglet actif | **Aucun `aria-current="page"`** : un lecteur d'écran ne sait pas quel onglet est actif |
| Accessibilité des onglets | **Aucune barre de navigation sémantique** au sens ARIA (`role="tablist"`, etc.) ; le `<nav>` est correct mais les 5 liens portent des ancres vides |
| Navigation au clavier | Les onglets sont des `<a href="#…">` : focalisables, mais activables sans effet |
| `hover:` sur les liens | `active:text-slate-600` présent ✔ (retour tactile correct) |

### 16.5 Zoom désactivé

```html
<meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

**Quatrième fichier consécutif** avec le zoom pincement interdit. Manquement WCAG 1.4.4 et 1.4.10. Sur cette page, l'aggravation est nette : il existe des textes à `text-[8px]` (badge `LIVE`), `text-[9px]` (labels `Départ` / `Arrivée`), `text-[10px]` (au moins 8 occurrences). **La page la plus riche en information est aussi la plus petite en typographie.**

### 16.6 Typographie — inventaire des très petites tailles

| Taille | Occurrences |
| --- | --- |
| `text-[8px]` | badge `LIVE` sur le FAB — **le plus petit texte de toute l'application** |
| `text-[9px]` | labels `Départ`, `Arrivée` (2 occurrences) |
| `text-[10px]` | `Corridor Fiable`, badges `FRANCE ↔ BÉNIN` et `Départs quotidiens`, `VOYAGE CONFIRMÉ` (×3), catégories de colis (×3), `Réseau Cotonou actif` (11 px), `Faites glisser →`, pastilles de réassurance (×9), `Protégé par Zecolis Shield`, `text-slate-500` des cartes |
| `text-[11px]` | `Vous allez où ?`, `2 colis • 1 voyage réservé`, notes et avis, liens « en savoir plus » (×3) |
| `text-[11.5px]` | — |

**Les badges `VOYAGE CONFIRMÉ`, les catégories de colis et les 9 pastilles de réassurance sont à 10 px.** C'est exactement le contenu qui porte les **affirmations de garantie** : il est rendu dans la plus petite taille possible, la moins lisible. Contraste aggravé : `text-[#0B2A5B]` sur `bg-blue-50` fonctionne, mais `text-slate-400` sur `#F8FAFC` (texte `Vous allez où ?`, `Faites glisser →`) est à **≈ 2,9:1**, sous le seuil AA.

### 16.7 Largeur, débordements et défilement horizontal

| Point | Observation |
| --- | --- |
| `max-w-[390px]` sur le `<main>` | ✔ conforme à § 4.5 |
| Carrousels `-mx-4 px-4` | Le débordement de 16 px de chaque côté est **correct** (les cartes touchent les bords de l'écran), et neutralisé au niveau du `<body>` par `overflow-x-hidden` |
| Cartes `w-[285px]` / `w-[280px]` / `310px` | Sur 390 px : 1 carte + ~65 px de la suivante → **indice de défilement correct** |
| `min-w-[310px] max-w-[310px]` sur les cartes de réassurance | Sur 342 px utiles : ~32 px de la suivante visible. **Indice plus faible** que les autres carrousels |
| Sous 320 px | `mx-4` (32 px) laisse 288 px ; les cartes de 310 px débordent → le carrousel reste utilisable, mais la carte de réassurance est **plus large que l'écran utile** |
| Paysage / pliants | **Aucun traitement.** En paysage sur un téléphone, `max-w-[390px]` produit une colonne étroite centrée avec de grands vides latéraux, et le dock flotte au milieu de l'écran |
| `min-h-screen` sur le `<main>` | En paysage (390 px de haut), la page est plus haute que le viewport : le défilement est nécessaire, ✔ il fonctionne |

### 16.8 Autres points d'accessibilité

| Point | Observation |
| --- | --- |
| Contraste du sous-titre `text-slate-400` | ≈ 2,9:1 sur `#F8FAFC` — **non conforme AA** |
| Contraste `text-slate-500` | ≈ 4,0:1 — **limite, sous AA** pour le texte courant |
| Contraste des prix `text-[#FF6B00]` sur blanc | ≈ 3,1:1 — **non conforme AA** pour du texte de 12–14 px ; c'est pourtant le prix, information financière principale |
| Contraste `★` en `amber-500` | ≈ 2,3:1 — non conforme |
| `aria-hidden` sur les SVG | ✔ **present sur toutes les icônes Lucide pré-rendues** |
| Icônes SVG manuels (FAB, modale) | **sans `aria-hidden`**, mais les boutons portent `aria-label` ✔ |
| Alt des images | **détaillés et pertinents** ✔ — meilleure pratique, à saluer |
| `aria-label` des boutons icône | ✔ présents : `Notifications`, `Ouvrir le scanner Zecolis`, `Fermer le scanner`, `Activer le flash`, `Publier une annonce`, `Inverser le trajet` |
| **Focus visible** | **Aucun `:focus-visible` dans le fichier.** Les `<a>` et `<button>` ont un focus navigateur par défaut, mais `outline` n'est retiré nulle part : la visibilité dépend donc du navigateur, et sur fond blanc les contours bleus par défaut sont peu contrastés |
| Modale — gestion du focus | **Absente** : pas de `role="dialog"`, pas de `aria-modal="true"`, **pas de piège de focus**, pas de fermeture par `Échap`. Un utilisateur clavier peut rester « derrière » la modale. |
| Modale — Annuler / OTP | Le bouton « Saisir le code PIN / OTP manuel » **ferme la modale** au lieu d'ouvrir un champ : l'étiquette annonce une action qui n'a pas lieu. |
| Carrousels | Aucun `role="region"` + `aria-label`, aucun `aria-roledescription="carrousel"`, barre de défilement masquée sans hint |
| Pagination des cartes de réassurance | 3 points sans `aria-label`, sans état `active` |
| Pagination des 2 autres carrousels | **Absente** → l'utilisateur ignore qu'il y a 3 éléments |
| Statuts (`VOYAGE CONFIRMÉ`, `Colis prêt`, `Pli vérifié`) | Texte simple, pas de `role="status"` — acceptable en statique |
| Notifications / badge `2` | Pastille sans `aria-label` (« 2 notifications non lues ») → **inaccessible** |
| Barre `w-[70%]` | Barre de progression **sans `role="progressbar"` ni `aria-valuenow`** |
| Avatar avec badge « vérifié » | Le badge de vérification est purement visuel, **sans texte alternatif** — un lecteur d'écran ne lira pas « profil vérifié » |
| `animate-pulse` / `animate-ping` | Animations non désactivées par `prefers-reduced-motion` : **5 animations permanentes** — 4 `animate-pulse` (point réseau, point d'activité, point vert du FAB, ligne de balayage) et 1 `animate-ping` (halo du FAB). Sur `prefers-reduced-motion`, ces animations doivent être arrêtées. |
| Sélection de texte | `selection:bg-[#FF6B00]/20` ✔ **autorisée** — seul fichier des cinq dans ce cas |

## 17. Points à vérifier

1. **Nom de fichier d'origine** — non communiqué.
2. **Barre de navigation entièrement morte** — 4 onglets + bouton Publier sans effet. **Priorité 1 : c'est la navigation principale du § 4.5.**
3. **Modale scanner : doublon du fichier 1** — deux implémentations du même écran, avec des modes différents. Laquelle fait référence ? **À arbitrer avant tout développement.**
4. **Bouton « Saisir le code PIN / OTP manuel » qui ferme la modale** au lieu d'ouvrir un champ — bug ou étiquette erronée ?
5. **`Importer une image` = `alert()`** — aucun `input type="file"` ; à remplacer.
6. **Recherche d'itinéraire non saisissable** — `Départ` et `Arrivée` sont des `<span>`. Le § 2.3 exige une recherche par itinéraire : écrans manquants.
7. **Aucune logique applicative** — 683 lignes, 7 lignes de JS. Confirmer qu'il s'agit d'une maquette visuelle et non d'une page en développement.
8. **Données fictives à confirmer ou supprimer** — `Riham`, `#COL-7852`, 3 profils, 3 notes, 94 avis, 3 dates de mai 2025, 6 prix, progression 70 %. **§ 3 : ne pas inventer.**
9. **Modèle de tarification** — €/kg pour les trajets et 2 colis, forfait pour le 3ᵉ. Quelle règle ? Qui la fixe ? Le cahier des charges § 9.1 interdit d'inventer un processus de paiement.
10. **« Départs quotidiens »** — promesse de service : sourcée ou à retirer ?
11. **« Corridor Fiable »** — argumentaire marketing : validé par le donneur d'ordre ?
12. **29 affirmations de capacité** (garantie, séquestre, OTP, remboursement, inspection, identité vérifiée, avis certifiés, assistance 24/7, `LIVE`, `Détection en direct`…). **§ 3 : à confirmer une par une ou à retirer.**
13. **« Code OTP unique » vs « Scellé IATA » du fichier 1** — deux mécanismes inventés, incompatibles. Lequel est retenu ?
14. **Statut `Pli vérifié`** — concept non documenté : valider ou retirer.
15. **`Profil complété à 70%` / `Étape 2/3`** — quelles sont les 3 étapes ? (§ 6.6, § 6.7)
16. **Statut `VOYAGE CONFIRMÉ` avec 2 couleurs de badge** — incohérence interne à corriger.
17. **Mention « Je propose de l'espace valise »** — confirme que la capacité est celle de la valise du voyageur, pas un volume négocié. Valider ce modèle.
18. **`Paris (CDG)` et `Paris (ORY)`** — deux aéroports pour une même ville ; règle de choix à définir.
19. **Absence totale de « correspondance colis ↔ voyage »** — le § 3 est le cœur du produit ; aucune trace dans les fichiers reçus. **Point majeur à traiter.**
20. **Absence de notifications / alertes de réception** — le § 6 en prévoit ; la cloche est décorative.
21. **Safe areas sur deux éléments fixes** — dock en bas et FAB : `pt-3` / `pb-2` sans `env()`. **Priorité 2.**
22. **18 cibles tactiles sous 44 px sur 28 éléments interactifs** (12 boutons + 6 liens), dont les 3 liens « en savoir plus » à 17 px et les 6 CTA de carte à 38 px. Les 4 onglets de navigation sont en revanche conformes (≈ 55 px).
23. **Zoom désactivé** — 4ᵉ fichier consécutif ; correction globale.
24. **Contraste** — prix en `#FF6B00` (3,1:1), `★` en `amber-500` (2,3:1), `text-slate-400` (2,9:1) : **les informations financières et les notes ne sont pas lisibles par un utilisateur malvoyant.**
25. **Modale non conforme ARIA** — ni `role="dialog"`, ni piège de focus, ni fermeture par `Échap`. **À corriger.**
26. **Pagination codée en dur** et absente sur 2 carrousels ; barres de défilement masquées → étendue du contenu invisible.
27. **Aucune animation désactivée sous `prefers-reduced-motion`** — 5 animations permanentes (4 `animate-pulse`, 1 `animate-ping`).
28. **Base de progression sans `role="progressbar"`** ; badge de notification sans `aria-label` ; onglet actif sans `aria-current`.
29. **Palette** — au moins 20 teintes hors § 4.2, **9 dégradés**, **12 `backdrop-blur`** (glassmorphism), **8 ombres marquées**, `rounded-3xl` sur 3 éléments. **Divergence majeure avec § 4.3 et § 4.4.**
30. **Police non chargée** — `fontFamily` déclare Inter, aucune requête Google Fonts dans le fichier. **Seul fichier des cinq dans ce cas.**
31. **Convention de nommage de la palette** — 3 conventions en 4 fichiers (`zecolis.*`, `brand.*`, `zecolis-*`), aucune réellement utilisée. **Source de vérité à établir.**
32. **Aucune image maîtrisée** — 11 URLs `lh3.googleusercontent.com`, dont un logo différent sur chaque fichier. **À remplacer par des assets versionnés.**
33. **3 photos de banque d'images** présentées comme des scènes du produit (aéroport de Cotonou, emballage « Authentic photograph of… », pli scellé à cachet de cire), avec des `alt` rédigés au présent. **À arbitrer.**
34. **`touch-action: manipulation` + `overscroll-behavior-y: none`** — pas de pull-to-refresh sur un écran d'accueil ; confirmer si c'est voulu.
35. **Aucun test navigateur possible ici** —sticky, carrousels, modale, safe areas et contrastes restent à vérifier sur appareil.

## 18. En résumé

Le **deuxième écran principal** de l'application — celui qui définit la navigation du § 4.5, l'identité visuelle perçue et le modèle d'offre (trajets au kg contre valise). C'est aussi le fichier le plus riche en contenu : 8 sections, 2 carrousels de marché, un bandeau de réassurance, une progression de profil et une modale scanner.

Mais **presque tout est décoratif** : 683 lignes pour **7 lignes de JavaScript**. Douze ancres mortes, trois boutons sans gestionnaire, un avatar « cliquable » qui ne fait rien, une recherche d'itinéraire dont les deux champs sont du texte figé, un bouton d'inversion qui n'inverse rien, une cloche sans gestionnaire, et un bouton « Publier » sans effet. **La barre de navigation principale du § 4.5 est présente à l'écran et entièrement inopérante.** Le seul comportement réel de la page est l'ouverture et la fermeture d'une modale scanner qui est elle-même un **doublon du fichier 1** — sans caméra, sans décodage, avec un bouton « Importer une image » qui déclenche une `alert()` et un bouton « Saisir le code PIN / OTP » qui ferme la modale au lieu d'ouvrir un clavier.

Deux constats de fond à remonter au donneur d'ordre. **Premier : les données.** Utilisateur « Riham », envoi `#COL-7852`, trois profils vérifiés, trois notes (4,9 / 5,0 / 4,9) et 94 avis, trois dates de mai 2025, six prix, progression à 70 % : le § 3 interdit d'inventer ces éléments, et le modèle de tarification lui-même (€/kg contre forfait, capacité valise) est **affirmé sans aucune règle documentée**. **Second : les 29 affirmations de capacité** — séquestre garanti, code OTP, inspection conjointe, identité vérifiée, `LIVE`, « Détection en direct » — dont le § 3 interdit la présentation sans confirmation système, et qui entrent en contradiction avec le fichier 1 (qui invente de son côté un mode « Scellé IATA »). Enfin, l'absence totale de la « correspondance colis ↔ voyage » dans les quatre fichiers reçus est à noter : **c'est la fonction centrale du produit et elle n'existe nulle part dans le code transmis.**

Côté design, c'est le fichier le moins conforme à § 4 : 9 dégradés et 12 effets `backdrop-blur`, tous deux explicitement interdits, plus de 20 teintes hors palette, 8 ombres marquées et `rounded-3xl` sur le hero, le dock et le viseur. Il est aussi le seul à déclarer une police **sans jamais la charger**. Côté mobile : le défilement vertical fonctionne, mais deux éléments fixes (dock et bouton flottant) sont placés sans aucune safe area — les onglets du bas tombent dans la zone du geste iOS — et huit cibles tactiles sont sous 44 px, dont les quatre onglets de navigation.