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
