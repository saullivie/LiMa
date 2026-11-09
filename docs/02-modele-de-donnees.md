# Modèle de données (phase C) — squelette général

Statut : brouillon. Les exemples sont fictifs. Aucun champ n'est encore défini.

## Principes

- Une information n'est saisie qu'une seule fois.
- Trois catégories : données permanentes, processus récurrents, événements ponctuels.
- Les échéances sont calculées par du code à partir des activités.

## Les 7 types d'éléments

### Données permanentes

| Type | Définition | Exemples fictifs |
| --- | --- | --- |
| Acteur | Personne ou organisation | un utilisateur, un garage, une administration |
| Lieu | Endroit | un domicile, un atelier |
| Ressource | Objet ou stock | un véhicule, un outil, un consommable |

### Processus récurrents

| Type | Définition | Exemples fictifs |
| --- | --- | --- |
| Activité | Action qui revient, avec fréquence, dépendances et possibilités de regroupement | entretien annuel, renouvellement d'assurance |

### Événements ponctuels

| Type | Définition | Exemples fictifs |
| --- | --- | --- |
| Échéance | Occurrence datée d'une activité | contrôle technique avant une date donnée |
| Transaction | Dépense ou paiement | une facture réglée |
| Document | Fichier rattaché aux autres éléments | facture, contrat, photo |

## Rôles des acteurs

Un acteur porte un ou plusieurs **rôles** plutôt que d'avoir un type distinct par rôle :

- utilisateur
- prestataire (rend un service : entretien, réparation, soin)
- fournisseur (vend des biens : matériel, consommables)
- administration
- assureur
- ami, famille

Un même acteur peut cumuler plusieurs rôles. Les listes « prestataires » et « fournisseurs » de l'application sont obtenues par filtre sur le rôle.

## Relations principales

- Une activité concerne une ou plusieurs ressources et peut impliquer un acteur (prestataire ou fournisseur).
- Une échéance découle d'une activité et se déroule dans un lieu.
- Une transaction se rattache à un acteur et, le cas échéant, à une échéance ou une ressource.
- Un document se rattache à un ou plusieurs éléments des autres types.

## Champs du type Acteur

| Champ | Description |
| --- | --- |
| Type | Personne ou organisation |
| Nom, prénom | Prénom pour une personne seulement |
| Rattachement | Pour une personne : lien vers l'organisation (autre acteur) |
| Rôles | Un ou plusieurs (voir plus haut) |
| Téléphones, mails | Plusieurs valeurs possibles |
| Adresse | Lien vers un Lieu |
| Qualité pro | Note de 1 à 5, pour prestataires, fournisseurs et organisations ; l'historique des évaluations pourra venir plus tard |
| Affinité | Note subjective sur la relation (échelle à définir) |
| Disponibilités | Horaires ou jours de disponibilité, pour planifier les rendez-vous |
| Notes | Texte libre |

**Valeur calculée, non saisie :** la distance au domicile, déduite de l'adresse (Lieu) de l'acteur et du Lieu « domicile ». Elle sert notamment à regrouper les déplacements.

**Visibilité :** les données d'un espace sont communes ; qualité pro et affinité sont visibles par tous ses utilisateurs.

## Champs du type Lieu

| Champ | Description |
| --- | --- |
| Nom | Par exemple « atelier », « garage » |
| Type | Domicile, commerce, administration, terrain, etc. |
| Adresse | Adresse postale |
| Coordonnées GPS | Pour calculer les distances |
| Horaires d'ouverture | Jours et plages horaires |
| Accès | Parking, contraintes, remarques pratiques |
| Notes | Texte libre |

Un lieu peut être marqué comme **domicile** : c'est le point de départ des calculs de distance.

## À définir ensuite

- Les champs des autres types (ressource, activité, échéance, transaction, document).
- Le détail des relations (cardinalités).
- Les règles de calcul des échéances et de regroupement des activités.
