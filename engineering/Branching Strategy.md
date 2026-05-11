# Git Branching Strategy

Last Updated: May 12, 2024

## 1. Branch Model

Git Flow model with main branches:

- **main**: Production releases
- **develop**: Integration branch
- **feature**: New features
- **release**: Release preparation
- **hotfix**: Critical fixes

## 2. Main Branches

### 2.1 Main

- Production code only
- Tagged with versions
- Merge from release
- Merge from hotfix
- Protected branch
- Requires review

### 2.2 Develop

- Integration branch
- Latest development
- Merge from features
- Release merges
- Base for features
- Should be stable

## 3. Supporting Branches

### 3.1 Feature Branches

- Naming: feature/feature-name
- From: develop
- Merge back: develop
- Pull request required
- Code review required
- Delete after merge

### 3.2 Release Branches

- Naming: release/v1.2.0
- From: develop
- Merge: main and develop
- Bug fixes only
- Version update
- Final testing

### 3.3 Hotfix Branches

- Naming: hotfix/issue-name
- From: main
- Merge: main and develop
- Critical fixes only
- Urgent release
- Tagged version

## 4. Pull Requests

- Required for all branches
- Code review mandatory
- CI/CD must pass
- Squash commits
- Delete branch after merge
- Link to issues

## 5. Contact

- Git Support: git@lutervyn.com
- DevOps: devops@lutervyn.com
