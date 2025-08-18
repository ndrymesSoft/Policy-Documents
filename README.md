# **GitFlow + Custom Client Deployment Policy**

## **Purpose**

This document defines our branching strategy and deployment policies for both general and **client-specific** development workflows. It ensures structured deployments, support for customizations, and clean release processes.

---

## **Core Branches**

* `main`: Production-ready, general-purpose code only
* `develop`: Integration branch for all internal features
* `release`: The Branch for all releases that are either QA already deployed 
* `release/x.y.z`: Release preparation for general production
* `hotfix/x.y.z`: Urgent general production fixes

---

## **Client-Specific Branches**

* `client/<client-name>/develop`: Development base for client-specific customizations
* `release/<client-name>/x.y.z`: Client-specific release branch
* `hotfix/<client-name>/x.y.z`: Urgent fixes for client deployments
* `feature/client/<client-name>/<feature-name>`: Specific feature for a client

> Example: `client/visabank/develop`, `release/nestbank/v2.1.0`, `feature/client/gtbank/overdraft-limit`

---

## **Development Workflow**

### 1. Internal Feature Development

```bash
git checkout -b feature/cash-reconciliation develop
```

* Branch from `main` for core platform features
* Submit PRs to `develop`
* On Merge, submit PRs from `feature-branch` to `release`
* Merge the the PR to `release` for QA testing
---

### 2. Client-Specific Feature Development

```bash
git checkout -b feature/client/visabank/custom-tax-calculation client/visabank/develop
```

* Use `client/<client-name>/develop` as base
* Custom logic must not leak into `develop` or `main`
* Submit PRs to corresponding `client/<client-name>/develop`

---

## **Release Process**

### 3. General Release

```bash
git checkout -b release/v3.2.0 develop
```

1. QA and stakeholder sign-off
2. Tag the release:

   ```bash
   git tag -a v3.2.0 -m "General Release v3.2.0"
   ```
3. Merge to `main`
4. Merge back to `develop`
5. Delete the release branch

---

### 4. Client-Specific Release

```bash
git checkout -b release/visabank/v2.3.0 client/visabank/develop
```

1. QA with client-specific test cases
2. Tag the release:

   ```bash
   git tag -a visabank-v2.3.0 -m "Client Release: VisaBank"
   ```
3. Deploy to client’s production
4. Merge back to `client/visabank/develop` (optional)

---

## **Cherry-Pick Deployment Strategy**

> Use when you need to deploy a selective fix or feature without releasing all of `develop`.

```bash
git checkout -b selective-release-v3.2.1 main
git cherry-pick abc123  # commit hash of desired change
```

* Test this branch independently
* Tag and deploy as a minor patch
* Document cherry-picked commits in the release note

---

## **Hotfixes**

### General Hotfix

```bash
git checkout -b hotfix/v3.2.1 main
```

1. Apply urgent fix
2. Test and deploy
3. Merge into `main` and `develop`
4. Tag as `v3.2.1`

---

### Client-Specific Hotfix

```bash
git checkout -b hotfix/visabank/v2.3.1 release/visabank/v2.3.0
```

1. Apply urgent fix for the client only
2. Deploy and tag:

   ```bash
   git tag -a visabank-v2.3.1 -m "Hotfix for VisaBank"
   ```
3. Merge into `client/visabank/develop` (optional)

---

## **Naming Conventions**

| Purpose         | Format                                  |
| --------------- | --------------------------------------- |
| General Feature | `feature/payment-retry`                 |
| Client Feature  | `feature/client/gtbank/ledger-override` |
| General Release | `release/x.y.z`                         |
| Client Release  | `release/<client-name>/x.y.z`           |
| General Hotfix  | `hotfix/x.y.z+1`                        |
| Client Hotfix   | `hotfix/<client-name>/x.y.z+1`          |

---

## **Key Policies**

* No direct commits to `main`, `develop`, or any client branch
* All PRs require at least one peer review
* PR titles must include a JIRA ticket or short description
* Client-specific features must remain isolated in their branches
* Every release must include:

  * Tag
  * Change summary
  * Cherry-picked commits (if any)
  * Custom logic (if applicable)

---

## **CI/CD Rules**

* `main` → triggers production deployment
* `develop` → triggers staging builds
* `client/*/develop` or `release/*` → triggers client UAT or prod build
* Cherry-pick or hotfix branches → manual deploy after test

---
