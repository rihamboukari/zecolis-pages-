# Mot de passe oublié

| Rubrique | Valeur |
| --- | --- |
| **Nom de la page** | Mot de passe oublié (titre du document : « ZECOLIS - Mot de passe oublié ») |
| **Fichier source** | Nom de fichier d'origine **non communiqué**. Copie conservée sous `SOURCES_HTML_ORIGINALES/mot_de_passe_oublie.html` (nom de travail). |
| **Catégorie retenue** | `06_PROFIL_ET_COMPTE` — récupération d'accès. Même arbitrage que les fichiers 2 et 3 : un dossier `AUTHENTIFICATION` dédié devient cohérent (les trois écrans d'accès sont reçus). **À confirmer.** |
| **Statut d'analyse** | Analysé (fichier unique, autonome) |
| **Nature** | Maquette HTML autonome à deux vues (formulaire puis confirmation), avec **envoi simulé**. Aucun appel réseau. |

---

## 1. Objectif de la page

Permettre à un utilisateur ayant oublié son mot de passe de demander un lien de réinitialisation en fournissant son adresse e-mail, puis l'informer que la demande a été prise en compte.

**C'est le seul fichier reçu qui gère correctement la confidentialité de la réponse** : le message de confirmation ne révèle pas si un compte existe (voir § 10).

## 2. Utilisateur concerné et rôle métier

Utilisateur **non connecté**, quel que soit son rôle. Aucun élément du fichier ne mentionne les rôles : la page est neutre, ce qui est cohérent avec § 2.1.

## 3. Point d'entrée dans la navigation

**Seul des trois écrans d'accès à disposer d'un bouton retour réel** :

```html
<button onclick="window.history.back()" aria-label="Retour" ...>
```

| Élément | Cible | Effet |
| --- | --- | --- |
| Bouton retour (⭠) | `window.history.back()` | **Réel** — mais dépend de l'historique du navigateur : si la page est ouverte directement, il ne se passe rien |
| « Se connecter » (fin de formulaire) | `href="#"` | **Inopérant** — ne mène nulle part |
| Bouton « Retour à la connexion » (vue confirmation) | `resetToInitialState()` | **Réel dans un sens** — revient au formulaire, **pas** à l'écran de connexion malgré son libellé |

**Incohérence de libellé à relever :** le bouton de la vue de confirmation annonce « Retour à la connexion » mais ramène au formulaire de saisie e-mail. Le libellé promet une navigation, le code fait une réinitialisation de formulaire.

Côté entrée : le fichier 2 (Connexion) pointe vers cet écran par « Oublié ? » → `href="javascript:void(0)"`, **lien mort**. Le parcours est donc toujours non raccordé, mais c'est le seul des trois fichiers dont la sortie est partiellement réelle.

## 4. Structure de l'écran (ordre de lecture)

`body` en `flex flex-col justify-between`, `max-w-[390px] mx-auto`, **deux `<section>` mutuellement exclusives** :

| Zone | Contenu |
| --- | --- |
| 1. En-tête | Bouton retour (44×44) · pastille logo + « Réinitialisation » · pastille « Sécurisé » |
| 2. Vue 1 — `#reset-form-container` | Carré icône clé · `h1` « Mot de passe oublié ? » · sous-titre · champ e-mail · bouton principal · lien « Se connecter » |
| 3. Vue 2 — `#confirmation-container` *(masquée)* | Carré icône mail-check · `h2` « Vérifiez votre e-mail » · texte avec l'e-mail saisi · encart anti-spam · bouton « Retour à la connexion » |
| 4. Pied | Message « Vos informations restent sécurisées. » + indicateur d'accueil iOS simulé |

## 5. Sections et composants visuels

### 5.1 Configuration Tailwind inline — **convention différente des fichiers 2 et 3**

| Jeton | Valeur | § 4.2 |
| --- | --- | --- |
| `brand-navy` | `#0B2A5B` | ✔ |
| `brand-dark` | `#1D1D4B` | ✔ |
| `brand-orange` | `#FF6B00` | ✔ |
| `brand-green` | `#22A55A` | ✔ |
| `brand-bg` | `#F8FAFC` | ✔ |
| `brand-secondary` | `#64748B` | ✔ (= texte secondaire de § 4.2) |
| `brand-muted` | `#94A3B8` | ✘ hors palette |
| `brand-border` | `#F1F5F9` | ✔ |

**Deux écarts méthodologiques par rapport aux fichiers 2 et 3 :**

1. **Préfixe `brand.*` au lieu de `zecolis.*`** — troisième convention de nommage (sur les 4 fichiers qui déclarent une palette : `zecolis.*` dans les fichiers 2 et 3, `brand.*` ici, `zecolis-*` dans le fichier 5). Le fichier 1 n'a aucune configuration.
2. **Aucune ombre personnalisée** déclarée : le fichier utilise `shadow-xs` et `shadow-md shadow-[#0B2A5B]/20` de Tailwind standard, là où les fichiers 2 et 3 déclarent `btn-navy`, `input-subtle`, `pill-soft`.

**Et surtout : aucun de ces 8 jetons n'est utilisé.** Toutes les couleurs sont écrites **en dur** dans les classes (`bg-[#0B2A5B]`, `text-[#1D1D4B]`, `border-[#F1F5F9]`…). La configuration est un résidu de plus, sans effet.

### 5.2 Police — **divergence confirmée avec § 4.4**

```js
fontFamily: { sans: ['Plus Jakarta Sans', 'Inter', 'system-ui', 'sans-serif'] }
```
```css
@import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap');
body { font-family: 'Plus Jakarta Sans', sans-serif; }
```

- **Plus Jakarta Sans** est chargée et placée **en première position**, devant Inter.
- **Inter n'est jamais chargée** dans ce fichier.
- Or § 4.4 fixe Inter.

> **Deuxième fichier à diverger de § 4.4 sur la police** (après le fichier 1). Le § 4.4 tranche : c'est le fichier 4 qui est à corriger.

### 5.3 Chargement de Tailwind — **première utilisation des plugins**

```html
<script src="https://cdn.tailwindcss.com?plugins=forms,container-queries"></script>
```

Premier fichier à charger les plugins `forms` et `container-queries` (le fichier 5 fera de même). Le plugin `forms` **modifie le rendu natif des champs** (couleur de fond, bordure, `appearance: none`). C'est probablement la raison pour laquelle ce fichier écrit les champs entièrement en dur, alors que les fichiers 2 et 3 les stylent en `<input class="...">` sans cette contrainte.

Conséquence : **les cinq fichiers n'utilisent pas la même base Tailwind** — `forms` est actif sur 2 fichiers sur 5, donc les mêmes classes ne produisent pas le même rendu. Voir `09_POINTS_A_VERIFIER.md`, registre D.

### 5.4 Bouton retour

`w-11 h-11` (44×44 px) `rounded-full bg-white border-[#F1F5F9] shadow-xs active:scale-95`, SVG `arrow-left` **pré-rendu** (donc sans `aria-hidden` ni passe-partout), `aria-label="Retour"` ✔.

**Seule cible tactile de tout le fichier atteignant exactement 44 px.** Bien conçu.

### 5.5 Pastilles d'en-tête

| Élément | Détail |
| --- | --- |
| Logo | `<img>` distante `lh3.googleusercontent.com/...AB6AXuDIyiOkLs9h1...` — **URL différente des fichiers 2 et 3**, `h-7`, `alt="ZECOLIS Logo"` |
| Libellé | `Réinitialisation`, `text-xs font-semibold text-[#0B2A5B]` |
| Pastille sécurité | `shield-check` + `SÉCURISÉ` en `text-[10px] uppercase` (ici **10 px**, et non 9,5 px) |

### 5.6 Icône-tuile

Carré `w-[52px] h-[52px] rounded-2xl` avec fond teinté et icône Lucide centrée :

| Vue | Fond | Icône | Couleur |
| --- | --- | --- | --- |
| Formulaire | `bg-[#FFF3EB]` | `key-round` | `#FF6B00` |
| Confirmation | `bg-emerald-50` | `mail-check` | `#22A55A` |

`#FFF3EB` est **hors palette § 4.2** (orange très pâle non déclaré). `emerald-50` est une couleur Tailwind standard, hors palette également.

### 5.7 Bloc titre

- Vue 1 : `h1` `text-2xl font-extrabold` — « Mot de passe » en `#1D1D4B` + « oublié ? » en `#FF6B00`.
- Vue 2 : `h2` `text-2xl font-extrabold` — « Vérifiez votre » + « e-mail » en `#22A55A`.

`h1` puis `h2` : hiérarchie cohérente entre les deux vues. Le § 4.4 impose `h1` unique → **respecté**.

### 5.8 Champ e-mail

| Propriété | Valeur |
| --- | --- |
| `label` | `Adresse e-mail` — **casse normale**, `text-[11px] uppercase` appliqué (rendu en capitales), `text-[#94A3B8]` |
| `for` / `id` | `email-input` / `email-input` (kebab-case, **contrairement aux fichiers 2 et 3** qui utilisent le camelCase) |
| Icône | SVG `mail` pré-rendu, `w-5 h-5` à gauche, **sans** bascule de couleur au focus (contrairement aux fichiers 2 et 3 qui utilisent `group-focus-within`) |
| `input` | `type="email"`, `name="email"`, `required`, `placeholder="votre@email.com"`, **pas d'`id` en camelCase** |
| Style | `h-14 pl-12 pr-4 bg-white rounded-2xl border-[#F1F5F9] focus:border-[#0B2A5B] focus:ring-1 shadow-sm` |
| Erreur | **aucun bloc d'erreur dans le fichier** |

### 5.9 Bouton principal

```html
w-full h-14 bg-[#0B2A5B] text-white rounded-2xl font-bold text-base
shadow-md shadow-[#0B2A5B]/20 active:scale-[0.98] mt-4
```

`#btn-text` (libellé) + `#btn-spinner` (SVG `animate-spin` inline, `hidden`). Le spinner est **un SVG écrit à la main**, pas une icône Lucide — **seule occurrence du fichier**.

Ombre : **une seule ombre** `shadow-md`, colorée `#0B2A5B/20`. Plus légère que la double ombre `btn-navy` des fichiers 2 et 3 → **plus conforme à § 4.4** (« ombres légères »).

### 5.10 Vue de confirmation

| Élément | Contenu |
| --- | --- |
| `h2` | `Vérifiez votre e-mail` |
| Paragraphe | « Si un compte est associé à l'adresse **`<strong id="submitted-email-text">`**, vous recevrez les instructions pour réinitialiser votre mot de passe. » |
| Encart | « Pensez à vérifier votre dossier de courriers indésirables (spams) si vous ne trouvez pas notre message d'ici quelques minutes. » — `bg-white rounded-2xl p-4 border-[#F1F5F9] shadow-xs` |
| Bouton | `Retour à la connexion` → `onclick="resetToInitialState()"` |

**Point qualité à souligner :** la formulation « Si un compte est associé à l'adresse … » est **volontairement neutre**. Elle n'affirme ni l'existence ni l'inexistence du compte. C'est la bonne pratique contre l'énumération de comptes, et elle est respectée ici. Le § 3 du cahier des charges impose de ne pas présenter une vérification comme réussie sans confirmation système : ce texte ne le fait pas.

L'e-mail saisi est réinjecté dans la page via `textContent` (§ 9.2) — **pas de risque d'injection HTML**, c'est correct.

## 6. Textes visibles (intégralité)

| Zone | Texte |
| --- | --- |
| Titre onglet | `ZECOLIS - Mot de passe oublié` |
| En-tête | `Réinitialisation` · `SÉCURISÉ` |
| Titre | `Mot de passe oublié ?` |
| Sous-titre | `Entrez votre adresse e-mail pour recevoir un lien de réinitialisation sécurisé.` |
| Label | `Adresse e-mail` |
| Placeholder | `votre@email.com` |
| Bouton | `Réinitialiser mon mot de passe` → `Envoi en cours...` |
| Lien bas de formulaire | `Vous vous souvenez de votre mot de passe ?` / `Se connecter` |
| Titre confirmation | `Vérifiez votre e-mail` |
| Texte confirmation | `Si un compte est associé à l'adresse <e-mail>, vous recevrez les instructions pour réinitialiser votre mot de passe.` |
| Encart | `Pensez à vérifier votre dossier de courriers indésirables (spams) si vous ne trouvez pas notre message d'ici quelques minutes.` |
| Bouton confirmation | `Retour à la connexion` |
| Pied | `Vos informations restent sécurisées.` |

> Les textes ci-dessus sont recopiés depuis le HTML, accents réintroduits. Le fichier est bien encodé en UTF-8 (§ 13).

## 7. Champs et données

| Champ | Type | Attributs | Validation navigateur | Validation JS |
| --- | --- | --- | --- | --- |
| E-mail | `email` | `id="email-input"`, `name="email"`, `required` | **Activée** (pas de `novalidate`) | Présence non vide uniquement |

**Le seul fichier reçu où la validation native du navigateur n'est pas désactivée** : le `<form>` ne porte pas `novalidate`. Le champ vide est donc bloqué par le navigateur (bulle « Veuillez remplir ce champ »).

**Conséquence importante :** le `if (!emailValue) return;` du script (ligne 167) est **inatteignable** sur un champ vide, puisque le navigateur bloque déjà la soumission. Ce garde-fou est redondant — et le vrai validateur, lui, n'existe pas (§ 10).

| Traitement | Implémenté |
| --- | --- |
| Trim de l'e-mail | ✔ `value.trim()` |
| Regex de format | ✘ **aucune** — `toto` passe |
| Normalisation en minuscules | ✘ aucune |
| Blocage des adresses jetables | ✘ aucun |
| Limitation de tentatives | ✘ aucune (ni rate limit, ni cooldown, ni captcha) |

Aucun champ pour : nouveau mot de passe, code de vérification, confirmation par code. **L'écran de définition du nouveau mot de passe manque** — c'est une étape indispensable du parcours.

## 8. Boutons et actions

| # | Élément | Type | Comportement réel dans le code |
| --- | --- | --- | --- |
| 1 | Retour (⭠) | `<button>` | **Réel** — `window.history.back()` |
| 2 | Réinitialiser mon mot de passe | `<button type="submit">` | **Simulé** — 900 ms, puis bascule de vue |
| 3 | Importer / renvoyer l'e-mail | — | **Absent** |
| 4 | Se connecter | `<a href="#">` | **Inopérant** |
| 5 | Retour à la connexion | `<button type="button">` | **Partiellement réel** — revient au formulaire, libellé trompeur |
| 6 | Bouton œil | — | **Absent** (sans objet ici) |

Sur 5 actions : 2 réelles (dont 1 au libellé inexact), 1 simulée, 1 inopérante.

## 9. Interactions et comportements observés

### 9.1 Soumission — `handleFormSubmit(event)`

Séquence exacte :

1. `event.preventDefault()` — aucune navigation
2. `emailValue = emailInput.value.trim()`
3. **Si vide → `return`** (inatteignable, cf. § 7)
4. `submitBtn.disabled = true`, `btnText = 'Envoi en cours...'`, spinner visible
5. `setTimeout` **900 ms** :
   - `#submitted-email-text.textContent = emailValue`
   - `#reset-form-container` → `hidden`
   - `#confirmation-container` → visible
   - `lucide.createIcons()` relancé (les icônes de la vue révélée sont réinjectées)
   - bouton réinitialisé (`disabled = false`, libellé d'origine, spinner masqué)

Aucune vérification de format, aucun appel réseau, aucune gestion d'erreur, aucun cas « aucun compte associé ».

### 9.2 Réinitialisation — `resetToInitialState()`

1. `#confirmation-container` → `hidden`
2. `#reset-form-container` → visible
3. `reset-password-form.reset()` — vide le champ
4. `lucide.createIcons()`

Fonctionnel et correct. **Seule logique de ce fichier réellement irréprochable.**

### 9.3 Ce que la soumission ne fait PAS

- **Aucun appel réseau** : ni `fetch`, ni XHR, ni SDK de messagerie.
- **Aucun token, aucun lien de réinitialisation n'est généré ni affiché.**
- **Aucune écriture du texte de confirmation** avant la temporisation — si l'utilisateur appuie sur « Retour à la connexion » pendant les 900 ms, le texte est vide (sans conséquence visible).
- **Aucune protection contre l'envoi multiple** au-delà du `disabled` de 900 ms.
- **Aucun échec possible** : l'écran de confirmation s'affiche quoi qu'il arrive, y compris pour une adresse inexistante ou malformée.

> **Conclusion : la page affiche un parcours de récupération complet et plausible, mais aucun e-mail n'est jamais envoyé.** C'est le même schéma que le fichier 2 (Connexion) — un effet d'interface complet posé sur une absence totale de logique serveur.

### 9.4 Autres comportements

- `lucide.createIcons()` au chargement, puis à chaque bascule de vue.
- **Aucune animation entre les deux vues** malgré `transition-opacity duration-300` sur les deux `<section>` : la bascule se fait par ajout/suppression de `hidden`, donc **l'opacité n'est jamais animée**. Transition déclarée sans effet — code mort.
- Aucun `focus()` automatique sur le champ au chargement ; aucun `focus()` sur le titre de la vue de confirmation → **le changement de vue n'est pas annoncé** aux lecteurs d'écran.
- Aucune conservation de l'e-mail en mémoire après `reset()`.

## 10. États possibles

| État | Implémenté | Observation |
| --- | --- | --- |
| Repos (formulaire) | **Oui** | État initial |
| Champ vide au clic | **Oui, par le navigateur** | Bulle native — le script ne gère pas ce cas |
| E-mail malformé (`toto`) | **Non** | **La vue de confirmation s'affiche quand même** : aucun contrôle de format |
| Chargement | **Oui (simulé)** | 900 ms, `disabled` + « Envoi en cours... » + spinner |
| Succès | **Oui (simulé)** | Bascule vers la vue de confirmation |
| « Aucun compte associé » | **Non — et c'est volontaire** | La formulation neutre l'absorbe, ce qui est la bonne pratique |
| Erreur réseau / serveur | **Non** | Aucun cas |
| Adresse déjà en cours de traitement | **Non** | Cas métier absent |
| Rate limit / captcha | **Non** | **Absence notable** sur un endpoint d'envoi d'e-mail |
| Lien expiré | **Non applicable** | L'écran de saisie du nouveau mot de passe n'existe pas |
| Retour à l'état initial | **Oui** | `resetToInitialState()` |

## 11. Navigation vers les autres écrans

| Vers | État |
| --- | --- |
| Écran précédent (historique) | **Possible** via le bouton retour — non garanti si la page est ouverte en direct |
| Écran Connexion | **Non** — `href="#"` ; et le bouton « Retour à la connexion » ne navigue pas |
| Écran de définition du nouveau mot de passe | **Manquant** — le § 9 du parcours s'arrête ici |
| Écran d'inscription | **Absent** — pas de lien « pas encore de compte ? » |

Depuis le fichier 2, **aucun accès** : « Oublié ? » est un `javascript:void(0)`.

## 12. Règles métier visibles dans le code

| Règle | Implémentation |
| --- | --- |
| Format de l'e-mail | **Aucune règle** — le garde-fou est la validation native `type="email"`, qui laisse passer `toto@x` selon le navigateur |
| Anti-doublon d'envoi | `disabled` pendant 900 ms seulement — **insuffisant** en usage réel |
| Limitation de débit | **Aucune** |
| Divulgation d'existence de compte | **Correctement évitée** ✔ |
| Durée de validité du lien | **Non déterminée** — aucun lien n'est produit |
| Usage unique du lien | **Non applicable** — aucun lien n'est produit |

Aucune règle métier Zecolis (annonce, colis, transaction) n'apparaît sur cette page.

## 13. Dépendances techniques

| Type | Valeur | Remarque |
| --- | --- | --- |
| Tailwind | `cdn.tailwindcss.com?plugins=forms,container-queries` | **Seul fichier à charger des plugins** — base de rendu différente des fichiers 2 et 3 |
| Police | `Plus Jakarta Sans` 400→800 via **`@import` CSS** | ✘ contraire à § 4.4 ; **Inter non chargée** |
| Icônes | `unpkg.com/lucide@latest` | Version non épinglée ; SVG **pré-rendus** (résultat d'un `createIcons()` précédent collé dans le HTML) |
| Logo | `lh3.googleusercontent.com/...AB6AXuDIyiOkLs9h1...` | Image distante non maîtrisée, `alt="ZECOLIS Logo"` |
| CSS | 1 bloc `@import` + 3 règles | 8 lignes |
| JS | 1 bloc inline : 2 fonctions + init | 47 lignes, **toutes utilisées** (contrairement aux fichiers 1 et 3) |
| Build | Aucun | Ni Vite, ni React, ni TypeScript, ni Firebase, ni Capacitor |
| Encodage | UTF-8 **sans BOM**, `<meta charset="utf-8">` | ✔ conforme |

> **Observation sur l'ordre des déclarations :** la `tailwind.config` est déclarée **après** le script Tailwind CDN et **avant** le script Lucide — c'est-à-dire **la même convention que les fichiers 2 et 3**. Les trois fichiers d'accès partagent donc ce point à vérifier : selon la version de la CDN, la configuration peut n'être pas prise en compte, et toute la palette se rabat alors sur du Tailwind standard. **Non testé ici.**

## 14. Éléments provisoires ou fictifs

### Simulé (non fonctionnel)

| Élément | Nature de la simulation |
| --- | --- |
| `Envoi en cours...` (900 ms) | Temporisation fixe |
| Vue de confirmation | Affichée sans aucun envoi |
| Spinner `animate-spin` | Animation CSS pure |
| Inscription « lien de réinitialisation sécurisé » | **Aucun lien n'est généré** |

### Fictif

| Élément | Observation |
| --- | --- |
| Badge `SÉCURISÉ` | Revendication de confiance statique (idem fichiers 2 et 3) |
| `Vos informations restent sécurisées.` | Mention statique |
| « lien de réinitialisation **sécurisé** » | Qualificatif non adossé à un mécanisme |
| Indicateur d'accueil iOS | Simulé (`w-32 h-1`) alors que l'appareil le dessine déjà |
| absence d'espace factice en tête | **Différence notable** : pas de `div.h-2`, mais `pt-4` seulement — voir § 16.2 |

### Non inventé — à noter positivement

Aucun montant, aucune note, aucun avis, aucun code de transaction, **aucune donnée pré-remplie**, et **aucune affirmation de succès non vérifié**. Le texte de confirmation est formulé avec une prudence telle qu'on la rencontre rarement dans une maquette : c'est le meilleur texte de tous les fichiers reçus.

## 15. Écarts avec l'identité visuelle de référence

| Point | Référence § 4 | Ce fichier | Écart |
| --- | --- | --- | --- |
| **Police** | Inter | **Plus Jakarta Sans** (Inter non chargée) | ✘ **Divergence directe** (comme le fichier 1) |
| Fond général | `#F8FAFC` | `#F8FAFC` | ✔ |
| Champs | `#FFFFFF` | `bg-white` | ✔ |
| Bordures | `#F1F5F9` | `border-[#F1F5F9]` | ✔ |
| Texte | `#0F172A` / `#64748B` | `#1D1D4B` / `#64748B` | ✔ |
| Navy / orange / vert | `#1D1D4B` / `#0B2A5B` / `#FF6B00` / `#22A55A` | en dur, valeurs identiques | ✔ |
| **Gris des labels** | `#64748B` | **`#94A3B8`** | ✘ hors palette (3ᵉ fichier) |
| **Placeholder** | non spécifié | **`#CBD5E1`** | ✘ hors palette |
| **Fond de tuile icône** | non spécifié | **`#FFF3EB`**, `emerald-50` | ✘ hors palette |
| **Ombres** | « légères », éviter les « épaisses » | `shadow-xs` et **une seule** `shadow-md` | ✔ **Meilleur que les fichiers 2 et 3** |
| Rayons | cartes ~16 px, boutons ~12 px | `rounded-2xl` (16 px), `rounded-full` (bouton retour) | ✔ |
| glassmorphism / dégradés | interdits | **absents** | ✔ |
| Icônes | Lucide | Lucide (+ 1 spinner SVG manuel) | ✔ |
| Badge `SÉCURISÉ` | non prévu | en-tête | ✘ à arbitrer (idem fichiers 2 et 3) |

**Bilan : 2 écarts de couleur (`#94A3B8`, `#CBD5E1`, + 2 fonds de tuile), 1 divergence de police majeure, mais des ombres plus légères que partout ailleurs.**

## 16. Problèmes mobiles et accessibilité

*(Analyse statique du code — aucun navigateur, aucun appareil réel utilisé. Aucun correctif n'a été appliqué à ce fichier.)*

### 16.1 Défilement — correct

| Élément | Valeur | Effet |
| --- | --- | --- |
| `<body>` | `h-full` | hauteur = viewport |
| `<main>` | `flex-1` (et non `h-full`) | grandit si le contenu dépasse |
| `overflow` | aucun `overflow-hidden` ; `overflow-x-hidden` seul | **scroll natif conservé** ✔ |

Page courte : pas de risque de débordement. La vue de confirmation est plus haute que le formulaire (encart + bouton supplémentaire) mais reste largement sous 844 px. **Aucun correctif requis.**

### 16.2 Zones de sécurité — manquantes

| Élément | Présent | Impact |
| --- | --- | --- |
| `viewport-fit=cover` | **Non** | — |
| `env(safe-area-inset-top)` | **Non** — `pt-4` (16 px) | **Le pire cas des cinq fichiers** : ni `pt-6`, ni espace factice. Sur iPhone à encoche, le bouton retour de 44 px est **sous l'encoche**. |
| `env(safe-area-inset-bottom)` | **Non** — `pb-2` (8 px) | **Le pire cas** : l'indicateur d'accueil simulé (`w-32 h-1`) se superpose à la barre de gestes réelle (≈ 34 px), et le pied est le dernier élément du `flex justify-between`. |

> **Correction prioritaire sur ce fichier** : `pt` et `pb` sont les plus faibles des cinq fichiers reçus, et c'est le seul où le bouton retour (44 px, la plus grosse cible de l'écran) est le plus exposé.

Point positif : **pas d'espace factice pour la barre d'état** — les fichiers 1, 2 et 3 simulent une barre d'état avec un `div.h-2`. Ce fichier ne le fait pas. C'est une incohérence de plus entre fichiers, mais dans le bon sens : à généraliser.

### 16.3 Zones tactiles

| Élément | Taille estimée | Minimum usuel |
| --- | --- | --- |
| Bouton retour | `w-11 h-11` = **44×44** | ✔ **exactement au minimum** |
| Champ e-mail | `h-14` = 56 px | ✔ |
| Bouton principal | `h-14` = 56 px | ✔ |
| Bouton « Retour à la connexion » | `h-14` = 56 px | ✔ |
| Lien « Se connecter » | `text-sm block` avec `mt-1` → ≈ **20 px** de haut | ✘ petit (mais `block` donne toute la largeur, ce qui aide au pouce) |

**Meilleur profil tactile des trois écrans d'accès**, grâce au bouton retour en 44×44. Le lien « Se connecter » reste sous-minimum.

### 16.4 `user-select: none` sur le `<body>` — défaut

```css
body { -webkit-tap-highlight-color: transparent; user-select: none; }
```

Pire que les fichiers 2 et 3, qui utilisaient la classe Tailwind `select-none` (donc une seule page affectée) : ici la règle est **dans le CSS du `body`**, donc potentiellement plus forte. Comme sur le fichier 3, l'utilisateur ne peut pas **sélectionner ni copier l'adresse e-mail qu'il vient de saisir** — ce qui est précisément le geste naturel pour vérifier une saisie ou la coller ailleurs.

Sur un champ unique, l'impact est moindre qu'un formulaire à 6 champs, mais le défaut reste réel. **Retirer `user-select: none`.**

### 16.5 Zoom désactivé

```html
<meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

**Troisième fichier consécutif** avec le zoom pincement interdit. Manquement WCAG 1.4.4 et 1.4.10, aggravé par la police Plus Jakarta Sans (x-hauteurs plus larges, texte plus serré). **Manquement commun aux cinq fichiers reçus : à corriger une fois pour toutes.**

### 16.6 Typographie

| Taille | Élément |
| --- | --- |
| `text-[10px]` | Badge `SÉCURISÉ` — taille **en dessous du seuil de lisibilité** du § 4.4. **Pas le plus petit des cinq fichiers** : le fichier 5 descend à `text-[8px]` (donc 8 px), et le fichier 1 utilise aussi 10 px. Les fichiers 2 et 3 sont à 9,5 px. |
| `text-[11px]` | Label `Adresse e-mail` |
| `text-xs` (12 px) | Libellé `Réinitialisation`, encart anti-spam, pied |
| `text-sm` (14 px) | Titres de section, sous-titre, lien « Se connecter » |
| `text-2xl` (24 px) | `h1` et `h2` ✔ conformes |

Aucun message d'erreur n'existe, donc rien d'illisible dans un état d'échec — c'est le seul fichier où ce risque n'existe pas.

### 16.7 Largeur et affichage

- `max-w-[390px]` sur le `<body>` : conforme à § 4.5. **Différence de placement** : ici la contrainte est sur le `<body>` lui-même, alors que les fichiers 2 et 3 la placent sur le `<main>`. Sans conséquence visuelle ici, mais incohérent.
- Sous 320 px : `px-6` (48 px) laisse 272 px utiles ; le champ et les boutons restent utilisables. **Aucun débordement horizontal.**
- Paysage / écrans pliants : aucun traitement.
- La bascule de vue est un simple `hidden` : sur un écran très bas, la vue de confirmation (plus haute) demandera un défilement — **fonctionnel**.

### 16.8 Autres points d'accessibilité

| Point | Observation |
| --- | --- |
| Changement de vue non annoncé | `#confirmation-container` apparaît sans `role="status"`, ni `aria-live`, ni déplacement de focus : **un lecteur d'écran ne signalera rien** à l'utilisateur |
| Titres | `h1` (vue formulaire) puis `h2` (vue confirmation) ✔ hiérarchie correcte |
| `label` / `for` | ✔ associés |
| Icônes décoratives | Les SVG `arrow-left`, `shield-check`, `key-round`, `mail`, `mail-check`, `shield` portent `aria-hidden="true"` ✔ — **mieux que les fichiers 2 et 3** |
| Bouton retour | `aria-label="Retour"` ✔ |
| Champ au focus | Le plugin `forms` + `focus:ring-1` : anneau visible ✔. Aucun `outline: none` explicite ici (contrairement aux fichiers 2 et 3) |
| Contraste | `#94A3B8` sur `#F8FAFC` ≈ **2,6:1** → **non conforme AA** (4,5:1 requis) pour le label `Adresse e-mail`. Même défaut que les fichiers 2 et 3, sur un texte plus petit. |
| Contraste du pied | `#64748B` sur `#F8FAFC` ≈ 4,8:1 ✔ |
| Bouton désactivé pendant l'envoi | Le champ reste éditable pendant les 900 ms : l'utilisateur peut modifier l'e-mail sans que l'affichage tienne compte. |

## 17. Points à vérifier

1. **Nom de fichier d'origine** — non communiqué.
2. **Aucun envoi d'e-mail réel** — aucun appel réseau. Quel service ? Comment le lien est-il généré ?
3. **Écran de définition du nouveau mot de passe manquant** — étape indispensable du parcours ; § 9 impose de ne rien inventer, donc à recevoir.
4. **Lien « Oublié ? » mort dans le fichier 2** — raccorder les deux écrans.
5. **Lien « Se connecter » mort (`href="#"`)** — à brancher.
6. **Bouton « Retour à la connexion » ne navigue pas** — libellé inexact ; corriger le libellé ou le comportement.
7. **Bouton retour par `history.back()`** — si la page est ouverte directement, il ne se passe rien. Prévoir un retour explicite.
8. **Aucun contrôle de format de l'e-mail côté JS** — ajouter une regex ou valider le retour serveur.
9. **Aucun rate limit ni captcha** — quelle protection sur l'envoi ?
10. **Divulgation d'existence de compte** — la formulation actuelle est correcte ; à conserver dans l'implémentation réelle.
11. **Police Plus Jakarta Sans** — confirmer la décision : § 4.4 fixe Inter. Alignement avec le fichier 1.
12. **Jeton `brand.*` déclaré et totalement inutilisé** — nettoyer ; et arbitrer sur un nommage unique (`zecolis.*`, `brand.*`, ou variables CSS).
13. **Plugins Tailwind `forms` et `container-queries`** — le plugin `forms` modifie le rendu des champs : les styles des fichiers 2 et 3 Appliqués avec ce plugin seraient différents. **Quelle est la base de référence ?**
14. **Contraste `#94A3B8` ≈ 2,6:1** — non conforme AA ; remplacer par `#64748B`.
15. **`#FFF3EB` et `emerald-50` hors palette** — arbitrer.
16. **Safe areas : `pt-4` / `pb-2`** — les plus faibles des cinq fichiers ; bouton retour exposé sous l'encoche. **À corriger.**
17. **Zoom désactivé** — 4ᵉ fichier consécutif ; correction globale (5 fichiers sur 5 sont concernés).
18. **`user-select: none` dans le CSS du `body`** — bloque le collage et la copie de l'e-mail.
19. **`transition-opacity` sans effet** — la bascule `hidden` annule la transition ; code mort à retirer ou à remplacer par une vraie transition.
20. **Aucun retour visuel ni message si l'envoi échoue** — ajouter un état d'erreur.
21. **URL du logo différente des fichiers 2 et 3** — trois URL Googleusercontent distinctes pour le même logo. Laquelle fait référence ?
22. **Aucun test navigateur possible ici** — vérifier la bascule de vue, le rendu du plugin `forms` et le contraste réel.

## 18. En résumé

Le **parcours de récupération le mieux pensé** des trois écrans d'accès : un bouton retour **réellement fonctionnel** en 44×44 px (la seule cible tactile au minimum de tout le fichier), deux vues distinctes, un pied de message, et surtout un **texte de confirmation formulé avec la bonne prudence** — « Si un compte est associé à l'adresse… » — qui ne divulgue pas l'existence d'un compte, contrairement à la tentation courante. Le JS est le seul des quatre fichiers sans code mort : les deux fonctions définies sont les deux fonctions utilisées.

Mais la logique reste entièrement **simulée** : 900 ms de temporisation, puis bascule de vue, sans aucun envoi d'e-mail, sans génération de jeton, et sans **aucun contrôle du format de l'e-mail** (`toto` affiche la confirmation). L'écran de définition du nouveau mot de passe **manque**. Visuellement, deux écarts avec § 4 : la **police Plus Jakarta Sans** (divergence directe avec § 4.4, comme le fichier 1) et le gris `#94A3B8` à contraste insuffisant. Ses ombres sont en revanche **plus légères que tous les autres fichiers**, et c'est le seul qui charge les plugins `forms` et `container-queries` — ce qui rend les styles de champs non comparables aux fichiers 2 et 3. Côté mobile : défilement correct et cibles correctes, mais `pt-4` / `pb-2` sans safe area (le bouton retour se retrouve sous l'encoche), zoom bloqué pour la quatrième fois, `user-select: none` qui empêche de copier l'e-mail saisi, et un changement de vue totalement muet pour les lecteurs d'écran.