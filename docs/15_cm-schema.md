# Overview gitstream.cm

This document is a reference for the gitStream 1.x, used in .cm/gitstream.cm file.

!!! note 

    You can edit the `.cm/gitstream.cm` and add your own checks and automations. Check out the [examples](18_examples.md) 

## Context variables

gitStream includes a collection of variables called contexts, these variables are intended 
for use with the [`checks`](#checks-section) conditions.

gitStream built-in context variables are described [here](21_gitstream-context.md).

## Filter functions

Filters are essentially functions that can be applied to variables. They are called with a pipe 
operator `|` and can take arguments. The logic expressions are based on Jinja2 syntax, supported by Nunjucks lib.

All Nunjucks filters are supported, see [documentation](https://mozilla.github.io/nunjucks/templating.html#builtin-filters).

gitStream built-in filter functions are described [here](23_gitstream-filters.md).

You can also add custom filters, see [instructions](28_custom-filters.md).

## Automation actions

Actions the desired automations that are triggered when all conditions are met, read more [here](25_gitstream-actions.md).

## Schema

The following sections are used in `.cm` file to describe the desired automations:

- [`manifest`](#manifest-section)
- [`automations`](#automations-section)

### `manifest` section

The first section in a `gitstream.cm` file is the `manifest`.

```yaml
manifest: 
  version: 1.0
```

The only field required is `version`.

| Key         | Required | Type    | Description                              |
| ----------- | ---------|---------|----------------------------------------- |
| `manifest`         | Y        | Map     | The manifest section root                |
| `manifest.version` | Y        | String  | Specify the `.cm` spec version: 0.1, 1.0 |

The manifest version field is used to parse the `.cm` file, in the future if breaking changes are 
introduced to the parser then older automation will be still supported.

### `automations` section

The `automations` section defines the automations and their conditions. 

```yaml
automations:
  mark_small_pr:
    if:
      - {{ checks.size.is.xsmall }}
    run:
      - action: add-labels@v1
        args:
          label: xsmall
```

Each automation includes its name, and few fields: `if` and `run`.

| Key        | Required  | Type    | Description                                     |
|------------|-----------|---------|------------------------------------------------ |
| `automations`  | Y        | Map     | The automations section root     |
| `automations.NAME`     | Y | Map | User defined name of the automation, can be any string       |
| `automations.NAME.if`  | Y | Map | List of conditions                               |
| `automations.NAME.run` | Y | Map | The automation to run if all conditions are met |

The `if` field includes the list of conditions. The conditions are checked when a pull request 
is opened or changed, if all the conditions pass, the automation is executed.

The `run` field includes the automation to execute. It includes the following fields:

| Key         | Required | Type    | Description                                     |
| ----------- | ---------|---------|------------------------------------------------ |
| `action`    | Y        | String  | The action pointer                              |
| `engine`    | N        | String  | The action engine, default is `gitstream`       |
| `args`      | N        | List    | The action inputs list                          |

For `gitstream` engine, the action is specified by: `name@version`

gitStream supported actions, see [documentation](25_gitstream-actions.md).

#### Dynamic actions arguments
For actions arguments values a dynamic value is supported using expressions based on Jinja2 syntax, and includes gitStream context variables. For example:

```yaml
automations:
  pr_complexity:
    if:
      - true
    run:
      - action: add-comment@v1
        args:
          comment: "Estimated {{ branch | estimatedReviewTime }} minutes to review"
```

### Reusing checks

You can define an accessory section, e.g. `checks`, that defines common conditions, and resue.  

```yaml
checks:
  size:
    is:
      small: {{ branch.diff.size < 20 }}
      medium: {{ branch.diff.size >= 20 and branch.diff.size < 100 }}
      large: {{ branch.diff.size >= 100 }}
automations:
  approve_small:
    if:
      - {{ checks.size.is.small }}
    run:
      - action: approve@v1
  mark_small_medium:
    if:
      - {{ checks.size.is.small or checks.size.is.medium }}
    run:
      - action: add-labels@v1
        args:
          labels: ['good-size']

```