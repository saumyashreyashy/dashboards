# Dashboard Git Version Control Guide

Reference: [Databricks Docs — Version control dashboards with Git](https://docs.databricks.com/aws/en/dashboards/automate/git-support)

---

## Prerequisites

1. **A Databricks Git folder** linked to a remote repository (GitHub, GitLab, Bitbucket, etc.)
2. **Git credentials** configured in Databricks:
   - Go to **Settings → User Settings → Linked Accounts**
   - Click **Link Git account**
   - Select your Git provider and enter your username + personal access token (PAT)
   - For GitHub PATs: GitHub → Settings → Developer Settings → Personal access tokens → Tokens (classic) → generate with `repo` scope

---

## 1. Add a Dashboard to Version Control

### New dashboard
Create the dashboard directly inside a Databricks Git folder — it's tracked from the start.

### Existing dashboard
1. Open the **Workspace** file browser
2. Find the dashboard
3. Right-click → **Move**
4. Select a Databricks Git folder as the destination

The dashboard is serialized as a `.lvdash.json` file inside the Git folder.

> **Note:** Git tracks the **draft** dashboard — no publishing required. Publishing and scheduling configurations (warehouse, schedule) are NOT tracked by Git.

---

## 2. Commit and Push Changes

Every time you edit the dashboard draft, the `.lvdash.json` file updates automatically.

### From the Databricks UI
1. Open the **Workspace** file browser
2. Navigate to the Git folder containing the dashboard
3. Click the **Git branch** button (top-right of the folder)
4. Review the changed files
5. Enter a descriptive commit message
6. Click **Commit & Push**

### From the Git folder dialog
1. Click the branch name in the folder header
2. The dialog shows uncommitted changes
3. Write a commit message and push

---

## 3. Revert to a Previous Version

### Discard uncommitted changes (revert to last commit)
1. Open the Git folder dialog (click the branch name)
2. Click the **3-dot menu (⋮)**
3. Select **Discard Changes** to revert all local changes to the last committed state

### Revert to an older commit
1. Open the Git folder dialog
2. Click the **3-dot menu (⋮)** → **Reset**
3. Choose the commit to reset to

> **Warning:** Resetting is destructive — it discards all changes after the selected commit.

---

## 4. Work on a Feature Branch

1. Open the Git folder dialog
2. Click the branch dropdown → **Create branch**
3. Name your branch (e.g., `feature/new-charts`)
4. Make dashboard changes — they're isolated to this branch
5. Commit and push when ready
6. Submit a **pull request** in your Git provider (GitHub, GitLab, etc.) to merge into `main`

---

## 5. Pull Latest Changes from Remote

1. Open the Git folder dialog
2. Click **Pull** to fetch and merge the latest changes from the remote branch

> Pull before starting new work to avoid merge conflicts.

---

## 6. Deploy to Production

Recommended workflow:

| Phase | Steps |
|-------|-------|
| **Development** | Work on feature branches in personal Git folders. Commit and push changes. |
| **Review** | Submit pull requests. Team reviews changes in the Git provider. |
| **Deployment** | Create a Git folder on the `main`/`production` branch in a shared (non-user) folder. Pull latest changes. Publish dashboards from this folder. |
| **Lockdown** | Remove edit access from the production folder. Updates only come through Git merges. |

---

## Important Limitations

- **Switching branches is destructive**: Dashboards that don't exist on the target branch are deleted. If you switch back, they reappear with **new URLs and IDs**, breaking published links, bookmarks, and API integrations.
- **Git doesn't track**: Publishing status, warehouse selection, schedule configuration, or sharing settings.
- **Git-based jobs** (referencing Git URLs instead of workspace asset IDs) don't work with dashboards.
- **No built-in CI/CD sync**: Databricks doesn't auto-pull from remote — set up CI/CD automation to pull updates and publish dashboards after sync.

---

## Quick Reference

| Task | Where |
|------|-------|
| Set up Git credentials | Settings → User Settings → Linked Accounts |
| Move dashboard to Git folder | Workspace → Right-click dashboard → Move |
| Commit & push | Git folder → Branch button → Commit & Push |
| Discard changes | Git folder → ⋮ menu → Discard Changes |
| Reset to older commit | Git folder → ⋮ menu → Reset |
| Create branch | Git folder → Branch dropdown → Create branch |
| Pull latest | Git folder → Pull |
| Publish (after deploy) | Dashboard → Share → Publish |
