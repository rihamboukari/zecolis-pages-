# Connexion

| Rubrique | Valeur |
| --- | --- |
| **Nom de la page** | Connexion (titre du document : « Zecolis Mobile - Connexion ») |
| **Fichier source** | Nom de fichier d'origine **non communiqué**. Copie conservée sous `SOURCES_HTML_ORIGINALES/connexion.html` (nom de travail). |
| **Catégorie retenue** | `06_PROFIL_ET_COMPTE` — authentification, accès au compte. Catégorie alternative : un dossier `AUTHENTIFICATION` dédié deviendrait pertinent si inscription, réinitialisation de mot de passe et autres écrans d'accès sont transmis. **Arbitrage à confirmer.** |
| **Statut d'analyse** | Analysé (fichier unique, autonome) |
| **Nature** | Maquette HTML autonome avec logique de formulaire **simulée**. Aucune authentification réelle. |

---

## 1. Objectif de la page

Permettre à un utilisateur de s'authentifier avec une adresse e-mail et un mot de passe.

C'est le **seul écran reçu** qui collecte des données saisies par l'utilisateur et qui gère des états de formulaire (erreur, chargement, succès).

## 2. Utilisateur concerné et rôle métier

Utilisateur **non connecté**, quel que soit son rôle métier. La page est commune aux expéditeurs et aux voyageurs (§ 2.1 : un même compte peut être les deux).

Le texte d'accueil — « Connectez-vous pour continuer à envoyer ou voyager » — reprend les deux rôles. C'est le seul élément du fichier qui fait référence aux rôles.

## 3. Point d'entrée dans la navigation

**Non déterminé.** L'écran ne contient **aucun lien sortant réel** :

| Élément | `href` | Effet |
| --- | --- | --- |
| « Oublié ? » | `javascript:void(0)` | **Inopérant** |
| « S'inscrire » | `javascript:void(0)` | **Inopérant** |

Aucun bouton retour. Aucune barre de navigation principale (§ 5.1) — ce qui est cohérent pour un écran d'accès.

**Conséquence importante : il n'existe aucun chemin de navigation depuis cette page vers l'application.** Ni inscription, ni mot de passe oublié, ni même un lien de retour.

## 4. Structure de l'écran (ordre de lecture)

Conteneur `max-w-[390px] mx-auto min-h-full flex flex-col justify-between px-6 pt-6 pb-5`, sur `body` en `flex flex-col`.

| Zone | Contenu |
| --- | --- |
| 1. Espace de respiration | `div.h-2 w-full` — 8 px factices stand-ins de la barre d'état |
| 2. En-tête | Pastille logo + libellé « Zecolis » · libellé « Connexion » · pastille « Sécurisée » |
| 3. Bloc titre | `h1` « Bon retour ! » + sous-titre sur 2 lignes ( `<br>` câblé ) |
| 4. Formulaire | E-mail · Mot de passe · Bouton principal · Invite à s'inscrire |
| 5. Pied | Message de réassurance + indicateur d'accueil iOS simulé |

## 5. Sections et composants visuels

### 5.1 Configuration Tailwind inline

Le fichier déclare un `tailwind.config` en JS, chose **absente du fichier 1**. Il définit la palette Zecolis comme couleurs de thème :

| Jeton Tailwind | Valeur | § 4.2 |
| --- | --- | --- |
| `zecolis-navy` | `#0B2A5B` | ✔ Navy secondaire |
| `zecolis-darknavy` | `#1D1D4B` | ✔ Navy profond |
| `zecolis-orange` | `#FF6B00` | ✔ Orange Zecolis |
| `zecolis-green` | `#22A55A` | ✔ Vert de confiance |
| `zecolis-bg` | `#F8FAFC` | ✔ Fond général |
| `zecolis-text` | `#0F172A` | ✔ Texte principal |
| `zecolis-muted` | `#64748B` | ✔ Texte secondaire |
| `zecolis-border` | `#F1F5F9` | ✔ Bordures |
| `zecolis-lightborder` | `#E2E8F0` | ✘ **Hors palette** — et **déclaré mais non utilisé** dans le fichier |

Ombres déclarées :

| Jeton | Valeur | Commentaire |
| --- | --- | --- |
| `btn-navy` | `0 8px 20px -4px rgba(11,42,91,.25), 0 4px 10px -2px rgba(11,42,91,.15)` | Double ombre marquée sur le bouton principal. § 4.4 demande des « ombres légères » et d'éviter les « ombres épaisses ». **À arbitrer.** |
| `input-subtle` | `0 2px 6px 0 rgba(15,23,42,.03)` |Conforme |
| `pill-soft` | `0 2px 8px rgba(15,23,42,.04)` | Conforme |

Police : `fontFamily.sans: ['Inter','sans-serif']`, Google Fonts Inter 400→800. **Conforme à § 4.4**, contrairement au fichier 1.

> **Point technique :** le `tailwind.config` est déclaré **après** le `<script src="https://cdn.tailwindcss.com">`. L'ordre importe — sur le CDN Tailwind, la configuration doit être définie avant ou immédiatement après le chargement du script selon la version. **À vérifier au navigateur** : si la palette `zecolis-*` ne se génère pas, toutes les couleurs du fichier se rabattent sur du Tailwind standard et la page s'affiche en couleurs par défaut.

### 5.2 En-tête

| Élément | Détail |
| --- | --- |
| Logo | `<img>` distante (`lh3.googleusercontent.com/...`, `w-7 h-7 rounded-sm`, `alt="Zecolis"`) |
| Wordmark | `Zé` en `text-zecolis-darknavy` + `Colis` en `text-zecolis-orange`, `font-extrabold text-[15px] tracking-tight` |
| Libellé de page | `Connexion`, `text-[11px] font-medium` |
| Pastille sécurité | `bg-white rounded-full border border-slate-100 shadow-pill-soft`, icône `shield-check` en `text-zecolis-green`, texte `Sécurisée` en `text-[9.5px] uppercase tracking-wider font-bold` |

### 5.3 Bloc titre

- `h1` : `text-[30px] leading-[1.2] font-extrabold tracking-tight text-zecolis-darknavy`, texte `Bon retour ` + `!` en `text-zecolis-orange`.
- Sous-titre : `text-[14px] text-zecolis-muted`, `Connectez-vous pour continuer à envoyer` + `<br>` + `ou voyager.`

### 5.4 Champ e-mail

| Propriété | Valeur |
| --- | --- |
| `label` | `EMAIL`, `text-[11px] font-semibold uppercase tracking-wider`, `for="emailInput"` |
| Icône | SVG Lucide `mail` **en dur dans le HTML** (pas un `<i data-lucide>`), `w-5 h-5`, `stroke-[1.8]`, bascule de couleur via `group-focus-within:text-zecolis-navy` |
| `input` | `type="email"`, `name="email"`, `id="emailInput"`, `required`, `placeholder="votre@email.com"` |
| Style | `h-14 pl-12 pr-4 bg-white rounded-2xl border border-[#F1F5F9] shadow-input-subtle text-[15px] font-medium` |
| Focus | `focus:border-zecolis-navy focus:ring-1 focus:ring-zecolis-navy transition-all duration-150` |
| Erreur | `#emailError`, `hidden` par défaut, icône `alert-circle` + `Veuillez entrer une adresse e-mail valide.` en `text-rose-500 text-[11.5px]` |

### 5.5 Champ mot de passe

| Propriété | Valeur |
| --- | --- |
| `label` | `MOT DE PASSE`, mêmes styles que `EMAIL` |
| Lien « Oublié ? » | `text-[12px] font-semibold text-zecolis-orange hover:text-orange-600`, `active-scale`, `href="javascript:void(0)"` |
| Icône | SVG Lucide `lock` en dur, `stroke-[1.8]` |
| `input` | `type="password"`, `name="password"`, `id="passwordInput"`, `required`, `placeholder="••••••••"` |
| Style | `h-14 pl-12 pr-12` + `tracking-widest`, et `focus:tracking-normal` |
| Bouton œil | `id="togglePasswordBtn"`, `type="button"`, `aria-label="Afficher ou masquer le mot de passe"`, icône Lucide `eye` en dur avec `id="eyeIcon"` |
| Erreur | `#generalError`, `hidden` par défaut, icône `alert-circle` + `Adresse e-mail ou mot de passe incorrect.` |

`tracking-widest` sur un champ mot de passe n'a d'effet que sur du texte masqué ; il produit l'espacement visuel attendu pour les caractères de remplacement.

### 5.6 Bouton principal

`h-14 bg-zecolis-navy hover:bg-[#082046] active:bg-[#061836] text-white text-[16px] font-bold rounded-2xl shadow-btn-navy active-scale`

Contient deux éléments :

| ID | Élément | État initial |
| --- | --- | --- |
| `btnText` | `<span>` | `Se connecter` |
| `btnSpinner` | `<div>` | `hidden animate-spin rounded-full h-5 w-5 border-2 border-white/30 border-t-white` |

### 5.7 Invite à l'inscription

`Pas encore de compte ?` + `S'inscrire` en `text-zecolis-orange font-bold hover:underline inline-block`, `href="javascript:void(0)"`.

### 5.8 Pied

- `shield` Lucide en `text-zecolis-green stroke-[2.2]` + `Vos informations restent sécurisées.` en `text-[11.5px] text-zecolis-muted`.
- Indicateur d'accueil iOS simulé : `w-32 h-1 bg-slate-300 rounded-full mt-4`.

## 6. Textes visibles (intégralité)

| Zone | Texte |
| --- | --- |
| Titre onglet | `Zecolis Mobile - Connexion` |
| Wordmark | `ZéColis` |
| En-tête | `Connexion` · `SÉCURISÉE` |
| Titre | `Bon retour !` |
| Sous-titre | `Connectez-vous pour continuer à envoyer` / `ou voyager.` |
| Labels | `EMAIL` · `MOT DE PASSE` |
| Placeholders | `votre@email.com` · `••••••••` |
| Lien | `Oublié ?` |
| Erreur e-mail | `Veuillez entrer une adresse e-mail valide.` |
| Erreur générique | `Adresse e-mail ou mot de passe incorrect.` |
| Bouton | `Se connecter` → `Connexion…` → `Connecté ✓` |
| Inscription | `Pas encore de compte ?` · `S'inscrire` |
| Pied | `Vos informations restent sécurisées.` |

## 7. Champs et données

| Champ | Type HTML | Attributs | Validation navigateur | Validation JS |
| --- | --- | --- | --- | --- |
| E-mail | `email` | `id=emailInput`, `name=email`, `required` | **Désactivée** (`novalidate` sur le `<form>`) | Regex `/^[^\s@]+@[^\s@]+\.[^\s@]+$/` sur `value.trim()` |
| Mot de passe | `password` | `id=passwordInput`, `name=password`, `required` | **Désactivée** | Présence non vide uniquement |

`novalidate` + validation JS maison : cohérent dans l'intention. Le `required` et le `type="email"` restent présents mais inactifs.

Aucun champ pour : code de vérification, Google/social login, téléphone, récupération de compte. **Aucun lien vers une page d'inscription.**

Aucune donnée fictive dans les champs — les valeurs sont vides au chargement, et c'est un progrès par rapport au fichier 1.

## 8. Boutons et actions

| # | Élément | Type | Comportement réel dans le code |
| --- | --- | --- | --- |
| 1 | Afficher / masquer le mot de passe | `<button>` + `onclick` | **Implémenté** : bascule `type` password↔text, toggle `tracking-widest`, change `data-lucide` de `eye` à `eye-off`, relance `lucide.createIcons()` |
| 2 | Se connecter | `<button type="submit">` | **Simulation** — voir § 9 |
| 3 | Oublié ? | `<a href="javascript:void(0)">` | **Inopérant** |
| 4 | S'inscrire | `<a href="javascript:void(0)">` | **Inopérant** |

Sur 4 actions : 1 réelle, 1 simulée, 2 mortes.

## 9. Interactions et comportements observés

### 9.1 Affichage du mot de passe — `togglePasswordVisibility()`

Séquence exacte :

1. `pwdInput.type` bascule `password` → `text` (ou l'inverse)
2. `tracking-widest` retiré (ou ajouté)
3. `eyeIcon.setAttribute('data-lucide', 'eye-off')` (ou `'eye'`)
4. `lucide.createIcons()` est relancé

**Risque non vérifié :** l'élément visé par `getElementById('eyeIcon')` est le `<svg>` **déjà rendu** dans le HTML, portant à la fois `data-lucide` et `id="eyeIcon"`. `lucide.createIcons()` remplace les éléments portant `data-lucide` par un nouvel élément. Si l'attribut `id` n'est pas transmis au nouvel élément, **`eyeIcon` devient `null` et la fonction échoue dès le deuxième clic**. Le premier clic fonctionne parce que le `id` est encore présent au moment de l'appel.

**À vérifier au navigateur.** Non testé ici.

**Observation secondaire :** d'autres icônes (`mail`, `lock`, `alert-circle` ×2, `shield`) sont écrites comme SVG bruts avec `class="lucide lucide-..."`, c'est-à-dire du **résultat d'une exécution précédente de Lucide** collé dans le HTML. Elles conservent `data-lucide` et sont donc également targeted par `createIcons()`, sans que ce soit nécessaire. Trace d'un copier-coller depuis le DOM rendu.

### 9.2 Soumission du formulaire — `handleLoginSubmit(event)`

Séquence exacte :

1. `event.preventDefault()` — aucune navigation
2. Masquage de `#emailError` et `#generalError`
3. Test regex sur l'e-mail
4. **Si e-mail vide ou invalide** → affichage de `#emailError`, `emailInput.focus()`, `return`
5. **Si mot de passe vide** → `passwordInput.focus()` et `return` — **aucun message d'erreur affiché**
6. **Si valide** → `btn.disabled = true`, `btnText = "Connexion…"`, spinner visible
7. `setTimeout` **900 ms** → `btn.disabled = false`, `btnText = "Connecté ✓"`, bouton `bg-zecolis-navy` → `bg-zecolis-green`
8. `setTimeout` **1800 ms** → retour à `bg-zecolis-navy`, `btnText = "Se connecter"`

### 9.3 Ce que la soumission ne fait PAS

- **Aucun appel réseau** : pas de `fetch`, pas de XHR.
- **Aucune référence à Firebase** : aucun import du SDK, aucun `signInWithEmailAndPassword`, aucun `onAuthStateChanged`.
- **Aucune vérification de identifiants** : `toto@x.fr` / `a` déclenche « Connecté ✓ ».
- **Aucune navigation après succès** : le bouton repasse à son état initial, la page ne change pas.
- **Aucun stockage de session**, aucun `localStorage`, aucun cookie.

> **Conclusion : cette page affiche une authentification factice.** Le bandeau vert « Connecté ✓ » est un effet visuel, pas un résultat d'authentification. Toute décision de développement doit partir de ce constat.

### 9.4 Autres comportements

- `lucide.createIcons()` au chargement.
- `.active-scale:active { transform: scale(0.98) }` sur les liens et le bouton — retour tactile.
- `transition-all duration-150` sur le bouton et les champs.
- `transition-colors` sur icônes de champ via `group-focus-within`.
- `group-focus-within` : le `<div>` parent porte `class="relative group"`.

## 10. États possibles

| État | Implémenté | Observation |
| --- | --- | --- |
| Repos | **Oui** | Bouton actif, aucune erreur |
| Erreur e-mail | **Oui** | `#emailError` affiché sur regex invalide |
| Erreur mot de passe vide | **Partiellement** | Focus posé, **aucun message** — l'utilisateur ne sait pas pourquoi rien ne se passe |
| Erreur identifiants | **Non atteignable** | `#generalError` existe dans le HTML avec le texte « Adresse e-mail ou mot de passe incorrect. », mais **aucun chemin du script ne l'affiche** : il n'y a pas de vérification d'identifiants |
| Chargement | **Oui (simulé)** | `disabled` + libellé « Connexion… » + spinner, pendant 900 ms fixes |
| Succès | **Oui (simulé)** | Bouton vert + « Connecté ✓ » pendant 1800 ms |
| Erreur réseau / serveur | **Non** | Aucun cas |
| Session expirée | **Non** | — |
| Compte verrouillé / non vérifié | **Non** | — |
| Champ désactivé pendant chargement | **Oui** | `btn.disabled = true` uniquement sur le bouton, **pas sur les champs** |

## 11. Navigation vers les autres écrans

**Aucun.** Les deux liens sortants sont des `javascript:void(0)`.

Conséquences :

- Pas d'accès à l'inscription (le § 7 du cahier des charges prévoit une acceptation de documents à l'inscription).
- Pas d'accès à la réinitialisation de mot de passe.
- Pas de retour possible.
- Aucune barre de navigation principale — cohérent pour un écran d'accès.

La page ne peut pas être reliée aux autres écrans à partir du code reçu.

## 12. Règles métier visibles dans le code

| Règle | Implémentation |
| --- | --- |
| Format e-mail | Regex simple `^[^\s@]+@[^\s@]+\.[^\s@]+$` — un seul `@`, un point après, pas d'espace |
| Mot de passe | Présence obligatoire ; **aucune règle de longueur, de complexité ou de format** |
| Anti-doublon d'envoi | Bouton `disabled` pendant les 900 ms simulées |
| Aucune autre | Pas de limitation de tentatives, pas de verrouillage, pas de jeton, pas de réinitialisation |

Aucune règle de gestion métier Zecolis (annonce, colis, transaction) n'apparaît sur cette page.

## 13. Dépendances techniques

| Type | Valeur | Remarque |
| --- | --- | --- |
| Tailwind | `https://cdn.tailwindcss.com` + `tailwind.config` inline | Script CDN, aucun build. **Ordre de déclaration à vérifier.** |
| Police | Google Fonts `Inter` 400/500/600/700/800 | **Conforme à § 4.4** |
| Icônes | `https://unpkg.com/lucide@latest` | Version non épinglée. Mélange de `<i data-lucide>` (1 icône) et de SVG bruts pré-rendus (6 icônes). |
| Logo | `lh3.googleusercontent.com/aida-public/AB6AXuBKGvn-dD18T9yL2TYuowMrW...` | Image distante non maîtrisée, `alt="Zecolis"` |
| CSS | 1 bloc : `*` (tap-highlight, box-sizing), `body`, `input:focus`, `.active-scale` | 15 lignes |
| JS | 1 bloc inline : 2 fonctions + init | ≈ 65 lignes |
| Build | Aucun | Ni Vite, ni React, ni TypeScript, ni Firebase, ni Capacitor |

**Aucun lien Firebase, React ou Capacitor.** Comme pour le fichier 1, il s'agit d'une maquette autonome.

## 14. Éléments provisoires ou fictifs

### Simulé (non fonctionnel)
| Élément | Nature de la simulation |
| --- | --- |
| `Connexion…` (900 ms) | Temporisation fixe |
| `Connecté ✓` + bouton vert (1800 ms) | Succès affichés sans aucune authentification |
| Spinner `animate-spin` | Animation CSS |
| `disabled` pendant le chargement | Empêche le double envoi |

### Fictif
| Élément | Observation |
| --- | --- |
| Indicateur d'accueil iOS | Simulé (`w-32 h-1`), alors que l'appareil le dessine déjà |
| Espace de respiration `h-2` en tête | 8 px factices stand-ins de la barre d'état réelle |
| Badge `SÉCURISÉE` | **Revendication de confiance statique.** Aucun code ne vérifie quoi que ce soit : ni HTTPS, ni état de session, ni configure de sécurité. Le § 3 du cahier des charges impose de ne pas présenter une vérification comme réussie si le système ne la confirme pas. |
| `Vos informations restent sécurisées.` | Même nature : mention statique, sans mécanisme derrière. |

### Non inventé — à noter positivement
Aucun montant, aucune note, aucun avis, aucun code de transaction fictif n'apparaissent sur cette page, contrairement au fichier 1.

## 15. Écarts avec l'identité visuelle de référence

| Point | Référence § 4 | Ce fichier | Écart |
| --- | --- | --- | --- |
| Police | Inter | **Inter** | ✔ Conforme |
| Fond général | `#F8FAFC` | `#F8FAFC` | ✔ Conforme |
| Cartes | `#FFFFFF` | `bg-white` | ✔ Conforme |
| Bordures | `#F1F5F9` | `border-[#F1F5F9]`, `border-slate-100` | ✔ Globalement conforme (`slate-100` = `#F1F5F9`, équivalent) |
| Couleurs de texte | `#0F172A` / `#64748B` | `text-zecolis-text` / `text-zecolis-muted` | ✔ Conforme |
| Navy / orange / vert | `#1D1D4B` / `#0B2A5B` / `#FF6B00` / `#22A55A` | via jettons `zecolis-*` | ✔ Conforme |
| **Couleur d'erreur** | **Aucune couleur d'erreur dans la palette** | **`text-rose-500`** | ✘ **Palette incomplète** : le rouge n'existe pas dans § 4.2. Décision à arbitrer (ajouter un rouge à la palette, ou réemployer une couleur existante). |
| **Gris des labels** | Texte secondaire `#64748B` | **`#94A3B8`** | ✘ Couleur hors palette, plus claire que le texte secondaire défini |
| **Placeholder** | Non spécifié | **`#CBD5E1`** | ✘ Hors palette |
| Ombres | « légères », éviter les « épaisses » | `btn-navy` : double ombre 20 px + 10 px | ✘ À arbitrer |
| Rayons | Cartes ~16 px, boutons ~12 px | `rounded-2xl` (16 px) sur champs et bouton | ✔ Conforme |
| glassmorphism / dégradés | Interdits | **Absent** | ✔ Conforme |
| Icônes | Lucide | Lucide | ✔ Conforme |

**Bilan : le fichier 2 est nettement plus fidèle à la référence § 4 que le fichier 1.** Il en reste 3 écarts de couleurs (`rose-500`, `#94A3B8`, `#CBD5E1`) et 1 d'ombre.

## 16. Problèmes mobiles et accessibilité

*(Analyse statique du code — aucun navigateur, aucun appareil réel utilisé. Aucun correctif n'a été appliqué à ce fichier.)*

### 16.1 Défilement — correct

Contrairement au fichier 1, **le défilement vertical fonctionne** :

| Élément | Valeur | Effet |
| --- | --- | --- |
| `<body>` | `h-full` | hauteur = viewport |
| `<main>` | `min-h-full` (et non `h-full`) | **le conteneur grandit si le contenu dépasse** |
| `overflow` | **aucune classe `overflow-hidden`** | scroll natif conservé |

Sur un clavier mobile qui recouvre la moitié de l'écran, la page défile et le champ masqué devient atteignable. **Aucun correctif requis ici.**

Point mineur : `overscroll-behavior-y: none` sur `body` supprime l'effet de rebond et le rafraîchissement par glissement. Acceptable pour un écran de formulaire.

### 16.2 Zones de sécurité — manquantes

| Élément | Présent | Impact |
| --- | --- | --- |
| `viewport-fit=cover` | **Non** | — |
| `env(safe-area-inset-top)` | **Non** — `pt-6` (24 px) + `h-2` (8 px factices) | Sur iPhone à encoche, le contenu démarre **sous l'encoche**. Les 8 px du `div.h-2` ne remplacent pas les 44–54 px de la barre d'état réelle. |
| `env(safe-area-inset-bottom)` | **Non** — `pb-5` (20 px) | L'indicateur d'accueil simulé se superpose à la **barre de gestes réelle** (~34 px). |

### 16.3 Zones tactiles trop petites

| Élément | Taille estimée | Minimum usuel |
| --- | --- | --- |
| « Oublié ? » | `text-[12px]` sans padding → **≈ 16 px** | 44 px |
| « S'inscrire » | `text-[13.5px]` `inline-block` → **≈ 18 px** | 44 px |
| Bouton œil | hauteur 56 px (h-14), largeur ≈ 40 px | ✔ hauteur OK, largeur limite |
| Champs e-mail / mot de passe | `h-14` = 56 px | ✔ Excellent |
| Bouton principal | `h-14` = 56 px | ✔ Excellent |

Les deux liens de service sont les cibles les plus petites de l'écran, et précisément les plus difficiles à toucher.

### 16.4 `select-none` sur le `<body>` — défaut grave sur un formulaire

`class="select-none"` appliqué au `<body>` désactive la sélection de texte **sur toute la page, y compris à l'intérieur des champs de saisie**. Conséquences :

- l'utilisateur ne peut plus sélectionner ni corriger une partie de l'e-mail saisie ;
- le **collage** depuis le presse-papiers peut être refusé ou altéré selon le navigateur ;
- les gestionnaires de mots de passe et l'autofill sont prone à être affectés.

Sur une page de connexion, c'est un défaut fonctionnel, pas seulement esthétique. Le fichier 1 était moins exposé (aucun champ) ; ici il touche directement la saisie.

### 16.5 Zoom désactivé — défaut d'accessibilité

```html
<meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

`maximum-scale=1.0` + `user-scalable=no` **interdit le pincement-zoom**. C'est un manquement aux critères WCAG 1.4.4 (Resize Text) et 1.4.10 (Reflow), et une difficulté concrète pour les personnes malvoyantes — d'autant plus que la page contient déjà du texte en `text-[9.5px]`.

### 16.6 Typographie trop petite

| Taille | Élément |
| --- | --- |
| `text-[9.5px]` | Badge `SÉCURISÉE` — **le plus petit texte de l'application**, sur un mot de 8 lettres en capitales |
| `text-[11px]` | Labels `EMAIL`, `MOT DE PASSE`, libellé `Connexion` |
| `text-[11.5px]` | Les 2 messages d'erreur, message de réassurance du pied |

Les messages d'erreur sont les plus concernants : ils sont censés être lus au moment où l'utilisateur échoue.

### 16.7 Largeur et affichage

- `max-w-[390px]` : conforme au viewport de référence § 4.5. Sur les grands téléphones (430–480 px de large), la colonne de 390 px est centrée avec des marges vides — choix assumé, à confirmer.
- Sous 320 px, `px-6` (48 px) laisse 272 px de largeur utile ; les champs restent utilisables. **Aucun débordement horizontal.**
- Le `<br>` câblé dans le sous-titre force 2 lignes. Sur un écran étroit, `pour continuer à envoyer` peut déborder visuellement ou se couper sans espace. **À vérifier.**
- Aucun traitement du mode paysage ni des écrans pliants.

### 16.8 Autres points d'accessibilité

| Point | Observation |
| --- | --- |
| Erreurs non annoncées | `#emailError` et `#generalError` n'ont ni `role="alert"` ni `aria-live` : un lecteur d'écran n'annoncera pas l'erreur |
| `input:focus { outline: none }` | Règle globale. Les champs ont un anneau de focus visible (`focus:ring-1`), donc acceptable, mais la règle est large |
| Icônes décoratives | Les SVG `mail`, `lock`, `alert-circle`, `shield` portent `aria-hidden="true"` ✔ |
| Bouton œil | `aria-label="Afficher ou masquer le mot de passe"` ✔ |
| Champs | `label` associé par `for` / `id` ✔ |
| Champs désactivés au chargement | Non désactivés pendant les 900 ms : l'utilisateur peut modifier l'e-mail pendant que le faux chargement tourne |

## 17. Points à vérifier

1. **Nom de fichier d'origine** — non communiqué.
2. **Aucun élément d'authentification réel.** Firebase Auth est-il prévu ? Quel fournisseur ? Le § 8 mentionne Firebase sans détailler les services.
3. **Écran d'inscription** — prévu mais non reçu. Le § 7 du cahier des charges prévoit l'acceptation de documents à l'inscription : cet écran est indispensable.
4. **Réinitialisation de mot de passe** — lien présent mais mort.
5. **`tailwind.config` déclaré après le script CDN** — la palette `zecolis-*` se génère-t-elle réellement ? Non testé.
6. **Persistance de `id="eyeIcon"`** à travers `lucide.createIcons()` — le bouton œil surviv-il à un 2ᵉ clic ? Non testé.
7. **`#generalError` inatteignable** — un message d'erreur existe mais aucun chemin ne l'affiche. Faut-il le brancher sur un vrai échec d'authentification ?
8. **Mot de passe vide sans message** — bug d'ergonomie à corriger.
9. **Badge `SÉCURISÉE` et message du pied** — revendication de confiance statique. Le § 3 interdit de présenter une vérification comme réussie sans confirmation système. À arbitrer.
10. **`rose-500` absent de la palette** — quelle couleur d'erreur retenir ?
11. **`#94A3B8` et `#CBD5E1` hors palette** — à remplacer par `#64748B` ou à ajouter.
12. **`zecolis-lightborder: #E2E8F0`** — déclaré dans la config mais non utilisé. Le retirer ou l'utiliser ?
13. **Ombre `btn-navy`** — trop marquée au regard de § 4.4 ?
14. **Ni inscription, ni navigation depuis la page** — le parcours d'entrée dans l'application n'existe pas.
15. **Aucun lien vers les documents légaux** — § 7 les prévoit au profil, pas à la connexion. Absence conforme ou à revoir ?
16. **`<br>` câblé** dans le sous-titre — à tester sur 320 px.
17. **Doublon à surveiller** — la palette `zecolis-*` définie ici est-elle aussi défini ailleurs, avec des valeurs potentiellement différentes ? À comparer avec les fichiers suivants.

## 18. En résumé

Seul écran reçu qui **saisit des données**. Le défilement mobile est **correct**, contrairement au fichier 1. Palette et police **conformes à § 4** (3 couleurs et 1 ombre hors norme). En revanche : **authentification entièrement simulée** — aucun réseau, aucun Firebase, aucune vérification d'identifiants, et le bouton affiche « Connecté ✓ » pour n'importe quelle adresse valide avec n'importe quel mot de passe. Défauts mobiles à corriger : `user-scalable=no` (zoom bloqué), `select-none` sur le `<body>` (bloque la saisie et le collage), zones de sécurité iOS absentes, et deux liens de service d'environ 16–18 px de haut.