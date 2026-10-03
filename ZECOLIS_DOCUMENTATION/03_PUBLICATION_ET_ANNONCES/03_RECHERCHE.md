# 03 — Recherche (results feed)

| Rubrique | Valeur |
| --- | --- |
| **Fichier analysé** | `SOURCES_HTML_ORIGINALES/recherche.html` (28 862 octets, 551 lignes) — **original non modifié** |
| **Nature du travail** | Analyse statique du code uniquement |
| **Limite** | **Aucun navigateur, aucun appareil réel.** Tous les constats sont déduits du code. |
| **Catégorie retenue** | `03_PUBLICATION_ET_ANNONCES` — l'écran melange la recherche de voyageurs, la recherche de colis à transporter et la publication. La catégorie est **à arbitrer** (voir § 9, R-Q1). |
| **Corrections mobile** | `03_PUBLICATION_ET_ANNONCES/03_RECHERCHE_CORRECTIONS_MOBILE.md` |

> **Nom de fichier provisoire.** Le donneur d'ordre n'a communiqué aucun nom d'origine.

---

## 1. Vue d'ensemble

**Le fichier le plus ambitieux reçu après l'accueil, et le seul qui ait pensé au mode d'emploi à double entrée** (expéditeur / voyageur). C'est aussi le seul écran où la barre de navigation principale du § 4.5 est reconstituée dans son état complet (5 entrées, bouton central orange).

| Indicateur | Valeur |
| --- | --- |
| Boutons | **18** |
| Liens `<a>` | **0** |
| Champs de saisie `<input>` / `<select>` / `<textarea>` | **0 / 0 / 0** |
| Attributs `id` | 12 |
| Attributs `data-purpose` | 12 (repères de section) |
| Balises `aria-*` | **1** (`aria-label` du bouton d'inversion) |
| `role` / `tabindex` / `sr-only` | **0 / 0 / 0** |
| Scripts | 3 (CDN Tailwind + plugins, `tailwind.config`, script d'interaction) |
| Images | 5 (logo, avatar, 2 portraits, 1 colis) |
| SVG | 17 |
| Animations permanentes | 2 (`animate-pulse`, `animate-ping`) |
| Zones en dur (`h-` / `min-h-[844px]`) | conteneur `min-h-[844px]`, barre basse `fixed` |
| `touch-action` | **0 occurrence** |
| `user-select: none` | 1 (CSS du `body`) |

---

## 2. Structure de l'écran

| Ordre | Section (`data-purpose`) | Contenu |
| --- | --- | --- |
| 1 | `ios-status-bar` | barre d'état simulée : `9:41`, réseau, Wi-Fi, batterie |
| 2 | `app-header` | logo ZéColis + « Air Parcel Hub » · cloche (avec point orange) · avatar |
| 3 | `corridor-ribbon` | `● Axe Actif` à gauche · `FRANCE ⇄ BÉNIN` à droite |
| 4 | `search-main-content` `editorial-header` | `Trouvez ce qu'il vous faut.` + baseline |
| 5 | `intent-toggle` | 2 onglets : `📦 Envoyer un colis` / `✈️ Voyager avec un colis` |
| 6 | `main-search-card` | titre de mode + pastille `Mode Expéditeur` · 2 champs Départ / Arrivée + bouton d'inversion · 2 blocs de critères (Période, Capacité) |
| 7 | `filter-chips` | `Affiner votre recherche` + `Réinitialiser` (span) + 4 puces défilables horizontalement |
| 8 | `search-cta` | bouton orange pleine largeur `Rechercher des voyageurs` |
| 9 | `results-feed` | en-tête (`12 voyageurs disponibles` + `En direct` + `Trier`) + **3 cartes** + barre basse `fixed` |

---

## 3. Défauts mobile

### 3.1 Zoom bloqué

```html
<meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover" name="viewport"/>
```

Le zoom et le pincement sont désactivés. **6ᵉ fichier consécutif** dans le dossier. Agravé ici par **35 textes entre 9 et 11 px** (§ 3.4).

`viewport-fit=cover` est en revanche **présent** : c'est le premier fichier du dossier à le déclarer. Mais `env(safe-area-inset-*)` est utilisé **0 fois** (§ 3.2).

### 3.2 Zones de sécurité déclarées mais jamais utilisées

| Élément | Attendu | Présent |
| --- | --- | --- |
| `viewport-fit=cover` | oui | **oui** |
| `env(safe-area-inset-top)` | barre d'état / Dynamic Island | **0 occurrence** |
| `env(safe-area-inset-bottom)` | barre de gestes iOS | **0 occurrence** |
| `env(safe-area-inset-left/right)` | paysage | **0 occurrence** |
| `100dvh` | hauteur dynamique | **0 occurrence** (à la place : `min-h-[844px]`) |

Le `<header>` est `sticky top-0` avec `pt-2` : il se colle **sous l'encoche**. La barre basse est `fixed bottom-0` avec `pb-5` : ses onglets tombent **dans la barre de gestes**, et la pilule d'accueil `w-32 h-1` doublée est dessinée 12 px au-dessus d'elle.

### 3.3 Hauteur figée et barre fixe

| Élément | Code | Effet |
| --- | --- | --- |
| Conteneur | `min-h-[844px]` | sur un écran plus bas (iPhone SE : 667 px, ou une fenêtre de navigateur de 600 px), le conteneur dépasse la fenêtre et **la barre basse `fixed` flotte au milieu de l'écran** |
| Compteur de bas de page | `pb-28` (112 px) | compensait la barre basse haute de ~80 px + pilule. Valeur figée, non reliée à la zone de geste réelle |
| Barre basse | `fixed bottom-0 left-1/2 -translate-x-1/2` | centrée par transformation, sans `bottom: env(safe-area-inset-bottom)` |

### 3.4 Typographie

| Taille | Occurrences | Détail |
| --- | --- | --- |
| `text-[9px]` | **5** | `Air Parcel Hub`, 2 libellés `Période` / `Capacité`, badge `En direct` |
| `text-[10px]` | **22** | axe actif, libellés `TRAJET` / `DÉPART`, prix `/ kg`, badges de capacité, `Réinitialiser`, libellés de_MODE |
| `text-[11px]` | **8** | chapô, `Affiner votre recherche`, badges de vérification, airlines, réponses |
| ≥ 12 px | reste | conforme |

**35 textes sous 12 px**, dont les **informations financières** (prix par kg, rémunération) et les **données de trajet** (dates, villes). C'est le fichier le plus dense en micro-texte après l'accueil (63).

### 3.5 Cibles tactiles — 15 sur 18 sont sous 44 px

| # | Élément | Taille réelle | Verdict |
| --- | --- | --- | --- |
| 1 | Cloche de notifications | `w-9 h-9` = **36 px** | sous 44 px |
| 2 | Onglet `Envoyer un colis` | `py-2.5` + `text-xs` ≈ 36 px | sous 44 px |
| 3 | Onglet `Voyager avec un colis` | idem ≈ 36 px | sous 44 px |
| 4 | Bouton d'inversion départ/arrivée | `w-8 h-8` = **32 px** | sous 44 px |
| 5 | Puce `📅 18–25 Sept.` | `py-1.5` + `text-xs` ≈ 28 px | sous 44 px |
| 6 | Puce `⚖️ ≤ 10 kg` | ≈ 28 px | sous 44 px |
| 7 | Puce `💰 ≤ 10 €/kg` | ≈ 28 px | sous 44 px |
| 8 | Puce `Vérifié uniquement` | ≈ 28 px | sous 44 px |
| 9 | CTA `Rechercher des voyageurs` | `h-13` **invalide** + `py-3.5` ≈ 48 px | conforme par accident |
| 10 | Bouton `Trier` | `py-1` + `text-xs` ≈ **24 px** | sous 44 px |
| 11 | `Voir l'annonce` (carte 1) | `py-2` + `text-xs` ≈ **32 px** | sous 44 px |
| 12 | `Voir l'annonce` (carte 2) | ≈ 32 px | sous 44 px |
| 13 | `Voir le colis` | `py-1` + `text-xs` ≈ **24 px** | sous 44 px |
| 14 | Onglet `Accueil` | icône 20 + gap 4 + texte 10 px ≈ 38 px | sous 44 px |
| 15 | Onglet `Recherche` | ≈ 38 px | sous 44 px |
| 16 | Bouton central `Publier` | `w-13 h-13 w-12 h-12` → **48 px** (seul `w-12 h-12` s'applique) | conforme |
| 17 | Onglet `Messages` | ≈ 38 px | sous 44 px |
| 18 | Onglet `Profil` | ≈ 38 px | sous 44 px |

Hors inventaire : l'**avatar** est un `<div>` de 36 px, et les **2 blocs de critères** (« Période », « Capacité ») sont des `<div cursor-pointer>` d'environ 36 px qui n'ont **aucun gestionnaire**.

`touch-action` : **0 occurrence** dans tout le fichier.

### 3.6 Sélection de texte bloquée

```css
body { … user-select: none; }
```

Bloque la copie des prix, des dates, des villes et des noms. Utile sur un fond de carte, nuisible pour une liste de résultats que l'utilisateur veut comparer ou copier.

### 3.7 Barre de statut et pilule d'accueil factices

Doublement de la barre d'état réelle (`9:41` + 3 icônes) et de la barre de gestes (`w-32 h-1 bg-slate-900/40`), alors que l'appareil affiche déjà les deux. Le conteneur est en outre `border-x border-slate-100`, ce qui donne l'apparence d'un cadre d'appareil.

---

## 4. Logique applicative : ce qui fonctionne et ce qui ne fonctionne pas

### 4.1 Ce qui fonctionne (3 fonctionnalités réelles)

| Fonctionnalité | Implémentation | Verdict |
| --- | --- | --- |
| Bascule de mode expéditeur / voyageur | `btnSender` / `btnTraveler` → 7 textes modifiés | **fonctionnel mais incomplet** (§ 4.3) |
| Inversion départ / arrivée | échange des deux textes + rotation 200 ms | **fonctionnel mais incomplet** (§ 4.2) |
| Bascules visuelles des onglets | remplacement de `className` | fonctionne |

Bilan : **2 fonctionnalités réellement fonctionnelles sur 16 éléments interactifs annoncés.**

### 4.2 Bug : l'inversion ne déplace pas les libellés de pays

```js
const currentDep = depText.textContent;
depText.textContent = arrText.textContent;
arrText.textContent = currentDep;
```

Seuls les deux textes (`#departure-text`, `#arrival-text`) sont échangés. Les deux pastilles de pays — `Bénin 🇧🇯` et `Paris / Régions 🇫🇷` — sont **des `<span>` statiques sans `id`** : après inversion, l'écran affiche

> **DÉPART** France 🇧🇯 … **ARRIVÉE** Cotonou 🇫🇷

soit des pays et des drapeaux attachés à la mauvaise ville.

### 4.3 Bug : le changement de mode ne change pas le fil de résultats

`feedTitle.textContent = "28 colis en attente"` est modifié, mais **les 3 cartes restent identiques** : 2 cartes voyageurs (Koffi D., Amina S.) et 1 carte colis. L'interface annonce 28 colis et affiche 2 voyageurs. Aucune information n'indique à l'utilisateur que le jeu est figé.

Le bloc de critères est bien mis à jour (`Capacité / Max 5 kg • Valise` → `Espace disponible / 10 kg disponibles`), ce qui accentue l'incohérence.

### 4.4 Bug : les puces de filtre ne filtrent rien

```js
chips.forEach(chip => {
  chip.addEventListener('click', () => {
    if (!chip.classList.contains('active')) {
      chip.classList.toggle('ring-2');
      chip.classList.toggle('ring-orange-300');
    }
  });
});
```

| Problème | Conséquence |
| --- | --- |
| la classe `active` **n'est jamais posée** | la condition `!contains('active')` reste toujours vraie et le contour s'alterne |
| aucun filtre n'est appliqué au fil | les 4 puces ne changent rien aux cartes |
| pas de `aria-pressed` | l'état actif n'est pas exposé |
| aucun retour | l'appui ne produit aucun message |

La première puce a `active` **en dur dans son HTML** : elle n'est donc pas « activée » par le script, elle l'est par le markup — et rien ne reflète plus l'état réel.

### 4.5 `Réinitialiser` est un `<span>`

```html
<span class="text-[#FF6B00] text-[10px] cursor-pointer font-bold">Réinitialiser</span>
```

`cursor-pointer` promet une action ; il n'y a ni `button`, ni gestionnaire, ni rôle. Aucun moyen de remettre les filtres à zéro.

### 4.6 Les 2 champs de lieu ne sont pas des champs

**0 `<input>` dans tout le fichier.** Départ et Arrivée sont des `<div>` :

```html
<div class="bg-slate-50 … border … rounded-xl px-3.5 py-2.5 flex items-center justify-between transition-colors">
```

Ils ont l'apparence d'un champ (fond `slate-50`, bordure, rayon 12 px) et un `hover:bg-slate-100/70` qui suggère l'interactivité, mais **ne sont ni cliquables, ni focusables, ni Associated à un `label`**. Un utilisateur ne peut pas saisir « Lagos » ni « Marseille ». C'est la même limitation que sur l'accueil (C5-2), mais ici **en pleine page de recherche**.

### 4.7 Liste des 11 actions sans effet

| # | Élément | État dans l'original |
| --- | --- | --- |
| 1 | Cloche de notifications | `<button>` sans gestionnaire |
| 2 | Avatar de profil | `<div>` sans gestionnaire (et sans `role`) |
| 3 | Bloc « Période » | `<div cursor-pointer>` sans gestionnaire |
| 4 | Bloc « Capacité » | `<div cursor-pointer>` sans gestionnaire |
| 5 | `Réinitialiser` | `<span>` |
| 6 | `Trier` | `<button>` sans gestionnaire |
| 7 | `Voir l'annonce` (carte 1) | `<button>` sans gestionnaire |
| 8 | `Voir l'annonce` (carte 2) | `<button>` sans gestionnaire |
| 9 | `Voir le colis` | `<button>` sans gestionnaire |
| 10 | Onglets `Accueil`, `Messages`, `Profil` | `<button>` sans gestionnaire (3) |
| 11 | Bouton central `Publier` | `<button>` sans gestionnaire |
| 12 | CTA `Rechercher` | `<button>` sans gestionnaire — **le plus visible** |

`data-purpose` n'apparaît que sur 12 conteneurs : c'est le seul fichier du dossier à **documenter ses sections dans le code lui-même**.

---

## 5. Code mort et classes invalides

| Élément | Fichier | Effet |
| --- | --- | --- |
| `h-13` | ligne 261 (CTA) | **invalide en Tailwind v3** — la hauteur venait de `py-3.5` |
| `w-13 h-13` | ligne 445 (bouton `Publier`) | **invalide** — seuls `w-12 h-12` s'appliquent |
| `rounded-2xs` | ligne 71 (jauge de batterie) | **invalide** |
| `shadow-2xs` | lignes 165, 190, 236, 241, 246, 251 | **invalide** (nom v4) — 6 ombres qui n'existent pas |
| `shadow-xs` | cartes | **valide** en v3.4+ (le CDN `cdn.tailwindcss.com` sert la dernière v3) |
| `h-2.5`, `w-2.5` | barre d'état et jauge de batterie | **valides** — l'échelle d'espacement v3 contient bien le pas de 0.5 |
| Bloc Période / Capacité | `cursor-pointer` sans gestionnaire | promesse d'interaction sans effet |
| `overflow-x-hidden` sur le conteneur | coupe les débordements horizontaux | sans conséquence ici, mais masque d'éventuels débordements |

**4 classes invalides distinctes, 10 occurrences** (`h-13` ×2, `w-13` ×1, `rounded-2xs` ×1, `shadow-2xs` ×6) : c'est le fichier le plus d'épendant de la syntaxe Tailwind v4 tout en chargeant la v3 via le CDN `https://cdn.tailwindcss.com?plugins=forms,container-queries`.

---

## 6. Conformité au § 4 (design de référence)

| Point | Verdict |
| --- | --- |
| Fond `#F8FAFC` | **conforme** |
| Cartes blanches, bordures `slate-100/200` | **conforme** |
| Palette navy `#0B2A5B` / `#1D1D4B`, orange `#FF6B00`, vert `#22A55A` | **conforme** |
| Base Tailwind | CDN **avec plugins** `forms` et `container-queries` — comme les fichiers 4 et 5 (cf. B22) |
| Police | **Plus Jakarta Sans** chargée seule, **Inter déclarée en second mais jamais chargée** → divergence § 4.4 (3ᵉ fichier divergent) |
| Verdict global | **le design est le plus proche de la référence depuis l'accueil** : aucun glassmorphism, aucun dégradé, aucune ombre marquée, rayons 12–22 px |

**Points conformes notables :** `data-purpose` sur chaque section, bouton central `Publier` à 48 px, en-tête `backdrop-blur-md` limité à un seul élément, aucun `alert()` bloquant, aucun code de submission accidentel.

**Divergences :** 35 textes sous 12 px · `shadow-2xs` et `rounded-2xs` invalides · `h-13`/`w-13` invalides · zoom bloqué · safe areas déclarées mais non utilisées · police.

---

## 7. Données fictives

**Tout le contenu de l'écran est codé en dur.** Aucune donnée ne provient d'un service.

### 7.1 Profils et offres

| # | Élément | Valeur |
| --- | --- | --- |
| R-1 | Voyageur 1 | **Koffi D.**, `✓ Identité vérifiée`, ★ 4,9 (38 avis), 9 €/kg, 8 kg dispo, `Cotonou (COO) → Paris (CDG)`, départ **18 sept. 2025**, `Enregistré Air France direct` |
| R-2 | Voyageuse 2 | **Amina S.**, `✓ Vérifié`, ★ 5,0 (24 avis), 10 €/kg, 6 kg dispo, `Cotonou → Lyon / Paris`, départ **21 sept. 2025**, `Réponse rapide (< 15 min)` |
| R-3 | Colis | `Petit boîtier électronique scellé`, **3,5 kg**, `Document inclus`, **12 €/kg**, rémunération **~42 €**, `Cotonou → Paris Région` |
| R-4 | Compteurs | `12 voyageurs disponibles` / `28 colis en attente` |
| R-5 | Badge | `En direct` avec `animate-ping` en boucle |

### 7.2 Contradictions internes

| # | Constat |
| --- | --- |
| R-6 | **Compteur « 28 colis » alors que 1 seule carte colis est affichée** (§ 4.3) |
| R-7 | **Prix incohérents entre le filtre et les cartes** : le filtre annonce `≤ 10 €/kg` alors que le colis est à **12 €/kg** |
| R-8 | **Le filtre « 18–25 Sept. »** ne correspond pas aux dates des cartes (18 et 21 sept. : c'est cohérent) mais **le bloc « Période » affiche « 18–24 Sept. »** → deux plages différentes dans le même écran |
| R-9 | `Réponse rapide (< 15 min)` — **aucun code de messagerie ni de notification** : rien ne peut répondre en 15 minutes |
| R-10 | `En direct` + `animate-ping` — aucun appel réseau, aucune donnée qui se met à jour |
| R-11 | `Document inclus` — Mention juridique sans mécanisme de contrôle (§ 6.8) |
| R-12 | Dates **2025** : le contenu est déjà périmé par rapport à la date du dossier |
| R-13 | `AIR PARCEL HUB` — **entité absente du cahier des charges** : ni le § 3, ni le § 4, ni le § 6 ne mentionnent un « Air Parcel Hub ». Le logo lui-même affiche `Air Parcel` |

---

## 8. Fonctionnalités affichées vs implémentées

| Fonctionnalité affichée | Implémentée ? |
| --- | --- |
| Bascule de mode Expéditeur / Voyageur | **Partiellement** — les libellés changent, le fil ne change pas, `aria-selected` absent |
| Inversion départ / arrivée | **Partiellement** — les villes changent, **les pays et drapeaux restent attachés à la mauvaise ville** |
| Saisie d'un lieu de départ / d'arrivée | **Non** — 0 `<input>`, blocs statiques |
| Sélection d'une période | **Non** — bloc décoratif |
| Sélection d'une capacité / d'un type de bagage | **Non** — bloc décoratif |
| Filtres rapides (4 puces) | **Non** — l'état ne se synchronise pas et aucun filtrage |
| `Réinitialiser` | **Non** — `<span>` sans effet |
| `Rechercher` | **Non** — aucun gestionnaire, aucun appel |
| `Trier` | **Non** |
| `Voir l'annonce` (×2) | **Non** |
| `Voir le colis` | **Non** |
| Cloche de notifications | **Non** |
| Avatar / profil | **Non** |
| Onglet `Accueil` | **Non** |
| Onglet `Recherche` | **Non** — l'écran affiché, aucun état actif exposé |
| Onglet `Messages` (badge `2`) | **Non** |
| Onglet `Profil` | **Non** |
| Bouton central `Publier` | **Non** |
| Indicateur `En direct` | **Non** — aucune donnée dynamique |
| Images et icônes | **Oui** — SVG inline et images distantes (fragilité réseau) |

**Bilan : 2 fonctionnalités réelles sur 20.** Aucun lien `<a href>` dans tout le fichier : la navigation est **entièrement morte**, comme sur l'accueil (C5-1).

---

## 9. Points à trancher

| # | Question |
| --- | --- |
| R-Q1 | La page appartient-elle à `03_PUBLICATION_ET_ANNONCES` ou à `02_NAVIGATION_ET_ACCUEIL` ? Elle est à la fois une recherche et un point d'entrée vers la publication |
| R-Q2 | Combien de filtres les 4 puces doivent-elles porter, et **sur quels critères réels** ? Le code actuel n'applique aucun filtre (cf. B1, B26) |
| R-Q3 | Le mode Expéditeur / Voyageur doit-il changer le contenu du fil, ou l'écran est-il volontairement une démonstration de deux états d'interface ? |
| R-Q4 | Que signifie « Air Parcel Hub » ? Cette marque n'apparaît nulle part dans le cahier des charges |
| R-Q5 | Quelle plage de dates est la bonne : « 18–24 Sept. » (bloc Période), « 18–25 Sept. » (puce) ou les dates des cartes ? |
| R-Q6 | La barre basse doit-elle comporter 5 entrées (`Accueil`, `Recherche`, `Publier`, `Messages`, `Profil`) comme ici, alors que § 4.5 en décrit une autre composition ? |
| R-Q7 | Les écrans de détail d'annonce et de détail de colis, la publication, la messagerie, le profil et les notifications sont-ils transmis ? Ils sont tous référencés ici |