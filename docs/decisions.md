# Journal des décisions

## 2026-11-09

- Aucune donnée personnelle dans le dépôt : il ne contient que le système de traitement. Les données ne doivent résider que sur des machines maîtrisées par l'utilisateur.
- Objectif : le système doit être réutilisable par n'importe qui, dans n'importe quel contexte.
- L'IA rappelle et propose ; la décision reste humaine.
- Une vraie base de données stocke les données ; du code calcule les échéances ; l'IA sert d'interface (extraction, questions, résumés).
- Interface : application web installable (PWA) en V1 ; une application native sera envisagée seulement si les limites de la PWA gênent.
- Périmètre de la V1 : bloc mobilité et archives liées, avec la boucle complète (saisie, extraction, échéances, rappels, résumé du jour, questions).
- Regroupement des 13 domaines en 5 blocs validé.
- Le projet est construit dans ce dépôt GitHub privé.
- Modèle de données : 7 types d'éléments (acteur, lieu, ressource, activité, échéance, transaction, document).
- Prestataire et fournisseur sont des rôles du type Acteur, et non des types séparés.
- Ressources : distinction entre biens suivis individuellement et consommables en stock ; l'historique des commandes est déduit des transactions d'achat, pas saisi.
- Relations : tout élément appartient à un espace ; le regroupement est un objet auxiliaire ; une échéance peut exister sans activité (événement ponctuel).
- Règles de calcul : calculs faits par du code, jamais par l'IA ; valeurs initiales modifiables (voir `03-regles-de-calcul.md`).
- Échéances par durée, par usage ou par saison, combinables « au premier des deux » ; une seule échéance ouverte à la fois par activité et par ressource.
- Le regroupement n'est proposé que si le gain est démontrable ; un regroupement refusé n'est pas reproposé.
- Aucune suppression automatique de document.
- Architecture technique (voir `04-architecture-technique.md`) : SQLite, backend local, PWA, serveur d'inférence local sans accès sortant, WireGuard direct pour l'accès distant, notifications par un serveur auto-hébergé, sauvegardes chiffrées côté client avec test de restauration avant toute donnée réelle.
- Choix techniques 1 à 4 retenus (SQLite, WireGuard direct, ntfy auto-hébergé, une seule machine en V1) ; choix 5 (modèles d'IA) à trancher par test.
- L'IA n'écrit jamais directement dans la base : elle propose, l'utilisateur valide. Le contenu d'un document n'est jamais une instruction.
- Les jalons 0 à 4 (système qui rappelle) passent avant les fonctions d'IA (documents, questions, regroupements).
- Contrats et abonnements : pas de type séparé ; un document de type contrat, une activité de renouvellement et des transactions.
- Les données extraites par l'IA restent « proposées » jusqu'à validation par un utilisateur.
- Le moyen de paiement ne stocke jamais de numéro de carte ni de compte.
- Champs de l'Acteur définis : l'entreprise est une organisation à laquelle on se rattache, l'adresse renvoie à un Lieu, la distance au domicile est calculée et non saisie.
