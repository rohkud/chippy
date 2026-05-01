# Publishing to GitHub Packages

This page explains how Chippy modules can be published as Maven packages using GitHub Packages.

## 1. Verify Local Publishing

Before publishing remotely, verify that the module(s) can be published locally:

=== "Publish one module (ex. chippy)"

    ```bash
    ./mill chippy.publishLocal
    ```

=== "Publish all modules"

    ```bash
    ./mill __.publishLocal
    ```

A successful local publish will place artifacts in `~/.ivy2/local`

!!! tip
    Always test `publishLocal` first before using GitHub Actions to help isolate issues that may arise in subsequent steps

## 2. Configure GitHub Actions
Create the GitHub workflow file, `.github/workflows/publish.yml`

Add this workflow to `.github/workflows/publish.yml` to enable publishing a specific Chippy package:
```yml
name: Publish Chippy package

on:
  workflow_dispatch:
    inputs:
      module:
        description: "Module to publish, e.g. rocketchip.dependencies.diplomacy, chippy, rocketchip. Look at documentation for more details: https://rohkud.github.io/chippy/available-packages/"
        required: true
        default: rocketchip.dependencies.diplomacy
      version:
        description: "Version to publish, e.g. 3.0.0"
        required: true

permissions:
  contents: read
  packages: write

jobs:
  publish:
    runs-on: ubuntu-latest

    steps:
      - name: Check out repository
        uses: actions/checkout@v4
        with:
          submodules: recursive

      - name: Set up JDK
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: "17"

      - name: Make Mill executable
        run: chmod +x ./mill

      - name: Set module and version
        run: |
          echo "MODULE=${{ github.event.inputs.module }}" >> $GITHUB_ENV
          echo "CHIPPY_VERSION=${{ github.event.inputs.version }}" >> $GITHUB_ENV

      - name: Publish selected module
        env:
          MILL_MAVEN_USERNAME: ${{ github.actor }}
          MILL_MAVEN_PASSWORD: ${{ secrets.GITHUB_TOKEN }}
        run: |
            ./mill mill.javalib.MavenPublishModule/ \
            --publishArtifacts "$MODULE.publishArtifacts" \
            --releaseUri https://maven.pkg.github.com/rohkud/chippy-forked \
            --snapshotUri https://maven.pkg.github.com/rohkud/chippy-forked

```

## 3. Run the Workflow
Push the workflow file:
```bash
git add .github/workflows/publish.yml
git commit -m "Add GitHub Packages publishing workflow"
git push origin main
```
You can now trigger publishing in two ways:

---

### Option A: GitHub Web UI

1. Go to the repository.
2. Open the **Actions** tab.
3. Select **Publish Chippy package**.
4. Click **Run workflow**.
5. Enter:

```text
module: rocketchip.dependencies.diplomacy
version: 3.0.0
```

---

### Option B: Command Line (GitHub CLI)

Install the GitHub CLI (`gh`) and authenticate:

```bash
gh auth login
```

Then run:

```bash
gh workflow run "Publish Chippy package" \
  --repo rohkud/chippy-forked \
  -f module=rocketchip.dependencies.diplomacy \
  -f version=3.0.0
```

!!! warning
    Always bump the version before republishing artifacts, since GitHub packages will reject publishing the same package version multiple times.

## 4. Verifying Published Packages
After publishing, verify it by going to the repository's **Packages**. You should see published artifacts like:
```
chippy_2.13
diplomacy_2.13
rocketchip_2.13
constellation_2.13
```