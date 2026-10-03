Fichier 9 - Onboarding 3/3 Confiance :

| Fonctionnalité affichée | Implémentée dans le fichier ? |
| --- | --- |
| Bouton `Commencer` (sortie de l'onboarding) | **Non** - `<button>` sans `id`, sans `onclick`, aucun gestionnaire |
| Bouton `Retour` (retour à l'étape 2) | **Non** - idem |
| Pagination (3 pastilles, en-tête **et** pied) | **Non** - `<span>` décoratifs, aucun `role`, aucun `aria-current` |
| `✓ Identité vérifiée` | **Non** - aucune procédure, aucun code, aucune donnée |
| `★ 4,9 Voyageur vérifié` | **Non** - note sans source, sans nombre d'avis, sans échelle |
| Barre de progression / barre d'état / pilule | **Non** - dessinées en dur |
| Animations d'interface | **Non** - 0 animation permanente (seule particularité du lot) |

Bilan : **0 action réelle sur 2.** C'est l'écran le plus vide du dossier, et celui qui porte la promesse la plus engageante du § 3.

Fichier 10 - Publier un voyage :

| Fonctionnalité affichée | Implémentée dans le fichier ? |
| --- | --- |
| Bouton retour (en-tête) | **Non** - `<button>` sans gestionnaire |
| Onglets `Je voyage` / `Je transporte un colis` | **Non** - aucun gestionnaire |
| Sélection `Date précise` / `Dates flexibles` | **Non** - aucune classe d'état, aucun effet |
| Inversion de déplacement (`Où se trouve cet espace`) | **Non** - aucun gestionnaire |
| Champ espace (kg) et prix | **Partiellement** - champs présents, **aucune validation, aucun calcul** |
| Ajout de photo | **Non** - pas de `input type="file"` |
| 3 règles d'ouverture (modale `#rules-modal`) | **Non** - aucun gestionnaire, **aucun `role="dialog"`, aucune fermeture par `Échap`**, focus non piégé |
| Case de consentement | **Non** - `required` non géré, et **3 cases déjà cochées** |
| Bouton `Publier mon voyage` | **Non** - déclenche un `alert(...)` |
| Confirmation `Votre voyage est publié !` | **Non** - `setTimeout` + `alert()`, **aucun appel réseau** |
| Champs description / titre | **Partiellement** - champs présents, non validés |

Bilan : **2 actions réelles sur 21** (et ce sont deux `alert()`), **10 actions sans gestionnaire**, **aucun lien de navigation**, et une fausse confirmation de publication.

Fichier 11 - Publier un colis :

| Fonctionnalité affichée | Implémentée dans le fichier ? |
| --- | --- |
| Bouton retour (en-tête) | **Non** - `<button>` sans gestionnaire |
| Onglets `Publier un colis` / `Publier un voyage` | **Non** - aucun gestionnaire |
| Type de colis (3 pastilles) | **Non** - aucun gestionnaire |
| `Utiliser ma balance connectée` | **Non** - aucun gestionnaire |
| Champs poids et dimensions (3) | **Non** - aucune validation, `0 required` dans tout le fichier |
| Champ `Contenu du colis` | **Non** - aucune validation |
| Ajout de photo | **Non** - pas de `input type="file"` |
| Départ / arrivée | **Non** - valeurs codées en dur, aucune saisie |
| 4 feuilles d'information (modale `role="dialog"`) | **Non** - **aucun gestionnaire** sur les 29 boutons qui les déclenchent |
| Bouton d'aide `Comment ça marche ?` | **Non** - `href="#"` |
| Navigation basse (5 entrées) | **Non** - **7 liens en `href="#"`** |
| 4 cases de consentement | **Non** - **`required` absent, 4 cases déjà cochées** |
| Bouton `Publier mon colis` | **Non** - aucun gestionnaire |
| Confirmation `Votre colis est publié !` | **Non** - `setTimeout`, **aucun appel réseau** |

Bilan : **1 action réelle sur 35**, **29 actions sans gestionnaire**, **7 liens `href="#"`**, aucune validation de champ, et une fausse confirmation de publication.

**Constat de fond sur les fichiers 10 et 11 :** ces deux écrans sont les plus longs du dossier après l'accueil (741 et 730 lignes) et les moins fonctionnels. Leur code est pourtant **propre** : identifiants uniques, tous les `getElementById` résolus, aucune classe CSS morte. Ce ne sont pas des fichiers cassés, ce sont des **maquettes complètes** dont les comportements n'ont jamais été écrits.

---

