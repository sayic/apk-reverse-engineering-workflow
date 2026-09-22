# APK Information

- Purpose: record basic APK information, the technology stack, hot-update mechanisms, and remote resource information. This is not an artifact inventory or task-status table.
- File structure:
  - One CSV file per APK.
  - One field per column and one APK per row; the first row contains headers and the second contains the APK information.
  - Keep CSV fields in the following order.

| Field (CSV column) | Meaning |
| --- | --- |
| `game` | Consistent game identifier, such as sample_game_a |
| `package_name` | Application package name |
| `project_version` | Project version used in project naming |
| `project_version_source` | Source file or field for the project version |
| `apk_version` | APK version |
| `apk_version_code` | Internal APK version number |
| `apk_path` | Relative path to the installation package |
| `original_filename` | Original installation-package filename |
| `sha256` | Installation-package SHA-256 |
| `engine` | Game engine |
| `engine_version` | Engine version |
| `code_backend` | Code execution backend, such as mono or il2cpp |
| `script_runtime` | Script runtime, such as xlua or javascript |
| `code_format` | Code material format, such as assemblies or Lua bytecode |
| `hot_update_method` | Brief description of the hot-update mechanism |
| `remote_resource_url` | Confirmed remote resource address |
| `catalog_version` | Resource manifest version |
| `catalog_path` | Path to the local resource manifest snapshot |
| `extraction_notes_path` | Documentation path for complex formats, decryption, and hot updates |

- Recording conventions:
  - Use `unknown` when undetermined and `none` when confirmed absent; do not guess.
  - Document multiple remote addresses or complex workflows separately and reference them with `extraction_notes_path`.
  - Register APKs with the same project version but different package hashes separately; do not overwrite old information.
  - Do not add `kind`, `resource_type`, task status, or an itemized artifact inventory.
  - Record unpacked, outputs, and report locations for the current task in context; keep detailed resource indexes alongside outputs.

Copy [apk.csv](../templates/info/apk.csv) into the task workspace's info/ directory, name it after the package's main filename, and fill in one data row. The template contains only headers. For inputs with the same name but different hashes, append a hash suffix according to the naming rules.
