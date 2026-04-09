# Release Process

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

This document defines the standard process I use for creating, testing, and publishing releases of the OLAMIP specification and related tooling. For now, all releases are managed and owned by me.

## 1. Versioning

I follow [Semantic Versioning 2.0.0](https://semver.org/):

- **MAJOR** version when I make incompatible changes to the format or protocol.  
- **MINOR** version when I add new functionality or sections in a backward‑compatible way.  
- **PATCH** version when I fix bugs, correct wording, or clarify rules without changing behavior.  

Example: `1.0.0` → `1.0.1` (patch), `1.1.0` (minor), `2.0.0` (major).

## 2. Branching Model

I use a simple Git workflow:

- `main` — the stable, always release‑ready branch.  
- Feature or fix branches (e.g., `spec/delta-formats`, `fix/tag-rules`) — short‑lived branches for changes.  
- Release branches (e.g., `release/v1.1.0`) — created immediately before publishing a release.  

## 3. Prerequisites for a Release

Before starting a release, I ensure:

- All changes planned for this release are merged into `main`.  
- Any automated checks (linting, schema validation, build scripts) pass.  
- All issues and notes that belong in this release are closed or documented.  
- `CHANGELOG.md` is updated with clear, human‑readable entries for this version.  
- Documentation is up to date, including:  
  - `file-format-specification.md`  
  - `olamip-delta-file-format-specification.md`  
  - `docs/introduction.md`  
  - `faq.md`  
- Example `olamip.json` and `olamip-delta.json` files still validate against the latest rules.  

## 4. Creating a Release Candidate

1. Create a release branch from `main`:

   ```bash
   git checkout main
   git checkout -b release/vX.Y.Z
   ```

2. Update version numbers (if used in tooling, scripts, or tagged assets, not just spec text).  
3. Update `CHANGELOG.md` with a header for the release and notes for this version.  
4. Commit the changes:

   ```bash
   git add .
   git commit -m "chore(release): prepare v1.1.0"
   ```

5. Push the branch:

   ```bash
   git push origin release/vX.Y.Z
   ```

6. Run local or CI validation again and ensure everything passes.  

## 5. Testing the Release Candidate

Because only one person manages releases, my personal testing is the main gate:

- I validate that example `olamip.json` and `olamip-delta.json` files still conform to the schema.  
- I verify that all examples in the two specification files still match the described rules.  
- I read through the updated sections in the spec and docs to catch any inconsistencies.  

If I find significant issues:

- I fix them on the `release` branch.  
- I update `CHANGELOG.md` and commit again.  
- I re‑validate before proceeding.  

## 6. Finalizing the Release

Once the candidate is stable:

1. Tag the release in Git:

   ```bash
   git tag -a vX.Y.Z -m "Release v1.1.0"
   git push origin vX.Y.Z
   ```

2. Create a GitHub release (or equivalent):

   - Use the tag `vX.Y.Z`.  
   - Paste the relevant section of `CHANGELOG.md` into the release description.  
   - Attach any generated artifacts (e.g., schema files, CLI tools, snapshot tarballs) if applicable.  

## 7. Post‑Release Steps

After the release is published:

- I update internal notes or roadmaps about what has shipped.  
- I move any items that were deferred into a backlog or notes for the next release.  
- If there is an outreach channel (e.g., Twitter, newsletter, GitHub Discussions), I optionally announce the new version and highlight changes.  

## 8. Governance and Oversight

Even with a single maintainer, I keep a governance layer in place:

- Any change that affects the core schema, required fields, or protocol version must be clearly documented in the changelog.  
- Significant changes to the release process itself (e.g., moving to a new versioning scheme or branching model) must be documented in a short governance note or decision record under `governance/decision-records/`.  

## 9. Rolling Back or Patching

If a release contains a critical bug:

- I create a patch release as soon as possible (e.g., `v1.1.1`).  
- I clearly document the fix in `CHANGELOG.md`.  
- If necessary, I update documentation or communication channels to note that the previous version is deprecated or discouraged.  

## 10. Automation and Future Scaling

Wherever possible, I automate:

- Repetitive steps such as:  
  - Checking JSON validity.  
  - Validating example files against the schema.  
  - Generating or linting changelog‑style text.  
- I keep the release process simple so that, if contributors later join, I can hand off or share the same steps.  

This release process is owned and maintained by me and can be updated via the normal workflow whenever I improve the process itself.
