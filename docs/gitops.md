# GitOps

[Back](../README.md)

- [GitOps](#gitops)
  - [GitOps](#gitops-1)

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
