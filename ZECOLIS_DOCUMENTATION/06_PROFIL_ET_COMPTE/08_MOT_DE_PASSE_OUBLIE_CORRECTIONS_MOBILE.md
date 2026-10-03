# Mot de passe oublié — Diagnostic mobile et version corrigée

| Rubrique | Valeur |
| --- | --- |
| **Fichier analysé** | `SOURCES_HTML_ORIGINALES/mot_de_passe_oublie.html` (11 755 octets, 204 lignes, original non modifié) |
| **Version corrigée produite** | `VERSIONS_CORRIGEES/mot_de_passe_oublie_mobile.html` |
| **Nature du travail** | Analyse statique du code + corrections mobile / accessibilité / navigation |
| **Limite** | **Aucun navigateur, aucun appareil réel.** Constats déduits du code. |
| **Analyse détaillée** | `06_PROFIL_ET_COMPTE/08_MOT_DE_PASSE_OUBLIE.md` |

---

## 1. Verdict

C'est **le fichier le moins cassé des cinq** : le formulaire fonctionne, l'écran de confirmation s'affiche, et la formulation ne divulgue pas l'existence d'un compte. Les défauts sont concentrés sur la navigation et le mobile.

| Gravité | Constat |
| --- | --- |
| Majeur | « Retour à la connexion » **ne va nulle part** : il réinitialise l'écran au lieu de naviguer |
| Majeur | « Se connecter » : `href="javascript:void(0)"` → lien mort |
| Majeur | Bouton retour : `window.history.back()` sans repli → **ne fait rien** si la page est ouverte directement |
| Majeur | `max-w-[390px]` appliqué au `<body>` → le fond s'arrête à 390 px sur grand écran |
| Majeur | Zones de sécurité absentes, faux indicateur d'iOS dessiné |
| Majeur | Aucun message d'erreur : `if (!emailValue) return;` renvoie en silence |
| Mineur | Validation e-mail déléguée au seul navigateur, jamais à l'application |
| Mineur | Texte à 10 px, animations sans `prefers-reduced-motion` |

---

## 2. Constats détaillés

### 2.1 « Retour à la connexion » ne navigue pas

**Code :** `<button … onclick="resetToInitialState()">Retour à la connexion</button>`

**Code appelé :** `resetToInitialState()` → masque `#confirmation-container`, affiche `#reset-form-container`, réinitialise le formulaire.

Le libellé promet une navigation vers l'écran de connexion ; le comportement revient au formulaire. **Le libellé et le comportement sont contradictoires.** L'utilisateur clique, voit revenir le formulaire, et comprend que la navigation est cassée.

### 2.2 Lien « Se connecter » mort

`<a class="… " href="javascript:void(0)">Se connecter</a>` — alors que `connexion.html` existe dans les sources.

### 2.3 Bouton retour sans repli

**Code :** `onclick="window.history.back()"`

Ouvert directement (URL collée, lien depuis une notification, ouverture dans un nouvel onglet), l'historique ne contient qu'une entrée : `history.back()` **quitte la page** ou ne fait rien selon le navigateur. Aucun repli vers l'écran de connexion.

### 2.4 Largeur limitée au `<body>`

**Code :** `<body class="h-full w-full max-w-[390px] mx-auto bg-[#F8FAFC] … overflow-x-hidden antialiased">`

Le `max-width` est sur le `<body>`, pas sur un conteneur interne. Conséquence : sur un écran large (desktop, tablette, téléphone en paysage), **seule la bande de 390 px est peinte en `#F8FAFC`**, et le reste de la page affiche la couleur par défaut du navigateur. Le fond de l'écran est visuellement coupé.

### 2.5 Zones de sécurité

| Élément | Attendu | Présent |
| --- | --- | --- |
| `viewport-fit=cover` | oui | **absent** (0 occurrence) |
| `env(safe-area-inset-*)` | oui | **absent** (0 occurrence) |
| Marges latérales en paysage | oui | **absent** |
| Indicateur d'accueil iOS dessiné | non | **présent** : `<div class="w-32 h-1 bg-slate-300 rounded-full mb-1">` |

Le faux indicateur d'accueil double la barre de gestes réelle d'iOS et occupe 34 px qui pourraient servir au contenu.

### 2.6 Validation silencieuse

**Code :** `if (!emailValue) return;`

Aucune erreur n'est affichée, aucun message, aucun retour : un appui sur le bouton avec un champ vide ne produit **aucun effet visible**. Le seul contrôle qui s'exerce est la validation native du navigateur (`required` + `type="email"`), qui affiche une bulle système non stylée et non traduite dans les MUI de la maquette. Un format comme `a@b`, accepté par la validation HTML, passe sans le moindre contrôle applicatif.

Aucun élément de message d'erreur n'existe dans le fichier : les identifiants déclarés sont `reset-form-container`, `email-input`, `submit-btn`, `btn-text`, `btn-spinner`, `confirmation-container`, `submitted-email-text`.

### 2.7 Accessibilité

| Manque | Impact |
| --- | --- |
| Pas de `role="alert"` / `aria-live` | le changement de vue n'est pas annoncé |
| La section de confirmation n'est pas focusable | le focus reste sur le bouton désormais invisible |
| Pas de `lang` sur les textes français ? | `<html lang="fr">` présent : conforme |
| Animations (fondu de vue) | `transition` sur l'opacité alors que l'affichage se fait par la classe `hidden` → **transition sans effet** |
| `lucide@latest` non épinglé | fragilité |

---

## 3. Corrections appliquées dans `mot_de_passe_oublie_mobile.html`

### 3.1 Navigation

| Correction | Avant | Après |
| --- | --- | --- |
| « Retour à la connexion » | `resetToInitialState()` (retour au formulaire) | **vrai lien** vers `connexion_mobile.html` |
| Bouton retour | `window.history.back()` sans repli | `history.back()` **si l'historique contient une entrée**, sinon repli vers `connexion_mobile.html` |
| « Se connecter » | `javascript:void(0)` | `connexion_mobile.html` |
| Retour navigateur après confirmation | vue de confirmation conservée | garde `pageshow` qui restaure la vue formulaire |

Les trois liens internes de la version corrigée pointent vers un fichier réellement présent (contrôle effectué).

### 3.2 Mobile

| Correction | Avant | Après |
| --- | --- | --- |
| Viewport | `maximum-scale=1.0, user-scalable=no` | zoom réautorisé + `viewport-fit=cover` |
| Largeur | `max-w-[390px]` sur le `<body>` | conteneur interne `max-w-[430px]` → **le fond couvre tout l'écran** |
| Zones de sécurité | aucune | `--sat/--sar/--sab/--sal` appliquées en haut, en bas et sur les marges |
| Hauteur | `justify-between` sur `h-full` | `.app-shell { min-height: 100dvh }` (repli `100vh`) |
| Faux indicateur d'accueil | `w-32 h-1` + `mb-1` | supprimé, remplacé par `.safe-bottom` |
| Champ e-mail | `text-base` déjà correct | conservé ; règle `input { font-size: 16px }` ajoutée pour tous les champs |
| En-tête | `justify-between` sans repli | `flex-wrap` sous 360 px pour éviter le chevauchement des 3 blocs |
| `touch-action` | absent | `.tap { touch-action: manipulation }` |
| `prefers-reduced-motion` | absent | ajouté |

### 3.3 Validation et retours

| Correction | Avant | Après |
| --- | --- | --- |
| `if (!emailValue) return;` | retour silencieux | contrôle de format réel (`/^[^\s@]+@[^\s@]+\.[^\s@]+$/`) + **message d'erreur visible** `role="alert"`, focus sur le champ fautif |
| Message d'erreur | inexistant | `<p id="email-error" role="alert">` relié au champ par `aria-describedby`, avec `aria-invalid` |
| Annonce du changement de vue | aucune | `aria-live="polite"` sur la section de confirmation + **focus déplacé** dessus |
| Faux e-mail | aucun | mention explicite en pied de page : « aucun e-mail n'est réellement envoyé » |
| Formulaire | pas de `method` | `method="post"` explicite + `novalidate` (la validation JS fait autorité et affiche un message stylé) |

La **formulation de la confirmation est conservée à l'identique** : « Si un compte est associé à l'adresse… » ne révèle pas si le compte existe. C'est le bon choix pour la sécurité, il est maintenu.

### 3.4 Accessibilité

| Correction | Détail |
| --- | --- |
| Changement de vue | `role="alert"` implicitement via `aria-live` + `tabindex="-1"` + focus |
| Bouton principal | `aria-busy` + `disabled` pendant le traitement |
| Bouton retour | `aria-label="Retour à l'écran précédent"` |
| Icônes | `aria-hidden="true"` sur les icônes décoratives |
| Focus | `:focus-visible` contour orange 2 px |
| Transition de vue | `view-switch` + animation `viewIn` **réellement déclenchée** (l'original animait une vue qui n'était pas affichée) |

---

## 4. Points restant à traiter

| # | Point | Nature |
| --- | --- | --- |
| 1 | Envoi réel de l'e-mail de réinitialisation | Backend — inexistant |
| 2 | **Écran de saisie du nouveau mot de passe après clic sur le lien** | **Absent des sources** : le flux s'arrête à l'envoi de l'e-mail |
| 3 | Lien de réinitialisation à durée de vie limitée, usage unique | Sécurité |
| 4 | Anti-énumération de compte (déjà bien formulé, à conserver) | Sécurité |
| 5 | `lucide@latest` non épinglé | Fragilité |
| 6 | Lien « forgot password » de l'écran connexion | **Câblé dans la version mobile corrigée** ; à faire dans les autres écrans |
| 7 | Paysage / tablette / écran pliant | Non traité |

---

## 5. Fichiers

| Fichier | Contenu |
| --- | --- |
| `SOURCES_HTML_ORIGINALES/mot_de_passe_oublie.html` | **Original, non modifié** |
| `VERSIONS_CORRIGEES/mot_de_passe_oublie_mobile.html` | Version corrigée — navigation + mobile + validation |

Les deux fichiers sont distincts. Aucun original n'a été écrasé.