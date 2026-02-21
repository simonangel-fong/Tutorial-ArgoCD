# GitOps

[Back](../README.md)

- [GitOps](#gitops)
  - [GitOps](#gitops-1)
  - [Git Repository Structure](#git-repository-structure)
  - [Branchin Strategies](#branchin-strategies)
    - [Trunk-based development Structure](#trunk-based-development-structure)
    - [GitHub Flow Structure](#github-flow-structure)
    - [GitFlow Structure](#gitflow-structure)
  - [Lab: Git Flow](#lab-git-flow)

---

## GitOps

- `GitOps`
  - a set of practices that uses `Git` repositories as the **"single source of truth**" for **infrastructure** and **application** configurations.
- By applying DevOps best practices, automates deployments, ensuring consistent, reproducible environments.
  - such as like version control, collaboration, and CI/CD

- Key Principles of GitOps
  - **Declarative Infrastructure**:
    - The **desired state** of the entire system is **defined in code** (Infrastructure as Code - IaC) and **stored** in a `Git repository`.
    - **Version Controlled**:
      - `Git` serves as the **single source of truth**, allowing all changes to be **tracked**, reviewed, and audited.
    - **Automated Synchronization**:
      - Any changes merged into the Git repository are **automatically applied** to the target environment by tools like Argo CD or Flux.
    - **Drift Detection**:
      - If the actual system state **diverges from the desired state** in Git, the `GitOps operator` **detects** this and **corrects** it.

- Benefits of GitOps
  - **Increased Security**:
    - Reduced need to manage manual access to production environments.
  - **Faster, Reliable Deployments**:
    - Automation reduces manual errors and accelerates delivery speeds.
  - **Faster Recovery**:
    - Because the entire state is in Git, reverting to a previous version is as simple as a git revert.
  - **Improved Collaboration**:
    - Teams can use pull requests for infrastructure changes, facilitating better visibility and control.

---

## Git Repository Structure

- Best Practices
  - use a **single repo** per app or env
  - use **branches** to manage different **stages** of the development and deployment process.
  - Store **configuration data** in a separate directory from **application code**.
  - Use git submodules to manage shared configuration data.

---

- Example:

- repo
  - app1/
    - base/
    - dev/
    - prod/
    - stage/
  - app2/
    - base/
    - dev/
    - prod/
    - stage/
  - common/
    - base/
    - dev/
    - prod/
    - stage/

> base dir: contains the base configuration for each env
> dev/prod/stage dir: contains the overrides and customizations

---

## Branchin Strategies

- `Trunk-based development (TBD)`
  - a branching strategies where developers merge **small**, **frequent updates** into a single "`trunk`" or `main branch`, rather than using long-lived feature branches.
  - good for small teams or projects requiring fast interation.

- `Gitflow`
  - a branching strategies that is designed for managing **large projects** with scheduled, versioned release cycles.
  - uses two permanent, parallel branches along with dedicated branches for `features`, `releases`, and `hotfixes` to manage code isolation and stability.
  - two permanent, parallel branches:
    - `main`: production-ready
    - `develop`: integration

- `GitHub Flow`
  - a branching strategies that is designed for teams that **deploy frequently**
  - `main branch`: always stable and production-ready.
  - `feature branches`:
    - used for changes
    - reviewed via Pull Requests, merged into main, and deployed immediately.

---

### Trunk-based development Structure

- Example

- repo
  - app1/
    - app1.yaml
    - README.md
  - app2/
    - app2.yaml
    - README.md
  - common/
    - common.yaml
    - README.md
  - kustomization.yaml
  - README.md

---

### GitHub Flow Structure

- Example

- repo
  - app1/
    - app1.yaml
    - README.md
  - app2/
    - app2.yaml
    - README.md
  - common/
    - common.yaml
    - README.md
  - kustomization.yaml
  - README.md

---

### GitFlow Structure

- Example
- repo
  - app1/
    - develop
    - feature/add-new-feature
    - feature/fix-bug
  - app2/
    - develop
    - feature/add-new-feature
    - feature/fix-bug
  - common
    - develop
    - feature/add-new-feature
    - feature/fix-bug
  - hotfix
    - app1
    - app2
  - release
    - app1
    - app2
  - README.md
  - .gitignore

---

## Lab: Git Flow

```sh
mkdir myapp
cd myapp

git init

# create main branch, create files, and commit
git branch -m main

touch app.yaml
git add app.yaml

git commit -m "init commit"
# [main (root-commit) 21970b5] init commit
#  1 file changed, 0 insertions(+), 0 deletions(-)
#  create mode 100644 app.yaml

# create dev branch
git checkout -b develop
# Switched to a new branch 'develop

# create feature branch
git checkout -b feature/my-new-feature
# Switched to a new branch 'feature/my-new-feature'

# edit file
tee app.yaml<<EOF
feature: my-new-feature
EOF

# commit at feature branch
git add app.yaml
git commit -m "Add my new feature"
# [feature/my-new-feature 57f8c93] Add my new feature
#  1 file changed, 1 insertion(+)

# merge feature to dev branch
git checkout develop
# Switched to branch 'develop'
git merge --no-ff feature/my-new-feature
# Merge made by the 'ort' strategy.
#  app.yaml | 1 +
#  1 file changed, 1 insertion(+)

# create release branch
git checkout -b release/my-new-feature
# Switched to a new branch 'release/my-new-feature'
# update the file before releasing
tee app.yaml<<EOF
feature: my-new-feature-release
EOF
# commit
git add app.yaml
git commit -m "release: my new feature"
# [release/my-new-feature f8bdc56] release: my new feature
#  1 file changed, 1 insertion(+), 1 deletion(-)

# sync develop branch with release branch after the update in release branch
git checkout develop
# Switched to branch 'develop'
git merge --no-ff release/my-new-feature
# Merge made by the 'ort' strategy.
#  app.yaml | 2 +-
#  1 file changed, 1 insertion(+), 1 deletion(-)

# sync main branch
git checkout main
# Switched to branch 'main'
git merge --no-ff develop
# Merge made by the 'ort' strategy.
#  app.yaml | 1 +
#  1 file changed, 1 insertion(+)
```

> 1. build feature: `feature`
> 2. merge to develop: `feature` -> `develop`
> 3. merge, and final update for release: `develop` -> `release`
> 4. merge to develop: `release` -> `develop`
> 5. merge to main: `develop` -> `main`
