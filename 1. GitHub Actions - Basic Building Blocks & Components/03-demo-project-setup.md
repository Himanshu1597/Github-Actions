<h2 style="color: #C0392B;">A Practical Demo Project & GitHub Repository Setup — Complete Notes</h2>

<h3 style="color: #8E44AD;">Why a Real Demo Project?</h3>

- Previous lessons used a very basic workflow with simple `echo` commands.
- From here onwards, a **slightly more realistic project** is used to properly understand GitHub Actions in practice.
- The project is a simple **ReactJS web application** — but you do **not** need to know ReactJS or web development to follow along.
- No meaningful ReactJS code will be written — it's just a realistic container to demonstrate automation.

---

<h3 style="color: #2980B9;">About the Demo Project</h3>

- The project is a simple ReactJS app built with **Vite** (a frontend build tool).
- It lives in the `02 Starting Project/` folder attached to this lecture.

**Project structure overview:**

```
02 Starting Project/
├── index.html
├── package.json                          ← scripts & dependencies
├── vite.config.js
├── .gitignore
├── public/
└── src/
    ├── App.jsx
    ├── main.jsx
    ├── components/
    │   ├── MainContent.jsx
    │   ├── MainContent.test.jsx          ← test file
    │   ├── HelpArea.jsx
    │   └── HelpBox.jsx
    └── test/
        └── setup.js
```

---

<h3 style="color: #27AE60;">Running the Project Locally</h3>

<h4 style="color: #A8E6CF;">Prerequisites</h4>

- **Node.js** must be installed on your system.
- All commands below are run from inside the project folder.

<h4 style="color: #74B9FF;">Available Commands</h4>

| Command | What it does |
|---------|--------------|
| `npm install` | Installs all third-party dependencies (creates `node_modules/`) |
| `npm run dev` | Starts a local development server to preview the app in the browser |
| `npm test` | Runs the automated test suite using **Vitest** |

**Order to follow locally:**
1. Run `npm install` first — must be done before anything else.
2. Then run `npm run dev` to preview the app, or `npm test` to run tests.

---

<h3 style="color: #F39C12;">The Test Suite</h3>

- The project includes **automated tests** written using `@testing-library/react` and **Vitest**.
- Test file: `src/components/MainContent.test.jsx`
- There are **2 tests** inside:

| Test | What it checks |
|------|---------------|
| `should render a button` | Verifies a button element exists in the UI |
| `should show the help area after clicking the button` | Verifies clicking the button shows a help section |

- By default, **both tests pass** when run with `npm test`.
- You don't need to understand the test code — just know that tests exist and can be run automatically.

---

<h3 style="color: #E74C3C;">The Goal: Automate Testing with GitHub Actions</h3>

**The problem with running tests only locally:**
- Developers might forget to run tests before pushing code.
- On a team, you can't guarantee everyone runs tests locally.

**The solution — automate via GitHub Actions:**
- Whenever a new version of the code is **pushed to the remote GitHub repository**, GitHub Actions automatically runs `npm test`.
- If tests fail → the team is notified immediately.
- This is one of the most common real-world uses of GitHub Actions: **Continuous Integration (CI)**.

> **Goal:** Every push to GitHub → automatically run `npm test` → catch broken code early.

---

<h3 style="color: #16A085;">Setting Up Version Control Locally</h3>

**Step 1 — Initialize a Git repository:**
```bash
git init
```

**Step 2 — Note the `.gitignore` file:**
- The project already has a `.gitignore` file.
- It ignores the `node_modules/` folder — this is standard practice.

**Why is `node_modules/` ignored?**

| Reason | Explanation |
|--------|-------------|
| It can be recreated | Anyone can run `npm install` to get it back |
| It's huge | Contains hundreds/thousands of files |
| It shouldn't be in version control | Not your code — it's third-party dependencies |

**Step 3 — Create the initial commit:**
```bash
git add .
git commit -m "Initial commit"
```

---

<h3 style="color: #D35400;">Creating a Remote Repository on GitHub</h3>

- Go to GitHub → **Repositories** → **New**
- Name it anything (e.g. `second-action-react-demo`)
- Can be **public or private** — GitHub Actions works on both
- **Do NOT add any files** (no README, no .gitignore) — because you are connecting it to an existing local repo that already has files
- Click **Create repository** → copy the repository URL

---

<h3 style="color: #2ECC71;">Connecting Local Repository to Remote</h3>

**Step 1 — Link your local repo to the remote:**
```bash
git remote add origin <your-repo-url>
```
- `origin` is just the conventional name/identifier for your remote repository.
- Replace `<your-repo-url>` with the URL copied from GitHub.

**Step 2 — Push your code (first time only):**
```bash
git push --set-upstream origin main
```
- This command creates the link between your **local `main` branch** and the **remote `main` branch**.
- You only need to run this once.
- After this, future pushes are just:
```bash
git push
```

**After pushing:**
- Reload your GitHub repo — all your code is now visible on GitHub.
- No workflow/action is connected yet — that comes in the next step.

---

<h3 style="color: #9B59B6;">Quick Summary</h3>

| Step | Command / Action |
|------|-----------------|
| Install dependencies | `npm install` |
| Preview app locally | `npm run dev` |
| Run tests locally | `npm test` |
| Initialize git | `git init` |
| First commit | `git add . && git commit -m "Initial commit"` |
| Connect to GitHub | `git remote add origin <url>` |
| First push | `git push --set-upstream origin main` |
| Future pushes | `git push` |

> The code is now on GitHub — but no automation is set up yet. The next step is to create a GitHub Actions workflow that automatically runs `npm test` on every push.
