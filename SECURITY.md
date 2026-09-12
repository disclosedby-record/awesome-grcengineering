# Security Policy

## What this repository is

`awesome-grcengineering` is a curated, documentation-only repository. `README.md`
is the canonical content source for the live cheat sheet at
[cheatsheet.grc.engineering](https://cheatsheet.grc.engineering), which fetches
this file at runtime. It also doubles as an [Awesome List](https://awesome.re).

It ships **no application code, no packages and no release artifacts**. Its
attack surface is therefore narrower than a typical software project, and is
limited to:

- **The content itself** — outbound links in `README.md`, which downstream
  readers and the cheat sheet site follow.
- **The CI/CD supply chain** — the GitHub Actions workflows under
  `.github/workflows/`, which run with repository credentials.
- **The consuming site** — `README.md` intentionally preserves raw HTML
  (`<br>`, `<u>`, `<em>`, `<span class="...">`) through to the rendered
  cheat sheet, so markdown content is an input to a live web page.

## Supported versions

This repository has no releases or versions. Only the current state of the
default branch (`main`) is supported. Fixes land on `main`.

## Reporting a vulnerability

**Report privately. Do not open a public issue for a security problem.**

Use [GitHub Security Advisories](https://github.com/grcengineering/awesome-grcengineering/security/advisories/new)
to open a private report on this repository. If you cannot use that, contact a
repository administrator through the
[GRC Engineering Discord](https://discord.gg/CG6EDDbG4B).

Please include what you found, where (file and line, or the specific link), and
why it is a problem. A proof of concept helps but is not required.

### What we will do

- **Acknowledge** your report within 7 days.
- **Assess and triage** it, and tell you whether we consider it in scope.
- **Fix** accepted reports on `main`, and credit you in the advisory unless you
  ask us not to.
- **Publish** an advisory once a fix has landed.

We do not operate a bug bounty and cannot offer payment.

## What is in scope

- A link in `README.md` that points to malware, a hijacked or expired domain, a
  typosquat, or a resource that has been taken over since it was added.
- Markdown or embedded HTML in `README.md` crafted to inject script or otherwise
  attack the rendering site (the raw-HTML passthrough above makes this real).
- Any weakness in the GitHub Actions workflows — script injection, an unpinned
  or compromised action, credential exposure, over-broad `permissions`, or
  misuse of `pull_request_target`.
- Exposed secrets or credentials anywhere in the repository or its history.
- Weaknesses in the supply-chain controls configured under `.sscsb/`.

## What is out of scope

- Vulnerabilities in the **third-party tools, sites and projects this list links
  to**. Report those to their own maintainers. Do tell us if a linked project is
  compromised or abandoned so we can remove or annotate the entry.
- The security of **cheatsheet.grc.engineering** itself, beyond content this
  repository supplies. Report site issues to that project.
- Editorial disagreement about whether a resource belongs in the list. That is a
  normal pull request or issue, not a security report.

## How this repository is secured

Supply-chain controls are managed with
[`sscsb`](https://github.com/p4gs/sscs-bootstrapper) and declared in
`.sscsb/config.toml`. Machine-readable posture is published in
`security-insights.yml` ([OpenSSF Security Insights](https://github.com/ossf/security-insights-spec)).

Active controls include:

- **Secret scanning** — TruffleHog in pre-commit, commit-msg and pre-push hooks
  and in CI, plus GitHub native secret scanning with push protection.
- **Commit signing** — human-only signature policy on protected branches, with
  approved signers recorded in `.sscsb/policy/signers.toml`.
- **Pinned CI** — every GitHub Action pinned to a full commit SHA, with
  least-privilege `permissions` and StepSecurity Harden-Runner egress auditing.
- **Static analysis** — OpenGrep and CodeQL (`actions`) analysing the workflow
  files, which are this repository's only executable surface.
- **Vulnerability scanning and SBOM** — Trivy, OSV-Scanner and a Syft SBOM.
- **Automated updates** — Renovate with digest pinning, plus Dependabot security
  updates.
