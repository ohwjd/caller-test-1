# mei-friend automation setup - caller template

This is a **GitHub repository template** for setting up a caller repository that works with [mei-friend](https://mei-friend.github.io)'s GitHub Actions automation.

To use it, click **"Use this template"** on GitHub to create your own repository — do not fork and edit this template directly. Then add your MEI files and trigger work packages from mei-friend.

For the full documentation, see [Automation in mei-friend](https://mei-friend.github.io/docs/advanced/automation/).

---

## Role in the automation architecture

A caller repository is the user's own GitHub repository where MEI files are stored and versioned. It contains a single small workflow file (`.github/workflows/caller.yml`) that:

1. Receives a `workflow_dispatch` event from mei-friend (sent via the GitHub API when the user clicks **"Run workflow"** in the GitHub Actions panel).
2. Forwards the event — together with the work package ID, file path, parameters, and commit message — to a **central repository**.
<!-- via a `uses:` reference (a reusable workflow invocation). -->
3. The central repository runs the requested processing on the file and commits any results back to this caller repository.

By default, `caller.yml` points to [`mei-friend/automation`](https://github.com/mei-friend/automation), the generic central repository. It can be re-pointed to any other central repository (e.g. a project-specific one such as [E-LAUTE](https://github.com/e-laute/automation)) — see [Switching central repository](#switching-central-repository) below.

---

## Quick start

1. Click **"Use this template"** on this repository's GitHub page to create your own caller repository.
2. Add your MEI files to it (or keep the demo files for testing).
3. In mei-friend, open **Settings > mei-friend > Use GitHub Actions** and check **"Show available GitHub Actions"**.
4. Paste the URL of a JSON work package definition into the **"Custom configuration"** field. For testing, use:
   `https://raw.githubusercontent.com/mei-friend/automation/refs/heads/main/work_packages.json`
5. Log in to GitHub in mei-friend and open a file from your caller repository.
6. From the GitHub menu, click **"GitHub Actions: Call automation workflow"**, choose a work package, fill in any parameters, and click **"Run workflow"**.

You need write access to the caller repository so that processing results can be committed back.

---

## Switching central repository

`.github/workflows/caller.yml` invokes the central repository on this line:

```yaml
jobs:
  call-shared:
    uses: mei-friend/automation/.github/workflows/central.yml@main
```

To use a different central repository, edit that `uses:` line — for example, to use the E-LAUTE central repository:

```yaml
jobs:
  call-shared:
    uses: e-laute/automation/.github/workflows/run_coordinator.yml@main
```

The receiving workflow in the central repository must accept the same input structure that `caller.yml` sends; if you target a custom central repo with a different input schema, `caller.yml` must be adjusted accordingly.

See [Setting up your own central repository](https://mei-friend.github.io/docs/advanced/automation/#setting-up-your-own-central-repository) for the full setup.

---

## Demo encodings included in the template

The template ships with two MEI files for testing the automation flow without having to upload your own data:

- **"Mondnacht am Meer"** for voice and piano by Ludwig Baumann, held in Badische Landesbibliothek Karlsruhe. _3 Lieder — Mus. Hs. 1324: V, pf / Ludwig Baumann._ [S.l.], 18XX. Badische Landesbibliothek Karlsruhe, Mus. Hs. 1324. <https://nbn-resolving.org/urn:nbn:de:bsz:31-57896> — License: CC BY-SA 4.0. Encoding by [@annplaksin](https://github.com/annplaksin).
- **6 Variations on "Nel cor più non mi sento"** (WoO 70) by Ludwig van Beethoven, Breitkopf und Härtel edition, 1862–90, plate B.168. License: CC BY 4.0. Encoding by [@wergo](https://github.com/wergo).
