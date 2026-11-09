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

## Champs du type Ressource

Deux sortes de ressources : les **biens suivis individuellement** et les **consommables en stock**.

**Champs communs**

| Champ | Description |
| --- | --- |
| Nom | |
| Catégorie | Véhicule, machine, outil, pièce, consommable, etc. |
| Lieu de rangement | Lien vers un Lieu |
| Documents | Documents rattachés (facture, notice, etc.) |
| Notes | Texte libre |

**Biens suivis individuellement** (véhicule, machine, outil)

| Champ | Description |
| --- | --- |
| Marque, modèle | |
| Numéro de série ou d'identification | |
| Date et prix d'achat | |
| Fournisseur | Lien vers un acteur |
| Compteur d'usage | Kilomètres ou heures ; dernier relevé d'un historique (date, valeur) |
| État | En service, en panne, vendu |

**Consommables en stock** (carburant, filtres, pièces)

| Champ | Description |
| --- | --- |
| Quantité en stock, unité | |
| Seuil de réapprovisionnement | Déclenche une proposition de commande |
| Fournisseur habituel | Lien vers un acteur |
| Délai de livraison habituel | Sert à proposer un seuil |
| Relevés de stock | Historique (date, quantité) |

**Historique des commandes :** ce n'est pas un champ saisi. Il est consulté depuis la fiche de la ressource et déduit des transactions d'achat qui lui sont rattachées (date, quantité, prix, fournisseur). Il permet de calculer la consommation moyenne et d'anticiper les réapprovisionnements. Les champs nécessaires (quantité, statut de commande) seront définis avec le type Transaction.

## Champs du type Activité

L'activité est le processus récurrent : c'est elle qui génère les échéances.

| Champ | Description |
| --- | --- |
| Nom | Par exemple « révision annuelle » |
| Catégorie / bloc | Mobilité, maison, administratif, etc. |
| Fréquence | Par durée (tous les 12 mois), par usage (tous les 10 000 km ou toutes les 200 heures) ou par saison |
| Ressources concernées | Liens vers des ressources |
| Acteurs impliqués | Liens vers des acteurs (prestataire, fournisseur) |
| Lieu | Lien vers un Lieu |
| Durée estimée | |
| Coût estimé | |
| Dépendances | Ce qu'il faut faire ou avoir avant |
| Regroupable avec | Autres activités pouvant se faire en même temps |
| Mode de traitement | À faire soi-même, à déléguer ou automatisable |
| Délai de rappel | Combien de temps avant l'échéance prévenir |
| Ancrage | Pour la fréquence par durée : depuis la dernière réalisation, ou date fixe du calendrier |
| Fenêtre saisonnière | Pour la fréquence par saison : début et fin |
| Tolérance d'avance | De combien l'échéance peut être avancée pour un regroupement |
| Obligation | Légale, contractuelle ou libre : détermine l'urgence |

## Champs du type Échéance

L'échéance est une occurrence datée d'une activité, créée automatiquement par le code. Elle alimente les rappels et le résumé du jour.

| Champ | Description |
| --- | --- |
| Activité d'origine | Lien vers une activité |
| Date prévue | |
| Date limite | Par exemple la limite légale d'un contrôle |
| Statut | À venir, proposée, planifiée, faite, reportée, annulée, en retard, à compléter (information manquante) |
| Date de réalisation | |
| Ressource, acteur, lieu | Repris de l'activité, modifiables |
| Rendez-vous pris | Date et heure |
| Urgence | Fixe le niveau de notification (normale ou urgente) |
| Compteur à la réalisation | Kilomètres ou heures, pour recalculer la prochaine échéance quand la fréquence dépend de l'usage |
| Regroupement | Lien vers un passage commun avec d'autres échéances |
| Documents, notes | Justificatif, facture, texte libre |

Le coût réel n'est pas saisi sur l'échéance : il provient d'une transaction liée.

## Champs du type Transaction

Une transaction est une dépense, un achat ou un paiement.

| Champ | Description |
| --- | --- |
| Type | Dépense, achat de consommable (commande), paiement récurrent, remboursement |
| Date | |
| Montant | |
| Acteur concerné | Fournisseur, prestataire, administration |
| Rattachement | Lien vers une échéance, une ressource ou une activité |
| Quantité, unité | Pour les achats de consommables : alimente l'historique des commandes |
| Statut de commande | Commandée, livrée, annulée |
| Catégorie budgétaire | Pour suivre les coûts par domaine |
| Moyen de paiement | Type seulement (carte, virement, espèces) ; **jamais de numéro de carte ni de compte** |
| Documents, notes | Facture, texte libre |

## Champs du type Document

Un document est un fichier rattaché aux autres éléments. Le fichier lui-même est stocké sur la machine locale de l'utilisateur, jamais dans ce dépôt.

| Champ | Description |
| --- | --- |
| Titre | |
| Type | Facture, contrat, attestation, pièce d'identification d'un bien, photo, notice, courrier, etc. |
| Fichier | Référence vers le fichier stocké localement |
| Date du document | Et date d'ajout |
| Éléments liés | Un ou plusieurs acteurs, ressources, activités, échéances ou transactions |
| Date de fin de validité | Pour les documents qui expirent ; génère une échéance |
| Durée de conservation | Pour les archives |
| Texte extrait | Pour la recherche par l'IA |
| Données extraites | Informations lues par l'IA, avec un statut : proposée par l'IA ou validée par un utilisateur |
| Notes | Texte libre |

Les données extraites par l'IA ne sont considérées comme fiables qu'après validation humaine, conformément au principe « l'IA propose, l'humain décide ».

## Contrats et abonnements

Pas de type séparé. Un contrat ou un abonnement se représente ainsi :

- un **Document** de type « contrat », dont les champs supplémentaires sont : acteur cocontractant, date de début, date de fin, tacite reconduction (oui ou non), préavis de résiliation ;
- une **Activité** de renouvellement, qui crée des échéances, dont une **date limite de résiliation** calculée à partir du préavis ;
- des **Transactions** pour les paiements, récurrents ou non.

L'IA remplit ces champs à partir de la photo ou du scan du contrat, et l'utilisateur valide.

## Objets auxiliaires

Deux objets complètent les 7 types sans en être de nouveaux : ils structurent le modèle.

- **Espace** : ensemble de données partagées par un ou plusieurs utilisateurs. Tout élément appartient à exactement un espace. C'est ce qui rend le système réutilisable par n'importe qui : un utilisateur seul a un espace, un foyer en partage un.
- **Regroupement** (passage commun) : ensemble d'échéances réalisées ensemble. Champs : date, acteur, lieu, statut (proposé, accepté, refusé, fait). Voir `03-regles-de-calcul.md`.

## Relations détaillées (cardinalités)

Notation : `0..1` (au plus un), `1` (exactement un), `0..n` (aucun ou plusieurs), `1..n` (au moins un).

| De | Vers | Cardinalité | Remarque |
| --- | --- | --- | --- |
| Tout élément | Espace | 1 | |
| Acteur (personne) | Acteur (organisation) | 0..1 | Une organisation a 0..n personnes |
| Acteur | Rôle | 1..n | |
| Acteur | Lieu (adresse) | 0..n | Une adresse principale ; un lieu peut être l'adresse de 0..n acteurs |
| Ressource | Lieu de rangement | 0..1 | |
| Ressource | Acteur (fournisseur) | 0..1 | Fournisseur habituel ; les autres apparaissent via les transactions |
| Activité | Ressource | 0..n | |
| Activité | Acteur | 0..n | |
| Activité | Lieu | 0..1 | |
| Activité | Activité | 0..n | Deux liens distincts : dépendances et « regroupable avec » |
| Activité | Échéance | 0..n | Une activité génère ses échéances |
| Échéance | Activité | 0..1 | Vide pour une échéance ponctuelle créée à la main (événement exceptionnel) |
| Échéance | Ressource | 0..n | Reprise de l'activité, modifiable |
| Échéance | Acteur | 0..n | Reprise de l'activité, modifiable |
| Échéance | Lieu | 0..1 | Repris de l'activité, modifiable |
| Échéance | Regroupement | 0..1 | Un regroupement contient 1..n échéances |
| Transaction | Acteur | 0..1 | |
| Transaction | Échéance | 0..1 | |
| Transaction | Activité | 0..1 | |
| Transaction | Ressource | 0..1 | V1 : une ressource par transaction d'achat ; des lignes multiples pourront être ajoutées si le besoin apparaît |
| Document | Acteur, Ressource, Activité, Échéance, Transaction | 0..n pour chacun | Relation plusieurs-à-plusieurs : un élément a 0..n documents |

## À définir ensuite

- Schéma de base de données détaillé (types de colonnes, index), à produire au jalon 1 du plan de `04-architecture-technique.md`.
