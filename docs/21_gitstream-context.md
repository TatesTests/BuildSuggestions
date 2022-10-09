# Context variables

gitStream includes a collection of variables called contexts. 

- [`branch`](#branch-context)
- [`files`](#files-context)
- [`source`](#source-context)
- [`repo`](#repo-context)

The following structures definitions:

- [`FileMetadata`](#filemetadata-structure)
- [`FileDiff`](#filediff-structure)
- [`Contributor`](#contributor-structure)

#### `branch` context

The `branch` context contains info regarding the branch changes compared to the latest main branch. 

!!! note  

    compared to the `source` context does not include actual source code.

| Values               | Type      | Description                                              |
|----------------------|-----------|--------------------------------------------------------- |
| `branch`             | Map       | Includes the info related to the current branch          |
| `branch.name`        | String    | The current branch, `feature-123-branch`                 |
| `branch.base`        | String    | The main branch, `main`                 |
| `branch.diff`        | Map       | Includes the info compared to the default branch, `main` |
| `branch.diff.size`   | Integer   | The sum of line changed: additions, edits and deletions   |
| `branch.diff.files_metadata`  | [`FileMetadata`]  | List of changed files including their relative path      |

The branch context doesn't include any source code, but only related metadata.

#### `FileMetadata` structure

The `branch.diff.files_metadata` mapping includes a list of `FileMetadata`:

| Values          | Type      | Description                                                     |
| ----------------|-----------|---------------------------------------------------------------- |
| `file` | String    | The name of the file before the changes, including its path     |
| `additions` | Integer   | The number of lines edited or added to the file  |
| `deletions` | Integer   | The number of lines removed from the file      |

#### `files` context

The `files` context includes the list of changed files in the branch compared to the main branch.

| Values  | Type      | Description                                |
|---------|-----------|------------------------ |
| `files` | [String]  | List of all changed files with their full path |

#### `source` context

The `source` context includes a list of `FileDiff` objects that can be used to get insights based on code changes. The changes compared to the latest main branch. 

| Values              | Type  | Description                                        |
|---------------------|-------|--------------------------------------------------- |
| `source`          | Map   | Info related to source code           |
| `source.diff`     | [Map] | Includes the info compared to the default branch, `main` |
| `source.diff.files` | [`FileDiff`] | List of changed files with their code changes |

The source context include all code changes, it is not safe to share it with unknown services.

#### `FileDiff` structure

The `source.diff.files` mapping includes a list of `FileDiff`:

| Values          | Type      | Description                                          |
| ----------------|-----------|----------------------------------------------------- |
| `original_file` | String    | The name of the file before the changes, including its path |
| `new_file`      | String    | The name of the file after the changes, including its path |
| `diff`          | String    | The content in diff format `+` for additions, `-` for deletions |
| `original_content` | String    | The content as is in the `main` branch     |
| `new_content`      | String    | The new content in this branch     |

#### `repo` context

The `repo` context includes metadata related to the repo.

| Values             | Type      | Description                                              |
|--------------------|-----------|-------------------------------------------------|
| `repo`             | Map       | Includes the info related to the current repo   |
| `repo.contributors`  | [`Contributor`]  | List of changed files including their relative path |

#### `Contributor` structure

The `repo.contributors` mapping includes a list of `Contributor`:

| Values          | Type      | Description                                         |
| ----------------|-----------|---------------------------------------------------- |
| `name` | Number    | Number of commits     |

