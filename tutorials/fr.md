# 🚀 Dev Guide – Git Workflow, Expo & EAS (Ultimate Version)

## 🧭 Introduction

Ce guide s’adresse aux développeurs **React Native** qui souhaitent adopter un **workflow professionnel**, clair et scalable.
Il couvre :

* Les bonnes pratiques **Git** et **conventions de commits** ;
* L’utilisation d’**Expo** et **EAS** pour le build et la publication ;
* L’**organisation complète d’un projet React Native** moderne ;
* Et un rappel des **hooks React essentiels** pour un développement fluide.

---

# 💾 Git & GitHub : Workflow professionnel du développeur React Native

## 🔍 Voir l’historique des commits

```
# Historique simple
git log --oneline

# Historique avec graph
git log --oneline --graph --decorate --all

# Voir les X derniers commits
git log --oneline -n 10
```

---

## ✂️ Réécrire l’historique avec `git rebase -i`

```
git rebase -i HEAD~X
```

* X = nombre de commits récents que tu veux réécrire (ex: HEAD~5 → les 5 derniers commits).
* Ouvre un éditeur avec la liste des commits (du plus ancien en haut, au plus récent en bas).

Exemple :

```
pick a1b2c3 feat(ui): Add login screen # le plus ancien
pick d4e5f6 fix: typo in Login button
pick g7h8i9 feat(api): Add auth endpoint # le plus récent
```

### Actions possibles :

* pick → garder le commit
* reword → modifier le message
* edit → modifier le commit (contenu ou message)
* squash (s) → fusionner ce commit avec le précédent
* fixup (f) → idem que squash mais sans garder le message
* drop → supprimer le commit

Exemple pour fusionner les deux derniers commits :

```
pick a1b2c3 feat(ui): Add login screen
squash d4e5f6 fix: typo in Login button
squash g7h8i9 feat(api): Add auth endpoint
```

> 💡 Toujours garder **au moins un `pick`**.  
> Sans ça, Git n’a pas de commit de base pour fusionner.


---

## 📝 Commits de travail (WIP)

```
git add .
git commit -m "WIP: <description rapide>"
```

Exemples :

```
git commit -m "WIP: implementing login API"
git commit -m "WIP: UI adjustments on navbar"
```

👉 Ces commits sont temporaires et pourront être squashés plus tard avec `git rebase -i`.

---

## ✅ Conventions de commits (Conventional Commits)

### Types disponibles :

* build: changements liés au build ou dépendances (npm, webpack…)
* ci: changements sur la CI/CD (GitHub Actions, Travis, etc.)
* docs: documentation uniquement
* feat: ajout d’une nouvelle fonctionnalité
* fix: correction de bug
* perf: amélioration de performance
* refactor: amélioration interne du code, sans modification du comportement.
* style: changements de style (indentation, espaces, etc.)
* test: ajout/correction de tests

### Scopes (dans les parenthèses)

* feat(ui): feature côté interface utilisateur
* feat(api): feature côté backend/API
* feat(auth): feature liée à l’authentification
* fix(build): correction sur la config du build
* style(lint): corrections liées au lint/formatage
* refactor(core): refacto du cœur du projet
* test(e2e): ajout/correction de tests end-to-end

### Exemples concrets :

```
git commit -m "feat(ui): add dark mode toggle"
git commit -m "fix(api): handle 500 errors on login route"
git commit -m "refactor(core): simplify state management logic"
git commit -m "style(ui): fix spacing on buttons"
git commit -m "docs(readme): add setup instructions"
git commit -m "test(e2e): add login flow test"
```

---

## ⚡ Workflow conseillé

1. Commits fréquents : même WIP, pour ne rien perdre.
2. Nettoyage avant push :

   ```
   git rebase -i HEAD~X
   ```

   → squash/fixup/reword pour avoir un historique clair.
3. Respecter les conventions :
   `type(scope): description`
4. Push vers GitHub 🚀

---

## 🔧 Commandes Git utiles en plus

```
git stash                 # Sauvegarder du travail temporaire sans commit
git stash pop             # Restaurer le stash

git reset --soft HEAD~1   # Annuler le dernier commit (sans perdre le code)
git reset --hard HEAD~1   # Supprimer totalement le dernier commit

git checkout <commit_hash>    # Revenir à un commit précis
git cherry-pick <commit_hash> # Copier un commit d'une branche à une autre
git diff <commit1> <commit2>  # Voir les différences entre 2 commits

git pull --rebase origin main # Récupérer les maj distantes proprement
git push --force-with-lease   # Forcer un push mais en évitant d’écraser les commits d’un autre

git branch -d <branch>        # Supprimer une branche locale (sécurisé)
git branch -D <branch>        # Supprimer une branche locale (forcé)
git checkout -b <branch>      # Créer + switcher direct sur une nouvelle branche

git tag v1.0.0                # Créer un tag de version
git push origin v1.0.0        # Pousser un tag
```

---

# 📱 Expo & EAS Guide

## ▶️ Démarrer l’app en local

```
npx expo start              # Démarre Expo en mode interactif (choix LAN / tunnel / dev client)
npx expo start --tunnel     # Utilise un tunnel, utile si le LAN ne marche pas
npx expo start -c           # Démarre en vidant le cache Metro (résout souvent des bugs)
```

---

## 📱 Lancer sur un appareil / émulateur

```
npx expo run:android        # Compile et lance sur un émulateur ou appareil Android connecté
npx expo run:ios            # Compile et lance sur iOS (Mac obligatoire)
```

---

## 🏗️ Builds avec EAS (Expo Application Services)

### Préparer le projet

```
npm install -g eas-cli
eas login                   # Se connecter avec son compte Expo
eas build:configure         # Configure EAS (crée eas.json et configure ton app)
```

### Faire un build Android

```
eas build -p android --profile preview     # Build Android (.apk pour test)
eas build -p android --profile production  # Build Android (.aab requis pour le Play Store)
```

### Faire un build iOS

```
eas build -p ios --profile preview     # Build iOS pour test
eas build -p ios --profile production  # Build iOS pour l’App Store
```

### Options utiles

```
eas build --local                       # Lance le build en local
eas build -p android --profile development # Build rapide pour dev Android
```

### Publier sur les stores (EAS Submit)

```
eas submit -p android --latest          # Envoie le dernier build Android vers le Play Store
eas submit -p ios --latest              # Envoie le dernier build iOS vers l’App Store
```

👉 `eas submit` permet d’envoyer ton `.aab` ou `.ipa` directement aux stores (sans passer par un upload manuel).

---

## 📦 Autres commandes Expo utiles

```
expo install <package>   # Installe un package compatible avec ton SDK Expo
npx expo doctor          # Vérifie que tout est bien configuré
expo prebuild            # Génère android/ et ios/ si tu dois customiser le natif
```

---

💡 Bon à savoir :

* `.apk` → pratique pour tester / partager rapidement.
* `.aab` → obligatoire pour publier sur le Play Store.
* `preview` → profil léger, idéal pour tests.
* `production` → pour déploiement officiel (avec signatures).
* `eas submit` → pour uploader directement ton build sur le Play Store / App Store.

---

# 🧱 Organisation d'un projet React Native (Structure Pro)

## 🗂 Structure globale type
   
Voici une arborescence moderne et claire pour un projet React Native utilisant **Expo Router** :

```
app/                    # Routage de l'app (pages et layouts Expo Router)
  _layout.tsx
  index.tsx
  game-board/
    _layout.tsx
    index.tsx
  challenge-vote/
    index.tsx

src/                    # Coeur logique de l'application
  api/                  # Appels réseau et gestion de la communication avec le backend
  components/           # Composants réutilisables (partagés entre plusieurs features)
  constants/            # Valeurs fixes globales (couleurs, textes, configs)
  data/                 # Données statiques, mocks ou seeds
  features/             # Regroupe les fonctionnalités principales de l'app
  hooks/                # Hooks personnalisés globaux
  stores/               # États globaux (Zustand, Jotai, Redux...)
  utils/                # Fonctions utilitaires pures et helpers
  env.d.ts              # Typage des variables d'environnement
```

---

## ⚙️ Détail de chaque dossier

### 📁 **app/**

> Contient toutes les *routes* de ton application. C’est Expo Router qui les gère.

Chaque sous-dossier = une page.

* `_layout.tsx` : layout de la page ou du groupe (barre de navigation, providers...)
* `index.tsx` : composant de la page.

Exemple :

```tsx
// app/game-board/index.tsx
import GameBoardScreen from "@/src/features/game-board/GameBoardScreen";
export default function GameBoardPage() {
  return <GameBoardScreen />;
}
```

---

### ⚙️ **src/api/**

> Tous les fichiers qui gèrent la **communication avec ton backend** (REST, GraphQL, etc.)

Exemples :

* `client.ts` : configuration Axios ou fetch client
* `challengeApi.ts` : fonctions pour interagir avec l'API des challenges

---

### 🧱 **src/components/**

> Contient les **composants transversaux** (utilisables dans plusieurs features).

Organisation conseillée :

```
components/
  ui/          # Composants purement visuels, sans logique métier
  features/    # Composants réutilisables avec un peu de logique
  providers/   # Contexts ou providers partagés entre features
```

Exemples :

* `ui/FloatingText.tsx` → animation visuelle pure (UI)
* `features/ChallengeDisplay.tsx` → composant logique réutilisable
* `providers/WobbleProvider.tsx` → provider global

---

### ⚙️ **src/constants/**

> Valeurs fixes utilisées dans tout le projet.

Exemples :

```ts
export const Colors = {
  primary: "#FFA000",
  background: "#1C1C1C",
  text: "#FFFFFF",
};
```

Peut contenir aussi :

* `urls.ts` : endpoints API
* `storageKeys.ts` : clés AsyncStorage

---

### 🧩 **src/data/**

> Données statiques ou initiales (mock data, seeds, exemples).

Exemples :

* `initialChallenges.ts` : liste des défis de base
* `index.ts` : export global

> Si ces données sont propres à une feature, mieux vaut les placer dans `features/<nom>/constants/`.

---

### ⚙️ **src/features/**

> Chaque feature = un module autonome regroupant tout ce qui la concerne.

Exemple :

```
features/
  game-board/
    GameBoardScreen.tsx
    components/
      ui/
      features/
      providers/
    constants/
    hooks/
```

Une feature contient :

* `components/` → tout l’affichage spécifique à cette feature
* `constants/` → valeurs propres à la feature
* `hooks/` → hooks locaux à la feature

💡 Chaque feature est une mini-app isolée.

---

### 🪝 **src/hooks/**

> Contient les hooks personnalisés **globaux** (utilisables partout dans l’app).

Exemples :

* `useColorScheme.ts` : gestion du thème clair/sombre
* `useThemeColor.ts` : hook utilitaire d’adaptation de couleur
* `useColorGradientStyle.ts` : hook pour animer des couleurs

> Si un hook ne concerne qu’une seule feature, place-le dans `features/<nom>/hooks/`.

---

### 🧠 **src/stores/**

> Gère **l’état global** de l’application (souvent via Zustand, Redux, etc.)

Exemples :

* `playersStore.ts` → liste et état des joueurs
* `uiStores.ts` → gestion des modales, toasts, etc.
* `gameDataStore.ts` → état de la partie en cours

> Ces fichiers contiennent les sources de vérité accessibles depuis plusieurs features.

---

### 🔧 **src/utils/**

> Fonctions utilitaires pures, sans dépendance à React ni à l’état global.

Exemples :

```ts
export function getRandomInt(min: number, max: number) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}
```

> Tu peux y ranger tout ce qui est logique de bas niveau, conversions, calculs, validations, etc.

---

## 🌿 Exemple de structure finale

```
app/
  _layout.tsx
  index.tsx
  game-board/
    _layout.tsx
    index.tsx

src/
  api/
  components/
    ui/
    features/
    providers/
  constants/
  data/
  features/
    game-board/
      GameBoardScreen.tsx
      components/
        ui/
        features/
        providers/
      constants/
      hooks/
  hooks/
  stores/
  utils/
```

---

## 🧠 Résumé rapide

| Dossier         | Rôle principal                    | Contenu typique                         |
| --------------- | --------------------------------- | --------------------------------------- |
| **app/**        | Routage et pages (Expo Router)    | `_layout.tsx`, `index.tsx`              |
| **api/**        | Requêtes et communication backend | `client.ts`, `authApi.ts`               |
| **components/** | Composants partagés               | `ui/`, `features/`, `providers/`        |
| **constants/**  | Valeurs fixes globales            | `Colors.ts`, `storageKeys.ts`           |
| **data/**       | Données mock ou statiques         | `initialChallenges.ts`                  |
| **features/**   | Modules autonomes du projet       | `game-board/`, `challenge-vote/`        |
| **hooks/**      | Hooks globaux                     | `useThemeColor.ts`, `useColorScheme.ts` |
| **stores/**     | Gestion de l’état global          | `playersStore.ts`, `uiStores.ts`        |
| **utils/**      | Fonctions pures utilitaires       | `getRandomInt`, `formatDate`, etc.      |

---

💬 **Règle d’or :**

> Si c’est global → dans `src/` directement.
> Si c’est spécifique à une feature → dans `src/features/<nom>/`.

C’est ce qui différencie un projet *propre et scalable* d’un projet vite ingérable.

---

# ⚙️ Bonnes pratiques de projet

## 📂 Aliases d’imports (tsconfig.json)

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

➡️ Permet d’écrire `@/src/utils/helpers` au lieu de `../../../utils/helpers`.

---

## 🧱 Conventions de fichiers

* Un **composant = un fichier** (en `PascalCase`)
* Un **hook = un fichier** (en `camelCase`)
* Un **index.ts** par dossier pour regrouper les exports
* Les fichiers d’UI → `ui/`, la logique → `features/`
* Préfère les fonctions pures et isolées dans `utils/`

---

## 🧰 Outils recommandés

* **ESLint + Prettier** → formatage automatique & linting
* **TypeScript strict** → éviter les erreurs runtime
* **Husky + lint-staged** → vérification automatique avant commit

---

# ⚛️ Les Hooks React essentiels

### 🪝 `useState`

Permet de gérer un état local dans un composant.

```tsx
const [count, setCount] = useState(0);
```

### ⚡ `useEffect`

Exécute du code en réaction à un changement ou au montage du composant.

```tsx
useEffect(() => {
  console.log('Mounted or updated');
  return () => console.log('Cleanup');
}, [dependency]);
```

### 🔁 `useRef`

Stocke une valeur persistante entre les rendus (souvent utilisé pour des références DOM ou des valeurs mutables).

```tsx
const inputRef = useRef<TextInput>(null);
```

### 🧠 `React.memo`

Optimise un composant pour éviter les re-rendus inutiles.

```tsx
export default React.memo(MyComponent);
```

### ⚙️ Autres hooks utiles

* `useCallback` → mémorise une fonction pour éviter de la recréer à chaque rendu.
* `useMemo` → mémorise le résultat d’un calcul coûteux.
* `useContext` → accède à un contexte global sans prop drilling.

---

# 🌍 Available Versions
- 🇬🇧 [English](./en.md)
- 🇫🇷 [French](./fr.md)