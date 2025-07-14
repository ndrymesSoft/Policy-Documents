# GitFlow Policy Document

## Purpose

This document defines our GitFlow branching strategy and policies for development workflows.

## Core Branches

* `main`: Production-ready code only
* `develop`: Integration branch for completed features
* `release/x.y.z`: Release preparation branches
* `hotfix/x.y.z`: Urgent production fixes

> Note: The `dev2` branch is deprecated. Use `develop` for all development work.

## Development Workflow

### 1. Feature Development

```bash
git checkout -b feature/feature-name develop
```

* Create feature branches from `develop`
* Submit PRs back to `develop`

### 2. Release Process

```bash
git checkout -b release/x.y.z develop
```

* Create release branch when `develop` is ready
* Perform QA and final adjustments

### 3. Release Deployment

1. Merge release branch to `main`
2. Tag release: `git tag -a x.y.z -m "Release x.y.z"`
3. Merge back to `develop`
4. Delete release branch

### 4) Hotfixes

```bash
git checkout -b hotfix/x.y.z+1 main
```

* Fix urgent production issues
* Merge to both `main` and `develop`

## Naming Conventions

* Features: `feature/xyz`
* Bugfixes: `bugfix/xyz`
* Releases: `release/x.y.z`
* Hotfixes: `hotfix/x.y.z+1`

## Key Policies

* No direct commits to `main` or `develop`
* All PRs require peer review
* PR titles must include ticket ID
* QA approval required for release merges

## CI/CD Rules

* `main` merges → production builds
* `develop`/`release/*` merges → staging builds
