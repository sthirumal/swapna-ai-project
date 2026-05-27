# Upload AOE FAQ files to `aoe-faq-repo` (internal GitHub)

Use this checklist when you want to publish **only** the AOE FAQ–related paths from a local repo (whose `origin` points somewhere else) into the empty internal repository:

**https://github.com/AdobeManagedServices-Innovation/aoe-faq-repo**

This workflow uses a **new folder** and a **fresh `git init`**, so your existing project’s remotes are **not** changed.

---

## What gets uploaded

From the **root** of your source repo, copy these paths and preserve the directory layout:

| Path                                                   | Purpose                                              |
| ------------------------------------------------------ | ---------------------------------------------------- |
| `.cursor/skills/aoe-faq/`                              | Cursor skill (`SKILL.md`, `aoe-faq.md`, `README.md`) |
| `docs/aoe-faq-skill-strategy.md`                       | Strategy doc                                         |
| `docs/aoe-faq-implementation-status-and-next-steps.md` | Status / next steps                                  |

Relative links between the skill and `docs/` assume this layout.

---

## Prerequisites

- Access to **Adobe GitHub** and the **AdobeManagedServices-Innovation** org (SSO as needed).
- **Git** installed locally.
- The **target** GitHub repo exists and is still **empty** (no commits), or you intend to push a new branch and merge per your team’s process.

---

## Ordered steps

1. Open a **terminal**.

2. Set **`SRC`** to the absolute path of the repo that **already contains** these files (for example your `faq-version1` or `swapna-ai-project` root).

3. Set **`DST`** to a **new** directory path (must not reuse an existing git repo you care about; the folder can be created by the next step).

4. Create destination folders:

   ```bash
   mkdir -p "$DST/.cursor/skills" "$DST/docs"
   ```

5. Copy the AOE FAQ content from `SRC` to `DST`:

   ```bash
   cp -R "$SRC/.cursor/skills/aoe-faq" "$DST/.cursor/skills/"
   cp "$SRC/docs/aoe-faq-skill-strategy.md" "$DST/docs/"
   cp "$SRC/docs/aoe-faq-implementation-status-and-next-steps.md" "$DST/docs/"
   ```

6. Go to the new folder:

   ```bash
   cd "$DST"
   ```

7. Initialize a **new** repository:

   ```bash
   git init
   ```

8. _(Optional)_ Add a top-level `README.md` in `$DST` describing the repo for viewers on GitHub.

9. Stage the copied files (and optional `README.md`):

   ```bash
   git add .cursor/skills/aoe-faq docs/aoe-faq-skill-strategy.md docs/aoe-faq-implementation-status-and-next-steps.md
   ```

10. Create the first commit:

    ```bash
    git commit -m "Initial import of AOE FAQ skill and docs"
    ```

11. Set the default branch name to **`main`**:

    ```bash
    git branch -M main
    ```

12. Add the internal repo as **`origin`** (only in **this** new folder):

    ```bash
    git remote add origin https://github.com/AdobeManagedServices-Innovation/aoe-faq-repo.git
    ```

13. Push:

    ```bash
    git push -u origin main
    ```

14. If **`git push` fails**, resolve **authentication** (Adobe SSO, HTTPS personal access token, or SSH key on your GitHub account) with your team’s standard; do not overwrite **`origin`** in your original source repo.

15. In the browser, open the GitHub repo and confirm files and paths look correct.

---

## One copy-paste block (after setting `SRC` and `DST`)

```bash
SRC="/path/to/your/existing/repo"
DST="/path/to/new/aoe-faq-repo-upload"

mkdir -p "$DST/.cursor/skills" "$DST/docs"
cp -R "$SRC/.cursor/skills/aoe-faq" "$DST/.cursor/skills/"
cp "$SRC/docs/aoe-faq-skill-strategy.md" "$DST/docs/"
cp "$SRC/docs/aoe-faq-implementation-status-and-next-steps.md" "$DST/docs/"

cd "$DST"
git init
git add .cursor/skills/aoe-faq docs/aoe-faq-skill-strategy.md docs/aoe-faq-implementation-status-and-next-steps.md
git commit -m "Initial import of AOE FAQ skill and docs"
git branch -M main
git remote add origin https://github.com/AdobeManagedServices-Innovation/aoe-faq-repo.git
git push -u origin main
```

---

## GitHub “Quick setup” screen

This matches GitHub’s **second** option: **“…or create a new repository on the command line”** (folder was not a git repo until `git init`). You can skip GitHub’s example `echo "# aoe-faq-repo" >> README.md` if you already copied real content in step 5.

---

## Later updates

To refresh **this** repo after you change files in `SRC`:

1. Copy the same paths again into `$DST` (or only the files that changed).
2. In `$DST`: `git add -A`, `git commit -m "Describe change"`, `git push`.

Alternatively, treat `$DST` as your working clone of `aoe-faq-repo` and edit there, then push.

---

## What not to do

In your **original** project (the one whose `origin` is another URL), **do not** run `git remote add origin …` for `aoe-faq-repo` if `origin` already exists. Use the **separate folder** workflow above, or add a **second remote** with a different name (for example `aoe-faq`) only if you explicitly want that setup.

---

## Related docs in this repo

- [aoe-faq-implementation-status-and-next-steps.md](aoe-faq-implementation-status-and-next-steps.md) — canonical wiki, sync, governance.
- [.cursor/skills/aoe-faq/README.md](../.cursor/skills/aoe-faq/README.md) — skill usage and wiki sync.
