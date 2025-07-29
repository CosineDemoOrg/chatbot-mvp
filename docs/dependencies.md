# Dependency Management & Automated Updates

This repository uses **Dependabot** and a CI workflow to ensure all PHP (Composer) and JavaScript (Yarn/NPM) dependencies are kept up to date and secure.

## How Dependabot Works

- **Automated Update PRs:**  
  Dependabot monitors all Composer and NPM dependencies within `/core` and all `/modules/contrib/**` subdirectories.
- **Schedule:**  
  Checks for updates weekly.
- **Pull Requests:**  
  - Updates are grouped by dependency type (production vs development).
  - Security updates are prioritized and flagged.
  - PRs are automatically rebased if conflicts are detected.

## Where to Look

- **Update PRs:**  
  - Watch for incoming PRs from `dependabot` in your GitHub pull requests tab.
  - PR titles will reference the dependency name and type.
- **Security Alerts:**  
  - Dependabot will alert in the GitHub Security tab if a known vulnerability is detected.

## CI Automation: Dependency Audit

- Every push and PR runs a dependency audit via GitHub Actions (`.github/workflows/dependency-audit.yml`):
  - **PHP/Composer:**  
    - Validates all composer.json files in `/core` and `/modules/contrib/**`
    - Fails if any direct dependency is outdated or if validation fails.
  - **Node/Yarn:**  
    - Installs JS deps in `/core`
    - Fails if any moderate-or-higher vulnerability is found (`yarn npm audit --groups dependencies --level moderate`).

- **Red Build?**  
  - Review the Actions tab for logs.
  - **Composer:**  
    - Run `composer update` (core or affected contrib module), commit the updated `composer.lock`.
  - **Yarn:**  
    - Run `yarn upgrade` in `/core`, commit the new `yarn.lock`.

## Resolving Audit Failures

- **For Composer:**  
  - Update dependencies as needed, commit both `composer.json` and `composer.lock`.
  - For contrib modules, run the update from the module's directory.
- **For Yarn:**  
  - Run `yarn upgrade` in `/core`, resolve conflicts, commit `yarn.lock`.

## Notes

- No manual editing of `.github/dependabot.yml` or workflow files is required unless audit scope changes.
- You do **not** need to manually monitor for new versions—Dependabot and CI will do it for you.

---
For more, see:
- [Dependabot documentation](https://docs.github.com/en/code-security/supply-chain-security/keeping-your-dependencies-updated-automatically)
- [Yarn audit docs](https://classic.yarnpkg.com/en/docs/cli/npm-audit/)
- [Composer validate docs](https://getcomposer.org/doc/03-cli.md#validate)