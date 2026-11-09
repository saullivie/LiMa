# Règles de calcul (phases C et D)

Statut : brouillon complet. Les valeurs chiffrées sont des **valeurs initiales proposées**, toutes modifiables par l'utilisateur et à ajuster à l'usage. Les exemples sont fictifs.

## Principes

- Les calculs sont **déterministes et faits par du code**. L'IA ne calcule jamais une date.
- Les échéances sont recalculées dès qu'une donnée source change (réalisation, relevé de compteur, modification d'activité).
- Chaque échéance, rappel ou proposition **garde la règle appliquée et ses données d'entrée**, pour pouvoir expliquer « pourquoi ce rappel ».
- Le système **propose**, l'utilisateur **décide** (accepter, refuser, reporter).

## 1. Génération des échéances

### Trois modes de fréquence

| Mode | Règle |
| --- | --- |
| Par durée | Prochaine date prévue = point d'ancrage + période |
| Par usage | Prochaine échéance quand le compteur atteint le seuil ; la date est estimée (voir plus bas) |
| Par saison | Fenêtre de dates récurrente : date prévue = début de la fenêtre, date limite = fin de la fenêtre |

Une activité peut combiner durée et usage : l'échéance tombe **au premier des deux** (exemple : tous les 10 000 km ou tous les 12 mois).

### Point d'ancrage (mode durée)

Chaque activité précise son ancrage :

- **Depuis la dernière réalisation** : la période repart de la date réelle de réalisation (entretien, révision).
- **Date fixe du calendrier** : la date ne bouge pas, même si l'activité est faite en avance ou en retard (renouvellement annuel au 1er janvier, échéance légale).

### Mode usage : estimation de la date

- Le rythme d'usage est la moyenne calculée sur les relevés de compteur des 12 derniers mois (ou sur tous les relevés si moins d'un an).
- Date estimée = date du dernier relevé + (seuil − compteur actuel) ÷ rythme d'usage moyen.
- La date estimée est recalculée à chaque nouveau relevé.
- Sans relevé depuis 3 mois, le système propose un rappel « relever le compteur ».

### Cycle de vie d'une échéance

1. **À venir** : générée à l'avance, dans un horizon de 12 mois (l'occurrence suivante n'est créée qu'à la réalisation de la précédente, sauf mode date fixe).
2. **Proposée** : le rappel initial a été envoyé.
3. **Planifiée** : un rendez-vous ou une date de réalisation est fixé.
4. **Faite** : l'utilisateur confirme ; la date de réalisation et le compteur sont enregistrés ; l'occurrence suivante est calculée.
5. **Reportée** : la date prévue est décalée ; la **date limite ne bouge pas** ; un report au-delà de la date limite est refusé sans confirmation explicite.
6. **En retard** : passage automatique quand la date limite (ou, à défaut, la date prévue) est dépassée.
7. **Annulée** : l'utilisateur renonce à cette occurrence.

Une activité n'a qu'**une échéance ouverte à la fois** par ressource concernée.

## 2. Rappels et urgence

### Paliers de rappel

| Palier | Moment |
| --- | --- |
| Rappel initial | Date prévue − délai de rappel de l'activité (valeur initiale : 30 jours) |
| Relance | À mi-chemin entre le rappel initial et la date prévue, si l'échéance n'est pas planifiée |
| Dernier rappel | 3 jours avant la date limite (ou la date prévue s'il n'y a pas de date limite) |

Une échéance **planifiée** ne reçoit plus de relances, seulement un rappel la veille du rendez-vous.

### Niveau d'urgence

Une échéance est **urgente** si au moins une condition est vraie :

- sa date limite est **légale ou contractuelle** (contrôle, assurance, résiliation) et à 7 jours ou moins ;
- elle est **en retard** et son activité est une obligation légale ou contractuelle ;
- un consommable est **épuisé** (quantité à zéro).

Sinon elle est **normale**.

### Canaux

- **Normale** : incluse dans le résumé du jour, avec au plus une notification groupée par jour.
- **Urgente** : notification immédiate, répétée toutes les 4 heures (hors plages calmes) jusqu'à un accusé (fait, reporter, ignorer).
- **Plages calmes** : de 21 h à 7 h, aucune notification normale ; les notifications urgentes restent possibles.

### Résumé du jour

Envoyé le matin (valeur initiale : 7 h 30). Contenu, dans l'ordre :

1. échéances urgentes et en retard ;
2. échéances du jour et de la semaine ;
3. propositions de regroupement ;
4. stocks sous le seuil ;
5. relevés ou informations manquantes.

Un résumé vide n'est pas envoyé, sauf si l'utilisateur le demande.

## 3. Regroupement

### Fenêtre de réalisation

Pour chaque échéance ouverte, la fenêtre est :

[date prévue − tolérance d'avance ; date limite]

La **tolérance d'avance** est propre à chaque activité (valeur initiale : égale au délai de rappel). Une échéance n'est jamais avancée au-delà de sa tolérance, et jamais repoussée au-delà de sa date limite.

### Critères de regroupement

Deux échéances (ou plus) sont candidates à un passage commun si :

1. leurs fenêtres de réalisation **se chevauchent** ;
2. elles partagent **le même acteur**, ou **le même lieu**, ou des lieux distants de moins de 15 km (valeur initiale) ;
3. aucune dépendance ne l'interdit (une activité qui dépend d'une autre ne passe pas avant elle) ;
4. une date commune existe dans l'intersection des fenêtres et dans les disponibilités de l'acteur.

### Évaluation du bénéfice

Une proposition n'est faite que si le gain est démontrable :

- **distance évitée** = somme des trajets séparés − trajet du passage commun (valeur approchée à partir des distances au domicile) ;
- **passages évités** = nombre d'échéances regroupées − 1.

Seuil de proposition (valeur initiale) : au moins un passage évité et au moins 10 km économisés, ou au moins deux passages évités. Le coût en complexité reste faible : un regroupement n'est qu'une date commune proposée.

### Cycle d'un regroupement

- **Proposé** : visible dans le résumé du jour avec une date suggérée.
- **Accepté** : les échéances concernées passent en « planifiées » à cette date.
- **Refusé** : la paire d'échéances n'est plus proposée pour cette occurrence.
- **Fait** : à la réalisation, les échéances sont marquées faites ensemble.

### Autres regroupements

- **Par saison** : les activités d'une même fenêtre saisonnière et d'un même acteur sont proposées ensemble, avant le début de la saison d'usage.
- **Achats** : les réapprovisionnements d'un même fournisseur dont les fenêtres se chevauchent sont proposés en commande groupée.

## 4. Stocks et consommables

- Quand la quantité passe **au seuil ou en dessous**, une échéance de réapprovisionnement est créée (urgence normale ; urgente à zéro).
- **Consommation moyenne** : calculée à partir des achats et des relevés de stock des 12 derniers mois.
- **Date de rupture estimée** = date du dernier relevé + quantité restante ÷ consommation moyenne.
- **Seuil proposé** = consommation moyenne × délai de livraison habituel + marge de sécurité (valeur initiale : 20 %). Le système propose d'ajuster le seuil ; l'utilisateur valide.
- **Historique des commandes** : déduit des transactions d'achat rattachées à la ressource.

## 5. Contrats et abonnements

- **Date limite de résiliation** = date de fin − préavis, pour un contrat à tacite reconduction.
- Rappels dédiés : rappel initial 30 jours avant la date limite de résiliation, dernier rappel 7 jours avant, **urgent** à 7 jours ou moins.
- À l'approche de la date limite, le résumé propose de **revoir le contrat** (coût annuel cumulé d'après les transactions, comparaison avec d'autres offres).
- Un contrat sans tacite reconduction génère une échéance de renouvellement à la date de fin.

## 6. Documents

- Une **date de fin de validité** génère une échéance de renouvellement.
- Une **durée de conservation** dépassée génère une proposition de suppression : **jamais de suppression automatique**.
- Les données extraites par l'IA restent **proposées** jusqu'à validation humaine ; aucun calcul d'échéance ne repose sur une donnée non validée.

## 7. Exceptions et imprévus

- **Échéance ponctuelle** : créée à la main, sans activité d'origine (événement exceptionnel).
- **Bien vendu, en panne ou hors service** : ses activités sont suspendues ; leurs échéances ouvertes sont proposées à l'annulation. En panne, une activité de réparation peut être proposée.
- **Hors saison** : une activité saisonnière ne génère rien hors de sa fenêtre.
- **Imprévu** : un événement exceptionnel peut proposer de **reporter** d'autres échéances, sans jamais dépasser leur date limite.
- **Information manquante** (date de dernière réalisation, compteur) : l'échéance est créée avec le statut « à compléter » et une action associée, au lieu de rester silencieuse.

## 8. Paramètres modifiables

| Paramètre | Valeur initiale |
| --- | --- |
| Délai de rappel d'une activité | 30 jours |
| Relance | À mi-chemin |
| Dernier rappel | 3 jours avant la date limite |
| Seuil d'urgence légale ou contractuelle | 7 jours |
| Répétition des notifications urgentes | Toutes les 4 heures |
| Plages calmes | 21 h à 7 h |
| Heure du résumé du jour | 7 h 30 |
| Horizon de génération | 12 mois |
| Tolérance d'avance | Égale au délai de rappel |
| Distance de regroupement par lieu proche | 15 km |
| Seuil de proposition d'un regroupement | 1 passage et 10 km évités, ou 2 passages |
| Marge de sécurité des stocks | 20 % |
| Relevé de compteur manquant | Rappel après 3 mois |

## 9. Ce que fait l'IA, et ce qu'elle ne fait pas

| L'IA fait | Le code fait |
| --- | --- |
| Extraire des informations d'une photo ou d'un texte | Calculer toute date d'échéance |
| Rédiger les rappels et le résumé du jour | Décider du niveau d'urgence |
| Répondre aux questions en langage naturel | Détecter les regroupements et évaluer leur gain |
| Expliquer pourquoi une échéance existe | Appliquer les statuts et les paliers de rappel |
