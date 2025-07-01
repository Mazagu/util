# 🧠 Semantic Versioning (SemVer) – Complete Guide

**Semantic Versioning** is a versioning system that conveys **meaning** about the underlying changes in a release through its version number. It helps teams **communicate changes**, manage **dependencies**, and avoid **unexpected breakages**.

---

## 🔢 Format: `MAJOR.MINOR.PATCH`

Each version number is made of **three digits**, for example:

```
1.4.2
```

| Part   | Purpose                                | Example     |
|--------|----------------------------------------|-------------|
| MAJOR  | Breaking changes                       | `2.0.0`     |
| MINOR  | Backward-compatible feature additions  | `1.5.0`     |
| PATCH  | Backward-compatible bug fixes          | `1.5.3`     |

---

## 🧭 When to Increment?

| Scenario                                     | Example Before → After |
|---------------------------------------------|-------------------------|
| Breaking API contract                       | `1.4.0 → 2.0.0`         |
| Adding new backward-compatible features     | `1.4.0 → 1.5.0`         |
| Fixing bugs or performance improvements     | `1.4.0 → 1.4.1`         |

---

## 📌 Pre-release Tags and Build Metadata

You can add optional labels to denote **pre-release** or **build metadata**:

- Pre-release: use hyphen (`-`)
```
1.4.0-alpha.1
1.4.0-beta
```

- Build metadata: use plus (`+`)
```
1.4.0+build.123
```

> Pre-releases are considered **lower precedence** than the associated final release.

## 🚀 What is a Release Candidate (RC)?

A **Release Candidate** is a version that is **feature-complete** and potentially ready to become a final release — unless significant bugs emerge. It signals the final stages of testing before the stable release.

### 🔢 Format

RC versions use the **pre-release syntax** with a hyphen (`-`) followed by `rc` (short for "release candidate") and a number:

```
1.0.0-rc.1
1.0.0-rc.2
```

Each increment reflects an iteration toward stabilization.

---

## 📊 SemVer Precedence Rules

Pre-release versions have **lower precedence** than the associated final release:

| Version         | Meaning                              |
|----------------|---------------------------------------|
| `1.0.0-alpha.1` | Early prototype version               |
| `1.0.0-beta.1`  | More stable than alpha, less than RC |
| `1.0.0-rc.1`    | Release candidate                    |
| `1.0.0`         | Final release                        |

So:
```
1.0.0-alpha.1 < 1.0.0-beta.1 < 1.0.0-rc.1 < 1.0.0
```

---

## ✅ When to Use Release Candidates

- All major functionality is implemented.
- Bugs from earlier testing phases (alpha, beta) are fixed.
- You are ready for **final validation and user acceptance testing**.

---

## 👀 Best Practices

- Use RCs to **let internal teams or early adopters validate stability**.
- Clearly document what’s changed between `rc.1`, `rc.2`, etc.
- Avoid introducing **new features** during RC phase — it should be focused on **stabilization only**.

---

## 🔗 Example Workflow

1. Release `1.0.0-alpha.1` → internal development
2. Release `1.0.0-beta.1` → early user feedback
3. Release `1.0.0-rc.1` → bug fixes and performance stabilization
4. Release `1.0.0` → final stable version

---

## 🏷️ Version Tags in Package Managers

| Tool       | RC Usage Example         |
|------------|--------------------------|
| npm        | `npm install mylib@rc`   |
| pip        | `pip install mylib==1.0.0rc1` |
| Maven      | Use `<version>1.0.0-RC1</version>` |
| Cargo      | `1.0.0-rc.1` in `Cargo.toml` |

---

## 🔐 Why Semantic Versioning Matters

- ✅ **Predictability** for clients using your software
- ✅ **Safe upgrades** without breaking things
- ✅ **Clear contract** of stability and changes
- ✅ **Automation-friendly** for dependency managers

---

## 🚫 Common Mistakes to Avoid

- ❌ Publishing breaking changes as a **minor** or **patch**
- ❌ Using **v1.0.0** without API stability
- ❌ Skipping semantic versioning altogether

---

## 📦 SemVer in Package Managers

- **npm** (Node.js): uses SemVer with carets (`^`) and tildes (`~`)
- **pip** (Python): supports SemVer via constraints like `>=1.2.0, <2.0.0`
- **Composer** (PHP): strictly encourages SemVer
- **Cargo** (Rust), **Go Modules**, **Maven**: all follow SemVer principles

---

## 📘 Best Practices

- Start with `0.y.z` if your project is still unstable.
- Only release `1.0.0` when your API is stable and documented.
- Communicate breaking changes with release notes or changelogs.
- Use tooling like:
  - [Semantic Release](https://semantic-release.gitbook.io/) for automated versioning
  - [Commitizen](https://commitizen-tools.github.io/commitizen/) for standard commits
  - [Conventional Commits](https://www.conventionalcommits.org/) to link commits with version bumps

---

## 🧭 Summary Table

| Action                        | SemVer Change | Backward Compatible? |
|------------------------------|---------------|-----------------------|
| Breaking API change          | MAJOR         | ❌ No                 |
| New feature (compatible)     | MINOR         | ✅ Yes                |
| Bug fix or small patch       | PATCH         | ✅ Yes                |

---

## 🔗 Resources

- [semver.org](https://semver.org)
- [Keep a Changelog](https://keepachangelog.com)
- [Conventional Commits](https://www.conventionalcommits.org)
- [npm semver calculator](https://semver.npmjs.com/)

---
