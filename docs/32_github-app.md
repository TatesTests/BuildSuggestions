# GitHub app installation 

To start using gitStream with GitHub source code hosting, a GitHub app should be installed. The installation start with installing the app from the GitHub Apps marketplace.

gitStream can be installed for one repo, specific repos, or all repos in your organization. 

Note: we recommend installing for all repositories, as it will also cover new repos you add in the future. You can change this setting at any time if you change your mind.

The permissions are: 

| Permissions           | Reason |
|----------------------|-------------------------------------------------------|
| Write access to dedicated gitStream app files | Used to set the gitStream workflow files |
| Write access to code | To allow gitStream to approve PRs once all conditions are met |
| Read access to administration, issues, and metadata | To read the code on monitored repositories |
| Read and write access to actions, checks, pull requests, and workflows | Trigger workflows, create and update pull requests and their checks, and modify workflow files |
| User email | Used to identify user |

Once installation completes, the following files are added to each of the selected repos under their root directory.

```
.
├─ .cm/
│  └─ gitstream.cm
├─ .github/
│  └─ workflows/
│     └─ gitstream.yml
```

| File and path        | Customizable | Reason |
|----------------------|--------------|----------------------------------------|
| .cm/gitstream.cm     | Y            | Used to specify gitStream automations, you can edit this file |
| .github/workflows/gitstream.yml | N | Used by gitStream to execute automation in your repo so source code doesn't get to outside services |

## Set GitHub repo settings

!!! attention

    To get the full potential using gitStream, you need to set it as a required check.

To make sure gitStream can block PRs from merging under certain conditions, you should set the following:

1. Go to repo `settings`
2. On the left pane select `Code and automation` > `Branches` 
3. Set `Branch protection rules` for your desired branch 
4. Enable `Require status checks to pass before merging`
5. Search for `status checks in the last week for this repository`
6. Select `gitStream.cm` as required check
