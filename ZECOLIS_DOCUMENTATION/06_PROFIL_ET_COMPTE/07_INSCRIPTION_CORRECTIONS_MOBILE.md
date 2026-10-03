# Inscription — Diagnostic mobile et version corrigée

| Rubrique | Valeur |
| --- | --- |
| **Fichier analysé** | `SOURCES_HTML_ORIGINALES/inscription.html` (14 245 octets, 168 lignes, original non modifié) |
| **Version corrigée produite** | `VERSIONS_CORRIGEES/inscription_mobile.html` |
| **Nature du travail** | Analyse statique du code + corrections mobile / accessibilité / bugs JavaScript |
| **Limite** | **Aucun navigateur, aucun appareil réel.** Les bugs JavaScript sont déduits de la lecture du code (analyse statique), sans exécution. |
| **Analyse détaillée** | `06_PROFIL_ET_COMPTE/07_INSCRIPTION.md` |

---

## 1. Verdict

**Le formulaire d'inscription ne fonctionne pas.** Ce n'est pas une question de mise en page : deux bugs JavaScript font que la validation n'est jamais exécutée et que les mots de passe partent dans l'URL.

| Gravité | Constat |
| --- | --- |
| **Critique** | `onsubmit="handleSignupSubmit(event)"` — **la fonction n'existe pas** → `ReferenceError` → `preventDefault()` jamais appelé → **soumission native en GET avec les deux mots de passe dans l'URL** |
| **Critique** | Les boutons « œil » plantent : `togglePasswordVisibility()` cherche `#eyeIcon`, qui n'existe pas (`eyeIcon1` / `eyeIcon2`) |
| Majeur | Même en corrigeant l'appel, la fonction disponible est `handleLoginSubmit` : elle référence `generalError` et `loginSubmitBtn`, **absents du fichier** → nouveau plantage |
| Majeur | Mot de passe 8 caractères, confirmation et CGU **jamais vérifiés**, alors que l'interface l'annonce |
| Majeur | 4 liens `javascript:void(0)` : CGU, confidentialité, « Se connecter » |
| Majeur | Zoom interdit, `select-none`, champs à 15 px, zones de sécurité absentes |
| Mineur | Faux indicateur d'accueil, 6 libellés à 11 px, 1 à 9,5 px |

---

## 2. Bugs JavaScript (le cœur du problème)

### 2.1 Bug critique — fonction de soumission inexistante

**Code :** `<form class="space-y-4" id="signupForm" novalidate="" onsubmit="handleSignupSubmit(event)">`

**Code :** `function handleLoginSubmit(event) { … }` — c'est le **seul** gestionnaire défini (ligne 115 de l'original).

Séquence exacte :

1. l'utilisateur appuie sur « Créer mon compte » ;
2. le navigateur exécute `handleSignupSubmit(event)` ;
3. `handleSignupSubmit` n'est pas définie → `ReferenceError` **immédiate** ;
4. `event.preventDefault()` (ligne 116) n'est jamais atteint ;
5. le formulaire **n'a ni `method` ni `action`** → soumission **GET vers l'URL de la page** ;
6. l'URL devient `inscription.html?firstName=…&password=…&confirmPassword=…&terms=on`.

Les deux mots de passe sont écrits dans l'URL : ils restent dans l'historique de navigation, peuvent être copiés dans le champ d'adresse, et sont envoyés en clair au serveur dans les journaux d'accès.

`novalidate=""` est présent : **aucune validation native ne rattrape ce cas**.

### 2.2 Bug critique — boutons « œil » inopérants

**Code appelé :** `onclick="togglePasswordVisibility('passwordInput', 'eyeIcon1')"` et `onclick="togglePasswordVisibility('confirmPasswordInput', 'eyeIcon2')"`

**Code défini :** `function togglePasswordVisibility() { … }` — **sans aucun paramètre**, il utilise en dur :

| Ligne | Code | Conséquence |
| --- | --- | --- |
| 99 | `getElementById('passwordInput')` | les deux boutons agiraient sur le **champ du mot de passe**, jamais sur la confirmation |
| 100 | `getElementById('eyeIcon')` | **cet id n'existe pas** (le HTML définit `eyeIcon1` et `eyeIcon2`) → `null.setAttribute(…)` → `TypeError` |

Résultat : **aucun des deux boutons ne fonctionne**, et la console affiche une erreur à chaque appui. Fonction déjà documentée dans l'analyse de page.

### 2.3 Bug majeur — gestionnaire de connexion copié-collé

Si l'on corrige seulement le nom de la fonction appelée, le gestionnaire disponible est `handleLoginSubmit`, qui référence :

| Identifiant utilisé | Présent dans `inscription.html` ? |
| --- | --- |
| `emailInput`, `passwordInput`, `emailError`, `btnText`, `btnSpinner` | oui |
| `generalError` | **non** |
| `loginSubmitBtn` | **non** (le bouton s'appelle `signupSubmitBtn`) |

`emailError.classList.add('hidden')` (ligne 126) plante sur `null` : la validation s'arrête au tout premier reset. Trois identifiants à reprendre, plus le texte du bouton qui repasse à « Se connecter » (ligne 152) sur un écran d'inscription.

### 2.4 Règles annoncées mais jamais appliquées

| Élément présent dans le HTML | Vérifié par le gestionnaire ? |
| --- | --- |
| « 8 caractères minimum » (`passwordHelper`) | **non** |
| Confirmation du mot de passe (`confirmPasswordError` prévu) | **non** |
| Case CGU obligatoire (`termsCheckbox` `required`) | **non** (`novalidate` désactive le contrôle natif) |
| Prénom / Nom requis | **non** |

---

## 3. Autres constats

### 3.1 Liens morts

| Élément | `href` |
| --- | --- |
| « Conditions Générales d'Utilisation » | `javascript:void(0)` |
| « Politique de confidentialité » | `javascript:void(0)` |
| « Se connecter » | `javascript:void(0)` |

### 3.2 Mobile

| Constat | Détail |
| --- | --- |
| Zoom interdit | `maximum-scale=1.0, user-scalable=no` |
| Zones de sécurité | `viewport-fit` et `env(safe-area-inset-*)` : **0 occurrence** |
| `select-none` | sur le `<body>` : collage du mot de passe impossible |
| Champs à `text-[15px]` | auto-zoom iOS à la focus, aggravé par le zoom verrouillé |
| `overflow-x-hidden` sur le `<body>` | risque de coupe horizontale sur écran étroit |
| Grille Prénom / Nom | `grid-cols-2` sans repli : deux champs de ~160 px à 320 px d'écran |
| Faux indicateur d'accueil | `w-32 h-1` + `mt-4` |
| `touch-action` | absent |

### 3.3 Typographie et cibles

| Taille | Occurrences |
| --- | --- |
| `text-[9.5px]` | 1 |
| `text-[11px]` | 6 |
| `text-[11.5px]` | 4 |
| `text-[12.5px]` | 1 (texte CGU) |

| Élément | Taille d'origine | Problème |
| --- | --- | --- |
| Case CGU | `w-4 h-4` = **16 × 16 px** | très en dessous des 44 px |
| Boutons « œil » | `pr-4` + icône ≈ 28 px | trop petit |
| Bouton principal | `h-14` = 56 px | conforme |

---

## 4. Corrections appliquées dans `inscription_mobile.html`

### 4.1 Bugs corrigés

| Bug | Correction |
| --- | --- |
| `handleSignupSubmit` inexistante → GET avec mots de passe | **fonction écrite et branchée** via `addEventListener('submit', …)`, `event.preventDefault()` en première ligne, `method="post"` explicite sur le formulaire |
| `togglePasswordVisibility()` sans paramètres | **fonction paramétrée** `(inputId, iconId)` ; les deux boutons déclarent leur cible via `data-toggle-password="champ:icône"` et agissent chacun sur **leur** champ et **leur** icône |
| `generalError` / `loginSubmitBtn` inexistants | gestionnaire réécrit avec les bons identifiants : `signupSubmitBtn`, `signupForm`, `passwordMatchError`, `termsError` |
| Règles annoncées non appliquées | contrôle réellement implémenté : format e-mail, **8 caractères minimum**, confirmation identique, **case CGU obligatoire**, prénom et nom non vides |
| Confirmation mensongère du script d'origine | pas de « compte créé » : message honnête « la création de compte n'est pas branchée » |
| Texte « Se connecter » sur le bouton d'inscription | libellé propre à chaque écran |

### 4.2 Liens réels

| Élément | Avant | Après |
| --- | --- | --- |
| CGU / confidentialité | `javascript:void(0)` | bouton explicite + message « cette page n'a pas encore été rédigée » (le fichier `conditions.html` **n'existe pas** dans les sources : le lien aurait été cassé) |
| « Se connecter » | `javascript:void(0)` | `connexion_mobile.html` |
| Page CGU | — | absence signalée explicitement à l'utilisateur ; aucun fichier `conditions.html` n'a été inventé |

### 4.3 Mobile

| Correction | Avant | Après |
| --- | --- | --- |
| Viewport | zoom interdit | zoom réautorisé + `viewport-fit=cover` |
| Zones de sécurité | aucune | `--sat/--sar/--sab/--sal` en haut, en bas et sur les marges |
| Hauteur | `min-h-full` | `min-height: 100dvh` (repli `100vh`) |
| Police des champs | `text-[15px]` | `text-base` (16 px) : plus d'auto-zoom iOS |
| `select-none` | sur le `<body>` | limité aux `input`/`button`/`label` : collage réautorisé |
| Case CGU | 16 × 16 px | zone de 44 × 44 px (`.checkbox-tap`) + `<span class="sr-only">` pour le libellé complet |
| Grille Prénom / Nom | `grid-cols-2` sans repli | repli en 1 colonne sous 360 px (`@media (max-width: 359px)`) |
| `touch-action` | absent | `.tap { touch-action: manipulation }` |
| Faux indicateur d'accueil | `w-32 h-1` | supprimé au profit de `.safe-bottom` |
| `prefers-reduced-motion` | absent | ajouté |

### 4.4 Accessibilité

| Correction | Détail |
| --- | --- |
| Erreurs | `role="alert"` + `aria-live="polite"` + `aria-describedby` ; le même paragraphe sert à l'erreur de confirmation **et** à l'erreur de longueur, le texte étant mis à jour |
| Champs fautifs | `aria-invalid` + focus sur le premier champ en erreur |
| Boutons « œil » | `aria-pressed` + `aria-label` alterné |
| Case CGU | libellé complet en `sr-only`, description liée au champ |
| Bouton principal | `aria-busy` + `disabled` pendant le traitement |
| Retour final | `role="status"` + `aria-live="polite"` |

---

## 5. Points restant à traiter

| # | Point | Nature |
| --- | --- | --- |
| 1 | Page CGU / politique de confidentialité | Contenu à rédiger, lien à câbler |
| 2 | Création de compte réelle (Firebase, API) | Backend — inexistant |
| 3 | Règle de robustesse du mot de passe (1 chiffre, 1 lettre, longueur 12+) | À arbitrer : seule la longueur 8 est annoncée |
| 4 | Vérification d'e-mail avant activation | Fonctionnel |
| 5 | Textes sous 12 px | Design |
| 6 | `lucide@latest` non épinglé | Fragilité |
| 7 | Confirmation de mot de passe obligatoire côté serveur | Sécurité |

---

## 6. Fichiers

| Fichier | Contenu |
| --- | --- |
| `SOURCES_HTML_ORIGINALES/inscription.html` | **Original, non modifié** |
| `VERSIONS_CORRIGEES/inscription_mobile.html` | Version corrigée — bugs JS + mobile + accessibilité |

Les deux fichiers sont distincts. Aucun original n'a été écrasé.