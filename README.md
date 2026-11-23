# QA & Test Automation Governor (Elite Full-Stack Edition)

[![GitHub Stars](https://img.shields.io/github/stars/Henry-19/qa-test-automation-governor?style=for-the-badge&logo=github)](https://github.com/Henry-19/qa-test-automation-governor)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)
[![skills.sh](https://img.shields.io/badge/skills.sh-Compatible-0a0a0a.svg?style=for-the-badge)](https://skills.sh)

A production-grade AI Agent Skill synthesized via **Meta-Skill Synthesizer** for auditing, auto-refactoring, and end-to-end testing full-stack localhost applications. Combines **Cross-Skill Architectural Alignment** (DB, API Contracts, UI/UX 5-States, Rust/FinOps Performance) with a **6-Layer Automated Test Pyramid** (`tsc`/Build, DB Integrity, Unit/Boundary, API Security/IDOR, and Playwright E2E with zero-console/network-error traps) and an **Autonomous Self-Healing Bug Fix Loop**.

## ⚡ Quick Install

```bash
npx skills add Henry-19/qa-test-automation-governor -g
```

---

## Directory Structure

```text
qa-test-automation-governor/
├── SKILL.md                                  # Core 6-layer QA, cross-skill audit & self-healing rules
├── README.md                                 # Overview, synthesis provenance & usage guide
├── references/
│   └── qa-verification-patterns.md           # Playwright console/5xx trap, API security probe & report templates
└── checklists/
    └── qa-audit.md                           # 15-point full-stack QA & localhost sign-off checklist
```

---

## Synthesis Provenance Matrix

| Source Skill / Repository | Strengths Extracted | Limitations Identified | Mitigations Engineered |
| :--- | :--- | :--- | :--- |
| `omer-metin/skills-for-antigravity@qa-engineering` | 70/20/10 Test Pyramid, Arrange-Act-Assert, zero tolerance for flaky tests and `waitForTimeout` | Lacks cross-skill architectural auditing and autonomous bug-fixing loop for existing vibe-coded projects | Added Section 2 **Cross-Skill Pre-Test Audit & Auto-Refactoring Matrix** and Gate 6 **Autonomous Self-Healing Fix Loop** |
| `alirezarezvani/claude-skills@senior-qa` | Boundary value analysis, API status code matrix, and regression suite structuring | Passive reporting style (lists bugs without fixing them); no production build verification | Mandated `npm run build` / `cargo build` check in Gate 1 and in-place root-cause code patching before re-running tests |
| `vasilyu1983/ai-agents-public@qa-testing-playwright` | Semantic Playwright locator hierarchy (`getByRole`, `getByLabel`) and viewport testing | Does not trap silent browser `console.error`, uncaught `pageerror`, or background `5xx` network failures | Engineered the **Zero-Console & Network-5xx Playwright Fixture** in `references/qa-verification-patterns.md` |

---

## Usage Guide

1. Activate this skill when a project has been coded up to a working localhost state and needs comprehensive architectural refinement, automated test generation, bug fixing, and live verification.
2. Follow the 6-Layer Pipeline in `SKILL.md` (`Static/Prod Build` $\rightarrow$ `DB Integrity` $\rightarrow$ `Unit/Boundary` $\rightarrow$ `API Contract/Security` $\rightarrow$ `Playwright Live Localhost E2E` $\rightarrow$ `Self-Healing Report & Localhost Preview`).
3. Inspect `.specs/06_QA_TEST_REPORT.md` and `checklists/qa-audit.md` before production deployment.
