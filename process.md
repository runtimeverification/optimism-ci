# Bootstraping a Kontrol verification project
​
This document describes the steps to spin up a Kontrol verification repository with proper CI set up.
​
### Summary
​
In short, these are the steps to be followed in order to successfully set up CI
​
1. Create fresh repo from [CI template](https://github.com/runtimeverification/audit-kontrol-template) and add any existing verification work
2. Enable status checks
3. Add propper branch protection for `master`
4. Activate auto-update PRs for newer Kontrol versions
​
Next we describe each step.
​
## CI Template explainer
​
After creating a repository with the [github template for setting up CI](https://github.com/runtimeverification/audit-kontrol-template), you'll have a [workflow](https://github.com/runtimeverification/audit-kontrol-template/blob/2fdd6364ad3a8e17f8cc8fc01b066bb0665447e3/.github/workflows/test-pr.yml#L46) to be run for each PR commit.
​
What this workflow does is execute whatever is in the file [`verify`](https://github.com/runtimeverification/audit-kontrol-template/blob/master/verify), which, at the moment, is `bash kontrol/run-kontrol.sh 2>&1 | tee kontrol/log.out`. To execute the repo locally, one can just `$(cat verify)`.
​
Under the `kontrol` folder are supposed to be all the files regarding the execution of Kontrol. These include all log files, execution scripts and bug reports.
​
## Status checks
​
The status checks will run `./kontrol/run-kontrol.sh`, with the only requirement that the execution is successful. This enables for maximum flexibility for the verification engineer to define what means to be a succesful run. To change `./kontrol/run-kontrol.sh` to your command of choice, modify [this line](https://github.com/runtimeverification/audit-kontrol-template/blob/master/.github/workflows/test-pr.yml#L46) in the new repository's `.github/workflows/test-pr.yaml`.
​
Status checks must be manually run first. After the first manual run, they can be automatically triggered. To run them for the first time, do the following:
​
- Ask Freeman to enable actions as an org admin for the newly created repository
- Ask Freeman to add org level secrets to the repository.  (DOCKERHUB_PASSWORD, JENKINS_GITHUB_PAT, JENKINS_GITHUB_PAT)
​
Now we have to go to the newly created repository, create a new branch and prepare a first pass at a simple proof to run. 

Create a pull request with the new branch. This will trigger the first run of the status checks for a PR.
​
## Branch protection
​
Before enabling auto-updates we need to set up proper branch protection. In particular, we'll require two things:
​
- Every commit to `master` must come from a PR
- In order to merge any PR to master it needs to be approved and successfully pass the status checks
​
To do this, in the GitHub page of your repo head to Settings > Branches and check the following options
​
- Require a pull request before merging (first shown option)
- Require status checks to pass before merging (second shown option). To require a status check, it needs to have been run once
- Within the second shown opiton, also select "Require branches to be up to date before merging"
​
## Automatic Kontrol updates
​
After making sure that `master` is well protected, we'll want to configure the repo to automatically open new PRs for new versions of Kontrol. These PRs will trigger the execution of `run-kontro.sh`, which will ensure that the next version of Kontrol is compatible with the existing verification efforts. To achieve this, follow the next steps:
​
- Add the Automation CI Team (named Jenkins) to the repository with 'write' permissions
- In the [Devops Repository](https://github.com/runtimeverification/devops/), add the the new repo to the [automerge.json](https://github.com/runtimeverification/devops/blob/master/automerge.json) list and the[ deps/runtimeverification/kontrol.json](https://github.com/runtimeverification/devops/blob/master/deps/runtimeverification/kontrol.json) file for automated updates from new versions of kontrol
​
​
---------------------------------
​
​
After completing all of these steps, the newly created verification repo will enjoy proper CI and have mechanisms in place to ensure proper reproducibility.