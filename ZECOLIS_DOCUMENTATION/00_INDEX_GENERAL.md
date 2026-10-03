# 00 — Index général

**Statut du document : en cours — 8 fichiers distincts reçus (lots 1 à 6), pour 9 transmissions. L'inventaire n'est pas définitif.**

Cet index est mis à jour lot par lot, au fur et à mesure de la transmission des fichiers. Il ne sera déclaré définitif que lorsque le donneur d'ordre l'indiquera.

## 1. Sources d'information utilisées

| Source | Nature | Statut |
| --- | --- | --- |
| Cahier des charges Zecolis (sections 1 à 13, transmis par le donneur d'ordre) | Document texte | Reçu, pris en compte |
| Fichiers HTML des pages | Code source | **8 fichiers distincts reçus** (lots 1 à 6) — le lot 6 comportait 4 transmissions dont 1 doublon |

Répertoire source inspecté au début de la mission : `C:\Users\USER PC\Desktop\zecolis pages`
Contenu initial : répertoire vide (0 fichier).

## 2. Inventaire des fichiers reçus

| # | Nom de fichier | Page représentée | Catégorie | Document produit | Doublon | Dépendances | Statut d'analyse |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | `scanner_colis_qr.html` *(nom de travail)* | Écran Scanner Colis & QR | 04 — Transactions et paiements *(catégorie à arbitrer)* | `04_TRANSACTIONS_ET_PAIEMENTS/04_SCAN_COLIS_QR.md` · `04_TRANSACTIONS_ET_PAIEMENTS/04_SCAN_COLIS_QR_CORRECTIONS_MOBILE.md` | **Oui — doublon confirmé avec le fichier 5** : la modale `#qr-scanner-modal` de l'accueil reprend le même viseur, la même ligne de balayage et les mêmes coins orange/vert. Modes différents (3 onglets / 2 boutons) | Tailwind CDN, Lucide CDN, Google Fonts, image distante | Analysé |
| 2 | `connexion.html` *(nom de travail)* | Connexion | 06 — Profil et compte *(ou `AUTHENTICATION`, à arbitrer)* | `06_PROFIL_ET_COMPTE/06_CONNEXION.md` · `06_PROFIL_ET_COMPTE/06_CONNEXION_CORRECTIONS_MOBILE.md` | Non | Tailwind CDN + `tailwind.config` inline, Lucide CDN, Google Fonts, image distante | Analysé · **version mobile corrigée produite** |
| 3 | `inscription.html` *(nom de travail)* | Inscription | 06 — Profil et compte *(idem)* | `06_PROFIL_ET_COMPTE/07_INSCRIPTION.md` · `06_PROFIL_ET_COMPTE/07_INSCRIPTION_CORRECTIONS_MOBILE.md` | **Oui — doublon de code avec le fichier 2** : même `tailwind.config`, même bloc CSS, et le script contient `handleLoginSubmit` recopié | Tailwind CDN + `tailwind.config` inline, Lucide CDN, Google Fonts, image distante | Analysé · **version mobile corrigée produite** |
| 4 | `mot_de_passe_oublie.html` *(nom de travail)* | Mot de passe oublié | 06 — Profil et compte *(idem)* | `06_PROFIL_ET_COMPTE/08_MOT_DE_PASSE_OUBLIE.md` · `06_PROFIL_ET_COMPTE/08_MOT_DE_PASSE_OUBLIE_CORRECTIONS_MOBILE.md` | Non | Tailwind CDN **avec plugins `forms` et `container-queries`**, `tailwind.config` inline (`brand.*`), Lucide CDN, Google Fonts (Plus Jakarta Sans), image distante | Analysé · **version mobile corrigée produite** |
| 5 | `accueil.html` *(nom de travail)* | Accueil + navigation principale | 02 — Navigation et accueil | `02_NAVIGATION_ET_ACCUEIL/02_ACCUEIL.md` · `02_NAVIGATION_ET_ACCUEIL/02_ACCUEIL_CORRECTIONS_MOBILE.md` | Modale scanner = doublon du fichier 1 | Tailwind CDN **avec plugins**, `tailwind.config` inline (`zecolis-*`), Lucide CDN, **11 images distantes Googleusercontent**, **aucune police chargée** | Analysé · **version mobile corrigée produite** |
| 6 | `onboarding_expediteur_1.html` *(nom de travail)* | Onboarding 1/3 — Expéditeur | 02 — Navigation et accueil *(catégorie à arbitrer)* | `02_NAVIGATION_ET_ACCUEIL/02_ONBOARDING.md` · `02_NAVIGATION_ET_ACCUEIL/02_ONBOARDING_CORRECTIONS_MOBILE.md` | **Oui — doublon exact** : l'écran a été transmis deux fois dans le lot 6 | Tailwind CDN, `tailwind.config` inline (`zecolis.*`), Inter + Plus Jakarta Sans, 1 image distante | Analysé · **version mobile corrigée produite** |
| 7 | `onboarding_voyageur_2.html` *(nom de travail)* | Onboarding 2/3 — Voyageur | 02 — Navigation et accueil *(idem)* | `02_NAVIGATION_ET_ACCUEIL/02_ONBOARDING.md` · `02_NAVIGATION_ET_ACCUEIL/02_ONBOARDING_CORRECTIONS_MOBILE.md` | Non | Tailwind CDN, `tailwind.config` inline (`brand.*`, **8 jetons inutilisés**), Plus Jakarta Sans, 1 image distante | Analysé · **version mobile corrigée produite** |
| 8 | `recherche.html` *(nom de travail)* | Recherche — double entrée expéditeur / voyageur + fil de résultats | 03 — Publication et annonces *(à arbitrer)* | `03_PUBLICATION_ET_ANNONCES/03_RECHERCHE.md` · `03_PUBLICATION_ET_ANNONCES/03_RECHERCHE_CORRECTIONS_MOBILE.md` | Non | Tailwind CDN **avec plugins** `forms` et `container-queries`, `tailwind.config` inline (`brand.*`), Plus Jakarta Sans, 5 images distantes | Analysé · **version mobile corrigée produite** |
| 9 | `onboarding_confiance_3.html` *(nom de transmission)* | Onboarding 3/3 — Confiance | 02 — Navigation et accueil | `02_NAVIGATION_ET_ACCUEIL/02_ONBOARDING.md` *(étendu)* · `02_NAVIGATION_ET_ACCUEIL/02_ONBOARDING_CORRECTIONS_MOBILE.md` *(étendu)* | Non | Tailwind CDN, `tailwind.config` inline (`brandNavy/Dark/Orange/Green`, **4 jetons inutilisés**), Inter, 2 images distantes | Analysé · **version mobile corrigée produite** |
| 10 | `publier_voyage.html` *(nom de travail)* | Publier un voyage — trajectory, space, price, rules | 03 — Publication et annonces | `03_PUBLICATION_ET_ANNONCES/03_PUBLIER_VOYAGE.md` · `03_PUBLICATION_ET_ANNONCES/03_PUBLIER_VOYAGE_CORRECTIONS_MOBILE.md` | Non | Tailwind CDN **avec plugins** `forms` et `container-queries`, `tailwind.config` inline, 2 images distantes | Analysé · **version mobile corrigée produite** |
| 11 | `publier_colis.html` *(nom de travail)* | Publier un colis — contents, weight, dimensions, budget | 03 — Publication et annonces | `03_PUBLICATION_ET_ANNONCES/03_PUBLIER_COLIS.md` · `03_PUBLICATION_ET_ANNONCES/03_PUBLIER_COLIS_CORRECTIONS_MOBILE.md` | Non | Tailwind CDN **avec plugins** `forms` et `container-queries`, `tailwind.config` inline, 2 images distantes | Analysé · **version mobile corrigée produite** |

### 2.1 Catégories couvertes

| Catégorie | État |
| --- | --- |
| `00_INDEX_GENERAL` | Ce document |
| `01_PRESENTATION_PROJET` | Document transversal créé |
| `02_NAVIGATION_ET_ACCUEIL` | **4 pages** (Accueil + 3 écrans d'onboarding) + leurs analyses de corrections mobiles |
| `03_PUBLICATION_ET_ANNONCES` | **3 pages** (Recherche, Publier un voyage, Publier un colis) + leurs 3 analyses de corrections mobiles |
| `04_TRANSACTIONS_ET_PAIEMENTS` | 1 page (Scanner) + son analyse de corrections mobiles |
| `05_MESSAGERIE` | **Vide** — l'onglet `Messages` de l'accueil n'est qu'une ancre morte |
| `06_PROFIL_ET_COMPTE` | **3 pages** (Connexion, Inscription, Mot de passe oublié) + leurs 3 analyses de corrections mobiles |
| `07_DOCUMENTS_LEGAUX` | **Vide** — les CGU et la politique de confidentialité sont citées dans l'inscription mais **ne sont pas des documents** |
| `08_ANALYSE_GLOBALE` | **Vide** — à produire en fin de transmission |
| `09_POINTS_A_VERIFIER` | Document transversal, alimenté à chaque lot |

## 3. Correspondance fichiers HTML → documents

| Fichier HTML d'origine | Document Markdown produit |
| --- | --- |
| `SOURCES_HTML_ORIGINALES/scanner_colis_qr.html` | `04_TRANSACTIONS_ET_PAIEMENTS/04_SCAN_COLIS_QR.md` |
| ↑ (original, non modifié) | `04_TRANSACTIONS_ET_PAIEMENTS/04_SCAN_COLIS_QR_CORRECTIONS_MOBILE.md` |
| ↑ (version corrigée mobile) | `VERSIONS_CORRIGEES/scanner_colis_qr_mobile.html` |
| `SOURCES_HTML_ORIGINALES/connexion.html` | `06_PROFIL_ET_COMPTE/06_CONNEXION.md` |
| ↑ (original, non modifié) | `06_PROFIL_ET_COMPTE/06_CONNEXION_CORRECTIONS_MOBILE.md` |
| ↑ (version corrigée mobile) | `VERSIONS_CORRIGEES/connexion_mobile.html` |
| `SOURCES_HTML_ORIGINALES/inscription.html` | `06_PROFIL_ET_COMPTE/07_INSCRIPTION.md` |
| ↑ (original, non modifié) | `06_PROFIL_ET_COMPTE/07_INSCRIPTION_CORRECTIONS_MOBILE.md` |
| ↑ (version corrigée mobile) | `VERSIONS_CORRIGEES/inscription_mobile.html` |
| `SOURCES_HTML_ORIGINALES/mot_de_passe_oublie.html` | `06_PROFIL_ET_COMPTE/08_MOT_DE_PASSE_OUBLIE.md` |
| ↑ (original, non modifié) | `06_PROFIL_ET_COMPTE/08_MOT_DE_PASSE_OUBLIE_CORRECTIONS_MOBILE.md` |
| ↑ (version corrigée mobile) | `VERSIONS_CORRIGEES/mot_de_passe_oublie_mobile.html` |
| `SOURCES_HTML_ORIGINALES/accueil.html` | `02_NAVIGATION_ET_ACCUEIL/02_ACCUEIL.md` |
| ↑ (original, non modifié) | `02_NAVIGATION_ET_ACCUEIL/02_ACCUEIL_CORRECTIONS_MOBILE.md` |
| ↑ (version corrigée mobile) | `VERSIONS_CORRIGEES/accueil_mobile.html` |
| `SOURCES_HTML_ORIGINALES/onboarding_expediteur_1.html` | `02_NAVIGATION_ET_ACCUEIL/02_ONBOARDING.md` |
| `SOURCES_HTML_ORIGINALES/onboarding_voyageur_2.html` | ↑ (même document — les trois écrans forment une seule séquence) |
| `SOURCES_HTML_ORIGINALES/onboarding_confiance_3.html` | ↑ (même document — l'étape 3, reçue au lot 8) |
| ↑ (les trois originaux, non modifiés) | `02_NAVIGATION_ET_ACCUEIL/02_ONBOARDING_CORRECTIONS_MOBILE.md` |
| ↑ (versions corrigées mobiles) | `VERSIONS_CORRIGEES/onboarding_expediteur_mobile.html` · `VERSIONS_CORRIGEES/onboarding_voyageur_mobile.html` · `VERSIONS_CORRIGEES/onboarding_confiance_mobile.html` |
| `SOURCES_HTML_ORIGINALES/recherche.html` | `03_PUBLICATION_ET_ANNONCES/03_RECHERCHE.md` |
| ↑ (original, non modifié) | `03_PUBLICATION_ET_ANNONCES/03_RECHERCHE_CORRECTIONS_MOBILE.md` |
| ↑ (version corrigée mobile) | `VERSIONS_CORRIGEES/recherche_mobile.html` |
| `SOURCES_HTML_ORIGINALES/publier_voyage.html` | `03_PUBLICATION_ET_ANNONCES/03_PUBLIER_VOYAGE.md` |
| ↑ (original, non modifié) | `03_PUBLICATION_ET_ANNONCES/03_PUBLIER_VOYAGE_CORRECTIONS_MOBILE.md` |
| ↑ (version corrigée mobile) | `VERSIONS_CORRIGEES/publier_voyage_mobile.html` |
| `SOURCES_HTML_ORIGINALES/publier_colis.html` | `03_PUBLICATION_ET_ANNONCES/03_PUBLIER_COLIS.md` |
| ↑ (original, non modifié) | `03_PUBLICATION_ET_ANNONCES/03_PUBLIER_COLIS_CORRECTIONS_MOBILE.md` |
| ↑ (version corrigée mobile) | `VERSIONS_CORRIGEES/publier_colis_mobile.html` |

Les **onze pages reçues disposent désormais d'une version mobile corrigée** et de son document de corrections. Les onze originaux sont intacts.

### 3.1 Graphe de navigation entre versions corrigées

La deuxième passe du lot 8 a consisté à **relier les onze versions mobiles entre elles** : tous les liens pointent désormais vers un fichier `*_mobile.html` existant, et les écrans absents produisent un statut explicite au lieu d'un `href="#"` silencieux.

```
accueil ──┬─→ recherche ──┬─→ publier_colis ──┬─→ publier_voyage
          │              └─→ publier_voyage  │
          ├─→ publier_colis                 └─→ recherche
          ├─→ publier_voyage
          └─→ scanner_colis_qr ──→ accueil

connexion ──→ inscription ──→ onboarding_expediteur ──→ onboarding_voyageur ──→ onboarding_confiance ──→ accueil
     ↑            │                     ↑                                                │
     └────────────┴─────────────────────┴────────────────────────────────────────────────┘
                            mot_de_passe_oublie → connexion
```

| Contrôle | Résultat |
| --- | --- |
| Liens internes `href` vers un fichier inexistant | **0** sur les 11 fichiers |
| Liens `href="#"` résiduels dans le code | **0** (les 4 occurrences restantes sont des commentaires de correction) |
| Écrans non livrés référencés | messaging, profil, notifications, suivi de colis, marketplace, détail d'annonce, détail de colis — **tous renvoient un statut « indisponible »** |
| Liens vers un original ou un fichier hors `VERSIONS_CORRIGEES/` | **0** |

## 4. Arborescence des livrables

```
ZECOLIS_DOCUMENTATION/
├── 00_INDEX_GENERAL.md          (ce document)
├── 01_PRESENTATION_PROJET.md
├── 02_NAVIGATION_ET_ACCUEIL/
│   ├── 02_ACCUEIL.md
│   ├── 02_ACCUEIL_CORRECTIONS_MOBILE.md
│   ├── 02_ONBOARDING.md                       (écrans 1, 2 et 3)
│   └── 02_ONBOARDING_CORRECTIONS_MOBILE.md
├── 03_PUBLICATION_ET_ANNONCES/
│   ├── 03_RECHERCHE.md
│   ├── 03_RECHERCHE_CORRECTIONS_MOBILE.md
│   ├── 03_PUBLIER_VOYAGE.md
│   ├── 03_PUBLIER_VOYAGE_CORRECTIONS_MOBILE.md
│   ├── 03_PUBLIER_COLIS.md
│   └── 03_PUBLIER_COLIS_CORRECTIONS_MOBILE.md
├── 04_TRANSACTIONS_ET_PAIEMENTS/
│   ├── 04_SCAN_COLIS_QR.md
│   └── 04_SCAN_COLIS_QR_CORRECTIONS_MOBILE.md
├── 05_MESSAGERIE/               (vide)
├── 06_PROFIL_ET_COMPTE/
│   ├── 06_CONNEXION.md
│   ├── 06_CONNEXION_CORRECTIONS_MOBILE.md
│   ├── 07_INSCRIPTION.md
│   ├── 07_INSCRIPTION_CORRECTIONS_MOBILE.md
│   ├── 08_MOT_DE_PASSE_OUBLIE.md
│   └── 08_MOT_DE_PASSE_OUBLIE_CORRECTIONS_MOBILE.md
├── 07_DOCUMENTS_LEGAUX/         (vide)
├── 08_ANALYSE_GLOBALE/          (vide)
├── 09_POINTS_A_VERIFIER.md
├── SOURCES_HTML_ORIGINALES/
│   ├── scanner_colis_qr.html      (original, non modifié)
│   ├── connexion.html             (original, non modifié)
│   ├── inscription.html           (original, non modifié)
│   ├── mot_de_passe_oublie.html   (original, non modifié)
│   ├── accueil.html               (original, non modifié)
│   ├── onboarding_expediteur_1.html  (original, non modifié — doublon archivé une seule fois)
│   ├── onboarding_voyageur_2.html    (original, non modifié)
│   ├── recherche.html                (original, non modifié)
│   ├── onboarding_confiance_3.html   (original, non modifié)
│   ├── publier_voyage.html           (original, non modifié)
│   └── publier_colis.html            (original, non modifié)
└── VERSIONS_CORRIGEES/
    ├── scanner_colis_qr_mobile.html        (corrections scroll / mobile)
    ├── connexion_mobile.html               (mobile, accessibilité, retour honnête)
    ├── inscription_mobile.html             (bugs JS corrigés, mobile, accessibilité)
    ├── mot_de_passe_oublie_mobile.html     (navigation, mobile, validation)
    ├── accueil_mobile.html                 (mobile, accessibilité, navigation câblée)
    ├── onboarding_expediteur_mobile.html   (défilement rétabli, navigation câblée)
    ├── onboarding_voyageur_mobile.html     (défilement rétabli, étape 3 câblée)
    ├── onboarding_confiance_mobile.html    (créé au lot 8 — fin de séquence)
    ├── recherche_mobile.html               (mobile, 3 bugs fonctionnels, retours honnêtes)
    ├── publier_voyage_mobile.html          (créé au lot 8 — formulaire complet)
    └── publier_colis_mobile.html           (créé au lot 8 — formulaire complet)
```

Les dossiers restent vides tant que la page correspondante n'a pas été transmise.

## 4.1 Vue d'ensemble de l'application reconstruite

```
                    ┌──────────────────────────────┐
                    │  ACCUEIL (fichier 5)         │
                    │  navbar 5 entrées + FAB      │
                    └──┬────┬────┬────┬────┬───┬────┘
       Accueil  Recherche  Publier  Messages  Profil   Scanner (modale)
         ▲          ▲          ▲        ▲        ▲          ▲
         │          │          │        │        │          │
   tous des # : LIENS MORTS  tous LIENS MORTS   seule action réelle
                                                          │
                                            ┌─────────────┴──────────────┐
                                            │ DOUBLON : écran Scanner     │
                                            │ fichier 1 vs modale fichier 5│
                                            └────────────────────────────┘

Parcours d'accès — ROMPU dans les originaux :
    ┌──────────────┐   S'inscrire (js:void)   ┌──────────────┐
    │  Connexion   │ ────X──────────────────▶ │  Inscription  │
    │ (fichier 2)  │ ◀───X─────────────────── │ (fichier 3)  │
    └──────┬───────┘    Se connecter (js:void)└──────────────┘
           │
           │ « Oublié ? » (javascript:void(0))
           ▼
    ┌──────────────────────┐
    │ Mot de passe oublié  │  ──▶ écran « choisir un nouveau mot de passe »
    │   (fichier 4)        │      ABSENT (non reçu)
    └──────────────────────┘

    Onboarding — 3 ÉTAPES, 3 REÇUES (lot 8) :
    ┌───────────────────┐  Suivant (mort) ┌───────────────────┐  Suivant (mort) ┌──────────────┐
    │ 1/3 Expéditeur    │ ──────────────▶ │ 2/3 Voyageur      │ ─────────────▶ │ 3/3 Confiance │
    │ (fichier 6)       │                 │ (fichier 7)       │                │ (fichier 9)   │
    └───────────────────┘                 └───────────────────┘                └──────┬───────┘
                              Commencer / Retour (mort)                              │
                                                                                    ▼
                                                                          ┌──────────────────┐
                                                                          │ Accueil          │
                                                                          └──────────────────┘

    Recherche (fichier 8) — reçu, mais Navigation INOPERANTE :
    ┌──────────────────────────────────────────────────────────────────────────────┐
    │ double entrée expéditeur / voyageur · 0 champ de saisie · 0 lien <a>         │
    │ 2 fonctionnalités réelles sur 20 · 11 actions sans gestionnaire               │
    └───────────────┬──────────────┬─────────────┬──────────────┬─────────────────┘
         ✗ Détail   ✗ Détail   ✗ Publier   ✗ Messages   ✗ Profil / Notifications
       d'annonce (×2)  de colis

    Publication — reçue au lot 8 (fichiers 10 et 11) :
    ┌─────────────────────┐        ┌─────────────────────┐
    │ Publier un voyage   │ ◀────▶ │ Publier un colis    │
    │ (fichier 10, 741 l.)│  ongl. │ (fichier 11, 730 l.)│
    └─────────────────────┘        └─────────────────────┘
      ✗ bouton retour mort      ✗ bouton retour mort
      ✗ 10 actions sans gestionnaire
      ✗ 29 actions sans gestionnaire
      ✗ 7 liens en href="#"
      ✗ modale sans role ni Échap
      ✗ fausse confirmation « publié ! »

    Fonction centrale du § 3 « correspondance colis ↔ voyage » :
    TOUJOURS ABSENTE. Les deux formulaires de publication sont indépendants,
    le scanner ne lit aucun code et la recherche ne retourne aucun résultat réel.
```

## 5. Règles de traçabilité appliquées

- Chaque fichier HTML reçu est conservé avec son nom d'origine (copie non modifiée dans `SOURCES_HTML_ORIGINALES/`).
- **Lorsqu'un même écran est transmis plusieurs fois**, une seule copie est archivée et le doublon est signalé dans l'inventaire et dans le journal de réception.
- Les versions corrigées sont **séparées** dans `VERSIONS_CORRIGEES/` et accompagnées d'un document listant chaque modification. Aucun original n'est écrasé.
- Chaque page produit un document Markdown distinct.
- Toute information absente du code est marquée « Non déterminé à partir du fichier transmis ».
- Les dossiers ne contiennent que des documents correspondant à leur catégorie.

## 6. Journal de réception des lots

| Lot | Fichiers reçus | Commentaire |
| --- | --- | --- |
| 1 | 1 — écran Scanner Colis & QR | Maquette HTML autonome. Thème sombre, glassmorphism, police Plus Jakarta Sans : écarts majeurs avec la référence § 4. Scroll bloqué (`overflow-hidden` + `h-[844px]`). Aucun lien Firebase / React / Capacitor. Version mobile corrigée produite. |
| 2 | 1 — Connexion | Maquette HTML autonome. **Palette, police Inter et fond conformes à § 4.** Authentification entièrement simulée (aucun réseau). Scroll correct. Défauts : zoom bloqué (`user-scalable=no`), `select-none` sur le `<body>`, safe-area absente, liens de service ~16 px. |
| 3 | 1 — Inscription | **Copier-coller du fichier 2** (même `tailwind.config`, même CSS, fonction `handleLoginSubmit` recopiée). **Défaut critique : `handleSignupSubmit` n'existe pas → soumission native en GET, les deux mots de passe sont écrits dans l'URL.** Boutons œil cassés (`TypeError`), 3 messages d'erreur morts, règle « 8 caractères minimum » jamais appliquée, 3 liens de service morts dont les CGU et la politique de confidentialité. |
| 4 | 1 — Mot de passe oublié | Seul écran avec un **bouton retour réel** (44×44 px) et un JS sans code mort. **Police Plus Jakarta Sans** (divergence avec § 4.4). Seul fichier à charger les plugins `forms` et `container-queries` → rendu des champs non comparable aux fichiers 2 et 3. Aucun contrôle du format e-mail ; aucun envoi réel. **Texte de confirmation formulé correctement** (ne divulgue pas l'existence d'un compte). Safe areas les plus faibles des quatre (`pt-4` / `pb-2`). |
| 5 | 1 — Accueil (le plus riche) | 683 lignes pour **7 lignes de JavaScript**. **Barre de navigation principale du § 4.5 entièrement inopérante** (4 onglets + bouton Publier, 12 ancres mortes). Recherche d'itinéraire à champs non éditables. Modale scanner = **doublon du fichier 1**. **9 dégradés, 12 `backdrop-blur`, 8 ombres marquées, >20 teintes hors palette** : le fichier le moins conforme à § 4. 2 éléments fixes sans safe area (les onglets du bas tombent dans la zone du geste iOS), 18 cibles < 44 px sur 28, contrastes non AA sur les prix et les notes. **29 affirmations de capacité** non sourcées (§ 3) et données entièrement fictives. |
| 6 | **2** — Onboarding 1/3 Expéditeur et 2/3 Voyageur (**4 transmissions, 3 écrans distincts : doublon exact sur l'écran 1**) | Écrans **plein cadre photo**, sans rapport avec les écrans applicatifs clairs de § 4 (thème sombre, glassmorphism, dégradés). **Les 4 boutons de la paire sont morts** : ni `id`, ni `onclick`, ni gestionnaire — 0 action réelle. `overflow: hidden` sur le `<body>` des deux fichiers **+ `h-[844px]`** → contenu coupé sans défilement possible. Safe areas absentes, fausse barre d'état et fausse pilule d'accueil. `Paiement garanti` / `✓ Instantané` sans mécanisme (§ 3). **L'étape 3 de l'onboarding n'a pas été transmise.** |
| 7 | 1 — Recherche | **Seul écran à double entrée** (expéditeur / voyageur) et **seule reconstitution complète de la barre du § 4.5** (5 entrées). 2 fonctionnalités réelles sur 20 ; **11 actions sans gestionnaire**, dont le CTA « Rechercher ». **0 `<input>`** : départ et arrivée sont des `<div>` — la recherche n'est pas saisissable. **0 `<a href>`** : navigation morte. Inversion départ/arrivée : **les pays et drapeaux restent attachés à la mauvaise ville**. Mode Voyageur annonce « 28 colis » sans changer les cartes. 4 puces de filtre qui **ne filtrent rien**. Zoom bloqué, `env(safe-area-inset-*)` utilisé 0 fois malgré `viewport-fit`, barre basse `fixed` sur conteneur `min-h-[844px]`, **15 cibles < 44 px sur 18**, 35 textes < 12 px, **7 classes Tailwind invalides** (`h-13`, `w-13`, `rounded-2xs`, `shadow-2xs`). Design **le plus proche de § 4** depuis l'accueil (aucun glassmorphism, aucun dégradé). Données entièrement fictives et datées de septembre 2025 ; marque « Air Parcel Hub » absente du cahier des charges. |
| 8 | **3** — Onboarding 3/3 Confiance, Publier un voyage, Publier un colis | **L'étape 3 de l'onboarding est enfin reçue** : la série 1 → 2 → 3 est complète, mais l'écran 3 reste **0 action réelle sur 2** et **promet une vérification d'identité et des avis qui n'existent nulle part** (`✓ Identité vérifiée`, `★ 4,9`). **Les deux formulaires de publication sont les écrans les plus longs du dossier après l'accueil** (741 et 730 lignes) et les moins fonctionnels : **10 et 29 actions sans gestionnaire**, `alert()` et `prompt()`, **7 liens en `href="#"`**, et surtout une **fausse confirmation « Votre voyage / colis est publié ! »** déclenchée par un `setTimeout` sans appel réseau. Sur les deux : zoom bloqué, **safe areas déclarées mais jamais utilisées**, bouton d'action principal en `position: fixed` masqué par le clavier virtuel, modales d'aide sans `role="dialog"` ni fermeture par Échap, cases de consentement **pré-cochées**, `shadow-2xs` / `animate-in` / `slide-in-from-bottom` invalides. 41 `getElementById` résolus sans orphelin et 66 `id` uniques : le code est propre, ce sont les **comportements** qui manquent. |

## 7. Synthèse par fichier

| Fichier | Conformité § 4 (design) | Logique applicative | Défilement mobile | Défauts mobiles principaux |
| --- | --- | --- | --- | --- |
| 1 — Scanner | **Faible** (thème sombre, 5 écarts) | 1 action réelle sur 9 | **Bloqué** | scroll bloqué, safe areas, viseur fixe, ~60 cibles < 44 px |
| 2 — Connexion | **Bonne** (10/13, 4 couleurs hors palette) | 2 actions réelles sur 4, authentification factice | Correct | zoom bloqué, `select-none`, safe areas, liens 16–18 px |
| 3 — Inscription | **Bonne** (mêmes écarts que le fichier 2) | **1 action sur 7**, soumission en GET avec mots de passe en clair | Correct | idem fichier 2 + 3 liens légaux à 16 px |
| 4 — MdP oublié | Correcte (sauf police et 4 couleurs) | 2 actions réelles, envoi simulé | Correct | `pt-4`/`pb-2` sans safe area, zoom bloqué, `user-select:none` |
| 5 — Accueil | **Faible** (dégradés + glassmorphism interdits) | 6 actions réelles sur 20 | Correct | 2 éléments fixes sans safe area, **18 cibles < 44 px sur 28**, 12 ancres mortes, 0 champ de saisie, contrastes non AA |
| 6 — Onboarding 1/3 Expéditeur | **Faible** (écran plein cadre photo, hors § 4.2) | **0 action réelle sur 2** | **Bloqué** | `overflow: hidden` + `h-[844px]`, safe areas, « Passer » à 26 px, barre de progression invisible |
| 7 — Onboarding 2/3 Voyageur | **Faible** (idem) | **0 action réelle sur 2** | **Bloqué** | idem + icône réseau `opacity="0"`, `group-hover` sans parent, 8 jetons de couleur inutilisés |
| 8 — Recherche | **Bonne** (le plus conforme depuis l'accueil : ni glassmorphism ni dégradé) | 2 actions réelles sur 20, **0 lien** | Correct mais barre basse `fixed` sur `min-h-[844px]` | zoom bloqué, `env()` jamais utilisé, **15 cibles < 44 px sur 18**, 0 champ de saisie, 11 actions mortes |
| 9 — Onboarding 3/3 Confiance | **Faible** (écran plein cadre photo) | **0 action réelle sur 2** | **Bloqué** | `overflow: hidden` sur le `<body>` sans conteneur de repli, safe areas, « Retour » à 24 px, pagination **dupliquée**, `✓ Identité vérifiée` et `★ 4,9` sans aucun mécanisme |
| 10 — Publier un voyage | **Bonne** (fond clair, Inter, orange `#FF6B00`) | **2 actions sur 21**, 2 `alert()`, 1 `prompt()`, fausse confirmation « publié ! » | Correct | zoom bloqué, safe areas absentes, **10 actions mortes**, 3 cases **pré-cochées**, modale sans `role` ni Échap, `shadow-2xs` invalide |
| 11 — Publier un colis | **Bonne** (idem) | **1 action sur 35**, **29 actions mortes**, **7 liens `href="#"`** | Correct | zoom bloqué, safe areas absentes, fausse confirmation « publié ! », modale sans `role` ni Échap, 4 cases pré-cochées, `animate-in` / `slide-in-from-bottom` invalides |

**Points communs aux onze fichiers :** aucun appel réseau réel, aucun lien Firebase / React / Capacitor, `env(safe-area-inset-*)` **absent ou inutilisé dans les onze**, `select-none` / `user-select: none` dans sept, et **aucun nom de fichier d'origine communiqué** (les transmissions du lot 8 en portent un pour l'onboarding 3/3).

**Points communs aux dix fichiers applicatifs (hors onboarding) :** zoom pincement désactivé (`user-scalable=no`) dans **neuf** sur dix — le fichier 7 (onboarding 2) est le seul à ne pas le bloquer. `select-none` ou équivalent dans sept. Barre basse ou dock sans zone de geste dans sept.

**Points communs aux quatre formulaires longs (recherche, publier voyage, publier colis, inscription) :** le `<body>` est en `min-h-[844px]` avec un conteneur `max-h-screen`, et le bouton d'action principale est un `position: fixed` collé au bas de l'écran — masqué par le clavier virtuel sur iOS.

## 7.1 Versions mobiles corrigées produites

Les onze pages reçues ont été reprises dans `VERSIONS_CORRIGEES/` sous forme de **versions mobiles distinctes**. **Aucun original n'a été modifié.**

| Page | Version mobile | Corrections principales | Document associé |
| --- | --- | --- | --- |
| Scanner | `scanner_colis_qr_mobile.html` | défilement rétabli, `100dvh`, safe areas, viseur adaptatif, 7 cibles à 44 px | `04_SCAN_COLIS_QR_CORRECTIONS_MOBILE.md` |
| Connexion | `connexion_mobile.html` | zoom réautorisé, `select-none` levé (collage rétabli), champs à 16 px, liens « Oublié ? » / « S'inscrire » câblés, **fausse réussite « Connecté ✓ » supprimée** | `06_CONNEXION_CORRECTIONS_MOBILE.md` |
| Inscription | `inscription_mobile.html` | **`handleSignupSubmit` écrite** (fin de la soumission GET avec mots de passe dans l'URL), **boutons « œil » réparés**, règle des 8 caractères + confirmation + CGU réellement appliquées | `07_INSCRIPTION_CORRECTIONS_MOBILE.md` |
| Mot de passe oublié | `mot_de_passe_oublie_mobile.html` | « Retour à la connexion » devient une vraie navigation, repli du bouton retour, `max-w` déplacé hors du `<body>`, message d'erreur e-mail | `08_MOT_DE_PASSE_OUBLIE_CORRECTIONS_MOBILE.md` |
| Accueil | `accueil_mobile.html` | champs Départ / Arrivée réels, inversion du trajet, recherche simulée avec retour, 12 ancres mortes converties en retours explicites, modale scanner accessible (focus, Échap), carousels focalisables | `02_ACCUEIL_CORRECTIONS_MOBILE.md` |
| Onboarding 1/3 Expéditeur | `onboarding_expediteur_mobile.html` | défilement rétabli, `100dvh` + photo en `position: fixed`, safe areas, « Passer » et « Suivant » **câblés** (vers l'accueil et vers l'écran 2), progression et pagination annoncées | `02_ONBOARDING_CORRECTIONS_MOBILE.md` |
| Onboarding 2/3 Voyageur | `onboarding_voyageur_mobile.html` | idem · **étape 3 non reçue : retour explicite à l'appui de « Suivant »**, icône réseau `opacity="0"` restaurée, `group` ajoutée, `aria-label` en écart WCAG 2.5.3 retirés | `02_ONBOARDING_CORRECTIONS_MOBILE.md` |
| Recherche | `recherche_mobile.html` | zoom réautorisé, `env()` réellement appliqué, **3 bugs fonctionnels corrigés** (pastilles de pays inversées, mode Voyageur honnête, état des puces de filtre), `Réinitialiser` rendu fonctionnel, 15 cibles portées à 44 px, 11 actions mortes converties en retours expliqués | `03_RECHERCHE_CORRECTIONS_MOBILE.md` |
| Onboarding 3/3 Confiance | `onboarding_confiance_mobile.html` | **créé** · défilement rétabli, `100dvh`, safe areas, pagination annoncée (elle était dupliquée et décorative), « Commencer » et « Retour » **câblés**, `rounded-2xs` et `via-45%`/`via-40%` corrigées, **réserve visible sur les badges de vérification** | `02_ONBOARDING_CORRECTIONS_MOBILE.md` |
| Publier un voyage | `publier_voyage_mobile.html` | **créé** · formulaire complet fonctionnel (date, inversion, espace, prix, description, photo, aperçu), onglets voyage/colis câblés, consentement **non pré-coché**, modales accessibles (focus, Échap, restauration), **fausse confirmation supprimée** au profit d'un récapitulatif « non publié » | `03_PUBLIER_VOYAGE_CORRECTIONS_MOBILE.md` |
| Publier un colis | `publier_colis_mobile.html` | **créé** · formulaire complet (type, quantité, poids personnalisé, dimensions, photo, trajet, date, emballage, remise, budget, aperçu), validation des champs obligatoires, feuilles d'information accessibles, **7 liens `href="#"` supprimés**, navigation basse câblée | `03_PUBLIER_COLIS_CORRECTIONS_MOBILE.md` |

### 7.2 Deuxième passe : navigation entre versions corrigées

Après la création des trois fichiers du lot 8, **une seconde passe a consisté à relier les onze versions entre elles**, les liens restants étant des statuts « indisponible » ou des boutons sans effet.

| Fichier | Câblage réalisé |
| --- | --- |
| `accueil_mobile.html` | carte « Envoyer un colis » → `publier_colis_mobile.html` · carte « Voyager avec un colis » → `publier_voyage_mobile.html` · onglet Recherche → `recherche_mobile.html` · FAB Publier → `publier_colis_mobile.html` · lien « scanner QR dédié » dans la modale → `scanner_colis_qr_mobile.html` · **le formulaire de recherche navigue désormais vers `recherche_mobile.html`** au lieu d'annoncer que la page manque |
| `recherche_mobile.html` | onglet courant → `recherche_mobile.html` · bouton central Publier → `publier_colis_mobile.html` · le CTA « Rechercher des voyageurs » ouvre un bloc proposant **deux liens réels** vers les formulaires de publication |
| `scanner_colis_qr_mobile.html` | « Retour » (`href="#"`) → `accueil_mobile.html` · onglets de mode désormais `role="tablist"` avec `aria-selected` et retour vocal · import photo → vrai `<input type="file">` · bouton Son → `aria-pressed` · OTP → panneau de saisie réel · « Aide » → statut explicite · **3 formulations trompeuses corrigées** (« AI Live », « La validation s'effectue automatiquement », « Séquestre débloqué ») |
| `onboarding_expediteur_mobile.html` | **aucune modification nécessaire** : « Suivant » pointait déjà sur `onboarding_voyageur_mobile.html` |
| `connexion_mobile.html` | le message « Adresse e-mail ou mot de passe incorrect » **ne prétendait pas qu'une vérification avait eu lieu** → remplacé par un message d'absence de service, et un bloc « mode démonstration » offre désormais **deux liens réels** (accueil, onboarding) |
| `inscription_mobile.html` | idem : après le message d'absence de branchement, un bloc « mode démonstration » offre **deux liens réels** (onboarding expediteur, accueil) |

**Règles appliquées à l'ensemble des onze versions** : zoom et pincement réautorisés · `viewport-fit=cover` + `env(safe-area-inset-*)` · `100dvh` · cibles tactiles à 44 px · `touch-action: manipulation` · `prefers-reduced-motion` · retours reliés par `role="status"` / `aria-live` / `aria-describedby` / `aria-invalid` · **aucune fausse réussite, aucun faux backend annoncé, aucun appui silencieux** · identité visuelle conservée à l'identique.

**Limite inchangée :** ces corrections sont **statiques**. Aucun navigateur, aucun appareil réel, aucun test visuel n'a pu être effectué. Les safe areas, le focus, le clavier virtuel, le rendu des icônes Lucide et le comportement des animations restent à vérifier sur matériel.

## 8. Ce qu'il reste à recevoir

Écrans référencés dans le code mais **non reçus** :

| Écran | Où il est référencé |
| --- | --- |
| **Recherche / résultats d'itinéraire** | **reçu** (`recherche.html`) — mais **les écrans de détail et la saisie des lieux ne le sont pas** |
| **Étape 3 de l'onboarding** | indicateurs « 3 étapes » des écrans 6 et 7 — **non reçue** |
| Sortie de l'onboarding (« Passer ») | aucune cible dans les originaux ; l'accueil a été retenu comme destination |
| Publication d'un voyage | `#voyager`, bouton central `Publier` (accueil **et** recherche) |
| Publication d'un colis | `#envoyer`, carte « Envoyer un colis », onglet `Envoyer un colis` (recherche) |
| Messagerie | onglet `Messages` + badge `2` (accueil **et** recherche) |
| Profil / informations personnelles | onglet `Profil`, `#profile-edit`, avatar (accueil **et** recherche) |
| Notifications | bouton cloche (accueil **et** recherche) |
| Suivi de colis | `#suivi` |
| Marketplace | `#marketplace` |
| Détail d'un voyage ou d'un colis | boutons `Voir le voyage` / `Voir le colis` / `Voir l'annonce` (×2) / `Voir le colis` (recherche) |
| Choix d'un nouveau mot de passe | parcours du fichier 4 |
| Garantie / Séquestre / Douane | `#garantie`, `#paiement-protection`, `#guide-douane` |
| Connexion / Inscription / Mot de passe oublié | **reçus**, mais **non reliés entre eux dans les originaux** ; les liens ont été câblés dans les versions mobiles corrigées |
| Conditions générales / Politique de confidentialité | `javascript:void(0)` dans l'inscription — **contenu à rédiger** |

**Bilan des écrans :** 8 pages distinctes reçues — 4 écrans d'accès (accueil, connexion, inscription, mot de passe oublié), 2 écrans d'un onboarding de 3 étapes, 1 écran de recherche. **Les écrans fonctionnels du § 2 restent absents** : publication de voyage, publication de colis, messagerie, profil, notifications, suivi de colis, détail d'annonce, détail de colis, marketplace, et l'étape 3 de l'onboarding.