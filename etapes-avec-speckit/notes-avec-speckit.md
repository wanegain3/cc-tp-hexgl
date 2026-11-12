# Prompts principaux

## A1 -  Récupérer le projet

```text
cd /formation
```

```text
git clone https://github.com/wanegain3/cc-tp-hexgl.git cc-tp-hexgl
```

## A2 - Explorer le legacy

```text
Explore ce projet legacy sans modifier aucun fichier.

Génère directement `docs/architecture.md` avec :

- stack et points d’entrée
- flux principal de lancement
- principaux modules, fichiers et assets
- dépendances et zones legacy à risque
- fonctions/modules testables automatiquement, notamment avec les tests QUnit disponibles dans formation/tests/
- bugs, dettes ou anomalies remarquables.

Le document doit décrire uniquement l’état actuel du projet.

Ne propose aucune solution.
Ne refactore rien.
Ne modifie aucun autre fichier.
```

Cf. /A2-explorer pour un exemple de architecture.md

## A3 - Figer les comportements à préserver

```text
En t'appuyant sur docs/architecture.md, commence par lancer le jeu et réaliser deux captures de référence : l'écran d'accueil et le jeu en cours. Enregistre-les dans formation/tests/captures/. Si les captures automatiques sont impossibles, signale-le sans bloquer la suite.

Ensuite, identifie quelques comportements existants à protéger avant le refactoring. Une suite de tests QUnit existe déjà dans formation/tests/ : examine-la et complète-la avec quelques tests de non-régression pertinents.

Contraintes :
- Conserver et enrichir `formation/tests/browser-tests.js`, sans créer de nouveau framework de tests.
- Privilégier les fonctions simples à tester automatiquement (Utils, Timer…), sans lancer le jeu ni dépendre du rendu WebGL.
- Charger les scripts supplémentaires nécessaires dans `formation/tests/index.html`.
- Ne pas modifier le code fonctionnel.
- Ne pas ajouter de dépendances, de `package.json` ni de commande `npm test`.
- Ne pas figer le logo, les couleurs, les textes ou les éléments de branding qui seront volontairement modifiés plus tard.

À la fin :
1. Exécute les tests avec `formation/tests/ci-chrome.bat` sous Windows ou `ci-chrome.sh` sous Linux.
2. Indique clairement les tests réussis, échoués ou impossibles à exécuter.
3. Documente la commande de test et l’emplacement des éventuelles captures dans `CLAUDE.md` pour future réutilisation
4. Explique brièvement ce que chaque nouveau test protège.
5. Si les tests réussissent, crée un commit local des modifications de documentation et de tests, sans push.
```

Cf. /A3-figer-les-comportements pour un exemple des nouveaux tests et des captures, ainsi que le claude.md

## Installation de Github Spec Kit et de ses dépendances

Python
```text
winget install Python.Python.3.14
```

uv windows
```text
winget install --id astral-sh.uv -e
```

uv macOS
```text
brew install uv
```

uv Linux
```text
curl -LsSf https://astral.sh/uv/install.sh | sh
```

GitHub Spec Kit
```text
uv tool install specify-cli
```

```text
specify version
```

## Initialisation de Github Spec Kit

Initialisation dans un projet existant (à lancer dans le répertoire du projet cc-tp-hexgl/HexGL/ ):
```text
specify init --here --force --integration claude
```
Ajout du plugin git :
```text
specify extension add git
```

## Préparation du workflow Spec Kit

Créer la constitution :
```text
/speckit-constitution Crée la constitution de ce projet HexGL legacy selon ces principes :
- Préserver le gameplay et les comportements existants.
- Conserver Three.js et CoffeeScript sans modernisation.
- Réutiliser les tests QUnit de formation/tests/.
- Ne pas ajouter de dépendances, de build ni de backend.
- Privilégier les modifications minimales et réversibles.
- Respecter l'architecture existante et documenter les décisions importantes.
- Exiger des tests et une validation humaine pour les changements importants.

Vérification :
- Tests QUnit après chaque phase significative et en fin de ticket.
- Vérification du rendu et des interactions en fin de ticket, avec Playwright si disponible, sans répétitions inutiles.
- Comparer aux captures initiales uniquement les éléments à préserver ; valider les changements visuels selon le ticket.
- Course complète uniquement si le gameplay risque d'être affecté.
- Signaler les vérifications impossibles sans les considérer comme réussies.

Consignes :
- Appuie-toi sur docs/architecture.md et CLAUDE.md.
- Définis des règles durables, indépendantes des tickets.
- N'ajoute aucune contrainte non demandée et ne modifie pas le code applicatif.
- Rédige la constitution et les futurs documents en français, avec DOIT, NE DOIT PAS et DEVRAIT plutôt que MUST, MUST NOT et SHOULD.
- Conserve les noms techniques en anglais.
- Livre une constitution finalisée, sans Sync Impact Report temporaire, commentaire de brouillon ni détails propres à l'environnement de test.
```

Valider la constitution :
```text
Je valide la constitution et les choix proposés. Crée un commit Git contenant uniquement l'installation de Spec Kit, son extension Git et la constitution, sur la branche actuelle. Pas de push.
```

## B1. Récupérer le besoin et préparer Git

```text
git status
```

```text
git branch
```

Seulement sans l'extension Git :
```text
git switch -c 001-company-event-acme
```

## B2. Produire spec.md

```text

/speckit-specify

Spécifie le besoin décrit dans @formation/tickets/company-event.md.

Les documents d'identité visuelle mentionnés dans le ticket sont dans formation/identite/.
Appuie-toi également sur @docs/architecture.md pour comprendre l'existant.

Rédige le contenu métier de la spécification et de sa checklist en français, en conservant les termes professionnels usuels.
Préserve à l'identique la structure des templates, les intitulés de sections, les identifiants et tous les marqueurs nécessaires au fonctionnement de Spec Kit.

Ne modifie pas le code applicatif.
```

Cf. /B2-produire-spec pour une exemple de specification fonctionnelle

## B3. Revue de spec.md

```text
/speckit-clarify
```

Cf. /B3-revoir-spec pour une exemple de specification fonctionnelle revue

## B4. Produire plan.md

```text
/speckit-plan Appuie-toi sur @docs/architecture.md pour respecter l'architecture legacy de HexGL.

Conserve Three.js et CoffeeScript, sans nouvelle dépendance, build ni backend.
Privilégie les modifications minimales et réversibles.

Rédige le contenu métier et technique en français, tout en préservant les intitulés structurels et marqueurs de Spec Kit.

Ne modifie pas le code applicatif.
```

Cf. /B4-produire-plan pour une exemple de plan.md

## B5. Revoir plan.md

```text
Je valide les décisions proposées dans le plan et ses documents complémentaires.

Aligne si nécessaire spec.md, plan.md et les autres artefacts Spec Kit pour assurer leur cohérence.

Corrige les éventuelles contradictions entre exigences, choix techniques et stratégie de validation.

Respecte la constitution et ne modifie pas le code applicatif.
```

Cf. /B5-revoir-plan pour une exemple de plan.md revu


## B6. Produire tasks.md

```text
/speckit-tasks
```

Cf. /B6-produire-tasks pour une exemple de tasks.md


## B7. Revoir tasks.md et analyser la cohérence des artefacts

```text
/speckit-analyze
```

```text
Corrige tous les problèmes identifiés par /speckit-analyze, quelle que soit leur sévérité.
Privilégie des corrections simples et proportionnées, avec des validations réalisables seul, sans complexité ni contraintes arbitraires.
Optimise la stratégie de validation :
- Évite les tests redondants.
- Regroupe les vérifications navigateur en fin de ticket.
- Prévois QUnit après les phases significatives qui le justifient et en fin de ticket.
- N'exige une course complète que si le gameplay risque d'être affecté.
Aligne tous les artefacts Spec Kit concernés et mets à jour la checklist si nécessaire.
Respecte la constitution et le périmètre du ticket.
Ne modifie pas le code applicatif et ne crée aucun commit.
```

```text
/speckit-analyze
```

```text
Corrige les derniers problèmes pertinents du rapport, en privilégiant les solutions simples et sans ajouter de complexité inutile.
Ne crée ni nouvelles exigences, ni tâches, ni tests sauf nécessité réelle.
Ne modifie pas le code applicatif et ne crée aucun commit.
```

Cf. /B7-revoir-tasks-analyser-coherence pour une exemple de fichiers revus et cohérents


## B8 - Implémenter avec Spec Kit

Implémentation complète (à privilégier pour ce TP) :
```text
/speckit-implement Implémente toutes les phases dans l'ordre, en exécutant les tests prévus à chaque point de contrôle, sans les reporter ni les regrouper.
```

ou bien, par étapes :
```text
/speckit-implement Implémente uniquement la première phase non terminée de tasks.md, puis arrête-toi. Respecte les dépendances, exécute les vérifications prévues et mets à jour les cases à cocher.
```

Cf. /B8-implementer-avec-speckit pour les captures d'écran de l'accueil en version large et en version mobile


## B9 - Vérification finale

Vérification implémentation vs spec. :
```text
/speckit-converge Vérifie la conformité de l'implémentation à spec.md. Réutilise les résultats des tests déjà exécutés et évite les vérifications redondantes. Signale les contrôles nécessitant une validation humaine.
```

Si besoin de correction, réutiliser /speckit-implement, par exemple :
```text
/speckit-implement Exécute uniquement les tâches documentaires restantes, sans modifier le code applicatif ni relancer les tests déjà réussis.
```

## B10 - Capitaliser et documenter

```text
Mets à jour si nécessaire la documentation du projet et les artefacts Spec Kit pour refléter l'implémentation finale. Corrige les références obsolètes, évite les doublons et les modifications inutiles. Ne modifie pas le code applicatif et ne crée aucun commit.
```

Cf. /B10-capitaliser-et-documenter pour la doc mise à jour.

## B11 - Revoir les modifications et créer un commit

```text
/diff
```

```text
Vérifie le statut Git et crée un commit local contenant uniquement les fichiers pertinents pour le ticket. Ne fais aucun push.
```

## B12 - Automatisations Git

Cf. /B12-automatisations-git pour les fichiers du chapitre sur Git, GitHub, et CI / CD


### Connecter le dépôt distant

Connexion à votre propre dépôt :
```text
cd ..
```

```text
git remote -v
```

```text
git remote set-url origin https://github.com/votre-compte/cc-tp-hexgl-NOM.git
```

```text
git remote -v
```

Envoi de vos branches locales sur le dépôt distant :
```text
git push -u origin main
```

```text
git push -u origin 001-company-event-acme
```

### Hook pre-commit

```text
git config core.hooksPath .githooks
```

(pour sur linux/mac)
```text
chmod +x .githooks/pre-commit
```

```text
git add .githooks/pre-commit
```

```text
git commit -m "Ajout du hook pre-commit"
```

### Ajout de la CI

```text
git add .github/workflows/ci.yml
```

```text
git commit -m "Ajout de la CI"
```

```text
git push
```

### Ajout des github actions

```text
winget install --id GitHub.cli -e
```

```text
/install-github-app
```

```text
git switch main
```

```text
git pull
```

### Ajout de la CD

```text
git switch main
```

```text
git pull
```

```text
git add .github/workflows/cd.yml
```

```text
git commit -m "Ajout de la CD"
```

```text
git push
```
