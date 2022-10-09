# Examples

### Add label to PR by PR size

Automatically add a label to PRs that are very small to get faster reviewer response.

```yaml title=".cm/gitstream.cm"
automations:
  mark_good_pr:
    if:
      - {{ branch.diff.size <= 100 }}
    run:
      - action: add-labels@v1
        args:
          labels: [good_size]
  mark_big_pr:
    if:
      - {{ branch.diff.size > 100 }}
    run:
      - action: add-labels@v1
        args:
          labels: [big_size]
```

### Add Estimated Time for Review in PRs comment 

Automatically add a comment to all PRs with the estimated time for review to get faster reviewer response.

```yaml title=".cm/gitstream.cm"
automations:
  etr_on_all:
    if:
      - true
    run:
      - action: add-comment@v1
        args:
          comment: "Estimated {{ branch | estimatedReviewTime }} minutes to review"
```

### Automated check and approve low risk PRs 

Automatically add a comment to all PRs with the estimated time for review to get faster reviewer response.

```yaml title=".cm/gitstream.cm"
automations:
  approve_docs:
    if:
      - {{ files | allDocs }}
    run:
      - action: approve@v1
```


### Set 2 reviewers for large PRs 

Automatically require 2 reviewers for PRs that has more than 100 lines of code changed.

```yaml title=".cm/gitstream.cm"
automations:
  double_review:
    if:
      - {{ branch.diff.size > 100 }}
    run:
      - action: set-required-approvals@v1
        args:
          reviewers: 2
```

### Approve indentation changes in JavaScript files 

For PRs that include only code format change, approve merge automatically.

```yaml title=".cm/gitstream.cm"
automations:
  allow_formatting:
    if:
      - {{ files | isEveryExtension(['js', 'ts']) }}
      - {{ source.diff.files | isFormattingChange }}
    run:
      - action: approve@v1
      - action: add-labels@v1
        args:
          labels: [code-formatting]

```

!!! note

    Multiple actions can be listed in a single automation. The actions are invoked one by one.
    
    Multiple conditons can be listed for a single automation. All listed conditions must pass to triger the actions.
