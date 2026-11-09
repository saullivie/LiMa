# Cadrage

## Utilisateurs

Le système doit pouvoir être réutilisé par n'importe qui, dans n'importe quel contexte : un ou plusieurs utilisateurs, quelle que soit leur situation. Plusieurs utilisateurs peuvent partager un même espace de données commun.

## Objectifs

- Réduire la charge mentale.
- Éviter les oublis.
- Optimiser les coûts.

## Usages attendus

- **Rappels** d'échéances et d'actions.
- **Résumé du jour**.
- **Réponses à des questions** (par exemple « quand est le prochain contrôle technique ? »).
- **Saisie** par photo et par texte.

## Rôle de l'IA

Elle **rappelle et propose**. Elle n'envoie aucun message et ne prend aucun rendez-vous seule. Chaque proposition appelle une décision humaine (accepter, refuser, reporter).

## Données et confidentialité

- Ce dépôt ne contient aucune donnée personnelle, seulement le système de traitement.
- Les données ne doivent résider que sur des machines maîtrisées par l'utilisateur.
- Le traitement est assuré par une IA locale.

## Contraintes et infrastructure

- Un ordinateur local allumé en permanence héberge l'IA (matériel grand public).
- Temps de maintenance le plus faible possible, avec un fonctionnement fiable.

## Interface

Application web installable (PWA), avec notifications, compatible avec des téléphones Android de plusieurs générations.

Besoins : chiffrement des échanges, notifications urgentes.

## Périmètre de la V1

Une boucle complète, appliquée au bloc **mobilité** et aux **archives** liées :

1. Photo ou texte envoyé depuis le smartphone.
2. Extraction des informations par l'IA locale, création ou mise à jour d'une fiche.
3. Calcul des échéances et envoi de rappels.
4. Résumé du jour.
5. Questions en langage naturel.

Les autres blocs s'ajoutent ensuite sans changer l'architecture.

## Points encore ouverts

- Sauvegardes hors de la machine locale : comment respecter la règle « aucune donnée sur un serveur non maîtrisé » (par exemple chiffrement avant envoi, clé conservée par l'utilisateur) ?
- Accès distant sécurisé depuis le smartphone.
- Fiabilité : que se passe-t-il si la machine locale tombe en panne ? Mode dégradé, sauvegardes testées.
- Critères de réussite mesurables de la V1.
- Mise en œuvre des notifications urgentes (niveaux de priorité) et du chiffrement.
- Compatibilité du navigateur avec d'anciennes versions d'Android.
