# 🚀 Dev Guide – Git Workflow, Expo & EAS (Ultimate Version)

## 🧭 Introduction

This guide is for **React Native** developers who want to adopt a **professional**, clear, and scalable **workflow**.
It covers:

* Best **Git practices** and **commit conventions**;
* The use of **Expo** and **EAS** for building and publishing;
* The **complete organization of a modern React Native project**;
* And a recap of **essential React hooks** for smooth development.

---

# 💾 Git & GitHub: Professional Workflow for React Native Developers

## 🔍 Viewing Commit History

```
# Simple history
git log --oneline

# History with graph
git log --oneline --graph --decorate --all

# View the last X commits
git log --oneline -n 10
```

---

## ✂️ Rewriting History with `git rebase -i`

```
git rebase -i HEAD~X
```

* X = number of recent commits you want to rewrite (e.g., HEAD~5 → the last 5 commits).
* Opens an editor with the list of commits (oldest on top, newest at the bottom).

Example:

```
pick a1b2c3 feat(ui): Add login screen # oldest
pick d4e5f6 fix: typo in Login button
pick g7h8i9 feat(api): Add auth endpoint # newest
```

### Possible Actions:

* pick → keep the commit
* reword → edit the commit message
* edit → modify the commit (content or message)
* squash (s) → merge this commit with the previous one
* fixup (f) → same as squash but without keeping the message
* drop → delete the commit

Example to merge the last two commits:

```
pick a1b2c3 feat(ui): Add login screen
squash d4e5f6 fix: typo in Login button
squash g7h8i9 feat(api): Add auth endpoint
```

> 💡 Always keep **at least one `pick`**.
> Without it, Git has no base commit to merge onto.

---

## 📝 Work-In-Progress (WIP) Commits

```
git add .
git commit -m "WIP: <quick description>"
```

Examples:

```
git commit -m "WIP: implementing login API"
git commit -m "WIP: UI adjustments on navbar"
```

👉 These commits are temporary and can later be squashed using `git rebase -i`.

---

## ✅ Commit Conventions (Conventional Commits)

### Available Types:

* build: build or dependency changes (npm, webpack…)
* ci: CI/CD changes (GitHub Actions, Travis, etc.)
* docs: documentation only
* feat: adding a new feature
* fix: bug fix
* perf: performance improvement
* refactor: internal code improvement without behavior change
* style: formatting or style changes (indentation, spaces, etc.)
* test: adding/fixing tests

### Scopes (inside parentheses)

* feat(ui): feature on the user interface side
* feat(api): feature on the backend/API side
* feat(auth): authentication-related feature
* fix(build): fix in build configuration
* style(lint): linting or formatting fixes
* refactor(core): refactor of the project core
* test(e2e): adding/fixing end-to-end tests

### Concrete Examples:

```
git commit -m "feat(ui): add dark mode toggle"
git commit -m "fix(api): handle 500 errors on login route"
git commit -m "refactor(core): simplify state management logic"
git commit -m "style(ui): fix spacing on buttons"
git commit -m "docs(readme): add setup instructions"
git commit -m "test(e2e): add login flow test"
```

---

## ⚡ Recommended Workflow

1. Make frequent commits: even WIP, to avoid losing progress.
2. Clean up before pushing:

   ```
   git rebase -i HEAD~X
   ```

   → squash/fixup/reword for a clean history.
3. Follow the conventions:
   `type(scope): description`
4. Push to GitHub 🚀

---

## 🌿 Create a new branch properly (feature, fix, refactor...)

When you want to develop a **new feature**, fix a **bug**, or make a **refactor**,
⚠️ **never code directly on `main`**.

Always work in a dedicated branch to avoid conflicts and keep a clean history.

---

### 🧩 Basic steps

```bash
# 1. Update the main branch
git checkout main
git pull origin main

# 2. Create your working branch from main
git checkout -b feature/my-new-feature
# (or fix/bug-login, refactor/ui-cleanup, chore/update-readme...)

# 3. Code calmly and commit regularly
git add .
git commit -m "feat(ui): add dice roll animation"

# 4. Push your branch to GitHub
git push -u origin feature/my-new-feature
```

---

### ✅ When your feature is done

```bash
# 1. Update your main branch
git checkout main
git pull origin main

# 2. Switch back to your feature branch
git checkout feature/my-new-feature

# 3. Clean up your commit history
git rebase -i main
# -> keep the first commit as 'pick'
# -> mark all others as 'squash'
# -> write a clear, meaningful final commit message

# 4. Integrate the latest changes from main
git fetch origin
git rebase origin/main   # replay your cleaned-up commit on top of the latest main

# 5. Merge cleanly into main
git checkout main
git merge feature/my-new-feature
git push
```

---

### 🧹 Cleanup

```bash
# Delete the local and remote branch
git branch -d feature/my-new-feature
git push origin --delete feature/my-new-feature
```

---

### 💡 Git best practices (to never “break” main)

* 🚫 Never code **directly on `main`**.
* 🌿 **1 task = 1 branch.** (feature, fix, refactor, chore...)
* 🧠 Before creating a branch: `git pull origin main`.
* 💾 Commit often, even for WIP (work in progress).
* 🔄 Rebase or merge regularly with `main` to avoid conflicts.
* 🧹 Delete finished branches after merging.
* 🧱 Keep `main` stable: tested, reviewed, and ready to deploy.

---

💬 **Branch naming examples:**

* `feature/dice-roll-animation`
* `fix/auth-token-refresh`
* `refactor/game-logic`
* `chore/update-dependencies`

---

## 🔧 Other Useful Git Commands

```
git stash                 # Temporarily save uncommitted work
git stash pop             # Restore the stash

git reset --soft HEAD~1   # Undo last commit (keep changes)
git reset --hard HEAD~1   # Completely delete last commit

git checkout <commit_hash>    # Go back to a specific commit
git cherry-pick <commit_hash> # Copy a commit to another branch
git diff <commit1> <commit2>  # View differences between 2 commits

git pull --rebase origin main # Fetch and reapply local commits cleanly
git push --force-with-lease   # Force push safely (avoid overwriting others)

git branch -d <branch>        # Delete local branch (safe)
git branch -D <branch>        # Delete local branch (forced)
git checkout -b <branch>      # Create and switch to new branch

git tag v1.0.0                # Create a version tag
git push origin v1.0.0        # Push a tag
```

---

# 📱 Expo & EAS Guide

## ▶️ Start the App Locally

```
npx expo start              # Starts Expo in interactive mode (choose LAN / tunnel / dev client)
npx expo start --tunnel     # Uses a tunnel, useful if LAN doesn’t work
npx expo start -c           # Starts while clearing Metro cache (often fixes bugs)
```

---

## 📱 Run on Device / Emulator

```
npx expo run:android        # Builds and launches on an Android emulator or connected device
npx expo run:ios            # Builds and launches on iOS (Mac required)
```

---

## 🏗️ Builds with EAS (Expo Application Services)

### Prepare the Project

```
npm install -g eas-cli
eas login                   # Log in with your Expo account
eas build:configure         # Configures EAS (creates eas.json and sets up your app)
```

### Build Android

```
eas build -p android --profile preview     # Android build (.apk for testing)
eas build -p android --profile production  # Android build (.aab required for Play Store)
```

### Build iOS

```
eas build -p ios --profile preview     # iOS build for testing
eas build -p ios --profile production  # iOS build for App Store
```

### Useful Options

```
eas build --local                       # Run the build locally
eas build -p android --profile development # Fast dev build for Android
```

### Publish to Stores (EAS Submit)

```
eas submit -p android --latest          # Upload latest Android build to Play Store
eas submit -p ios --latest              # Upload latest iOS build to App Store
```

👉 `eas submit` lets you send your `.aab` or `.ipa` directly to the stores (no manual upload needed).

---

## 📦 Other Useful Expo Commands

```
expo install <package>   # Installs a package compatible with your Expo SDK
npx expo doctor          # Checks that everything is properly configured
expo prebuild            # Generates android/ and ios/ folders if you need to customize native code
```

---

💡 Good to Know:

* `.apk` → convenient for quick testing/sharing.
* `.aab` → mandatory for Play Store publishing.
* `preview` → lightweight profile, ideal for testing.
* `production` → for official deployment (with signing).
* `eas submit` → uploads your build directly to Play Store / App Store.

---

# 🧱 React Native Project Organization (Pro Structure)

## 🗂 Typical Global Structure

Here’s a modern and clean folder structure for a React Native project using **Expo Router**:

```
app/                    # App routing (pages and layouts via Expo Router)
  _layout.tsx
  index.tsx
  game-board/
    _layout.tsx
    index.tsx
  challenge-vote/
    index.tsx

src/                    # Core logic of the app
  api/                  # Network calls and backend communication
  components/           # Reusable components (shared across features)
  constants/            # Global fixed values (colors, text, configs)
  data/                 # Static data, mocks, or seeds
  features/             # Main features of the app
  hooks/                # Global custom hooks
  stores/               # Global states (Zustand, Jotai, Redux...)
  utils/                # Pure utility functions and helpers
  env.d.ts              # Environment variable typing
```

---

## ⚙️ Folder Details

### 📁 **app/**

> Contains all your app *routes*. Managed by Expo Router.

Each subfolder = one page.

* `_layout.tsx`: page or group layout (navigation bar, providers...)
* `index.tsx`: the page component.

Example:

```tsx
// app/game-board/index.tsx
import GameBoardScreen from "@/src/features/game-board/GameBoardScreen";
export default function GameBoardPage() {
  return <GameBoardScreen />;
}
```

---

### ⚙️ **src/api/**

> All files handling **communication with your backend** (REST, GraphQL, etc.)

Examples:

* `client.ts`: Axios or fetch client configuration
* `challengeApi.ts`: functions to interact with challenge API

---

### 🧱 **src/components/**

> Contains **cross-feature reusable components**.

Recommended organization:

```
components/
  ui/          # Purely visual components, no business logic
  features/    # Reusable components with some logic
  providers/   # Contexts or providers shared across features
```

Examples:

* `ui/FloatingText.tsx` → purely visual animation (UI)
* `features/ChallengeDisplay.tsx` → reusable logical component
* `providers/WobbleProvider.tsx` → global provider

---

### ⚙️ **src/constants/**

> Fixed values used throughout the project.

Examples:

```ts
export const Colors = {
  primary: "#FFA000",
  background: "#1C1C1C",
  text: "#FFFFFF",
};
```

May also include:

* `urls.ts`: API endpoints
* `storageKeys.ts`: AsyncStorage keys

---

### 🧩 **src/data/**

> Static or initial data (mock data, seeds, examples).

Examples:

* `initialChallenges.ts`: base challenge list
* `index.ts`: global export

> If data is specific to a feature, place it in `features/<name>/constants/`.

---

### ⚙️ **src/features/**

> Each feature = a standalone module containing everything related to it.

Example:

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

A feature includes:

* `components/` → all UI for that feature
* `constants/` → feature-specific constants
* `hooks/` → feature-specific hooks

💡 Each feature is a mini isolated app.

---

### 🪝 **src/hooks/**

> Contains **global custom hooks** (usable anywhere in the app).

Examples:

* `useColorScheme.ts`: handle light/dark theme
* `useThemeColor.ts`: color adaptation hook
* `useColorGradientStyle.ts`: animate color gradients

> If a hook is specific to a single feature, place it in `features/<name>/hooks/`.

---

### 🧠 **src/stores/**

> Manages **global state** (often via Zustand, Redux, etc.)

Examples:

* `playersStore.ts` → player list and state
* `uiStores.ts` → modal, toast, etc. management
* `gameDataStore.ts` → current game state

> These files contain sources of truth accessible across features.

---

### 🔧 **src/utils/**

> Pure utility functions, without React or global state dependencies.

Examples:

```ts
export function getRandomInt(min: number, max: number) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}
```

> Place low-level logic, conversions, calculations, validations, etc. here.

---

## 🌿 Example of Final Structure

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

## 🧠 Quick Summary

| Folder          | Main Role                     | Typical Content                         |
| --------------- | ----------------------------- | --------------------------------------- |
| **app/**        | Routing & pages (Expo Router) | `_layout.tsx`, `index.tsx`              |
| **api/**        | Backend communication         | `client.ts`, `authApi.ts`               |
| **components/** | Shared components             | `ui/`, `features/`, `providers/`        |
| **constants/**  | Global constants              | `Colors.ts`, `storageKeys.ts`           |
| **data/**       | Mock or static data           | `initialChallenges.ts`                  |
| **features/**   | App feature modules           | `game-board/`, `challenge-vote/`        |
| **hooks/**      | Global hooks                  | `useThemeColor.ts`, `useColorScheme.ts` |
| **stores/**     | Global state management       | `playersStore.ts`, `uiStores.ts`        |
| **utils/**      | Utility functions             | `getRandomInt`, `formatDate`, etc.      |

---

💬 **Golden Rule:**

> If it’s global → put it in `src/`.
> If it’s feature-specific → put it in `src/features/<name>/`.

That’s what separates a *clean and scalable project* from an unmanageable one.

---

# ⚙️ Project Best Practices

## 📂 Import Aliases (tsconfig.json)

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

➡️ Lets you write `@/src/utils/helpers` instead of `../../../utils/helpers`.

---

## 🧱 File Conventions

* One **component = one file** (in `PascalCase`)
* One **hook = one file** (in `camelCase`)
* One **index.ts** per folder to group exports
* UI files → `ui/`, logic → `features/`
* Prefer pure, isolated functions in `utils/`

---

## 🧰 Recommended Tools

* **ESLint + Prettier** → automatic formatting & linting
* **Strict TypeScript** → avoid runtime errors
* **Husky + lint-staged** → automatic checks before commit

---

# ⚛️ Essential React Hooks

### 🪝 `useState`

Allows you to manage local state within a component.

```tsx
const [count, setCount] = useState(0);
```

### ⚡ `useEffect`

Executes code in response to a change or when the component mounts.

```tsx
useEffect(() => {
  console.log('Mounted or updated');
  return () => console.log('Cleanup');
}, [dependency]);
```

### 🔁 `useRef`

Stores a persistent value between renders (often used for DOM references or mutable values).

```tsx
const inputRef = useRef<TextInput>(null);
```

### 🧠 `React.memo`

Optimizes a component to prevent unnecessary re-renders.

```tsx
export default React.memo(MyComponent);
```

### ⚙️ Other Useful Hooks

* `useCallback` → memoizes a function to avoid recreating it on every render.
* `useMemo` → memoizes the result of an expensive computation.
* `useContext` → accesses a global context without prop drilling.

---

# 🌍 Available Versions

* 🇬🇧 [English](./en.md)
* 🇫🇷 [French](./fr.md)