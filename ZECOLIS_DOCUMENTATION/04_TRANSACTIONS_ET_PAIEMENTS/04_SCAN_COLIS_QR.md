# Scanner Colis & QR — page « Scanner Zecolis »

| Rubrique | Valeur |
| --- | --- |
| **Nom de la page** | Écran Scanner Colis & QR (titre du document : « Zecolis Mobile - Écran Scanner Colis & QR ») |
| **Fichier source** | Nom de fichier d'origine **non communiqué** par le donneur d'ordre. Copie conservée sous `SOURCES_HTML_ORIGINALES/scanner_colis_qr.html` (nom de travail attribué pour le traçage, à renommer si le nom réel est fourni). |
| **Catégorie retenue** | `04_TRANSACTIONS_ET_PAIEMENTS` — la page concerne la validation d'une remise, donc une étape de transaction. Catégorie alternative possible : `03_PUBLICATION_ET_ANNONCES` si le scan est intégré au cycle de vie d'une annonce plutôt qu'à une transaction. **Arbitrage à confirmer.** |
| **Statut d'analyse** | Analysé (fichier unique, autonome) |

---

## 1. Objectif de la page

Permettre la validation d'une **remise de colis** par lecture d'un code (QR ou code-barres) sur l'appareil, ou par saisie manuelle d'un code secret / OTP.

Trois modes de scan sont annoncés dans l'interface : « Remise colis », « QR Voyageur », « Scellé IATA ». Seul le premier est présenté comme actif dans le code.

## 2. Utilisateur concerné et rôle métier

**Non déterminé à partir du fichier transmis.** L'écran affiche un contexte « dernier colis scanné » et un séquestre, ce qui suggère une manipulation par le voyageur (remise) ou par l'expéditeur (réception). Le code ne contient aucun élément identifiant un rôle, un utilisateur connecté ou un permissions.

## 3. Point d'entrée dans la navigation

**Non déterminé.** Le seul lien de navigation présent est un bouton de retour `href="#"`. Aucun appel à une route, aucun paramètre, aucune référence à une autre page.

## 4. Structure de l'écran (ordre de lecture)

Conteneur unique de **390 × 844 px** (`max-w-[390px] h-[844px]`), en `flex flex-col justify-between`, débordement masqué (`overflow-hidden` sur `html` et sur le conteneur) : **la page ne défile pas**.

| Zone | Contenu |
| --- | --- |
| Fond | Image décorative + deux voiles en dégradé + vignettage radial |
| 1. Barre d'état iOS simulée | Heure `9:41`, icônes `signal`, `wifi`, batterie |
| 2. En-tête | Retour, badge titre « Scanner Zecolis » + pastille pulsée + badge « AI Live », bouton torche |
| 3. Sélecteur de mode | 3 onglets : Remise colis (actif), QR Voyageur, Scellé IATA |
| 4. Zone centrale | Texte d'instruction, viseur 270 × 270 avec coins de cadrage, réticule, silhouette de QR, pastille « Capteur prêt », 2 boutons d'action rapide |
| 5. Zone basse | Carte de contexte du colis, bouton principal orange, ligne de confiance + lien « Aide », indicateur d'accueil iOS simulé |

## 5. Sections et composants visuels

### 5.1 Fond « caméra » (simulé)

- `<img>` distante (`lh3.googleusercontent.com/...`), `object-cover opacity-35 blur-[2px] brightness-75 scale-105`, `pointer-events-none`.
- Voile vertical `bg-gradient-to-b from-[#050B14]/90 via-transparent to-[#050B14]/95`.
- Vignettage `radial-gradient(circle_at_center, transparent 30%, rgba(5,11,20,0.85) 100%)`.
- **Aucune API caméra n'est utilisée** : ni `getUserMedia`, ni `<video>`, ni instruction d'autorisation. Il s'agit d'un fond d'image fixe, pas d'un flux vidéo.

### 5.2 Barre d'état iOS simulée

Heure `9:41` en dur, icônes Lucide `signal` et `wifi`, batterie dessinée en div (`w-5 h-2.5 border border-white/80`) avec remplissage blanc complet. Élément de simulation, sans rapport avec un état réel de l'appareil.

### 5.3 En-tête

| Élément | Détail |
| --- | --- |
| Retour | `<a href="#">`, 40 × 40, `rounded-full`, style `glass-pill`, icône `arrow-left` |
| Badge titre | `glass-pill`, pastille `animate-ping` + pastille fixe `bg-emerald-500`, texte `Scanner Zecolis` en `uppercase`, badge `AI Live` (`bg-orange-500/20`, bordure `orange-500/30`) |
| Torche | `<button id="torchBtn">`, icône `zap` avec `fill-amber-300/30` |

### 5.4 Sélecteur de mode (3 onglets)

| Onglet | Icône | État dans le code |
| --- | --- | --- |
| Remise colis | `package-check` | Actif : `bg-gradient-to-r from-[#0B2A5B] to-[#144288]`, `text-white`, bordure `blue-400/30` |
| QR Voyageur | `qr-code` | Inactif : `text-slate-400` |
| Scellé IATA | `shield-alert` | Inactif : `text-slate-400` |

Les trois sont des `<button>` **sans gestionnaire d'événement et sans identifiant**. Le changement de mode n'est pas implémenté ; l'onglet actif est figé dans le HTML.

### 5.5 Viseur

- Instruction : `Cadrez le code QR ou le code-barres du colis` / `La validation de la remise s'effectue automatiquement`.
- Viseur `270 × 270`, `rounded-3xl`, animation `pulseTarget` (scale 1 → 1.02, 3 s, alterné).
- 4 coins de cadrage, `border-4`, `w-9 h-9` : **haut gauche et haut droit en `#FF6B00`**, **bas gauche et bas droit en `#22A55A`**.
- Ligne de scan `.scanner-ray` : hauteur 3 px, `linear-gradient(90deg, transparent, #FF6B00 30%, #22A55A 70%, transparent)`, double `box-shadow` orange et vert, animation `scanRay` 2,4 s (top 6 % → 92 %).
- Réticule central : croix + cercle `w-3 h-3`, `opacity-40`.
- Silhouette décorative : icône Lucide `qr-code` en `w-36 h-36`, `opacity-15`, `pointer-events-none`.
- Pastille sous le viseur : `check-circle-2` vert + `Capteur prêt • Autofocus actif`, `bg-slate-900/90`, bordure `emerald-500/40`. **Texte statique, sans lien avec un état réel.**

### 5.6 Actions rapides

- `Importer une photo` (`image`, icône orange) — `<button>` sans gestionnaire.
- Bouton son `volume-2` — `<button>` sans gestionnaire, aucun état.

### 5.7 Carte de contexte du colis

| Partie | Contenu |
| --- | --- |
| Icône | `box` dans un conteneur `bg-orange-500/15`, bordure `orange-500/25` |
| Titre | `Colis #COL-7852` + badge `En attente` (`bg-emerald-500/20`, `text-emerald-400`) |
| Trajet | `Cotonou (COO) → France (CDG)` |
| Montant | `7 500 F` (orange) |
| Poids | `5 kg scellé` |

### 5.8 Bouton principal

`Saisir le code secret / OTP de remise` — pleine largeur, `bg-[#FF6B00]`, `hover:bg-[#ff7b1a]`, `active:scale-[0.99]`, `rounded-2xl`, `shadow-lg shadow-orange-600/30`, icône `key-round`. **Sans gestionnaire d'événement.**

### 5.9 Ligne de confiance

`shield-check` vert + `Séquestre débloqué après validation conjointe` à gauche ; bouton `Aide` souligné à droite. **Aide sans gestionnaire.**

### 5.10 Indicateur d'accueil iOS simulé

`w-32 h-1 bg-white/30 rounded-full mx-auto mt-4`.

## 6. Textes visibles (intégralité)

| Zone | Texte |
| --- | --- |
| Titre onglet navigateur | `Zecolis Mobile - Écran Scanner Colis & QR` |
| Barre d'état | `9:41` |
| Badge titre | `Scanner Zecolis` · `AI Live` |
| Onglet actif | `Remise colis` |
| Onglets inactifs | `QR Voyageur` · `Scellé IATA` |
| Instruction | `Cadrez le code QR ou le code-barres du colis` |
| Sous-instruction | `La validation de la remise s'effectue automatiquement` |
| Pastille viseur | `Capteur prêt • Autofocus actif` |
| Actions rapides | `Importer une photo` |
| Carte colis | `Colis #COL-7852` · `En attente` · `Cotonou (COO) → France (CDG)` · `7 500 F` · `5 kg scellé` |
| Bouton principal | `Saisir le code secret / OTP de remise` |
| Ligne confiance | `Séquestre débloqué après validation conjointe` · `Aide` |

## 7. Champs et données affichés

Aucun champ de saisie. Aucune donnée n'est lue dynamiquement : toutes les valeurs sont écrites en dur dans le HTML.

| Donnée | Valeur | Statut |
| --- | --- | --- |
| Référence colis | `#COL-7852` | **Fictive** |
| Statut | `En attente` | **Fictif** — n'appartient à aucun enum défini par ailleurs |
| Trajet | `Cotonou (COO) → France (CDG)` | **Fictif** — codes IATA aéroportuaires |
| Montant | `7 500 F` | **Fictif** — format FCFA |
| Poids | `5 kg` + mention `scellé` | **Fictif** |
| Mode actif | `Remise colis` | Figé |

## 8. Boutons et actions

| # | Élément | Type | Comportement dans le code |
| --- | --- | --- | --- |
| 1 | Retour | `<a href="#">` | Navigation vers `#` — **inopérant** |
| 2 | Torche | `<button id="torchBtn">` | **Seul élément interactif du fichier** : bascule une classe `bg-amber-400/25` + `border-amber-400/60` et retire `glass-pill`. Effet purement visuel, **aucun accès au flash de l'appareil** |
| 3 | Onglet « Remise colis » | `<button>` | Aucun gestionnaire |
| 4 | Onglet « QR Voyageur » | `<button>` | Aucun gestionnaire |
| 5 | Onglet « Scellé IATA » | `<button>` | Aucun gestionnaire |
| 6 | Importer une photo | `<button>` | Aucun gestionnaire — **pas de `input type="file"`, pas d'`accept` image** |
| 7 | Bouton son | `<button>` | Aucun gestionnaire |
| 8 | Saisir le code secret / OTP | `<button>` | Aucun gestionnaire — **aucun clavier, aucun champ, aucun modal** |
| 9 | Aide | `<button>` | Aucun gestionnaire |

Sur 9 actions : 1 implémentée (visuellement), 8 sans effet.

## 9. Interactions et comportements observés

- `lucide.createIcons()` au chargement : remplace chaque `<i data-lucide>` par le SVG correspondant.
- Torche : toggle de classes CSS, avec `active:scale-95` au clic.
- Animations CSS permanent : `pulseTarget` (viseur), `scanRay` (ligne de scan), `animate-ping` (pastille d'en-tête), `transition` sur les hover/active.
- `hover` et `active` présents sur la plupart des éléments cliquables —Retour, torche, onglets, actions rapides, bouton principal, Aide.
- `select-none` sur le `<body>` et `-webkit-tap-highlight-color: transparent` : anti-sélection et masquage du surlignage au tap.
- **Aucun** appel réseau, aucun `fetch`, aucun stockage, aucune persistance, aucun `localStorage`.

## 10. États possibles

| État | Présent ? | Observation |
| --- | --- | --- |
| Chargement | Non | Aucun indicateur de chargement. La caméra est simulée par une image déjà visible. |
| Erreur | Non | Aucun message d'erreur, aucun état « code non lu », « code invalide », « permission refusée ». |
| Vide | Non | La carte de contexte affiche toujours un colis. Aucun cas « aucun colis à scanner ». |
| Succès | **Non implémenté** | Aucun état de scan réussi, aucun retour visuel de lecture, aucune confirmation de remise. |
| Permission caméra | Non | Aucun dialogue d'autorisation. |
| Hors ligne / réseau | Non | Aucune gestion. |

## 11. Navigation vers les autres écrans

**Aucun lien fonctionnel.** `href="#"` sur le bouton retour. Aucun router, aucun hash d'état, aucune route. La cohérence des pages voisines ne peut pas être vérifiée à partir de ce fichier.

La page **n'affiche pas** la navigation principale basse (Accueil / Recherche / Publier / Messages / Profil). C'est cohérent avec la règle du § 4.5 pour une page secondaire, mais l'écran n'est rattaché à aucun parent : le point d'entrée reste inconnu.

## 12. Règles métier visibles dans le code

Aucune règle de validation, de calcul ou de conditionnement n'est implémentée. Les seules « règles » sont des libellés d'interface :

- « La validation de la remise s'effectue automatiquement » → **affirmé, mais aucun code de lecture, de décodage ou de validation n'existe dans le fichier**.
- « Séquestre débloqué après validation conjointe » → **affirmé, aucun mécanisme de séquestre, de paiement ou de double validation dans le fichier**.
- « Capteur prêt • Autofocus actif » → **affirmé, aucun capteur ni autofocus dans le fichier**.
- « Badge `En attente` en vert » → le vert est réservé aux états positifs et vérifications réussies (§ 4.3) ; un statut d'attente est ici signalé en vert.
- Le mode « Remise colis » suppose qu'un code QR **ou code-barres** est présent sur le colis : ce format de code n'est documenté nulle part dans les autres éléments.

## 13. Dépendances techniques

| Type | Valeur | Remarque |
| --- | --- | --- |
| Tailwind | `https://cdn.tailwindcss.com` | **Script CDN, pas de build.** Aucune configuration Tailwind, aucun `tailwind.config`. Les classes arbitraires (`w-[270px]`, `rounded-2xs`, `bg-[radial-gradient(...)]`) sont acceptées par le moteur JIT du CDN. |
| Icônes | `https://unpkg.com/lucide@latest` | **Version non épinglée.** Rupture possible de rendu si l'API change. |
| Police | Google Fonts `Plus Jakarta Sans` (400/500/600/700/800) | 5 graisses chargées |
| Image | `lh3.googleusercontent.com/aida-public/AB6AXuDkXVH...` | **Dépendance externe non maîtrisée**, peut disparaître. `alt` : `Arrière-plan caméra scan colis`. |
| HTML | `<html lang="fr">` | Langue correcte |
| CSS | 3 keyframes + 3 classes utilitaires maison (`.scanner-ray`, `.viewfinder-glow`, `.glass-pill`) | Tout le reste est utilitaire Tailwind |
| JS | 1 bloc inline, ~12 lignes | `lucide.createIcons()` + toggle torche |
| Build | Aucun | Ni Vite, ni React, ni TypeScript, ni Firebase, ni Capacitor |

**Aucun lien vers Firebase, Capacitor ni React.** Ce fichier est une maquette HTML autonome, pas un écran React fonctionnel.

## 14. Éléments provisoires ou fictifs

### Données fictives
`#COL-7852`, `En attente`, `Cotonou (COO) → France (CDG)`, `7 500 F`, `5 kg scellé`, heure `9:41`.

### Affirmations non adossées au code
| Affirmation | Base dans le fichier |
| --- | --- |
| `AI Live` | Aucun traitement d'image, aucun modèle, aucune inference. **Étiquette purement décorative.** |
| `La validation de la remise s'effectue automatiquement` | Aucun code de décodage ni de validation. |
| `Capteur prêt • Autofocus actif` | Aucun capteur, aucun autofocus. |
| `Séquestre débloqué après validation conjointe` | Aucun paiement, aucun séquestre, aucune double validation. |
| Fond « caméra » | Image fixe, pas un flux vidéo. |
| Icône `qr-code` en silhouette dans le viseur | Simule la présence d'un code à scanner. |

### Éléments de simulation d'appareil
Barre d'état iOS (heure, réseau, batterie), indicateur d'accueil iOS. Sans rapport avec l'état réel de l'appareil.

## 15. Écarts avec l'identité visuelle de référence

Le cahier des charges (§ 4) désigne la page **Profil** comme référence. Cet écran s'en écarte sur plusieurs points structurants.

| Point | Référence § 4 | Ce fichier | Écart |
| --- | --- | --- | --- |
| Police | **Inter** | **Plus Jakarta Sans** | **Majeur** |
| Fond général | `#F8FAFC` (clair) | `#050B14` (presque noir) | **Majeur** — inversion totale de thème |
| Fond des cartes | `#FFFFFF` (blanc) | `bg-slate-900/85`, `rgba(15,23,42,0.65)` (sombre translucide) | **Majeur** |
| Effets | « éviter le glassmorphism » | `.glass-pill` : `backdrop-filter: blur(14px)` + transparence + bordure blanche 12 % | **Contraire explicite** |
| Dégradés | « pas de dégradés multicolores », « ne pas transformer l'interface en écran orange » | Dégradé orange→vert sur la ligne de scan ; 2 voiles en dégradé ; dégradé radial ; dégradé bleu `#0B2A5B`→`#144288` sur l'onglet actif ; ombres colorées orange et vert | **Contraire explicite** |
| Couleurs hors palette | Palette fermée (navy, orange, verts, gris) | `#050B14`, `#144288`, `emerald-*`, `slate-900/800`, `amber-300/400`, `blue-400/30` | **Majeur** |
| Cartes / rayons | cartes ~16 px, boutons ~12 px | `rounded-3xl` (24 px) sur le viseur et le bouton principal, `rounded-2xl` (16 px), pastilles `rounded-full` (9999 px) | **Divergence** |
| Ombres | « ombres légères », « éviter les ombres épaisses » | `shadow-lg shadow-orange-600/30`, double `box-shadow` 16/24 px sur la ligne de scan | **Divergence** |
| Badge « AI Live » | Non prévu dans la liste des badges | Présent | **À arbitrer** |
| Statut d'attente en vert | Vert réservé aux états positifs | `En attente` en `emerald-400` sur fond `emerald-500/20` | **Contraire à la règle d'usage** |

Couleurs de la palette effectivement présentes et **conformes** : `#FF6B00` (bouton principal, coins hauts, icône colis), `#22A55A` (coins bas), `#0B2A5B` (dégradé de l'onglet actif).

## 16. Points à vérifier

1. **Nom de fichier d'origine** — non communiqué ; le nom de travail `scanner_colis_qr.html` est à remplacer.
2. **Catégorie** — `04_TRANSACTIONS_ET_PAIEMENTS` ou `03_PUBLICATION_ET_ANNONCES` ?
3. **Rôle utilisateur** — voyageur, expéditeur, ou les deux ? Aucune trace dans le code.
4. **Écran existant ou à créer ?** — la page est-elle déjà implémentée dans l'application React, ou cette maquette est-elle la seule version ?
5. **Modes « QR Voyageur » et « Scellé IATA »** — le concept de scellé IATA n'apparaît nulle part dans le cahier des charges. Est-il validé ? Qui Scanne quoi dans chaque mode ?
6. **Caméra réelle** — le parcours doit-il utiliser la caméra de l'appareil ? Sous Android/iOS avec Capacitor, quel plugin ?
7. **Permission caméra et vie privée** — non gérée. À prévoir, avec un impact sur les documents légale(consentement).
8. **Séquestre** — mécanisme réel, ou formulation à retirer ? Le cahier des charges impose de ne pas inventer de garanties ni de processus de paiement.
9. **Statut `En attente`** — appartient-il à l'enum des statuts de transaction ? Un statut d'attente ne devrait pas être vert.
10. **Format du code à scanner** — QR, code-barres, ou les deux ? Défini nulle part ailleurs.
11. **Parcours OTP** — d'où vient le code ? Qui l'encadre ? Durée de validité ? Que se passe-t-il s'il est erroné ? Aucun de ces états n'existe dans l'écran.
12. **Maquette ou produit ?** — l'écart de thème sombre complet est-il un choix voulu (écran « caméra », hors du cadre clair de référence) ou une dérive à corriger ?
13. **Dépendance image externe** — à remplacer par un asset local pour la fiabilité.
14. **Accessibilité** — textes en `text-[10px]` et `text-[11px]`, contrastes de texte secondaire faible sur fond sombre, `select-none` global, aucun `aria-label` sur les boutons icône (retour, torche, son). Zones tactiles de 36–40 px pour les boutons ronds (recommandation courante : 44 px).
15. **Doublon** — à vérifier avec les autres fichiers : un écran « QR code » est déjà listé au § 6.8 du cahier des charges comme page possible. Si un fichier `QR` distinct est transmis, comparer les deux.

## 17. En résumé

Écran de scan de colis **maquette HTML autonome** : 9 actions, dont **une seule** interactive (la torche, purement visuelle). Aucune caméra, aucun décodage, aucune validation, aucun paiement, toutes les données sont écrites en dur. Le code applique la palette `#FF6B00` / `#22A55A` / `#0B2A5B` de Zecolis, mais dans un **thème sombre avec glassmorphism et dégradés**, en contradiction avec la référence visuelle claire et sobre du projet (§ 4.3, § 4.4).