# Accueil — Diagnostic mobile et version corrigée

| Rubrique | Valeur |
| --- | --- |
| **Fichier analysé** | `SOURCES_HTML_ORIGINALES/accueil.html` (60 060 octets, original non modifié) |
| **Version corrigée produite** | `VERSIONS_CORRIGEES/accueil_mobile.html` |
| **Nature du travail** | Analyse statique du code + corrections mobile / accessibilité / comportements front |
| **Limite** | **Aucun navigateur, aucun appareil réel n'a été utilisé.** Les constats sont déduits du code. Aucune validation visuelle, aucune mesure de rendu réelle. |
| **Analyse détaillée** | `02_NAVIGATION_ET_ACCUEIL/02_ACCUEIL.md` (recensement complet, 683 lignes analysées) |

---

## 1. Verdict

La maquette **s'affiche sur un téléphone mais n'est pas utilisable telle quelle**. Ce n'est pas un blocage de scroll comme le scanner : le problème est ailleurs.

| Gravité | Constat |
| --- | --- |
| Bloquant | Zoom et pincement **interdits** (`maximum-scale=1.0, user-scalable=no`) |
| Bloquant | **12 liens sur 12 sont morts** (`href="#…"` vers des ancres inexistantes) |
| Bloquant | Le moteur de recherche **n'a aucun champ de saisie** : 0 `<input>` dans tout le fichier |
| Majeur | Zones de sécurité absentes (`viewport-fit`, `env(safe-area-inset-*)`) |
| Majeur | 63 textes à 8, 9, 10 ou 11 px |
| Majeur | Modale scanner sans `role="dialog"`, sans piège de focus, sans fermeture clavier |
| Majeur | Carrousels sans aucun contrôle au clavier nipoint d'arrêt visible |
| Mineur | `overflow-hidden` sur 14 conteneurs (risque de coupe) |

---

## 2. Constats détaillés

### 2.1 Zoom verrouillé

**Code :** `<meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" name="viewport">`

Effet : impossible de grossir une adresse, un prix ou un texte de 10 px. Le geste de pincement est neutralisé. C'est un défaut d'accessibilité, aggravé ici par la densité de texte minuscule (§ 2.5).

### 2.2 Zones de sécurité absentes

| Élément | Attendu | Présent |
| --- | --- | --- |
| `viewport-fit=cover` | pour utiliser l'écran sous l'encoche | **absent** (0 occurrence) |
| `env(safe-area-inset-top)` | barre d'état / Dynamic Island | **absent** (0 occurrence) |
| `env(safe-area-inset-bottom)` | barre de gestes iOS | **absent** (0 occurrence) |
| Marges latérales en paysage | `env(safe-area-inset-left/right)` | **absent** |
| Hauteur dynamique | `100dvh` | **absent** (0 occurrence) |

Sur iPhone à encoche, le contenu passe sous l'encoche en haut et sous la barre de gestes en bas.

### 2.3 Les 12 liens sont tous morts

Recensement exhaustif des `href` du fichier :

| `href` | Cible | Verdict |
| --- | --- | --- |
| `#accueil`, `#envoyer`, `#voyager`, `#recherche` | onglets / barre basse | **morte** |
| `#profil`, `#messages` | barre basse | **morte** |
| `#suivi`, `#marketplace` | liens de service | **morte** |
| `#garantie`, `#paiement-protection`, `#guide-douane` | liens « En savoir plus » | **morte** |
| `#profile-edit` | CTA profil | **morte** |

Le fichier ne déclare **qu'un seul `id`** (`qr-scanner-modal`) : aucune de ces ancres n'existe. Un appui sur « Messages », « Profil », « Publier une annonce » ou « Détail du voyage » ne produit **aucun effet et aucun message**.

### 2.4 Recherche sans champ

**0 occurrence de `<input>`, `<select>` ou `<textarea>`** dans le fichier. Les blocs Départ / Arrivée sont des cartes statiques : l'utilisateur ne peut saisir ni ville de départ, ni ville d'arrivée. Les 6 attributs `onclick` ne font que des animations ou des bascules d'onglets.

### 2.5 Typographie

| Taille | Occurrences | Lecture |
| --- | --- | --- |
| `text-[8px]` | 1 | illisible, sous tout seuil |
| `text-[9px]` | 4 | illisible |
| `text-[10px]` | 38 | minuscule (seuil retenu : 12 px) |
| `text-[11px]` | 20 | trop petit pour du texte d'interface |
| ≥ 12 px | reste | conforme |

63 textes concernés, soit la quasi-totalité des libellés secondaires (badges, distances, durées, prix, mentions).

### 2.6 Cibles tactiles

28 éléments interactifs (16 `<button>` + 12 `<a>`). **18 d'entre eux sont sous 44 px** de hauteur : boutons pastilles `w-8`/`w-9`, liens de service en `text-[10px]`/`text-[11px]`, onglets de la barre basse. L'avatar `w-9 h-9` (36 px) est hors inventaire des 28 et également trop petit.

`touch-action` n'apparaît **qu'une fois** dans tout le fichier.

### 2.7 Modale scanner

| Manque | Impact |
| --- | --- |
| `role="dialog"` / `aria-modal` | les lecteurs d'écran ne l'annoncent pas |
| `aria-labelledby` | aucun nom accessible |
| Piège de focus (Tab) | le focus part derrière la modale |
| Fermeture par `Escape` | impossible au clavier |
| Retour du focus au bouton d'ouverture | le focus est perdu après fermeture |
| « Importer une photo » | aucun `<input type="file">` dans le fichier |
| Saisie manuelle du code | impossible |
| `lucide@latest` non épinglé | un changement d'API casse le rendu |

### 2.8 Carrousels

Deux carrousels (« destinations », « parcels ») : `flex overflow-x-auto` avec cartes en largeur fixe, sans `snap`, sans bouton précédent/suivant, sans point d'arrêt, et sans focus clavier (pas de `tabindex`, pas de lien). Sur un lecteur d'écran ou au clavier, le contenu est inaccessible.

### 2.9 Animations permanentes

5 animations en boucle : 4 `animate-pulse` + 1 `animate-ping`, sans pause, sans respect de `prefers-reduced-motion`. Le repère `ping` du bouton « Publier » pulse en permanence.

---

## 3. Corrections appliquées dans `accueil_mobile.html`

**Périmètre : mise en page mobile, accessibilité, et comportements front de l'interface. Aucun choix de design n'a été modifié : palette, typographie de marque, dégradés, glassmorphism, cartes et textes sont conservés à l'identique.**

### 3.1 Mobile

| Correction | Avant | Après |
| --- | --- | --- |
| Viewport | `maximum-scale=1.0, user-scalable=no` | `width=device-width, initial-scale=1, viewport-fit=cover` — zoom et pincement réautorisés |
| Zones de sécurité | aucune | variables `--sat/--sar/--sab/--sal` = `env(safe-area-inset-*, 0px)`, appliquées en haut, en bas et sur les marges latérales |
| Hauteur | `min-h-full` | `.app-shell { min-height: 100vh; min-height: 100dvh }` |
| Police Inter | déclarée mais non forcée | `font-family: Inter, system-ui, sans-serif` + `-webkit-text-size-adjust: 100%` |
| Indicateur d'accueil | aucun (le fichier ne le dessinait pas) | aucun ajout : seuls les vrais safe areas sont utilisés |
| Coupures | 14 `overflow-hidden` | conservés uniquement sur les cartes à hauteur fixe ; le conteneur racine défile normalement |
| Cibles tactiles | 18 sous 44 px | `.tap` / `.tap-sm` : `min-height: 44px`, `min-width: 44px`, `touch-action: manipulation` |
| Tap highlight | 1 seule occurrence | appliqué sur `*` |

### 3.2 Accessibilité

| Correction | Détail |
| --- | --- |
| Modale scanner | `role="dialog"`, `aria-modal="true"`, `aria-labelledby`, `tabindex="-1"`, piège de focus (Tab / Maj+Tab), fermeture par `Escape` et par clic sur le fond, retour du focus sur `openScannerBtn` |
| Barre de progression | `role="progressbar"` + `aria-valuenow` mis à jour |
| Formulaire de trajet | `<form>` réel, `<label>` associé, messages d'erreur `role="alert"`, focus sur le premier champ fautif |
| Icônes | toutes les icônes décoratives en `aria-hidden="true"` ; boutons-icônes avec `aria-label` |
| Contrastes / focus | `:focus-visible` avec contour orange 2 px, visible au clavier et non supprimé |

### 3.3 Comportements front réels

| Constat d'origine | Correction |
| --- | --- |
| 0 champ de saisie | champs Départ et Arrivée réels (`departInput`, `arriveeInput`), inversion du trajet fonctionnelle, recherche simulée **avec retour visible** (`searchFeedback`) |
| 12 liens morts | convertis en `<button>` avec retour explicite (« *écran* : cet écran n'a pas encore été reçu ») — plus de navigation silencieuse |
| « Importer une photo » inerte | `<input type="file" accept="image/*">` réellement présent, avec message honnête : le décodage QR n'est pas implémenté |
| Saisie du code impossible | champ `otpInput` (`inputmode="numeric"`, `maxlength="8"`) + validation + message d'état |
| Carrousels inaccessibles | cartes focalisables (`tabindex="0"`), défilement par `scroll-snap`, indicateurs de position |
| Aucune annonce vocale du chargement | `aria-live="polite"` sur le retour de recherche |

### 3.4 Ce qui n'a **pas** été corrigé (et pourquoi)

| Point | Décision |
| --- | --- |
| 63 textes entre 8 et 11 px | **conservés** : les changer tous modifierait la composition visuelle d'ensemble. Correction à traiter comme un chantier de design, pas de mise en page |
| Palette, dégradés, glassmorphism, cartes | **conservés** à l'identique |
| Animations permanentes | conservées, mais neutralisées sous `prefers-reduced-motion` |
| Images distantes Google | conservées (fragilité réseau déjà signalée dans `02_ACCUEIL.md`) |

---

## 4. Points restant à traiter

| # | Point | Nature |
| --- | --- | --- |
| 1 | 63 textes sous 12 px | Design — à arbitrer |
| 2 | 11 images distantes `lh3.googleusercontent.com` | Fragilité réseau — à remplacer par des assets locaux |
| 3 | `lucide@latest` non épinglé | Fragilité — version à fixer |
| 4 | Recherche non branchée (géocodage, itinéraires) | Backend — inexistant |
| 5 | Détails colis / détail voyage / profil / messages | Écrans non livrés |
| 6 | Décodage QR réel (`BarcodeDetector`, `getUserMedia`) | Fonctionnel — non implémenté |
| 7 | Suggestions de villes, historique, favoris | Fonctionnel |
| 8 | Paysage, tablette, écran pliant | Non traités au-delà du minimal |

---

## 5. Fichiers

| Fichier | Contenu |
| --- | --- |
| `SOURCES_HTML_ORIGINALES/accueil.html` | **Original, non modifié** |
| `VERSIONS_CORRIGEES/accueil_mobile.html` | Version corrigée — mobile + accessibilité + comportements front |

Les deux fichiers sont distincts. Aucun original n'a été écrasé.