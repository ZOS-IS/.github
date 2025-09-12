## 📝 Format des titres de Pull Requests

Pour assurer un changelog clair et lisible automatiquement, **merci de nommer votre PR comme suit** :

```
[TYPE] Description concise de la modification
```

### ✅ Exemples :
- `[FIX] Correction de la couleur des graphiques oxygène`
- `[FEAT] Ajout d’un filtre sur les patients inactifs`
- `[REFACTOR] Nettoyage du composant Dashboard`
- `[CHORE] Mise à jour des dépendances`
- `[DOC] Ajout d’une section dans le README`

### 📌 Types acceptés

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

## Format des noms de branches

Pour garder un historique lisible et homogène, merci de nommer vos branches comme suit :

```sh
type/description-concise
```


### Exemples :
- `feat/ajout-filtre-patients-inactifs`
- `fix/couleur-graphiques-oxygene`
- `refactor/nettoyage-dashboard`
- `chore/mise-a-jour-dependances`
- `doc/ajout-readme-installation`
- `test/ajout-tests-repository`

### Types acceptés
- `feat`     Nouvelle fonctionnalité
- `fix`      Correction de bug
- `refactor` Refactorisation sans impact fonctionnel
- `chore`    Tâches de maintenance (CI, dépendances, etc.)
- `doc`      Documentation
- `test`     Ajout ou modification de tests
- `style`    Formatage ou amélioration visuelle
- `perf`     Optimisation des performances

### ⚠️Règles importantes
1. **Pas d’espaces ni de caractères spéciaux** dans les noms de branches (utiliser des `-` et des `_` comme séparateur).
2. Toute branche de développement est basée sur develop.
3. Toute branche de hotfix est basée sur main.
3. Ne pas utiliser `git pull` → préférer `git fetch && git rebase`.
4. Lors d’une collaboration sur une même fonctionnalité :
   - Créez des sous-branches :
     - `feat/ajout-filtre-patients-inactifs/alice`
     - `feat/ajout-filtre-patients-inactifs/bob`
   - Rebasez régulièrement sur la branche principale de la feature.

