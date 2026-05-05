# Using Packages

This guide shows how to use Chippy modules without cloning the full repository. 


## Prerequisites
- GitHub Personal Access Token (with `read:packages`)

!!! info 
    Even for public repositories, GitHub Packages requires authentication to download dependencies.

## 1. Setup

Add the GitHub Packages repository to your `build.mill`:

```scala
def repositories = Seq(
  "https://maven.pkg.github.com/ucb-substrate/chippy"
)
```

## 2. Authentication

GitHub Packages requires authentication when downloading Maven dependencies. Here are two common ways to provide credentials to Coursier.

=== "Temporary environment variable"

    Export your credentials in your terminal:

    ```bash
    export COURSIER_CREDENTIALS="maven.pkg.github.com <YOUR_USERNAME>:<YOUR_PAT>"
    ```

    This applies only to the current terminal session.

=== "Persistent credentials file"

    Create a Coursier credentials file:

    ```bash
    mkdir -p ~/.coursier
    nano ~/.coursier/credentials
    ```

    Add:

    ```text
    maven.pkg.github.com <YOUR_USERNAME>:<YOUR_PAT>
    ```

    Restrict file permissions:

    ```bash
    chmod 600 ~/.coursier/credentials
    ```

    This avoids re-exporting credentials every new terminal session.

    !!! warning
        Never commit your Personal Access Token or credentials file to a repository.

!!! tip
    Replace `<YOUR_USERNAME>` with your GitHub username and `<YOUR_PAT>` with a Personal Access Token that has `read:packages`.

## 3. Add Dependency

Add the desired Chippy module (e.g., diplomacy) to your `build.mill`:
```scala
def mvnDeps = Seq(
  mvn"edu.berkeley.cs::diplomacy:0.0.2"
)
```

## 4. Example Usage
To verify the setup, you can test using this minimal example
```scala
import org.chipsalliance.diplomacy.lazymodule.LazyModule

object Main {
  def main(args: Array[String]): Unit = {
    val x: Option[LazyModule] = None
    println("Successfully imported diplomacy: " + x)
  }
}
```


!!! note
    This example uses `diplomacy`, but the same process applies to other published Chippy modules such as `rocketchip`, `testchipip`, or `constellation`. The dependency declaration and import path will differ depending on the module.

## 5. Run
After you have created your Scala file, run it with:
```bash
./mill test.run
```

!!! success "Expected Output for the Example Above"
    ```text
    Successfully imported diplomacy: None
    ```