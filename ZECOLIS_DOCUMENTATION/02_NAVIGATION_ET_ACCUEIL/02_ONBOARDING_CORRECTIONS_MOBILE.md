# Onboarding (écrans 1, 2 et 3) — Diagnostic mobile et versions corrigées

| Rubrique | Valeur |
| --- | --- |
| **Fichiers analysés** | `SOURCES_HTML_ORIGINALES/onboarding_expediteur_1.html` (11 514 octets) · `SOURCES_HTML_ORIGINALES/onboarding_voyageur_2.html` (13 024 octets) · `SOURCES_HTML_ORIGINALES/onboarding_confiance_3.html` (6 728 octets) — **originaux non modifiés** |
| **Versions corrigées produites** | `VERSIONS_CORRIGEES/onboarding_expediteur_mobile.html` (13 474 octets) · `VERSIONS_CORRIGEES/onboarding_voyageur_mobile.html` (14 351 octets) · `VERSIONS_CORRIGEES/onboarding_confiance_mobile.html` (12 074 octets) |
| **Nature du travail** | Analyse statique du code + corrections mobile / accessibilité / comportements front |
| **Limite** | **Aucun navigateur, aucun appareil réel n'a été utilisé.** Les constats sont déduits du code. |
| **Analyse détaillée** | `02_NAVIGATION_ET_ACCUEIL/02_ONBOARDING.md` |
| **Lot 6** | 4 transmissions pour 2 écrans distincts : l'écran 1 a été reçu deux fois à l'identique. |
| **Lot 7** | L'étape 3 (`onboarding_confiance_3.html`) a été reçue : **la séquence est complète**. |

---

## 1. Verdict

Les trois écrans **s'affichent mais ne fonctionnent pas**. Ils sont purement décoratifs : aucun des 6 boutons de la série n'a de gestionnaire.

| Gravité | Constat |
| --- | --- |
| Bloquant | **0 action réelle sur 6 éléments interactifs** — « Passer », « Suivant » ×2, « Commencer », « Retour » ne font strictement rien |
| Bloquant | Défilement vertical **bloqué** : `overflow: hidden` sur le `<body>` des **trois** fichiers |
| Bloquant | Écrans 1 et 2 : hauteur **figée à 844 px** avec `max-h-screen` : sur écran plus bas (petit téléphone, paysage), **contenu coupé sans possibilité de défiler** |
| Majeur | **Étape 3 : `✓ Identité vérifiée` et `★ 4,9 Voyageur vérifié` sont des affirmations sans aucun mécanisme.** L'écran ne contient aucune logique, et sa promesse centrale ne peut pas être tenue |
| Majeur | Zones de sécurité **absentes** (`viewport-fit`, `env(safe-area-inset-*)`) sur les trois fichiers : l'en-tête passe sous l'encoche |
| Majeur | Fausse barre d'état iOS et fausse pilule d'accueil dessinées alors que l'appareil les affiche |
| Majeur | Barres de progression et paginations **non exposées** aux lecteurs d'écran (aucun `role`, `aria-current` ni `sr-only`) |
| Majeur | 3 boutons sous 44 px : « Passer » ×2 (≈ 26 px) et « Retour » de l'écran 3 (≈ 24 px) |
| Mineur | Écran 2 : icône réseau en `opacity="0"` (invisible), `group-hover` sans parent `.group`, `rounded-2xs` invalide |
| Mineur | Écran 3 : `rounded-2xs` invalide, `via-45%`/`via-40%` invalides (syntaxe v4), jauge de batterie qui affiche un niveau plein en permanence |
| Mineur | Code mort : `.text-wide` non utilisée, 4 jetons de couleur inutilisés (écran 1), **8 jetons inutilisés** (écran 2), **4 jetons inutilisés** (écran 3) |

---

## 2. Corrections appliquées dans les trois versions mobiles

**Périmètre : mise en page mobile, accessibilité, et comportements front. Aucun choix de design n'a été modifié : photos, dégradés, glassmorphism, pastilles, typographie et couleurs sont conservés à l'identique.**

### 2.1 Mobile

| Correction | Avant | Après |
| --- | --- | --- |
| Viewport écran 1 | `maximum-scale=1.0, user-scalable=no` | `width=device-width, initial-scale=1, viewport-fit=cover` — **6ᵉ fichier consécutif** où le zoom était bloqué |
| Viewport écran 2 | sans `viewport-fit` | `width=device-width, initial-scale=1, viewport-fit=cover` |
| Viewport écran 3 | `maximum-scale=1.0, user-scalable=no` | `width=device-width, initial-scale=1, viewport-fit=cover` — **7ᵉ fichier consécutif** où le zoom était bloqué |
| Hauteur (écrans 1 et 2) | `h-[844px] max-h-screen` | `.app-shell { min-height: 100vh; min-height: 100dvh }` |
| Hauteur (écran 3) | `h-full overflow-hidden` sur le `<body>` | `min-height: 100dvh` avec repli `100vh`, **défilement vertical rétabli** |
| Défilement | `overflow: hidden` sur `<html>` et `<body>` | `overflow-x: hidden` — **le défilement vertical est rétabli** |
| Photo plein cadre | `absolute inset-0` (dimensionnée par le conteneur de 844 px) | `.hero-bg { position: fixed; inset: 0 }` + `object-fit: cover` : la photo couvre toujours la fenêtre, même quand la page s'allonge |
| Barre d'état simulée | toujours visible | masquée sur écran tactile étroit (`@media (pointer: coarse), (max-width: 430px)`) — elle est conservée en aperçu sur grand écran |
| Pilule d'accueil | `w-32 h-1` dessinée | **supprimée**, remplacée par la zone de geste réelle |
| Safe areas | aucune | `--sat/--sar/--sab/--sal` = `env(safe-area-inset-*, 0px)`, appliquées en haut (en-tête), en bas (`.safe-bottom`) et sur les marges latérales (en-tête **et** bloc bas) |
| Largeur du cadre | `max-w-[390px]` | `max-w-[390px]` **conservée** (alignée sur les autres fichiers du dossier) |
| Sélection de texte | `user-select: none` sur le `<body>` (écran 1) / `select-none` sur le `<main>` (écran 2 et 3) | classe `.no-select` appliquée **uniquement aux éléments d'interface** — les titres et sous-titres redeviennent sélectionnables |
| Paysage très court | contenu coupé | media query `(orientation: landscape) and (max-height: 520px)` : `h1` ramené à 30 px, blocs resserrés |
| Tap highlight | aucun | appliqué sur `*` |
| Focus clavier | non défini | `:focus-visible` avec contour orange 2 px |
| Animations permanentes | 1 `animate-pulse` par écran (aucune sur l'écran 3) | conservée, neutralisée sous `prefers-reduced-motion` |
| Cibles tactiles | « Passer » ≈ 26 px, « Retour » ≈ 24 px | `.tap { min-height: 44px; min-width: 44px; touch-action: manipulation }` sur les 6 boutons |
| Police système | — | `-webkit-text-size-adjust: 100%` |

### 2.2 Accessibilité

| Correction | Détail |
| --- | --- |
| Barre de progression | `<ol role="list">` + `aria-current="step"` sur l'étape courante + texte `sr-only` (« Étape 1 sur 3 : envoyer un colis ») |
| Pagination | `<ol>` avec une pastille `sr-only` par étape (« Étape 2 active ») — elle n'est plus décorative |
| Pagination doublée (écran 3) | les deux jeux de pastilles (en-tête et pied) sont exposés séparément, le second avec `aria-label="Étape 3 sur 3"` |
| Icônes | tous les SVG décoratifs passent en `aria-hidden="true"` |
| Images | `alt=""` sur la photo de fond des écrans 1 et 2 (correct : purement décorative) ; sur l'écran 3 les deux `alt` **descriptifs** d'origine sont conservés, avec `width`/`height` |
| Écran 2 — écart WCAG 2.5.3 | les `aria-label` qui ne contenaient pas le texte visible (« Passer l'onboarding », « Écran suivant ») ont été **retirés** : le nom accessible est désormais le texte affiché |
| Zone d'attente | `<p class="sr-only" role="status" aria-live="polite">` pour annoncer les retours |
| Barre d'état simulée | `aria-hidden="true"` (information factice) |

### 2.3 Comportements front réels

| Constat d'origine | Correction |
| --- | --- |
| « Passer » sans effet (écrans 1 et 2) | **vrai lien** vers `accueil_mobile.html` |
| « Suivant » écran 1 sans effet | **vrai lien** vers `onboarding_voyageur_mobile.html` |
| « Suivant » écran 2 sans effet, étape 3 **non reçue** (lot 6) | **résolu au lot 7** : le bouton est devenu un **vrai lien** vers `onboarding_confiance_mobile.html`. La mention « l'étape 3 n'a pas encore été reçue » et la zone `#step3Status` ont été **supprimées** : elles ne sont plus vraies |
| « Commencer » écran 3 sans effet | **vrai lien** vers `accueil_mobile.html` — la fin de l'onboarding sort sur l'accueil |
| « Retour » écran 3 sans effet | **vrai lien** vers `onboarding_voyageur_mobile.html` (étape 2) |
| Icône réseau invisible (`opacity="0"`, écran 2) | attribut supprimé : l'icône s'affiche |
| `group-hover:translate-x-1` sans parent (écran 2) | classe `group` ajoutée sur le bouton : l'effet de survol fonctionne enfin |
| `rounded-2xs` invalide (écrans 2 et 3) | remplacée par `rounded-sm` |
| Jauge de batterie fige en « plein » (écran 3) | **supprimée** avec le reste de la barre d'état simulée |

**La séquence 1 → 2 → 3 → accueil est désormais navigable dans les versions mobiles**, ce qui n'était le cas d'aucun des trois originaux.

### 2.4 Code mort

| Élément | Traitement |
| --- | --- |
| `.text-wide` (écran 1) | supprimée — définie et jamais utilisée |
| `rounded-2xs` (écrans 2 et 3) | remplacée par `rounded-sm` |
| Jetons de couleur inutilisés (4 / 8 / 4 selon l'écran) | **conservés** : sans effet de rendu, et les retirer ne sert qu'à alléger le fichier. Nettoyage à faire au moment du passage à un vrai build |

---

## 3. Ce qui n'a **pas** été corrigé (et pourquoi)

| Point | Décision |
| --- | --- |
| Photo plein cadre, dégradés, glassmorphism, thème sombre | **conservés à l'identique** : c'est le parti pris graphique de ces écrans. Divergence avec § 4.2 et § 4.4 déjà signalée dans `02_ONBOARDING.md` § 4.2 |
| Textes à 10–12 px | **conservés** : en dessous du seuil retenu de 12 px, mais bien moins problématique que les 63 textes de l'accueil. Correction de design à arbitrer |
| `Paiement garanti`, `✓ Instantané` | **conservés** : ce sont des textes de marque. **Le code ne les justifie pas** — signalé dans `09_POINTS_A_VERIFIER.md` (B29, O-1 à O-5) |
| **`✓ Identité vérifiée`, `★ 4,9`, « Des profils vérifiés… » (écran 3)** | **conservés dans la maquette**, avec une **mention visible et explicite** ajoutée sous le bloc d'actions indiquant que ces éléments sont décoratifs et qu'aucune vérification ni aucun avis n'existe dans le projet. Retirer les badges eux-mêmes modifierait le design ; les laisser sans réserve reviendrait à mentir. Arbitrage ouvert : O-Q6 et O-Q7 |
| Images distantes Google Storage | conservées (2 URLs par écran), comme sur les autres fichiers |

---

## 4. Points restant à traiter

| # | Point | Nature |
| --- | --- | --- |
| 1 | **Définition de la vérification d'identité** (O-Q6) et **des avis / notes** (O-Q7) | § 3 — bloquant pour l'écran 3 tant que ce n'est pas défini |
| 2 | Destination de « Commencer » : accueil ou connexion ? (O-Q8) | Arbitrage |
| 3 | Destination de « Passer » (accueil reçu, mais l'onboarding est-il obligatoire ?) | Arbitrage (O-Q1) |
| 4 | `Paiement garanti` / `✓ Instantané` sans mécanisme | § 3 — à adosser ou retirer |
| 5 | Safe areas réelles, encoche et Dynamic Island, portrait et paysage | Non testable ici (G11) |
| 6 | Rendu de la photo plein cadre (poids, `object-position`) en paysage | Non testable ici |
| 7 | Textes à 10–12 px | Design — à arbitrer |
| 8 | Logo et photos : 2 URLs Google Storage non versionnées par écran | Fragilité réseau — à remplacer par des assets locaux |

---

## 5. Contrôles statiques effectués

| Contrôle | Écran 1 | Écran 2 | Écran 3 |
| --- | --- | --- | --- |
| Encodage UTF-8 sans BOM, aucun caractère CJK ou de remplacement | ✔ | ✔ | ✔ |
| Viewport sans `user-scalable` ni `maximum-scale` | ✔ | ✔ | ✔ |
| `viewport-fit=cover` + 4 variables `env(safe-area-inset-*)` | ✔ | ✔ | ✔ |
| `100dvh` présent | ✔ | ✔ | ✔ |
| `prefers-reduced-motion` présent | ✔ | ✔ | ✔ |
| `id` en double | aucun (0 `id`) | aucun (3 `id`, uniques) | aucun (0 `id`) |
| `getElementById` sans élément correspondant | — (aucun) | aucun | — (aucun) |
| Liens internes `href` → fichier existant | ✔ | ✔ | ✔ (`accueil_mobile.html`, `onboarding_voyageur_mobile.html`) |
| Classes Tailwind invalides (`rounded-2xs`, `to-95%`, `via-45%`, `via-40%`, `h-13`) | aucune | aucune | aucune |
| Syntaxe JS (`node --check`) | — | — | ✔ |

---

## 6. Fichiers

| Fichier | Contenu |
| --- | --- |
| `SOURCES_HTML_ORIGINALES/onboarding_expediteur_1.html` | **Original, non modifié** |
| `SOURCES_HTML_ORIGINALES/onboarding_voyageur_2.html` | **Original, non modifié** |
| `SOURCES_HTML_ORIGINALES/onboarding_confiance_3.html` | **Original, non modifié** |
| `VERSIONS_CORRIGEES/onboarding_expediteur_mobile.html` | Version corrigée — mobile + accessibilité + navigation |
| `VERSIONS_CORRIGEES/onboarding_voyageur_mobile.html` | Version corrigée — mobile + accessibilité + **navigation vers l'étape 3** |
| `VERSIONS_CORRIGEES/onboarding_confiance_mobile.html` | Version corrigée — mobile + accessibilité + navigation + réserve explicite sur les badges de confiance |

Les six fichiers sont distincts. Aucun original n'a été écrasé.