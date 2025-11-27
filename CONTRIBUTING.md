# Guide de contribution

---

## Table des matières

- [1. Stratégie de branches](#1-stratégie-de-branches)
  - [1.1 Branches principales](#11-branches-principales)
  - [1.2 Mise à jour d'une branche](#12-mise-à-jour-dune-branche)
  - [1.3 Obtenir une branche distante](#13-obtenir-une-branche-distante)
  - [1.4 Créer une nouvelle branche](#14-créer-une-nouvelle-branche)
  - [1.5 Supprimer une branche](#15-supprimer-une-branche)
  - [1.6 Créer une branche de release](#16-créer-une-branche-de-release)
- [2. Stratégie de développement](#2-stratégie-de-développement)
  - [2.1 Avant de commencer : le sprint](#21-avant-de-commencer--le-sprint)
  - [2.2 La branche intermédiaire](#22-la-branche-intermédiaire)
  - [2.3 Les branches individuelles](#23-les-branches-individuelles)
  - [2.4 Les Pull Requests](#24-les-pull-requests)
  - [2.5 Fin du sprint : intégration en développement](#25-fin-du-sprint--intégration-en-développement)
  - [2.6 Règles essentielles](#26-règles-essentielles)
- [3. Format des titres de Pull Requests](#3-format-des-titres-de-pull-requests)
- [4. Format des noms de branches](#4-format-des-noms-de-branches)
  - [4.1 Exemples](#41-exemples)
  - [4.2 Types acceptés](#42-types-acceptés)
  - [4.3 Règles importantes](#43-règles-importantes)

---

## TL;DR

Résumé rapide du document :

- Aucune contribution directe depuis `develop`
- Toute contribution se fait dans une branche individuelle, issue de la branche intermédiaire de sprint, elle-même issue de `develop`
- Une PR individuelle est ouverte depuis la branche de sprint
- Les PR sont intégrées avec la stratégie Squash & Merge
- **Jamais** de merge commit
- Jamais de PR manuelle vers main
- Une release = un tag

## 1. Stratégie de branches

### 1.1 Branches principales

#### `develop`
- Branche principale de *développement*.
- Toute nouvelle fonctionnalité, correction ou évolution **est issu de cette branche**.

#### `main`
- Branche dédiée à la **production**.
- Cette branche est protégée :- Aucun commit direct n’est autorisé, sauf hotfix spécifiquement autorisé.
  - Aucun merge manuel n’est autorisé.
  - **Seules les Pull Requests créées automatiquement par le flux de release peuvent y être intégrées.**

#### `release/**`
- Branches générées automatiquement lors d’un push de tag (`x.y.z` ou `vx.y.z`).
- Elles servent de base à la recette et à la préparation de la mise en production.
- Elles ne doivent pas être modifiées manuellement.
- Une PR `[RELEASE] <version>` est créée automatiquement depuis cette branche vers `main`.

### 1.2 Mise à jour d'une branche

Toute branche est mise à jour grâce au `rebase`.

**Les commits de merge sont strictement prohibés, quel que soit le contexte.**

Il découle que la commande `git pull` doit se voir ajoindre l'option `--rebase` lorsqu'elle est utilisée. Favorisez les commandes listées dans la section [Obtenir une branche distante](#13-obtenir-une-branche-distante)

Quelques exemples :

```sh
# Obtenir les révisions poussées sur la branche distante `sprint/2025-01-01-xxxxxxxx`
git rebase sprint/2025-01-01-xxxxxxxx
```

```sh
# Obtenir les révisions poussées sur la branche distante `sprint/2025-01-01-xxxxxxxx`
# en résolvant tous les conflits **en faveur de ma branche de développement**
git rebase sprint/2025-01-01-xxxxxxxx -s recursive -X theirs
```

```sh
# Obtenir les révisions poussées sur la branche distante `sprint/2025-01-01-xxxxxxxx`
# en résolvant tous les conflits **en faveur de la branche distante**
git rebase sprint/2025-01-01-xxxxxxxx -s recursive -X ours
```

Un guide exhaustif concernant le rebase est [disponible sur Nextcloud](https://team.zos-informationsystem.com/apps/collectives/Le%20coin%20du%20d%C3%A9veloppeur/La%20m%C3%A9thode%20ZOS-IS/Propret%C3%A9%20de%20branche?fileId=200264).

### 1.3 Obtenir une branche distante

Lorsqu'une branche existe sur le dépôt distant (GitHub) mais n’est pas encore présente dans votre dépôt local, il est nécessaire de la **récupérer explicitement**.

Cette opération peut se faire de manière sécurisée et conforme au présent guide, à l'aide d'une des séquences suivantes, où nous supposons l'obtention de la branche : `feat/rbac`

```sh
# Récupérer immédiatement la branche distante souhaitée, en écrasant toute éventuelle référence locale existante, puis la checkout
git fetch origin feat/rbac:feat/rbac \
  && git checkout feat/rbac
```

```sh
# Obtenir toutes les références distantes, puis créer une branche locale à partir de la branche souhaitée et la checkout
git fetch --all \
  && git checkout -b feat/rbac origin/feat/rbac
```

### 1.4 Créer une nouvelle branche

Cette section montre comment créer une nouvelle branche qui n'existe pas dans le dépôt distant.

Le principe est d'utiliser la commande `git checkout` afin de créer une nouvelle référence git depuis une branche de référence commune.
```sh
git checkout -b type/local-branch origin/distant-branch
```

1. Créer une branche dédiée au sprint en cours (voir [Branche intermédiaire](#22-la-branche-intermédiaire)) : `feat/rbac`
  - `git checkout -b feat/rbac origin/develop`
2. Créer une branche individuelle à partir de la branche intermédiaire (voir [Branches individuelles](#23-les-branches-individuelles)) : `feat/roles-handler`
  - `git checkout -b feat/roles-handler origin/feat/rbac`
  - Si la branche intermédiaire n’existe pas encore, reportez-vous à la création d'une [branche intermédiaire](#22-la-branche-intermédiaire)

### 1.5 Supprimer une branche

Les branches locales peuvent être librement supprimées à l'aide la commande :

```sh
git branch -D type/my-branch
```

Les branches distantes peuvent être supprimées sous réserve de leur stratégie de protection et de l'accréditation du contributeur :

```sh
# Syntaxe longue
git push origin --delete type/my-branch

# Syntaxe courte
git push origin :type/my-branch
```

### 1.6 Créer une branche de release

Les branches de releases peuvent être créées en poussant un nouveau tag. La création et la mise en ligne d'un nouveau tag est seule responsable du flux suivant :

- Création d'une branche de type `release/1.0.0`
- Création de la PR associée au format : `[RELEASE] 1.0.0`
- Intégration dans la branche `main`

Sauf autorisation explicite, aucune des actions précédentes ne peut être entreprise sans pousser un nouveau tag.

Avant de pousser un nouveau tag :
- La référence du hash du commit à laquelle s'arrête la nouvelle release doit être connu. Il est référencé dans la séquence suivante sous la variable shell : `${COMMIT_SHA}`.
- Le numéro de version de la release doit être connu. Il est référence dans la séquence suivante sous la variable shell : `${SEMVER}`
- Une description courte de la release a été convenue

Un nouveau tag peut être poussé en respectant à la lettre la séquence suivante :

```sh
git checkout --detach \
  && git fetch origin -f develop:develop \
  && git checkout -f develop \
  && git checkout "${COMMIT_SHA}" \
  && git tag -a "${SEMVER}" \
    -m "Titre de la release" \
    -m "Description de la release : ligne 1" \
    -m "Description de la release : ligne 2" \
    -m "Autant d'options -m que nécessaire" \
  && git push origin "${SEMVER}"
```

---

## 2. Stratégie de développement

Cette section explique **comment contribuer à l'évolution de la base de code** à l’intérieur d’un sprint.

### 2.1 Avant de commencer : le sprint

1. Un sprint commence par une réunion de **sprint planning**.
2. Dès ce moment, toutes les contributions réalisées jusqu’à la **sprint review** sont considérées comme appartenant au sprint en cours.

### 2.2 La branche intermédiaire

Au début du sprint, l’équipe concernée crée une **branche intermédiaire**, héritée de `develop`.
  - Cette branche représente *l’espace de travail du sprint*.
  - Elle est nommée conformément à la section [Format des noms de branches](#format-des-noms-de-branches) du présent guide.

### 2.3 Les branches individuelles

Chaque contributeur crée sa **branche individuelle de travail** depuis la branche intermédiaire.
  - Cette branche représente *l’espace de travail du développeur*.
  - Elle est nommée conformément à la section [Format des noms de branches](#format-des-noms-de-branches) du présent guide.

### 2.4 Les Pull Requests

Lorsqu’une branche individuelle est terminée  :
  - Le développeur ouvre une **PR vers la branche intermédiaire**.
  - L’intégration doit se faire selon la politique **Squash & merge**
   Le squash permet de garantir que chaque fonctionnalité est intégrée par un seul commit, ce qui facilite le changelog et permet un historique plus propre.

Les PR doivent strictement respecter le flux suivant :
- Branche individuelle → Branche intermédiaire
- Branche intermédiaire → `develop`

### 2.5 Fin du sprint : intégration en développement

Lorsque la sprint review est terminée :
  - La branche intermédiaire est intégrée dans `develop`.
  - L’intégration doit se faire selon la politique **Squash & merge**
  - Le squash permet de garantir que chaque fonctionnalité est intégrée par un seul commit, ce qui facilite le changelog et permet un historique plus propre.

### 2.6 Règles essentielles

- Tout part de `develop`. La branche `main` est totalement isolée.
- Les sprints utilisent une **branche intermédiaire**.
- Chaque développeur utilise une **branche individuelle**.
- Les merge commits sont strictement **prohibés**.
- Les PR individuelles sont intégrées **vers la branche intermédiaire**.
- À la fin du sprint, la branche intermédiaire est intégrée dans `develop`.

---

## 3. Format des titres de Pull Requests

Pour assurer un changelog clair et lisible automatiquement, **merci de nommer votre PR comme suit** :

```
[TYPE] Description concise de la modification
```

### 3.1 Exemples :
- `[FIX] Correction de la couleur des graphiques oxygène`
- `[FEAT] Ajout d’un filtre sur les patients inactifs`
- `[REFACTOR] Nettoyage du composant Dashboard`
- `[CHORE] Mise à jour des dépendances`
- `[DOC] Ajout d’une section dans le README`

### 3.2 Types acceptés

| Type       | Signification                                      |
|------------|----------------------------------------------------|
| `FEAT`     | Nouvelle fonctionnalité                            |
| `FIX`      | Correction de bug                                  |
| `REFACTOR` | Refactorisation sans impact fonctionnel            |
| `CHORE`    | Tâche de maintenance (CI, dépendances, etc.)       |
| `DOC`      | Documentation                                      |
| `TEST`     | Ajout ou modification de tests                     |
| `STYLE`    | Formatage ou amélioration visuelle                 |
| `PERF`     | Optimisation des performances                      |

>💡
>
> **Ce titre sera utilisé dans le changelog !**
> Il doit permettre de comprendre rapidement ce que fait la PR, **sans jargon interne ni référence technique seule**.
> [Exemple de changelog généré automatiquement](https://github.com/ZOS-IS/I-GEIA-MEDECINS/releases/tag/2.7.13)

---

## 4. Format des noms de branches

Pour garder un historique lisible et homogène, merci de nommer vos branches comme suit :

```sh
type/description-concise
```

### 4.1 Exemples

- `feat/ajout-filtre-patients-inactifs`
- `fix/couleur-graphiques-oxygene`
- `refactor/nettoyage-dashboard`
- `chore/mise-a-jour-dependances`
- `doc/ajout-readme-installation`
- `test/ajout-tests-repository`

### 4.2 Types acceptés
- `feat`     Nouvelle fonctionnalité
- `fix`      Correction de bug
- `refactor` Refactorisation sans impact fonctionnel
- `chore`    Tâches de maintenance (CI, dépendances, etc.)
- `doc`      Documentation
- `test`     Ajout ou modification de tests
- `style`    Formatage ou amélioration visuelle
- `perf`     Optimisation des performances

### 4.3 Règles importantes

1. **Pas d’espaces ni de caractères spéciaux** dans les noms de branches (utiliser des `-` et des `_` comme séparateur).
2. Toute branche de développement est basée sur develop.
3. Toute branche de hotfix est basée sur main.
4. Ne pas utiliser `git pull` → préférer `git fetch && git rebase`.