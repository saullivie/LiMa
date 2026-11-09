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
- Contrats et abonnements : pas de type séparé ; un document de type contrat, une activité de renouvellement et des transactions.
- Les données extraites par l'IA restent « proposées » jusqu'à validation par un utilisateur.
- Le moyen de paiement ne stocke jamais de numéro de carte ni de compte.
- Champs de l'Acteur définis : l'entreprise est une organisation à laquelle on se rattache, l'adresse renvoie à un Lieu, la distance au domicile est calculée et non saisie.
