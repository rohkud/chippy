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


