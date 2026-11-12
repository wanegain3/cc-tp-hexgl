# HexGL Constitution

## Core Principles

### I. Préserver le gameplay et les comportements existants

Le jeu DOIT conserver son gameplay, ses contrôles, ses règles de course (compte à rebours,
tours, checkpoints, fin de course) et ses comportements observables, sauf si le ticket demande
explicitement de les modifier. Tout changement de comportement DOIT être décrit dans le ticket
et validé par un humain. Les tests NE DOIVENT PAS figer le branding (logo, couleurs, textes)
destiné à être modifié.

### II. Conserver Three.js et CoffeeScript sans modernisation

Le projet DOIT continuer à utiliser la version de Three.js embarquée et le CoffeeScript
précompilé. Il NE DOIT PAS migrer vers une version récente de Three.js, vers les modules ES, ni
réécrire le code en JavaScript moderne, sauf ticket dédié. Les sorties `.js` versionnées DOIVENT
rester cohérentes avec leurs sources `.coffee`.

### III. Modifications minimales et réversibles

Chaque changement DOIT être le plus petit possible, limité au périmètre du ticket, et DOIT
pouvoir être annulé sans effet de bord. Les refactorings non demandés NE DOIVENT PAS être
mélangés à une correction ou à une fonctionnalité. Aucun code non demandé NE DOIT être ajouté.

### IV. Respecter l'architecture existante et documenter les décisions

Le projet DOIT respecter l'architecture décrite dans `docs/architecture.md` : espace de noms
global `bkcore`, ordre des balises `<script>`, répartition des modules. Toute décision importante
(nouveau fichier, changement de structure, dérogation à un principe) DEVRAIT être consignée dans
`docs/`. Les anomalies recensées dans `docs/architecture.md` NE DOIVENT PAS être corrigées dans
un ticket sans rapport avec elles.

### V. Tests QUnit obligatoires

Tout changement de code applicatif DOIT être accompagné de tests, ou l'absence de tests DOIT être
justifiée dans le ticket. Les tests DOIVENT réutiliser la suite QUnit de `formation/tests/` :
ajout du `<script>` dans `index.html` et des tests dans `browser-tests.js`. Aucun nouveau
framework de test NE DOIT être introduit. Les tests NE DOIVENT NI lancer le jeu, ni dépendre de
WebGL.

### VI. Validation humaine des changements importants

Un changement important (gameplay, rendu, physique, chargement des ressources, structure des
fichiers) NE DOIT PAS être considéré comme terminé sans validation humaine explicite. Les tests
automatisés ne remplacent pas cette validation.

## Contraintes techniques et périmètre

- Aucune dépendance ajoutée : le projet NE DOIT PAS introduire de bibliothèque, de
  `package.json`, de gestionnaire de paquets, de bundler, d'étape de build ni de backend.
- Le jeu reste une application statique : aucun nouveau service externe NE DOIT être ajouté.
- Le dossier `../etapes/` (corrigés de la formation, hors `HexGL/`) NE DOIT NI être utilisé, ni
  être consulté.
- Les ressources (`textures/`, `textures.full/`, `geometries/`, `audio/`) NE DOIVENT PAS être
  modifiées sans demande explicite.
- Les documents de gouvernance, de spécification et de planification DOIVENT être rédigés en
  français. Les identifiants techniques (noms de fichiers, de fonctions, d'API) restent en anglais.

## Processus de vérification

- Les tests QUnit DOIVENT être exécutés après chaque phase significative et en fin de ticket.
- Le rendu et les interactions DOIVENT être vérifiés en fin de ticket. Playwright DEVRAIT être
  utilisé lorsqu'il est disponible, sans répétitions inutiles.
- Les captures de `formation/tests/captures/` DOIVENT servir de référence uniquement pour les
  éléments à préserver. Les changements visuels voulus DOIVENT être validés selon le ticket.
- Une course complète NE DOIT être jouée que si le changement risque d'affecter le gameplay.
- Toute vérification impossible DOIT être signalée explicitement. Elle NE DOIT PAS être
  considérée comme réussie.

## Governance

- Cette constitution prime sur les autres pratiques. Chaque ticket et chaque plan DOIVENT être
  vérifiés pour leur conformité aux principes ci-dessus.
- Un amendement DOIT être proposé par écrit avec sa justification, validé par un humain, et
  reporté dans ce document. Les éléments impactés (tickets, documentation, tests) DOIVENT être
  listés avant l'amendement.
- Versionnage sémantique : MAJOR pour la suppression ou la redéfinition incompatible d'un
  principe ; MINOR pour l'ajout d'un principe ou une extension substantielle ; PATCH pour une
  clarification ou une correction de forme.
- Chaque ticket DEVRAIT indiquer sa conformité à chaque principe. Une revue humaine DOIT précéder
  l'intégration des changements importants.
- Pour les consignes d'exécution quotidiennes, se référer à `CLAUDE.md` et à `docs/architecture.md`.

**Version**: 1.0.0 | **Ratified**: 2026-11-11 | **Last Amended**: 2026-11-11
