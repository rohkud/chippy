# Getting Started

## Goal
Set up Chippy locally

## Prerequisites

You will need:

- Git
- A Java Development Kit (JDK)
- Network access to fetch Git submodules and Scala dependencies

## 1. Clone the repository

```bash
git clone https://github.com/ucb-substrate/chippy.git
cd chippy
```

## 2. Initialize submodules
Chippy depends on these submodules, which we must initialize to build anything
```bash
git submodule update --init --recursive
```

## 3. Publish all Chippy packages locally
TODO: Use github packages
```bash
./mill __.publishLocal
```

## Next steps
This page will become the main quickstart for new users.
