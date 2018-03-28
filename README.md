# Conditional Markup Language

The Conditional Markup Language is primary designed for description files in
package and build systems.

Detailed information about the format can be found in the [Format.md](Format.md)
document.


## Advantages

The following list shows the advantages of CML compared to already existing
markup languages:

* Less verbose syntax
* Single and multi line comments
* Conditional include of object keys


## Example

```
/*
 * This example describes a package called Core.Collections.
 */

name: "Core.Collections"
version: "1.0-beta"
description: "
  This package contains basic collections like maps, sets and lists.
  For more information see page XYZ.
"

sources:
- "src/collections.c"
- "src/lists.c"
- "src/maps.c"
- "src/sets.c"

dependencies:
  Core: "2.0"

[OS == "Windows"]
dependencies:
  Core.Windows: "2.0"
  Api.Windows: "1.0"
```
