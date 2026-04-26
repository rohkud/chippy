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

Add this workflow to `.github/workflows/publish.yml` to publish all eligible Chippy packages:
```yml
name: Publish Chippy packages

on:
  workflow_dispatch:
  push:
    tags:
      - "v*"

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
          java-version: '17'

      - name: Make Mill executable
        run: chmod +x ./mill

      - name: Publish all Chippy packages to GitHub Packages
        env:
          MILL_MAVEN_USERNAME: ${{ github.actor }}
          MILL_MAVEN_PASSWORD: ${{ secrets.GITHUB_TOKEN }}
        run: |
          ./mill mill.javalib.MavenPublishModule/ \
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
Then run the workflow manually:

1. Go to the GitHub repository.
2. Open the **Actions** tab.
3. Select **Publish Chippy packages**.
4. Click **Run workflow**.

## 4. Versioning
GitHub packages will reject publishing the same package version multiple times, so increment the package version in your `build.mill` file before publishing again:
```scala
def publishVersion = "0.0.2"
```
!!! warning
    Always bump the version before republishing artifacts.

## 5. Verifying Published Packages
After publishing, verify it by going to the repository's **Packages**. You should see published artifacts like:
```
chippy_2.13
diplomacy_2.13
rocketchip_2.13
constellation_2.13
```