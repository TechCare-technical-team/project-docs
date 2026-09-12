# GitHub Organization Workflow

## 1. Quick Start

If you are new to the project, follow this workflow:

```text
Issue
  ↓
Create feature/fix branch from dev
  ↓
Develop & Test
  ↓
Pull Request → dev
  ↓
Code Review + CI
  ↓
Merge → dev
  ↓
Integration Testing
  ↓
Stable
  ↓
Pull Request → main
  ↓
Final Review + CI
  ↓
Merge → main
```

### Start a New Task

```bash
git checkout dev
git pull origin dev

git checkout -b feature/user-login
```

Work on your changes, then:

```bash
git add .
git commit -m "feat: add user login"

git push -u origin feature/user-login
```

Open a Pull Request:

```text
feature/user-login → dev
```

After review, CI checks, and approval, merge into `dev`.

Once the integrated system is stable and tested, `dev` can be promoted to `main` through another Pull Request:

```text
dev → main
```

### Golden Rules

```text
DO NOT push directly to main.
DO NOT push directly to dev.
DO NOT develop directly on dev.
```

Develop on feature/fix branches, integrate on `dev`, and release from `main`.

---

# 2. Purpose

This document defines how our team uses GitHub to:

* Organize repositories
* Manage development branches
* Collaborate across teams
* Review code
* Track tasks
* Integrate different project components
* Maintain stable releases

The goal is to keep development consistent, predictable, and easy to maintain without introducing unnecessary bureaucracy.

---

# 3. Organization Structure

Each major project component should have its own repository.

```text
Organization
│
├── mobile
├── backend
├── cv
├── nlp
├── documentation
└── infrastructure
```

### Repository Responsibilities

| Repository       | Responsibility                                          |
| ---------------- | ------------------------------------------------------- |
| `mobile`         | Android / iOS / Flutter application                     |
| back end         | APIs, business logic, authentication, database          |
| `cv`             | Computer Vision models and services                     |
| `nlp`            | NLP models, processing, and services                    |
| `documentation`  | Project documentation and technical specifications      |
| `infrastructure` | Deployment, Docker, CI/CD, infrastructure configuration |

Each repository should have a `README.md` explaining its purpose, setup, and contribution guidelines.

---

# 4. Repository Ownership

Every repository should have a responsible team.

The owning team is responsible for:

* Maintaining the repository
* Reviewing Pull Requests
* Maintaining documentation
* Handling repository-related issues
* Maintaining CI/CD configuration
* Protecting the quality of the codebase

Ownership does not prevent other teams from contributing.

---

# 5. Branching Strategy

We use a two-level integration strategy:

```text
main
  ↑
 dev
  ↑
feature/* / fix/* / refactor/* / docs/*
```

## `main`

`main` contains:

* Stable code
* Tested code
* Release-ready code

`main` should always represent a version that is safe to release.

Changes should reach `main` only through a Pull Request from `dev`.

---

## `dev`

`dev` is the **integration and testing branch**.

Completed work is merged into `dev` before reaching `main`.

It is used to:

* Integrate work from different developers
* Integrate work from different teams
* Test features together
* Detect integration problems
* Verify cross-repository compatibility
* Stabilize the project before release

`dev` should remain usable, but it does not need to be release-ready at all times.

---

## Feature / Fix Branches

All actual development should happen on dedicated branches created from the latest `dev`.

Examples:

```text
feature/user-login
feature/image-upload

fix/login-validation
fix/expired-token

refactor/auth-service

docs/api-documentation

chore/update-dependencies
```

These branches should be short-lived and deleted after they are merged.

---

# 6. Branch Naming Convention

Use:

```text
<type>/<short-description>
```

### Types

| Type       | Usage                     |
| ---------- | ------------------------- |
| `feature`  | New functionality         |
| `fix`      | Bug fixes                 |
| `refactor` | Code restructuring        |
| `docs`     | Documentation             |
| `test`     | Test-related work         |
| `chore`    | Maintenance/configuration |
| `perf`     | Performance improvements  |
| `hotfix`   | Critical production fixes |

Examples:

```text
feature/user-authentication
fix/image-upload-crash
refactor/database-layer
docs/api-documentation
chore/setup-ci
```

Branch names should be:

* Short
* Descriptive
* Lowercase
* Hyphen-separated

---

# 7. Development and Integration

Developers should **never work directly on ****`dev`**.

Instead:

```text
feature/*
fix/*
refactor/*
docs/*
chore/*
```

are created from `dev`.

After completing and locally testing the work, open a Pull Request:

```text
feature/* → dev
```

Once approved and merged, the change becomes part of the shared integration environment.

---

# 8. Integration Testing

After changes are merged into `dev`, the affected teams should verify that the components work together.

For example:

```text
Mobile
   ↓
Backend API
   ↓
CV Service
   ↓
NLP Service
```

Integration testing should verify things such as:

* API compatibility
* Request and response formats
* Authentication
* Data flow
* Error handling
* Cross-service communication
* End-to-end functionality

A feature working correctly in isolation does not necessarily mean it works correctly with the rest of the system.

---

# 9. Bug Fixes

If a bug is found in `dev`, **do not fix it directly on ****`dev`**.

Create a fix branch from the latest `dev`:

```bash
git checkout dev
git pull origin dev

git checkout -b fix/authentication-bug
```

After fixing and testing:

```text
fix/authentication-bug → dev
```

The fix goes through the same review and CI process as any other change.

After merging, the integration testing process continues.

---

# 10. Promoting `dev` to `main`

When the current development cycle is complete and `dev` is stable, it can be promoted to `main`.

Before doing so, verify:

* Intended features are complete
* Critical bugs are fixed
* Integration testing has passed
* CI checks are passing
* No unintended breaking changes exist
* Required documentation is updated

Create:

```text
dev → main
```

through a Pull Request.

After final review and successful CI checks, merge into `main`.

---

# 11. Hotfixes

Hotfixes are reserved for critical production issues that cannot wait for the normal development cycle.

Create the branch from `main`:

```text
main
 ↓
hotfix/critical-auth-bug
 ↓
main
```

After merging the hotfix into `main`, the fix must also be brought back into `dev`:

```text
hotfix/critical-auth-bug
        ↓
      main
        ↓
       dev
```

This prevents the bug from reappearing in future releases.

---

# 12. Commit Convention

Use:

```text
<type>: <description>
```

### Common Types

| Type       | Usage                     |
| ---------- | ------------------------- |
| `feat`     | New functionality         |
| `fix`      | Bug fix                   |
| `refactor` | Code restructuring        |
| `docs`     | Documentation             |
| `test`     | Tests                     |
| `chore`    | Maintenance/configuration |
| `perf`     | Performance improvement   |
| `style`    | Formatting/styling        |

Examples:

```text
feat: add user authentication
feat: add image upload endpoint
fix: handle invalid token
refactor: simplify authentication service
docs: update API documentation
test: add login unit tests
chore: update dependencies
```

Avoid meaningless commits such as:

```text
update
changes
final
final final
fixed stuff
test
asdf
```

---

# 13. Pull Requests

Every meaningful change should be submitted through a Pull Request.

The PR title should clearly describe the change:

```text
feat: add Google authentication
```

A PR description should explain:

```text
## What changed?

Added Google authentication.

## Why?

Users need an easier authentication method.

## How was it implemented?

- Added Google OAuth integration
- Added authentication endpoint
- Added token validation

## Testing

- Tested successful login
- Tested invalid token
- Tested expired token
```

Keep Pull Requests focused.

Avoid combining unrelated work into a single PR.

---

# 14. Code Review

At least **one other team member** should review a Pull Request before it is merged.

Reviewers should consider:

* Correctness
* Readability
* Architecture
* Security
* Performance
* Tests
* Documentation
* Breaking changes

Reviews should be constructive and technical.

Good:

```text
This could cause a race condition when multiple requests
update the same resource. Can we handle this using X?
```

Avoid vague comments such as:

```text
This is wrong.
```

---

# 15. Pull Request Rules

A Pull Request should not be merged if:

* Required CI checks are failing
* Required approvals are missing
* Important review comments are unresolved
* The change introduces an obvious breaking issue
* Important reviewer feedback has not been addressed

Before merging a branch into `dev`, make sure it is based on the latest relevant `dev` state.

Before promoting `dev` to `main`, ensure the integration environment is stable.

---

# 16. Merge Strategy

The default merge strategy is:

```text
Squash and Merge
```

This keeps the target branch history clean.

For example, instead of:

```text
fix
fix again
test
oops
final fix
```

the target branch can contain:

```text
feat: add user authentication
```

Use regular merge commits only when preserving branch history is specifically useful.

---

# 17. Branch Protection

Both `main` and `dev` should be protected.

### `main`

Recommended rules:

* No direct pushes
* Pull Request required
* At least 1 approval
* Required CI checks must pass
* Required conversations must be resolved

### `dev`

Recommended rules:

* No direct pushes
* Pull Request required
* At least 1 approval
* Required CI checks must pass
* Required conversations must be resolved

---

# 18. Issues

GitHub Issues should be used to track actual work.

Issues can represent:

* Features
* Bugs
* Improvements
* Tasks
* Research
* Technical debt

Example:

```text
Title:
Add refresh token support

Requirements:
- Generate refresh token
- Store refresh token securely
- Add refresh endpoint
- Handle expiration
- Add tests
```

Avoid using Issues as general-purpose chat.

---

# 19. Issue Labels

Recommended labels:

```text
feature
bug
enhancement
documentation
research
refactor
testing
security
performance
good-first-issue
blocked
priority-high
priority-medium
priority-low
```

Labels should make the purpose and priority of an issue easy to understand.

---

# 20. Releases and Tags

Stable versions should be tagged.

Examples:

```text
v1.0.0
v1.1.0
v1.1.1
```

Use Semantic Versioning when applicable:

```text
MAJOR.MINOR.PATCH
```

For example:

```text
1.0.0 → Initial release
1.1.0 → New feature
1.1.1 → Bug fix
2.0.0 → Breaking change
```

---

# 21. CODEOWNERS

Important repositories should use a `CODEOWNERS` file when practical.

Example:

```text
* @backend-team
```

CODEOWNERS helps automatically request reviews from the responsible team.

---

# 22. Communication

GitHub should be the source of truth for technical work.

Use:

```text
Issues         → Tasks and bugs
Pull Requests  → Code changes
Projects       → Work tracking
README         → Repository documentation
Docs           → Detailed technical documentation
```

Team chat can be used for quick communication, but important technical decisions should eventually be documented in GitHub.

---

# 23. General Rules

1. **Never push directly to ****`main`****.**
2. **Never push directly to ****`dev`****.**
3. **Create development branches from ****`dev`****.**
4. **Merge completed work into ****`dev`**** first.**
5. **Use ****`dev`**** for integration and testing.**
6. **Promote only stable, tested code from ****`dev`**** to ****`main`****.**
7. **Keep Pull Requests focused.**
8. **Write meaningful commit messages.**
9. **Review code before merging.**
10. **Never commit secrets.**
11. **Document changes that affect other teams.**
12. **Do not silently introduce breaking API changes.**
13. **Keep repositories clean and focused.**
14. **Document important project-wide decisions.**
