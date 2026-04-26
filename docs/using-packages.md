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
  "https://maven.pkg.github.com/rohkud/chippy-forked"
)
```

## 2. Authentication

Export your credentials in your terminal:
```bash
export COURSIER_CREDENTIALS="maven.pkg.github.com <YOUR_USERNAME>:<YOUR_PAT>"
```
!!! tip
    Replace `<YOUR_USERNAME>` with your GitHub username and `<YOUR_PAT>` with your Personal Access Token.

## 3. Add Dependency

Add the desired Chippy module (e.g., diplomacy) to your `build.mill`:
```scala
def mvnDeps = Seq(
  mvn"edu.berkeley.cs::diplomacy:0.0.2"
)
```

## 4. Example Usage
To verify the setuup, you can test using this minimal example
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