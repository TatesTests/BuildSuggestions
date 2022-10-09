# Automation actions

gitStream executes actions in the order they are listed. If an action result fails, following actions will not be executed.

- [`approve`](#approve-action)
- [`merge`](#merge-action)
- [`set-required-approvals`](#set-required-approvals-action)
- [`add-reviewers`](#add-reviewers-action)
- [`require-reviewers`](#require-reviewers-action)
- [`request-change`](#request-change-action)
- [`add-labels`](#add-labels-action)
- [`add-comment`](#add-comment-action)
- [`update-check`](#update-check-action)


#### `add-comment` action

This action, once triggered, adds a comment to the PR.

| Args       | Type      | Description                                     |
| -----------|-----------|------------------------------------------------ |
| `comment`  | String    | Sets the comment, markdown is supported |

```yaml title="example"
automations:
  senior_review:
    if:
      - {{ files | isSomeInListRegex('core\/') }}
    run:
      - action: add-comment@v1
        args:
          comment: |
            Core service update
            (Updates API)
```


#### `add-labels` action

This action, once triggered, adds a label to the PR.

| Args       | Type      | Description                                     |
| -----------|-----------|------------------------------------------------ |
| `labels`    | [String]  | List of labels, any string can work |

```yaml title="example"
automations:
  senior_review:
    if:
      - {{ files | isSomeInListRegex('api\/') }}
    run:
      - action: add-labels@v1
        args:
          labels: [api-change]
```


#### `add-reviewers` action

This action, once triggered, sets a specific reviewer.

| Args       | Type      | Description                                     |
| -----------|-----------|------------------------------------------------ |
| `reviewers` | [String]    | Sets reviewers user name |

```yaml title="example"
automations:
  senior_review:
    if:
      - {{ files | isSomeInListRegex('src\/ui\/') }}
    run:
      - action: add-reviewers@v1
        args:
          reviewers: [popeye, olive]
```


#### `approve` action

This action, once triggered, approves the PR for merge.

```yaml title="example"
automations:
  small_change:
    if:
      - {{ source.diff.files | isFormattingChange }}
    run:
      - action: approve@v1
```


#### `merge` action

This action, once triggered, approve & merge the PR. It can set to wait for required checks to pass or ignore checks.

Syntax: 

| Args       | Type      | Description                                     |
| -----------|-----------|------------------------------------------------ |
| `wait_for_all_checks`| Boolean | By default `false`, so only Required checks can block merge, when `true` the action won't merrge even if non-Required check fail  |
| `squash_on_merge`| Boolean   | When merging use squash mode |


```yaml title="example"
automations:
  small_change:
    if:
      - {{ files | allDocs }}
    run:
      - action: merge@v1
```


#### `set-required-approvals` action

This action, once triggered, blocks PR merge till the desired reviewers approved the PR. The actions fail the check to prevent the PR for merge.

!!! note

    You should enable branch protection, so GitHub will prevent merging unless the gitStream action check pass successfully. 

| Args       | Type      | Description                                     |
| -----------|-----------|------------------------------------------------ |
| `approvals`| Integer   | Sets the number of required reviewer approvals for merge for that PR|

```yaml title="example"
automations:
  double_review:
    if:
      - {{ files | isSomeInListRegex('core\/') }}
    run:
      - action: set-required-approvals@v1
        args:
          approvals: 2
```


#### `request-change` action

This action, once triggered, request changes on the PR. As long as request change is set, gitStream will block the PR merge.

| Args       | Type      | Description                                     |
| -----------|-----------|------------------------------------------------ |
| `reviewers` | [String]    | Sets reviewers user name, merge is blocked till approved by any of the listed users |

```yaml title="example"
automations:
  senior_review:
    if:
      - {{ files | isLineInFileDiffRegex('oldFetch') | some }}
    run:
      - action: request-change@v1
        args:
          comment: |
            You have used deprected API, use `newFetch` instead.
```

#### `require-reviewers` action

This action, once triggered, sets a specific reviewer.

| Args       | Type      | Description                                     |
| -----------|-----------|------------------------------------------------ |
| `reviewers` | [String]    | Sets reviewers user name, merge is blocked till approved by any of the listed users |

```yaml title="example"
automations:
  senior_review:
    if:
      - {{ files | isSomeInListRegex('src\/ui\/') }}
    run:
      - action: require-reviewers@v1
        args:
          reviewers: ['popeye', 'olive']
```

####  `update-check` action

Used to workaround unnecessary checks, this action, update the defined check with the defined status if all conditions pass.

| Args       | Type      | Description                                     |
| -----------|-----------|------------------------------------------------ |
| `check_name`  | String    | The name of the check |
| `status`  | String    | The status of the check |
| `conclusion`  | String    | The conclusion of the check |


```yaml title="example"
automations:
  senior_review:
    if:
      - {{ files | isEveryInListRegex('.*.png$|.*.jpg$|.*.svg$|.*\.css$') }}
    run:
      - action : update-check@v1
        args:
          check_name: cypress-e2e
          status: completed
          conclusion: success
```