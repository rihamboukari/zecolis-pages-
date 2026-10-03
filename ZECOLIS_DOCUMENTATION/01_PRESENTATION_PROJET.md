# 01 — Présentation du projet

**Source :** cahier des charges Zecolis transmis par le donneur d'ordre.
**Aucun fichier de code n'a encore été analysé.** Ce document ne décrit donc que le contexte fourni ; il ne constitue pas une vérification du code réel.

## 1. Nom et nature du projet

**Zecolis** est une plateforme numérique de transport collaboratif de colis. Elle met en relation :

- des personnes souhaitant **envoyer un colis** (expéditeur) ;
- des **voyageurs** effectuant déjà un trajet et disposant d'une capacité de bagages disponible.

La finalité annoncée est de faciliter les échanges de colis entre la **France et le Bénin**, en particulier sur le corridor France ↔ Bénin, avec Cotonou côté béninois et Paris côté français.

Zecolis n'est pas présenté comme une application de livraison classique, mais comme une mise en relation entre deux besoins complémentaires.

## 2. Problème traité

- L'envoi de colis entre pays est décrit comme coûteux, compliqué ou difficile à organiser.
- Certains voyageurs ont une capacité de bagages sous-utilisée lors de trajets internationaux réguliers.
- La plateforme vise à rapprocher ces deux catégories d'utilisateurs.

## 3. Vision et principes

Cinq principes sont annoncés et doivent guider écrans, parcours, documents fonctionnels et architecture :

1. **Accessibilité** — simplifier la recherche d'une solution d'envoi ou d'un voyageur.
2. **Économie** — faciliter la mise en relation autour de tarifs convenus.
3. **Confiance** — permettre de consulter les informations disponibles sur les autres membres.
4. **Simplicité** — rendre les principales opérations compréhensibles depuis un téléphone.
5. **Traçabilité** — organiser les étapes des transactions et conserver leur historique.

## 4. Acteurs et rôles

### 4.1 Expéditeur

Fonctions prévues : recherche d'un voyageur ou d'une annonce, consultation du détail d'une annonce de voyage, publication d'une annonce de colis, description du colis et informations de transport, indication de destination / période / budget, consultation du profil du voyageur, messagerie, suivi de transaction, historique, favoris, accès aux fonctions de paiement ou de confirmation.

### 4.2 Voyageur

Fonctions prévues : recherche d'annonces de colis compatibles, publication d'une annonce de voyage avec itinéraire et date de départ, indication de la capacité de bagages disponible, définition du prix ou des modalités tarifaires, consultation des informations de l'expéditeur, communication, suivi des transactions, gestion des voyages publiés, historique.

### 4.3 Règle explicitement donnée

Un même compte peut agir comme expéditeur **et** comme voyageur. Il ne faut pas supposer deux types de comptes distincts sauf indication contraire dans le code ou le modèle de données.

## 5. Modèle des annonces

Deux catégories sont décrites :

| Catégorie | Auteur | Contenu |
| --- | --- | --- |
| Annonce de voyage | Voyageur | Trajet effectué + capacité de transport disponible |
| Annonce de colis | Expéditeur | Recherche d'une personne pouvant transporter le colis |

La compatibilité entre annonces doit pouvoir être vérifiée à partir de : départ, destination, dates, capacité ou poids, conditions tarifaires.

**À confirmer sur le code :** les données exactes, les champs obligatoires et les règles de correspondance ne sont pas déterminés à ce stade.

## 6. Corridor

Le corridor de référence est **France ↔ Bénin**. Cotonou est une ville importante du parcours béninois, Paris une destination française fréquemment citée en exemple. Les exemples de trajets dans les maquettes ne constituent pas une liste exhaustive des destinations.

## 7. Confiance et sécurité

Éléments cités dans les maquettes ou le projet : profils utilisateurs, informations de vérification, badges de vérification (uniquement si statut confirmé), évaluations ou avis (si données réellement disponibles), messagerie, suivi des transactions, confirmations de remise et de livraison, codes QR (si le parcours est pris en charge), informations relatives au paiement, à l'annulation et à l'assurance (selon fonctionnalités mises en œuvre).

Contraintes explicites :

- ne jamais présenter une vérification comme réussie si le système ne la confirme pas ;
- ne pas inventer de notes, d'avis, de garanties, de couvertures d'assurance ou de confirmations de livraison ;
- les informations affichées doivent refléter l'état réel du système.

## 8. Identité visuelle de référence

La capture d'écran de la page **Profil** est la référence visuelle prioritaire. Toutes les pages secondaires doivent conserver le même langage graphique.

### 8.1 Palette

| Élément | Couleur |
| --- | --- |
| Navy profond | `#1D1D4B` |
| Navy secondaire | `#0B2A5B` |
| Orange Zecolis | `#FF6B00` |
| Fond orange clair | `#FFF7ED` |
| Vert de confiance | `#22A55A` |
| Vert complémentaire | `#22C55E` |
| Fond général | `#F8FAFC` |
| Fond des cartes | `#FFFFFF` |
| Texte principal | `#0F172A` |
| Texte secondaire | `#64748B` |
| Bordures | `#F1F5F9` |

Règles : navy profond pour titres, trajets et éléments structurants ; orange pour actions principales, accents et éléments actifs ; vert réservé aux états positifs, étapes confirmées et vérifications réussies ; gris très clair en fond général, cartes blanches à bordures fines.

Interdits explicites : violet, cyan, bleu électrique, dégradés multicolores, transformation de toute l'interface en écran orange.

### 8.2 Typographie et composants

- Police principale : **Inter**.
- Style : moderne, sobre, professionnel, mobile-first, compact, premium sans effets décoratifs excessifs.
- Cartes blanches à coins modérément arrondis (~16 px).
- Boutons simples (~12 px de rayon).
- Bordures fines, ombres légères.
- Icônes Lucide, badges compacts, conteneurs d'icônes aux fonds pastel, espacements réguliers, hiérarchie typographique claire.

À éviter : grandes cartes illustrées, bannières disproportionnées, illustrations 3D, ombres épaisses, effets glassmorphism.

### 8.3 Format mobile

- Viewport de référence : **390 × 844 px**.
- Prise en compte : zones de sécurité, défilement vertical, zones tactiles, clavier mobile, lisibilité, largeur d'écran réduit.
- Les pages secondaires ouvertes depuis Profil ne doivent généralement pas afficher la navigation principale inférieure. Ce comportement doit être vérifié écran par écran et n'est pas étendu aux pages principales sans raison.

## 9. Organisation de l'application

Navigation principale annoncée en cinq entrées : **Accueil, Recherche, Publier, Messages, Profil**.

Intentions décrites par zone :

| Zone | Intention |
| --- | --- |
| Accueil | Message d'accueil personnalisé, identité du corridor, mise en avant de la recherche d'un voyageur, recherche départ/destination, actions envoyer un colis / voyager avec un colis, accès rapides. Rester orienté action, pas marketing. |
| Recherche | Retrouver annonces voyages et/ou colis selon le fonctionnement défini. Critères et filtres cohérents avec les champs réellement disponibles. |
| Publication | Deux parcours : « Je voyage » et « J'envoie un colis ». Formulaires adaptés au type d'annonce, réutilisant champs et règles existants. |
| Messages | Liste des conversations et conversation individuelle, avec présentation cohérente du participant et des références d'annonce. |
| Profil | Point d'accès aux pages secondaires (annonces, favoris, transactions, historique, vérification, informations personnelles, notifications, documents). |

## 10. Pages secondaires du profil

| Page | Objet | Éléments de référence cités |
| --- | --- | --- |
| Mes voyages | Gérer les annonces de voyage publiées | En-tête avec retour, titre, sous-titre « Voyages que vous avez publiés », accès publication, filtres par statut, cartes, trajet/date/capacité/tarif, détail et modification, menu d'actions, états vide/chargement/erreur. Ne doit afficher que les annonces de l'utilisateur connecté. |
| Mes colis | Gérer les annonces de colis publiées | En-tête avec retour, filtres par statut, cartes, catégorie/trajet/date/poids/budget, photo miniature si vraie photo, actions Voir/Modifier/menu, gestion des annonces terminées ou expirées selon possibilités système. |
| Mes favoris | Retrouver les annonces enregistrées | Filtres Tous / Voyages / Colis, cartes compactes, action ouvrir, cœur pour retirer, état vide avec accès à la recherche. Ce sont les annonces **d'autres** utilisateurs. Ne pas proposer de modifier ou supprimer les annonces d'autrui. |
| Transactions en cours | Opérations actives | Type d'opération, statut réel, trajet, autre participant, poids ou capacité, montant si disponible, prochaine action pertinente, progression des étapes si le workflow la prend en charge. Les transactions terminées ou annulées sont exclues. À ne pas confondre avec le portefeuille. |
| Historique des transactions | Opérations passées | Distinction terminées / annulées et autres états historiques réellement pris en charge ; type, statut, trajet, date, autre participant, montant et référence si disponibles, bouton « Voir le détail ». Les opérations toujours actives sont exclues. |
| Vérification du profil | Statut de vérification et poursuite des étapes | Distinguer, si le projet les prend en charge : profil non vérifié, vérification en cours, profil vérifié, action requise. Ne pas inventer de contrôle biométrique, validation par SMS ou vérification supplémentaire. |
| Informations personnelles | Consulter et modifier le compte | Photo, prénom/nom, nom affiché si prévu, date de naissance si prévue, e-mail, autres coordonnées facultatives uniquement si elles existent dans le modèle, statut de vérification en lecture seule si pertinent, bouton d'enregistrement. Données préremplies depuis le compte réel. Ne pas inventer de champs obligatoires ni de processus de validation. |

Autres pages possibles citées comme simple référence fonctionnelle (présence à confirmer dans les fichiers) : Notifications, Code QR, Portefeuille, Détail d'une annonce, Détail d'une transaction, Profil public, Paramètres, Documents et consentements, Pages légales.

## 11. Documents légaux et consentements

Catégories prévues :

1. Conditions générales d'utilisation (CGU).
2. Conditions générales de prestation ou de partenariat, selon le modèle retenu.
3. Politique de confidentialité.
4. Politique d'assurance des colis.
5. Politique de transport aérien collaboratif.
6. Politique de remboursement et d'annulation.
7. Charte de confiance et de sécurité.
8. Politique relative aux cookies.
9. Mentions légales.

Parcours envisagé : acceptation claire des documents essentiels à l'inscription, puis documents accessibles depuis le profil via une rubrique « Documents et consentements ». Des confirmations contextuelles peuvent être prévues pour des actions le nécessitant (publier une annonce, payer, annuler). Il ne faut pas imposer neuf cases à cocher systématiques sur l'écran d'inscription.

Le contenu juridique définitif et les exigences réglementaires doivent être validés séparément.

## 12. Technologies évoquées

- React 19
- TypeScript
- Vite
- Tailwind CSS
- Firebase
- Capacitor 8

Les écrans HTML/Tailwind peuvent être des maquettes autonomes, des prototypes ou des représentations de pages. Leur présence ne signifie pas que la logique fonctionnelle est implémentée.

Points d'analyse à effectuer sur les fichiers : composants et dépendances, données fictives, distinction entre interactions visuelles et fonctionnalités connectées, liens de navigation et routes, éviter de confondre une maquette HTML et un écran React fonctionnel, ne pas déclarer une fonctionnalité opérationnelle sans preuve dans le code.

Si les fichiers montrent une architecture différente de celle décrite, **signaler la différence** au lieu de modifier silencieusement les faits.

## 13. Règles de conception reprises

**Ne pas inventer :** champs de données, statuts, boutons, règles métier, montants, vérifications, fonctionnalités backend, routes de navigation, processus de paiement. Information manquante → la signaler comme inconnue ou point à confirmer.

**Préserver l'existant :** en cas de réorganisation ou d'analyse, préserver contenu et sens d'origine ; ne pas réécrire arbitrairement fonctionnalités, noms de composants ou textes métier. Toute amélioration se propose séparément, avec justification.

**Cohérence :** pages d'une même catégorie partageant palette, typographie, composants, espacements, conventions de navigation, styles de boutons, conventions d'affichage des statuts.

**Priorité mobile :** ne pas agrandir artificiellement les composants, éviter les grandes zones décoratives, rester utilisable sur un téléphone de largeur réduite.