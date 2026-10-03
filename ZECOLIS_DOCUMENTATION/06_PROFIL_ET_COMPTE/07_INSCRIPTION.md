# Inscription

| Rubrique | Valeur |
| --- | --- |
| **Nom de la page** | Inscription (titre du document : « Zecolis Mobile - Inscription ») |
| **Fichier source** | Nom de fichier d'origine **non communiqué**. Copie conservée sous `SOURCES_HTML_ORIGINALES/inscription.html` (nom de travail). |
| **Catégorie retenue** | `06_PROFIL_ET_COMPTE` — création de compte. **Arbitrage à confirmer** avec le fichier 2 (Connexion) : un dossier `AUTHENTIFICATION` dédié devient cohérent dès lors que les trois écrans d'accès sont reçus (Connexion, Inscription, Mot de passe oublié — tous les trois sont maintenant disponibles). |
| **Statut d'analyse** | Analysé (fichier unique, autonome) |
| **Nature** | Maquette HTML autonome. **Aucune création de compte réelle** — et pire : le formulaire est renvoyé par le navigateur sans être intercepté (§ 9.1). |

---

## 1. Objectif de la page

Permettre à un nouvel utilisateur de créer un compte Zecolis en saisissant identité, e-mail, mot de passe, confirmation et acceptation des documents contractuels.

Le § 7 du cahier des charges prévoit une acceptation de documents à l'inscription : **c'est le seul fichier reçu qui implémente cette étape**, et c'est aussi le seul point positif réel du fichier.

## 2. Utilisateur concerné et rôle métier

Utilisateur **non connecté**. Aucun rôle demandé à l'inscription : le sous-titre « Rejoignez Zecolis pour envoyer ou voyager » reprend les deux rôles (§ 2.1), et le formulaire ne demande **aucun** élément d'orientation (ni « je suis expéditeur », ni « je suis voyageur »).

**Conforme à § 2.1** : pas de création arbitraire d'un type de compte unique.

## 3. Point d'entrée dans la navigation

**Aucun point d'entrée réel, et aucun point de sortie réel.** L'écran est exactement l'inverse de ce qu'il devrait être : **tous ses liens de service sont morts**.

| Élément | `href` | Effet |
| --- | --- | --- |
| « Conditions Générales d'Utilisation » | `javascript:void(0)` | **Inopérant** |
| « Politique de confidentialité » | `javascript:void(0)` | **Inopérant** |
| « Déjà un compte ? Se connecter » | `javascript:void(0)` | **Inopérant** |

Aucun bouton retour, aucune barre de navigation principale. Le § 2 du fichier 2 (Connexion) pointe vers cet écran par un lien également mort (`S'inscrire` → `javascript:void(0)`).

**Conséquence : ni l'aller ni le retour entre Connexion et Inscription n'existent.** Le parcours d'entrée dans l'application est entièrement rompu dans le code.

## 4. Structure de l'écran (ordre de lecture)

Conteneur `max-w-[390px] mx-auto min-h-full flex flex-col justify-between px-6 pt-6 pb-5`, sur `body` en `flex flex-col justify-between`.

| Zone | Contenu |
| --- | --- |
| 1. Espace de respiration | `div.h-2 w-full` — 8 px factices stand-ins de la barre d'état |
| 2. En-tête | Pastille logo + libellé « Zecolis » · libellé « Inscription » · pastille « SÉCURISÉ » |
| 3. Bloc titre | `h1` « Créez votre compte ! » + sous-titre sur 1 ligne |
| 4. Formulaire | Prénom / Nom · E-mail · Mot de passe · Confirmation · Case CGU · Bouton · Invite à se connecter |
| 5. Pied | Message de réassurance + indicateur d'accueil iOS simulé |

Le fichier est un **copier-coller du fichier 2 (Connexion)** avec un formulaire plus long : même `tailwind.config`, même bloc CSS, mêmes en-têtes/pieds. La preuve est directe : la fonction `handleLoginSubmit` du fichier 2 a été recopiée ici (§ 9.3).

## 5. Sections et composants visuels

### 5.1 Configuration Tailwind inline

**Identique au fichier 2, à la virgule près** : mêmes 9 couleurs `zecolis.*`, mêmes 3 ombres, même police Inter.

| Jeton | Valeur | § 4.2 |
| --- | --- | --- |
| `zecolis-navy` | `#0B2A5B` | ✔ |
| `zecolis-darknavy` | `#1D1D4B` | ✔ |
| `zecolis-orange` | `#FF6B00` | ✔ |
| `zecolis-green` | `#22A55A` | ✔ |
| `zecolis-bg` | `#F8FAFC` | ✔ déclaré mais **non utilisé** (le fond est écrit en dur `#F8FAFC`) |
| `zecolis-text` | `#0F172A` | ✔ |
| `zecolis-muted` | `#64748B` | ✔ |
| `zecolis-border` | `#F1F5F9` | ✔ déclaré mais **non utilisé** (écrit en dur) |
| `zecolis-lightborder` | `#E2E8F0` | ✘ hors palette, **déclaré et inutilisé** (idem fichier 2) |

Ombres : `btn-navy` (double ombre marquée, à arbitrer), `input-subtle`, `pill-soft`.

> **Même problème que le fichier 2 :** le `tailwind.config` est déclaré **après** `<script src="https://cdn.tailwindcss.com">`. Si la CDN n'acccepte pas cette ordre, toute la palette se rabat sur du Tailwind standard. **Non testé au navigateur.**

### 5.2 En-tête

| Élément | Détail |
| --- | --- |
| Logo | `<img>` distante (`lh3.googleusercontent.com/...aida-public/AB6AXuALebbe5m...`), `w-7 h-7`, `alt="ZéColis"` — **URL différente du fichier 2** |
| Wordmark | `Zé` + `Colis` orange, `font-extrabold text-[15px]` |
| Libellé de page | `Inscription`, `text-[11px]` |
| Pastille | icône Lucide `shield-check` (via `<i data-lucide>`) + `SÉCURISÉ` en `text-[9.5px] uppercase` |

### 5.3 Bloc titre

- `h1` : `Créez votre compte ` + `!` en orange, `text-[28px] font-extrabold`.
- Sous-titre : `Rejoignez Zecolis pour envoyer ou voyager.` en `text-[14px] text-zecolis-muted`.

### 5.4 Champs

Cinq champs, tous en `h-14` (56 px), `rounded-2xl`, `bg-white`, `border-[#F1F5F9]`, `shadow-input-subtle`, `focus:border-zecolis-navy focus:ring-1`, icône Lucide `w-5` à gauche dans un conteneur `pointer-events-none` qui bascule de couleur via `group-focus-within`.

| # | Label | `id` / `name` | Type | Icône | Placeholder | `required` |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | `PRÉNOM` | `firstNameInput` / `firstName` | `text` | `user` | `Votre prénom` | ✔ |
| 2 | `NOM` | `lastNameInput` / `lastName` | `text` | `user` | `Votre nom` | ✔ |
| 3 | `EMAIL` | `emailInput` / `email` | `email` | `mail` | `votre@email.com` | ✔ |
| 4 | `MOT DE PASSE` | `passwordInput` / `password` | `password` | `lock` | `••••••••` | ✔ |
| 5 | `CONFIRMER LE MOT DE PASSE` | `confirmPasswordInput` / `confirmPassword` | `password` | `lock` | `••••••••` | ✔ |

Champs 1 et 2 dans une `grid grid-cols-2 gap-3` (deux colonnes de ~160 px sur 390 px de large). **Seule occurrence d'une grille à deux colonnes dans l'application** : les champs d'identité y sont à ~160 px, donc la saisie de « Koffi » ou « Ibrahim » y est à l'étroit.

Messages associés :

| ID | État initial | Contenu |
| --- | --- | --- |
| `#emailError` | `hidden` | `Veuillez entrer une adresse e-mail valide.` |
| `#passwordHelper` | visible | `8 caractères minimum` (icône `info`, gris) |
| `#passwordMatchError` | `hidden` | `Les mots de passe ne correspondent pas.` |
| `#termsError` | `hidden` | `Veuillez accepter les conditions pour continuer.` |

**Les quatre sont du texte mort** : aucun script du fichier ne les affiche ni ne les masque (§ 9.3).

### 5.5 Boutons œil

Deux boutons identiques, `type="button"`, `aria-label="Afficher ou masquer le mot de passe"` (le libellé ne distingue pas les deux champs), `absolute inset-y-0 right-0 pr-4` :

```html
onclick="togglePasswordVisibility('passwordInput', 'eyeIcon1')"
onclick="togglePasswordVisibility('confirmPasswordInput', 'eyeIcon2')"
```

La fonction appelée **ne déclare aucun paramètre** (§ 9.2) : les deux arguments sont ignorés.

### 5.6 Case d'acceptation des documents

```html
<label class="flex items-start gap-2.5 cursor-pointer select-none">
  <input id="termsCheckbox" type="checkbox" required class="mt-0.5 w-4 h-4 ...">
  <span class="text-[12.5px] text-[#64748B]">J'accepte les
    <a href="javascript:void(0)">Conditions Générales d'Utilisation</a> et la
    <a href="javascript:void(0)">Politique de confidentialité</a>.</span>
</label>
```

- Case `w-4 h-4` = **16 px**, mais la zone cliquable réelle est le `<label>` entier (≈ 60 px de haut grâce au retour à la ligne) ✔ globalement correct.
- `select-none` sur le label : empêche la sélection du texte des liens — **empêche de copier un titre de condition**.
- Couleurs en dur `#CBD5E1` (bordure de case) et `#64748B` (texte) : le second est conforme, le premier hors palette (idem fichier 2).

### 5.7 Bouton principal

`h-14 bg-zecolis-navy hover:bg-[#082046] active:bg-[#061836] text-white text-[16px] font-bold rounded-2xl shadow-btn-navy`, `id="signupSubmitBtn"`, `type="submit"`.

Contient `#btnText` (`Créer mon compte`) et `#btnSpinner` (`hidden animate-spin`) — **exactement les mêmes identifiants et le même contenu que le bouton du fichier 2**, seul le libellé change.

### 5.8 Pied

Identique au fichier 2 : SVG `shield` pré-rendu + `Vos informations restent sécurisées.`, puis `w-32 h-1 bg-slate-300` en guise d'indicateur d'accueil.

## 6. Textes visibles (intégralité)

| Zone | Texte |
| --- | --- |
| Titre onglet | `Zecolis Mobile - Inscription` |
| Wordmark | `ZéColis` |
| En-tête | `Inscription` · `SÉCURISÉ` |
| Titre | `Créez votre compte !` |
| Sous-titre | `Rejoignez Zecolis pour envoyer ou voyager.` |
| Labels | `PRÉNOM` · `NOM` · `EMAIL` · `MOT DE PASSE` · `CONFIRMER LE MOT DE PASSE` |
| Placeholders | `Votre prénom` · `Votre nom` · `votre@email.com` · `••••••••` ×2 |
| Aide | `8 caractères minimum` |
| Erreurs (invisibles) | `Veuillez entrer une adresse e-mail valide.` · `Les mots de passe ne correspondent pas.` · `Veuillez accepter les conditions pour continuer.` |
| Consentement | `J'accepte les Conditions Générales d'Utilisation et la Politique de confidentialité.` |
| Bouton | `Créer mon compte` |
| Bas de formulaire | `Déjà un compte ? Se connecter` |
| Pied | `Vos informations restent sécurisées.` |

## 7. Champs et données

| Champ | Validation navigateur | Validation JS |
| --- | --- | --- |
| Prénom | **désactivée** (`novalidate`) | **aucune** |
| Nom | **désactivée** | **aucune** |
| E-mail | **désactivée** (`type="email"` inactif) | **aucune** |
| Mot de passe | **désactivée** | **aucune** |
| Confirmation | **désactivée** | **aucune** |
| Consentement | **désactivée** | **aucune** |

**6 champs sur 6 sans validation.** Il n'existe :

- aucune règle de longueur réelle (le « 8 caractères minimum » est **un texte d'aide, pas un contrôle**) ;
- aucune confirmation que les deux mots de passe correspondent ;
- aucune obligation effective de cocher les CGU ;
- aucun contrôle de doublon d'e-mail ni de format ;
- aucun indicateur de robustesse du mot de passe.

Champs réellement collectés : identité complète, e-mail, mot de passe en clair, consentement. **Aucun** téléphone, code de vérification, adresse e-mail secondaire, moyen de paiement ou pièce justificative — le § 7 du cahier des charges n'est donc qu'un tiers satisfait.

## 8. Boutons et actions

| # | Élément | Type | Comportement réel dans le code |
| --- | --- | --- | --- |
| 1 | Œil mot de passe | `<button type="button">` | **Cassé** — `eyeIcon` vaut `null`, le clic lève une `TypeError` (§ 9.2) |
| 2 | Œil confirmation | `<button type="button">` | **Cassé**, identique |
| 3 | Case CGU | `<input type="checkbox">` | **Fonctionnelle** (navigateur) — mais son erreur ne s'affiche pas |
| 4 | Créer mon compte | `<button type="submit">` | **Renvoi natif du formulaire** — aucun `preventDefault` (§ 9.1) |
| 5 | Conditions Générales | `<a href="javascript:void(0)">` | **Inopérant** |
| 6 | Politique de confidentialité | `<a href="javascript:void(0)">` | **Inopérant** |
| 7 | Se connecter | `<a href="javascript:void(0)">` | **Inopérant** |

Sur 7 actions : 1 réellement utilisable (la case), 2 cassées, 4 inopérantes.

## 9. Interactions et comportements observés

### 9.1 Soumission du formulaire — **défaut critique**

```html
<form class="space-y-4" id="signupForm" novalidate onsubmit="handleSignupSubmit(event)">
```

`handleSignupSubmit` **n'existe pas** dans le script du fichier. Les fonctions définies sont `togglePasswordVisibility()` et `handleLoginSubmit(event)`.

Conséquence exacte au clic sur « Créer mon compte » :

1. le navigateur résout `onsubmit="handleSignupSubmit(event)"` ;
2. la fonction est **indéfinie** → `ReferenceError`, l'attribut `onsubmit` inline ne peut pas s'exécuter ;
3. **aucun `preventDefault()`** n'a été exécuté ;
4. le `novalidate` empêche le blocage par `required` ;
5. le formulaire est **soumis par le navigateur**.

Le `<form>` n'a ni `action` ni `method` : la méthode par défaut est **GET**, l'action par défaut est l'URL courante. Le navigateur navigue donc vers :

```
inscription.html?firstName=Riham&lastName=D.&email=a%40b.fr&password=Secret123&confirmPassword=Secret123
```

**Les deux mots de passe sont écrits en clair dans l'URL**, donc :

- dans l'historique de navigation ;
- dans le champ d'adresse, visible par toute personne à l'appareil ;
- dans les journaux du serveur ;
- souvent dans le `Referer` transmis aux pages suivantes.

> **C'est le défaut le plus grave de tous les fichiers reçus.** Un formulaire de création de compte ne doit jamais être soumis en GET. Correction minimale et obligatoire : déclaré `method="post"`, `onsubmit="return false"`, et surtout écrire la fonction manquante. Tant que la maquette est utilisée pour une démonstration, remplacer `onsubmit` par un `onsubmit="return false"` supprime le risque.
>
> **À corriger en priorité absolue.**

### 9.2 Affichage du mot de passe — fonction cassée

```js
function togglePasswordVisibility() {
  const pwdInput = document.getElementById('passwordInput');
  const eyeIcon  = document.getElementById('eyeIcon');   // ← aucun élément de cet id
  ...
  eyeIcon.setAttribute('data-lucide', 'eye-off');         // ← TypeError
}
```

| Point | Constat |
| --- | --- |
| Signature | **Aucun paramètre** alors que le HTML passe 2 arguments |
| Champ visé | Toujours `passwordInput` → le bouton du champ 5 agit sur le champ 4 |
| Icône visée | `getElementById('eyeIcon')` → `null` (les identifiants réels sont `eyeIcon1` et `eyeIcon2`) |
| Erreur | `TypeError: Cannot read properties of null (reading 'setAttribute')` au **premier** clic |
| Effet sur le type du champ | **Aucun** : l'erreur survient après `pwdInput.type = 'text'`, donc le premier mot de passe **se révèle**, puis tout s'arrête |
| Conséquence | Le clic suivant inverse le type sans rien signaler. Le bouton œil est **un mensonge fonctionnel**. |

Dans le fichier 2, la même fonction était **au moins cohérente** avec son HTML (`id="eyeIcon"` présent). Ici le copier-coller n'a pas été adapté : deux identifiants renommés, fonction jamais mise à jour.

### 9.3 Code JavaScript mort — `handleLoginSubmit` recopié

Le bloc `<script>` du fichier contient `handleLoginSubmit(event)`, **fonction de connexion**, complète et fonctionnelle (validation e-mail, état de chargement, « Connecté ✓ »). Elle :

- n'est **appelée par aucun élément** du fichier ;
- ne trouve aucun de ses éléments (`loginSubmitBtn`, `generalError` sont absents de l'inscription) ;
- provoquerait, si elle était appelée, un `TypeError` sur `generalError` / `loginSubmitBtn` / `btnSpinner` — ces identifiants n'existent pas dans la page.

C'est la trace la plus nette du copier-coller : **le fichier 2 a été dupliqué et le formulaire remplacé, sans relecture du script**.

| Élément mentionné dans le script | Présent dans l'inscription ? |
| --- | --- |
| `#emailInput` | ✔ |
| `#passwordInput` | ✔ |
| `#emailError` | ✔ |
| `#generalError` | ✘ **absent** |
| `#loginSubmitBtn` | ✘ **absent** (le vrai est `signupSubmitBtn`) |
| `#btnText` | ✔ |
| `#btnSpinner` | ✔ |

**Fonction de validation de la confirmation de mot de passe : absente.** `#passwordMatchError` et `#termsError` n'ont **aucune** contrepartie dans le script.

### 9.4 Autres comportements

- `lucide.createIcons()` au chargement ; icônes en `<i data-lucide>` pour l'en-tête et les champs.
- `.active-scale:active { transform: scale(0.98) }` sur le bouton, les liens et les boutons œil.
- `transition-all duration-150` sur les champs et le bouton ; `transition-colors` sur les icônes via `group-focus-within`.
- Les icônes `user`, `mail`, `lock` sont **dupliquées** : `user` apparaît 2 fois (champs 1 et 2).

## 10. États possibles

| État | Implémenté | Observation |
| --- | --- | --- |
| Repos | **Oui** | Aucun message visible |
| Erreur e-mail | **Non** | `#emailError` existe, aucun script ne l'affiche |
| Erreur « mots de passe différents » | **Non** | `#passwordMatchError` existe, **jamais utilisé** |
| Erreur « CGU non acceptées » | **Non** | `#termsError` existe, **jamais utilisé** |
| Erreur champs identité vides | **Non** | Aucun contrôle |
| Champ désactivé pendant l'action | **Non** | Aucun état de chargement réel n'est atteint |
| Chargement (spinner) | **Non atteignable** | `#btnSpinner` ne peut être affiché que par la fonction morte |
| Succès « compte créé » | **Non** | Aucun état de succès dans le fichier |
| Erreur « e-mail déjà utilisé » | **Non** | Cas métier absent |
| Erreur réseau / serveur | **Non** | — |
| Redirection après inscription | **Non** | Le navigateur recharge la page avec les mots de passe dans l'URL |

**Sur 11 états applicatifs plausibles, 1 est atteignable.** Le fichier affiche 4 états d'erreur et 1 état de chargement dans son HTML : **aucun n'est câblé**.

## 11. Navigation vers les autres écrans

**Aucun lien sortant fonctionnel.** Le parcours réel est :

```
Connexion ──(S'inscrire : javascript:void(0))──X── Inscription ──(Se connecter : javascript:void(0))──X
```

Les deux écrans sont **isolés l'un de l'autre** dans le code reçu. Un écran d'inscription doit normalement être atteint depuis Connexion et produire une redirection vers l'accueil : rien de tout cela n'existe.

## 12. Règles métier visibles dans le code

| Règle | Implémentation |
| --- | --- |
| Longueur du mot de passe | **8 caractères minimum** — affiché, **jamais vérifié** |
| Correspondance des deux mots de passe | **Vérifiée : non** |
| Complexité (majuscule, chiffre, symbole) | **Aucune** |
| Acceptation des CGU | **Obligatoire dans le HTML** (`required`), contournée par `novalidate` + soumission native |
| Unicité de l'e-mail | **Aucune** |
| Format de l'e-mail | Aucun contrôle actif (le `type="email"` est neutralisé) |
| Normalisation (trim, minuscules) | **Aucune** |
| Anti-doublon d'envoi | **Aucun** — et nécessaire : la soumission native est rejouable |

Aucune règle métier Zecolis (annonce, colis, transaction) n'apparaît sur cette page.

## 13. Dépendances techniques

| Type | Valeur | Remarque |
| --- | --- | --- |
| Tailwind | `cdn.tailwindcss.com` (sans plugins) + `tailwind.config` inline | Script CDN, aucun build. **Ordre de déclaration à vérifier** (idem fichier 2). Le fichier 4 utilise `?plugins=forms,container-queries` : **trois conventions de chargement différentes sur quatre fichiers**. |
| Police | Google Fonts `Inter` 400→800 (2 balises `preconnect` + `css2`) | **Conforme à § 4.4** |
| Icônes | `unpkg.com/lucide@latest` | Version non épinglée |
| Logo | `lh3.googleusercontent.com/aida-public/AB6AXuALebbe5m...` | Image distante non maîtrisée, `alt="ZéColis"` |
| CSS | 1 bloc : `*`, `body`, `input:focus`, `.active-scale` | 15 lignes |
| JS | 1 bloc inline : 2 fonctions + init | ≈ 75 lignes, dont ~50 mortes |
| Build | Aucun | Ni Vite, ni React, ni TypeScript, ni Firebase, ni Capacitor |
| Encodage | UTF-8 **sans BOM**, `<meta charset="utf-8">` en première position | ✔ conforme |

**Aucun lien Firebase, React ou Capacitor.** Maquette autonome.

## 14. Éléments provisoires ou fictifs

### Simulé (non fonctionnel)

| Élément | Nature de la simulation |
| --- | --- |
| `hidden` sur les 3 blocs d'erreur | États d'erreur **maquettes** : présents dans le HTML, jamais produits |
| `#btnSpinner` | Spinner jamais affiché |
| Libellé `Créer mon compte` | Promet une création de compte sans aucun code derrière |

### Fictif

| Élément | Observation |
| --- | --- |
| Badge `SÉCURISÉ` | **Revendication de confiance statique**, aucun contrôle derrière. Le § 3 interdit de présenter une vérification comme réussie sans confirmation système. |
| `Vos informations restent sécurisées.` | Mention statique — d'autant plus fausse ici que les mots de passe partent en GET dans l'URL (§ 9.1). |
| `8 caractères minimum` | Règle affichée mais jamais appliquée |
| Indicateur d'accueil iOS | Simulé (`w-32 h-1`) alors que l'appareil le dessine déjà |
| `div.h-2` en tête | 8 px factices stand-ins de la barre d'état réelle |

### Non inventé — à noter positivement

Aucun montant, aucune note, aucun avis, aucun code de transaction, **aucune donnée pré-remplie**. La page est sobre : c'est son principal mérite.

## 15. Écarts avec l'identité visuelle de référence

| Point | Référence § 4 | Ce fichier | Écart |
| --- | --- | --- | --- |
| Police | Inter | **Inter** | ✔ Conforme |
| Fond général | `#F8FAFC` | `#F8FAFC` | ✔ |
| Cartes / champs | `#FFFFFF` | `bg-white` | ✔ |
| Bordures | `#F1F5F9` | `border-[#F1F5F9]`, `border-slate-100` | ✔ |
| Texte | `#0F172A` / `#64748B` | `text-zecolis-text` / `text-zecolis-muted` | ✔ |
| Navy / orange / vert | `#1D1D4B` / `#0B2A5B` / `#FF6B00` / `#22A55A` | jettons `zecolis-*` | ✔ |
| **Couleur d'erreur** | absente de § 4.2 | `text-rose-500` sur les 3 messages | ✘ Palette incomplète (idem fichier 2) |
| **Gris des labels** | `#64748B` | **`#94A3B8`** | ✘ hors palette (idem fichier 2) |
| **Placeholder** | non spécifié | **`#CBD5E1`** | ✘ hors palette (idem fichier 2) |
| **Bordure de case** | non spécifiée | **`#CBD5E1`** | ✘ hors palette |
| Ombres | « légères » | `btn-navy` : double ombre 20 px + 10 px | ✘ à arbitrer (idem fichier 2) |
| Rayons | cartes ~16 px, boutons ~12 px | `rounded-2xl` (16 px) | ✔ |
| glassmorphism / dégradés | interdits | **absents** | ✔ |
| Icônes | Lucide | Lucide | ✔ |

**Bilan : visuellement, ce fichier est conforme à § 4 — exactement comme le fichier 2, avec les mêmes 4 écarts de couleur et le même écart d'ombre.** Le § 4 n'est pas le problème de cette page ; la logique l'est.

## 16. Problèmes mobiles et accessibilité

*(Analyse statique du code — aucun navigateur, aucun appareil réel utilisé. Aucun correctif n'a été appliqué à ce fichier.)*

### 16.1 Défilement — correct

Comme le fichier 2 : `<body min-h-full>`, `<main min-h-full>`, **aucun `overflow-hidden`**. Seul `overflow-x-hidden` est présent.

Cette page est **beaucoup plus haute** que la connexion (5 champs + messages + case + bouton) : le défilement est non seulement possible mais **nécessaire** sur la plupart des appareils. Il fonctionne.

| Point | Observation |
| --- | --- |
| Clavier mobile | Le champ qui remonte avec le clavier reste atteignable par défilement ✔ |
| `overscroll-behavior-y: none` | Supprime le rebond et le pull-to-refresh — acceptable |
| Estimation de hauteur | ≈ 780 px de contenu hors navigateur ; sous 844 px de viewport, aucun problème ; sur un iPhone SE (667 px), défilement nécessaire |

### 16.2 Zones de sécurité — manquantes

| Élément | Présent | Impact |
| --- | --- | --- |
| `viewport-fit=cover` | **Non** | — |
| `env(safe-area-inset-top)` | **Non** — `pt-6` (24 px) + `h-2` (8 px) | Le contenu démarre **sous l'encoche** ; 8 px ne remplacent pas 44–54 px |
| `env(safe-area-inset-bottom)` | **Non** — `pb-5` (20 px) | L'indicateur d'accueil simulé chevauche la barre de gestes réelle (≈ 34 px) |

Même défaut que les fichiers 1 et 2 : **à corriger de façon homogène sur toute l'application.**

### 16.3 Zones tactiles

| Élément | Taille estimée | Minimum usuel |
| --- | --- | --- |
| Champs (×5) | `h-14` = 56 px | ✔ |
| Bouton principal | `h-14` = 56 px | ✔ |
| Bouton œil (×2) | hauteur 56 px, largeur ≈ 40 px | hauteur ✔, largeur limite |
| **Zone cliquable de la case CGU** | label ≈ 60 px de haut, pleine largeur | ✔ |
| **Lien « Conditions Générales d'Utilisation »** | 2 lignes de `text-[12.5px]` → ≈ 16 px de haut | ✘ **très petit** |
| **Lien « Politique de confidentialité »** | ≈ 16 px | ✘ **très petit** |
| **« Se connecter »** | `text-[13.5px] inline-block` → ≈ 18 px | ✘ **très petit** |

Les trois cibles les plus petites sont précisément les trois liens de service — dont deux sont des **liens légaux obligatoires**. Un utilisateur ne peut pas ouvrir les CGU, qui sont justement ce qu'il est censé accepter.

### 16.4 `select-none` sur le `<body>` — défaut grave

Identique au fichier 2 : la sélection et le collage sont désactivés **y compris dans les 5 champs**. Sur un formulaire qui demande un nom, un e-mail et **deux mots de passe**, c'est un défaut fonctionnel majeur :

- pas de correction partielle d'une saisie (il faut tout retaper) ;
- collage altéré ou refusé selon le navigateur ;
- gestionnaires de mots de passe et mechanisms d'autocomplétion (**très utilisés sur ce type de formulaire**) potentiellement affectés.

**C'est le second défaut majeur du fichier, après la soumission en GET.**

### 16.5 Zoom désactivé

```html
<meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

Pincement-zoom interdit : manquement WCAG 1.4.4 et 1.4.10, aggravé par la présence de `text-[9.5px]` (badge `SÉCURISÉ`).

### 16.6 Typographie trop petite

| Taille | Élément |
| --- | --- |
| `text-[9.5px]` | Badge `SÉCURISÉ` (8 capitales) |
| `text-[11px]` | Les **5 labels** de champs + libellé `Inscription` |
| `text-[11.5px]` | Les **3 messages d'erreur** |

Les messages d'erreur sont les plus problématiques : ce sont précisément ceux qui doivent être lus au moment où l'utilisateur échoue.

### 16.7 Largeur, grille à deux colonnes, affichage

| Point | Observation |
| --- | --- |
| `max-w-[390px]` | ✔ conforme à § 4.5 ; marges vides sur les téléphones 430–480 px |
| `grid grid-cols-2 gap-3` sur 342 px utiles | chaque colonne ≈ **160 px**, moins les 44 px de padding gauche de l'icône → **≈ 116 px pour le texte**. « Ibrahim » + icône + curseur tient, mais c'est la zone la plus serrée de l'application. **À vérifier à 320 px de large** (colonnes d'environ 124 px). |
| `px-6` (48 px) sous 320 px | laisse 272 px utiles, divisés par 2 → 124 px par colonne. **Limite.** |
| Paysage / pliants | Aucun traitement |
| `<h1>` avec `!` en span inline | Aucun risque de coupure |

### 16.8 Autres points d'accessibilité

| Point | Observation |
| --- | --- |
| Erreurs non annoncées | `#emailError`, `#passwordMatchError`, `#termsError` sans `role="alert"` ni `aria-live` |
| Erreurs sans association au champ | `#emailError` n'a pas d'`aria-describedby` sur `#emailInput` ; idem pour les autres |
| Icônes de champ | `<i data-lucide>` sans `aria-hidden` → lues par les lecteurs d'écran comme des images sans nom |
| Boutons œil | Libellé identique pour les deux champs ; ne dit pas lequel il affiche |
| `aria-label="Retour"` | **Absent** — il n'y a pas de bouton retour sur cet écran |
| `label` / `for` | ✔ correctement associés sur les 5 champs |
| Case à cocher | `<label>` englobant ✔ ; `select-none` empêche de sélectionner le texte du consentement |
| Focus | `input:focus { outline: none }` global ; les champs ont `focus:ring-1` ✔ |

## 17. Points à vérifier

1. **Nom de fichier d'origine** — non communiqué.
2. **`handleSignupSubmit` manquant** — écrire la fonction (validation, consentement, appel réseau). Priorité 1.
3. **Soumission native en GET, mots de passe dans l'URL** — corriger immédiatement (`method="post"` + `onsubmit="return false"` en attendant). Priorité 1.
4. **`togglePasswordVisibility` sans paramètres** — câbler les 2 champs et les 2 icônes, ou utilisé `data-*`.
5. **3 messages d'erreur morts** (`emailError`, `passwordMatchError`, `termsError`) — à brancher.
6. **« 8 caractères minimum » jamais appliqué** — § 6 du cahier des charges ? Quelle règle de robustesse réelle ?
7. **`handleLoginSubmit` recopié** — supprimer ; trace de copier-coller à nettoyer.
8. **Aucun lien réel vers Connexion** ni depuis Connexion — le parcours d'entrée dans l'application n'existe pas.
9. **CGU et politique de confidentialité inaccessibles** — § 7 les exige. Quelles versions, quelles URLs, quel document de référence ?
10. **Aucun type de compte demandé** — § 2.1 : confirmer que le compte est neutre et que le rôle se choisit plus tard.
11. **Aucune étape de vérification d'e-mail** — § 6.6 prévoit une vérification de profil : elle est déjà prévue ici, ou pas du tout ?
12. **`rose-500`, `#94A3B8`, `#CBD5E1` hors palette** — arbitrer une couleur d'erreur, unifier les gris.
13. **`tailwind.config` après le script CDN** — vérifier que la palette se génère. Non testé.
14. **Ordre des trois `<script>`** : Tailwind CDN, puis `tailwind.config`, puis Lucide, puis le script applicatif. Dépend de la version de la CDN.
15. **URL du logo différente du fichier 2** — deux URLs Googleusercontent distinctes pour le même logo. Laquelle fait référence ?
16. **Zoom bloqué, `select-none` sur `<body>`, safe areas absentes** — corriger avec les autres fichiers (cohérence d'application).
17. **Grille `grid-cols-2` sur les champs d'identité** — à tester à 320 px ; alternative : deux champs pleine largeur.
18. **`zecolis-bg`, `zecolis-border`, `zecolis-lightborder` déclarés et inutilisés** — nettoyer ou utiliser (idem fichier 2).
19. **Division des écrans d'accès** — `06_PROFIL_ET_COMPTE` ou `AUTHENTIFICATION` dédié ? Les trois écrans sont maintenant reçus.
20. **Aucun test navigateur possible ici** — validation réelle des seuils, du zoom et du comportement de la case à cocher à effectuer sur appareil.

## 18. En résumé

L'écran d'inscription attendue par le § 7 **est présent dans sa structure** — c'est le seul apport réel de ce fichier : identité, e-mail, double mot de passe et acceptation des CGU. Visuellement, il est aussi conforme à § 4 que le fichier 2 (mêmes 4 écarts de couleur, même ombre).

La logique, en revanche, est en partie inexistante : **`handleSignupSubmit` n'existe pas**, donc le formulaire part en GET et **écrit les deux mots de passe en clair dans l'URL** — c'est le défaut le plus grave de tous les fichiers reçus. La fonction d'affichage du mot de passe est cassée (`TypeError` dès le 1ᵉʳ clic), les 3 messages d'erreur du HTML ne sont jamais affichés, la règle « 8 caractères minimum » est purement décorative, et les 3 liens de service (CGU, confidentialité, connexion) sont des `javascript:void(0)`. Le script contient en outre la fonction de connexion du fichier 2, recopiée et non appelée : preuve directe que la page a été dupliquée sans relecture. Côté mobile : défilement correct, mais zoom bloqué, `select-none` sur le `<body>` (bloque le collage des mots de passe), safe areas absentes, et trois liens — dont deux documents légaux — de 16–18 px de haut.