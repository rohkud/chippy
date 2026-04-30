# Available Packages

The following Chippy modules can be published as Maven packages.

| Package Name | Mill Module Path |
|-------------|------------------|
| diplomacy   | `rocketchip.dependencies.diplomacy` |
| cde         | `rocketchip.dependencies.cde` |
| hardfloat   | `rocketchip.dependencies.hardfloat` |
| rocketchip  | `rocketchip` |
| rocketchip-macros | `rocketchip.macros` |
| rocketchip-blocks | `rocketchip-blocks` |
| rocketchip-inclusive-cache | `rocketchip-inclusive-cache` |
| chipyard    | `chipyard` |
| chippy      | `chippy` |
| constellation | `constellation` |
| testchipip  | `testchipip` |

!!! note
    Use the **Mill Module Path** when specifying the `module` input when publishing.

## Usage

When running the publishing workflow, provide the module path and desired version, such as:

```text
module: rocketchip.dependencies.diplomacy
version: 3.0.0
```

## Identifying Publishable Modules
Run the command in your terminal:
```text
./mill resolve __.publish
```