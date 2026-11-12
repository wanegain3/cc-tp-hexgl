# Implementation Plan: Personnalisation événementielle ACME

**Branch**: `001-company-event-acme` | **Date**: 2026-11-12 | **Spec**: [spec.md](./spec.md)

**Input**: Feature specification from `/specs/001-company-event-acme/spec.md`

**Note**: This template is filled in by the `/speckit-plan` command; its definition describes the execution workflow.

## Summary

Présenter l'identité ACME sur l'écran d'accueil de HexGL sans toucher au gameplay : logo officiel, message « Bienvenue au ACME Racing Challenge », bouton « Démarrer la course », fond sombre aux accents de la charte, mention ACME dans les crédits, suivi analytics désactivé, et mise en page utilisable à 360 px de large. L'approche reste statique et minimale : une image ajoutée, un fichier HTML modifié (`index.html`), aucun fichier JavaScript modifié. Chaque changement est annulable par un retour Git.

## Technical Context

<!--
  ACTION REQUIRED: Replace the content in this section with the technical details
  for the project. The structure here is presented in advisory capacity to guide
  the iteration process.
-->

**Language/Version**: HTML5 / CSS3 pour les modifications ; JavaScript ES5 global et CoffeeScript 1.x précompilé inchangés

**Primary Dependencies**: Three.js ancien (`libs/Three.dev.js`), inchangé. Aucune dépendance ajoutée. Polices BebasNeue locales, inchangées.

**Storage**: N/A (aucune donnée persistée par la personnalisation)

**Testing**: Suite QUnit existante de `formation/tests/`, exécutée après la Phase 2 (retrait du suivi), après US1 (changement visuel) et en fin de ticket. Une phase est jugée significative si elle modifie l'écran d'accueil ou le gameplay : US3 ne modifie que le bloc des crédits, couvert par l'exécution de fin de ticket, et US2 ne modifie aucun fichier. Vérifications navigateur regroupées dans une seule session en fin de ticket ; Playwright si disponible, sinon signalé comme non réalisé.

**Target Platform**: Navigateur avec WebGL, bureau et mobile (largeur minimale 360 px)

**Project Type**: Application web statique (jeu), sans build ni backend

**Performance Goals**: Aucune cible nouvelle. L'ajout d'une image de logo (environ 364 Ko) est la seule charge supplémentaire de l'écran d'accueil.

**Constraints**: Gameplay inchangé ; aucune nouvelle dépendance, build ni backend ; Three.js et CoffeeScript conservés ; modifications minimales et réversibles ; aucun service externe ajouté.

**Scale/Scope**: Une page (`index.html`), un écran d'accueil et un écran de crédits ; aucun fichier de la boucle de jeu touché.

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

| Principe | Statut | Justification |
|---|---|---|
| I. Préserver le gameplay et les comportements existants | Conforme | Aucun fichier de `bkcore/`, `bkcore.coffee/` ni `launch.js` n'est modifié. Les modifications sont limitées à l'écran d'accueil et aux crédits. |
| II. Conserver Three.js et CoffeeScript sans modernisation | Conforme | Aucune bibliothèque ni source `.coffee` n'est modifiée. |
| III. Modifications minimales et réversibles | Conforme | Un fichier HTML modifié, une image ajoutée, aucun fichier supprimé. Chaque changement s'annule par un retour Git. |
| IV. Respecter l'architecture existante et documenter les décisions | Conforme, écart justifié | Le plan respecte l'architecture décrite dans `docs/architecture.md` (scripts globaux, styles de `index.html`). Les décisions sont consignées dans `research.md`, dans le dossier du ticket. Écart assumé avec « docs/ » : `docs/architecture.md` décrit l'état actuel et ne doit pas être modifié par un ticket (T018) ; aucune note n'est ajoutée à `docs/` pour ne pas élargir le périmètre. |
| V. Tests QUnit obligatoires | Justifié | Aucun code JavaScript n'est modifié, donc aucun nouveau test unitaire n'est pertinent. La suite QUnit existante est exécutée après la Phase 2, après US1 et en fin de ticket (processus de vérification de la constitution). Aucun test ne fige le branding. |
| VI. Validation humaine des changements importants | Validé pour les décisions | Les quatre décisions de rendu ont été validées par un humain le 2026-11-12 (voir `research.md`, section « Décisions validées »). La vérification visuelle du guide `quickstart.md` reste à réaliser avant intégration. |
| Contraintes techniques : aucune dépendance, build, backend | Conforme | Aucun `package.json`, aucun service externe nouveau. |
| Contraintes : `../etapes/` non consulté | Conforme | Aucune lecture de ce dossier pendant la planification. |

**Résultat** : aucune violation non justifiée. Le principe VI est satisfait pour les décisions ; la vérification visuelle reste un gate avant intégration.

## Project Structure

### Documentation (this feature)

```text
specs/001-company-event-acme/
├── plan.md              # Ce fichier (sortie de /speckit-plan)
├── research.md          # Décisions techniques, validées par un humain
├── data-model.md        # Valeurs de l'identité événementielle
├── quickstart.md        # Scénarios de validation
├── checklists/
│   └── requirements.md  # Checklist de qualité de la spécification
└── tasks.md             # Sortie de /speckit-tasks (non créée ici)
```

Aucun dossier `contracts/` n'est créé : le jeu n'expose aucune interface externe (ni API, ni format d'échange). Le contrat existant est la structure de `index.html`, documentée dans `docs/architecture.md`.

### Source Code (repository root)

```text
HexGL/
├── index.html                 # MODIFIÉ : écran d'accueil, bouton, message, crédits, suivi analytics retiré, styles ACME
├── css/
│   ├── acme-logo.png          # AJOUTÉ : copie du logo officiel (formation/identite/logo.png), sans modification
│   ├── title.png              # INCHANGÉ (ancienne identité HexGL, plus référencée par l'accueil)
│   ├── bg.jpg                 # INCHANGÉ (fond, recouvert par une couche sombre)
│   └── multi.css              # INCHANGÉ
├── launch.js / launch.coffee  # INCHANGÉS (le bouton conserve l'id `start`)
├── bkcore/                    # INCHANGÉ
├── bkcore.coffee/             # INCHANGÉ
├── libs/                      # INCHANGÉ
└── formation/tests/           # INCHANGÉ (suite QUnit de régression)
```

**Structure Decision** : Modifications localisées à `index.html` (markup, styles en bloc `<style>`, suppression du bloc analytics, ajout des crédits) et à un asset statique ajouté. Ce choix suit le pattern existant : la page contient déjà ses styles et ses scripts en ligne. Aucun fichier JavaScript n'est touché, ce qui protège le gameplay.

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

Aucune violation à justifier.

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| — | — | — |
