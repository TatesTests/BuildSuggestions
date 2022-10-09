
catch amplitude
catch old api usage

If there are functions with more than 20 lines 
add-label: code-smells, 💩
Approve
asset only changes 
Approve
if the author is relativle new to repo code base (less then 10 PRs merged by that author)
Require approval of someone who significantly contributed to the repo
flocast scenario: when PR has no conflicts and already approved between another branch 
Approve
only logging changes, e.g in python only lines that change logger.info|debug|error
Approve

- loadind custom filters
- style/docs/configuration changes only can be merged w\o review
- PRs with 1 file + tests can be merged w\o review
- PRs with more than 10 files needs two reviewers
- Linter 
- coverage
- gitLeaks
- categorize PRs, like test coverage, size, areas of code
- finding the right owner, they want an extension of code-owner
- have a code merge process that is complex in that there are two PRs that are created. The second one they want auto-merged. 
