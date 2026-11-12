# Modèle de données : Identité événementielle ACME

**Feature**: `001-company-event-acme` | **Date**: 2026-11-12

Cette fonctionnalité ne persiste aucune donnée et n'a pas de base. Le « modèle » ci-dessous décrit les valeurs de configuration de l'identité, telles qu'elles seront écrites dans `index.html` et ses styles.

## Entité : Identité événementielle

Ensemble des valeurs d'habillage de l'écran d'accueil et des crédits. Elle ne contient aucune donnée de course.

| Champ | Type | Valeur retenue | Source |
|---|---|---|---|
| `logo` | fichier image | `css/acme-logo.png`, copie non modifiée de `formation/identite/logo.png` | FR-001 |
| `logo.alt` | texte | « ACME » | accessibilité, R2 |
| `message_accueil` | texte | « Bienvenue au ACME Racing Challenge » | FR-002, Q3 |
| `libelle_demarrage` | texte | « Démarrer la course » | FR-004, Q3 |
| `palette.fond` | couleur | `#08090D` (couche sombre sur le fond) | FR-003, R1 |
| `palette.violet` | couleur | `#7628FF` | FR-003, R6 |
| `palette.bleu` | couleur | `#315BFF` | FR-003, R6 |
| `palette.cyan` | couleur | `#00C4E8` | FR-003, R6 |
| `palette.blanc` | couleur | `#FFFFFF` | FR-003, R6 |
| `credits.mention_acme` | texte | Section « Personnalisation événementielle ACME » | FR-012, Q2 |
| `suivi_analytics` | booléen | `désactivé` (bloc de suivi retiré) | FR-011, Q1 |
| `largeur_minimale` | pixels | `360` | FR-009, Q5 |

## Règles de validation

- Le logo DOIT être affiché avec son ratio d'origine : aucune déformation, aucune recoloration (spec, cas limites).
- Les couleurs visibles DOIVENT appartenir à la palette ci-dessus (SC-002).
- Le message et le libellé DOIVENT correspondre exactement aux textes de la spec.
- Le suivi analytics DOIT être absent de la page (FR-011).

## Transitions d'état

Aucune. L'écran d'accueil a le même cycle qu'avant : menu, aide, chargement, jeu. Seul le contenu visuel change.
