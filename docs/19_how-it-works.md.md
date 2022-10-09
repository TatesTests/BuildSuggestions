# How it works

Every time a dev opens a new Pull Request or changes a Pull Request, gitStream is triggered. Next, the `.cm` file is used to determine which automatic actions are invoked based on their conditions. The resulting actions use GitHub API to achieve the desired outcome.

Next steps are common practice of changing code in repo:

1. Create a branch 
2. Commit changes  
3. Push branch to remote repo 
4. Open Pull Request 

When a new PR is opened, gitStream GitHub app gets triggered:

5. gitStream gets event for new PR and calls the installed action in `.github/workflows/gitstream.yml`
6. The installed action pulls and runs gitStream action `linear-b/gitstream-github-action@v1`. This action runs locally in the repo and relies on 
    1. The provided PR context
    2. The repos automations as defined in `.cm/gitstream.cm`
7. It sends the list of applicable automations to the gitStream GitHub app
8. gitStream GitHub app iterates over the automations and invokes each action using GitHub APIs
9. The PR gets updated according to the desired automations
10. The PR is ready for further review or merge

## Automation results

Eventually, the gitStream app shows the following statuses:  

- Success - when the applicable automation finished and PR is good to go 
- Neutral - when there aren't any applicable automations for the PR
- Failed - when the applicable automation finished without completion