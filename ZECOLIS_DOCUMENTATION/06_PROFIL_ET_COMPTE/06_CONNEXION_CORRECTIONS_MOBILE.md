# Connexion — Diagnostic mobile et version corrigée

| Rubrique | Valeur |
| --- | --- |
| **Fichier analysé** | `SOURCES_HTML_ORIGINALES/connexion.html` (12 913 octets, 236 lignes, original non modifié) |
| **Version corrigée produite** | `VERSIONS_CORRIGEES/connexion_mobile.html` |
| **Nature du travail** | Analyse statique du code + corrections mobile / accessibilité / comportements front |
| **Limite** | **Aucun navigateur, aucun appareil réel.** Constats déduits du code, sans validation visuelle. |
| **Analyse détaillée** | `06_PROFIL_ET_COMPTE/06_CONNEXION.md` |

---

## 1. Verdict

Le formulaire **fonctionne** dans l'original (validation e-mail, chargement, messages d'erreur), mais il est **inadapté au téléphone** et il **ment sur la sécurité** :

| Gravité | Constat |
| --- | --- |
| Bloquant | Zoom **interduit** (`maximum-scale=1.0, user-scalable=no`) |
| Bloquant | `select-none` sur le `<body>` : **collage du mot de passe impossible** (gestionnaires de mots de passe, copier-coller) |
| Majeur | « Oublié ? » et « S'inscrire » : `href="javascript:void(0)"` → **navigation morte** |
| Majeur | Zones de sécurité absentes |
| Majeur | Champs à `text-[15px]` → **auto-zoom iOS** à la focus, puis page décalée |
| Majeur | Fausse réussite affichée : « Connecté ✓ » sans aucun appel serveur |
| Mineur | Texte à 9,5 px, 11 px, 11,5 px ; indicateur d'accueil factice |

---

## 2. Constats détaillés

### 2.1 Zoom verrouillé

`<meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" name="viewport">`

### 2.2 `select-none` sur le `<body>` — impact réel

**Code :** `<body class="h-full w-full bg-[#F8FAFC] antialiased select-none flex flex-col justify-between">`

La sélection de texte est interdite **sur tout le document**, y compris dans les champs de formulaire. Conséquences concrètes sur téléphone :

- impossible de coller un mot de passe depuis un gestionnaire (mailleur, 1Password, LastPass) ou depuis le presse-papiers ;
- impossible de copier une adresse e-mail affichée ;
- les champs `input` restent techniquement éditables au clavier, ce qui rend l'interdiction incohérente.

`touch-action: manipulation` est absent : les appuis conservent un délai sur certains navigateurs.

### 2.3 Liens morts

| Élément | `href` | Verdict |
| --- | --- | --- |
| « Oublié ? » | `javascript:void(0)` | **mort** — l'écran mot de passe oublié existe pourtant (`mot_de_passe_oublie.html`) |
| « S'inscrire » | `javascript:void(0)` | **mort** — l'écran inscription existe pourtant (`inscription.html`) |

Les deux écrans existent dans les sources : les liens n'ont jamais été câblés.

### 2.4 Zones de sécurité et hauteur

| Élément | Attendu | Présent |
| --- | --- | --- |
| `viewport-fit=cover` | oui | **absent** (0 occurrence) |
| `env(safe-area-inset-*)` | oui | **absent** (0 occurrence) |
| `env(…-left/right)` en paysage | oui | **absent** |
| Hauteur | adaptative | `<body class="h-full">` + `<main class="min-h-full … justify-between">` : correct sur grand écran, fragile sur iPhone SE (667 px) où le contenu dépasse et repousse le pied de page |

Le fichier dessine un spacer `<div class="h-2 w-full">` en haut et un faux indicateur d'accueil `<div class="w-32 h-1 …">` en bas : deux zones perdues qui ne remplacent pas les vraies zones de sécurité.

### 2.5 Auto-zoom iOS à la focus

Les deux champs sont en `text-[15px]` (15 px). Sur iOS Safari, tout champ dont la police est **< 16 px** déclenche un zoom automatique à la focus ; comme la page ne peut plus être dézoomée (`user-scalable=no`), l'utilisateur reste zoomé, ne voit plus le bas du formulaire, et doit dézoomer manuellement pour cliquer sur « Se connecter ».

### 2.6 Typographie

| Taille | Occurrences |
| --- | --- |
| `text-[9.5px]` | 1 |
| `text-[11px]` | 3 |
| `text-[11.5px]` | 3 |
| `text-[15px]` | 3 (dont les 2 champs de saisie) |

### 2.7 Tailles des cibles

| Élément | Taille d'origine | Cible |
| --- | --- | --- |
| Bouton « œil » | `pr-4` + icône 20 px ≈ **28 × 56 px** | 44 px de large minimum |
| « Oublié ? » | `text-[12px]` sans padding ≈ **18 px** de haut | 44 px |
| « S'inscrire » | `text-[14px]` ≈ **20 px** | 44 px |
| Pastille « Sécurisée » | `text-[11px]` + `py-0.5` ≈ **20 px** | 44 px |
| Bouton principal | `h-14` = 56 px | conforme |

### 2.8 Sécurité — fausse réussite

**Code :** `btnText.textContent = "Connecté ✓"` après un `setTimeout` de 900 ms.

Aucun appel réseau n'existe dans le fichier. L'interface affiche donc une **connexion réussie alors qu'aucun compte n'a été vérifié**, avec un fond vert pendant 1 800 ms. Pour un écran de connexion, c'est le défaut le plus grave du fichier.

---

## 3. Corrections appliquées dans `connexion_mobile.html`

### 3.1 Mobile

| Correction | Avant | Après |
| --- | --- | --- |
| Viewport | zoom interdit | zoom réautorisé + `viewport-fit=cover` |
| Zones de sécurité | aucune | `--sat/--sar/--sab/--sal`, appliquées en haut, en bas et sur les marges |
| Hauteur | `min-h-full` | `.app-shell { min-height: 100dvh }` (repli `100vh`) |
| Spacer `h-2` | 8 px perdues en haut | supprimé, remplacé par `pt-[calc(1rem+var(--sat))]` |
| Faux indicateur d'accueil | `w-32 h-1` + `mt-4` | supprimé, remplacé par `.safe-bottom` |
| Police des champs | `text-[15px]` | `text-base` (**16 px**) : plus d'auto-zoom iOS |
| `select-none` | sur le `<body>` | retiré du `<body>` ; `user-select: none` limité aux `input`, `button`, `label` → **collage et copie réautorisés** |
| `touch-action` | absent | `.tap { touch-action: manipulation }` |
| Cibles | 4 éléments < 44 px | `.tap` / `.tap-sm` : 44 px minimum, zone « œil » élargie à 44 px |
| `prefers-reduced-motion` | absent | ajouté |

### 3.2 Liens réels

| Élément | Avant | Après |
| --- | --- | --- |
| « Oublié ? » | `javascript:void(0)` | `mot_de_passe_oublie_mobile.html` |
| « S'inscrire » | `javascript:void(0)` | `inscription_mobile.html` |
| Formulaire | `<form>` sans `method` | `method="post"` explicite (aucune requête émise en maquette) |

Les quatre liens internes de la version corrigée pointent tous vers un fichier réellement présent (contrôle effectué).

### 3.3 Honnêteté du retour de connexion

| Avant | Après |
| --- | --- |
| « Connecté ✓ » + fond vert, sans aucune vérification | état d'échec assumé : message « Adresse e-mail ou mot de passe incorrect. », focus sur le champ, **aucune fausse réussite** |
| — | mention explicite en pied de page : « Connexion de démonstration : aucun appel serveur » |

Ce point relève d'une décision à valider de votre part : si vous préférez conserver la maquette de démonstration, l'état d'échec peut être remplacé par un message de type « Démonstration : authentification non branchée ».

### 3.4 Accessibilité

| Correction | Détail |
| --- | --- |
| Erreurs | `role="alert"` + `aria-live="polite"` + `aria-describedby` sur chaque champ |
| Champ fautif | `aria-invalid="true"` et focus porté sur le premier champ en erreur |
| Bouton « œil » | `aria-pressed` mis à jour, `aria-label` alterné (« Afficher » / « Masquer le mot de passe ») |
| Bouton principal | `aria-busy="true"` pendant le chargement, `disabled` pour éviter le double appui |
| Icône décorative | `aria-hidden="true"` sur les icônes Lucide |
| Focus | `:focus-visible` contour orange 2 px (n'est jamais supprimé) |

### 3.5 Ce qui n'a pas été corrigé

| Point | Décision |
| --- | --- |
| Textes à 9,5 / 11 / 11,5 px | **conservés** : la composition visuelle est préservée, la correction relève du design |
| Palette, logo, dégradé du bouton, glassmorphism | **conservés** à l'identique |
| Champ « Se souvenir de moi » / SSO social | **absents** de l'original, **non ajoutés** (ajout de fonctionnalité) |
| Illustration du mot de passe oublié | **non ajoutée** (nécessiterait une décision graphique) |

---

## 4. Points restant à traiter

| # | Point | Nature |
| --- | --- | --- |
| 1 | Authentification réelle (Firebase, API, sessions) | Backend — inexistant |
| 2 | Messages d'erreur indistinguables entre e-mail inexistant et mot de passe faux | Sécurité — arbitrage nécessaire |
| 3 | Rate limiting, CAPTCHA, 2FA | Fonctionnel |
| 4 | Textes sous 12 px | Design |
| 5 | `lucide@latest` non épinglé | Fragilité |
| 6 | Réinitialisation du mot de passe câblée (lien ajouté, flux non branché) | Backend |
| 7 | Paysage / tablette / écran pliant | Non traité |

---

## 5. Fichiers

| Fichier | Contenu |
| --- | --- |
| `SOURCES_HTML_ORIGINALES/connexion.html` | **Original, non modifié** |
| `VERSIONS_CORRIGEES/connexion_mobile.html` | Version corrigée — mobile + accessibilité + honnêteté du retour |

Les deux fichiers sont distincts. Aucun original n'a été écrasé.