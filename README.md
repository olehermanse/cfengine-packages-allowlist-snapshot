This module allows you to snapshot which packages are installed on a system, and then _enforce_ that list, uninstalling or giving warnings when other packages appear.
It is a fork of the original [`packages-allowlist`](https://github.com/nickanderson/cfengine-packages-allowlist) module by [Nick Anderson](https://github.com/nickanderson), with some of the behavior, input, and inventory changed.

## Configuration

This module has 1 input, the _mode_, which can be configured CMDB, CFEngine Build module input, or an augments file (`def.json`):

```json
{
  "variables": {
    "packages_allowlist_snapshot:state.mode": {
      "value": "init"
    }
  }
}
```

3 different values are accepted:

1. `"init"` - Initialize the module by capturing a snapshot of the currently installed packages.
2. `"warn"` - Print warnings if there are installed packages, which are not in the snapshot, or if there is no snapshot.
3. `"enforce"` - Uninstall packages which are not found in snapshot, print warnings if snapshot is missing.

## Inventory

2 different inventory attributes are provided by this module:

1. **Package enforcement status:** Helpful status to see what the module is doing on each host. Example values:
   * `Deactivated - No mode specified`.
   * `Initializing - Generated snapshot of 55 installed packages`.
   * `Compliant - All installed packages found in allowed list`.
   * `Warnings - Printing warnings about 3 unwanted packages`.
   * `Uninstalling - Uninstalling 3 unwanted packages`.
   * `Error - 0 allowed packages found`.
   * `Error - No snapshot file found`.
   * `Error - 0 installed packages found`.
2. **Unwanted packages:** List of packages which will be uninstalled when switching to `"enforce"` mode.
3. **Allowed packages:** List of packages which are allowed on the system (from the snapshot).
