# LiMa (Life Manager) — Système de gestion de vie

Système de traitement pour gérer les aspects pratiques d'une vie comme un système cohérent : obligations, échéances, entretien, documents, achats, déplacements. Conçu pour être réutilisable par n'importe qui, dans n'importe quel contexte.

Une IA locale traite les données ; l'accès se fait depuis smartphone via une application web installable (PWA).

## Aucune donnée dans ce dépôt

Ce dépôt contient uniquement le **système de traitement** : conception, code, configuration générique. Les données personnelles (fiches, documents, photos, échéances, contrats) n'y figurent jamais. Elles restent sur des machines maîtrisées par l'utilisateur. Le fichier `.gitignore` exclut les dossiers et formats de données.

## État du projet

Phase actuelle : **conception** (phases A à C du prompt maître : inventaire, classification, modélisation). Aucun code pour l'instant.

## Principes

- L'IA **rappelle et propose** ; la décision reste humaine.
- Les données vivent dans une vraie base ; l'IA n'est pas la base de données.
- Les échéances sont calculées par du code, pas par l'IA.
- Ne pas complexifier sans bénéfice démontrable.
- Minimiser le temps de maintenance du système lui-même.

## Organisation du dépôt

| Fichier | Contenu |
| --- | --- |
| `docs/prompt-maitre.md` | Prompt maître d'origine (méthode en phases A à G) |
| `docs/00-cadrage.md` | Cadrage : usages, données et confidentialité, périmètre V1 |
| `docs/01-inventaire.md` | Domaines génériques et regroupement en blocs |
| `docs/02-modele-de-donnees.md` | Modèle de données : types d'éléments, rôles, relations |
| `docs/decisions.md` | Journal des décisions |

## Prochaine étape

Modèle de données : squelette général (personnes, lieux, ressources, échéances, documents, transactions), puis fiche type d'un véhicule.
