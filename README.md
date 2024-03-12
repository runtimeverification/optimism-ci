# Audit Repository

# Use Kup to install Specific Versions of RV Tools On a Local Box for Development and personal Testing
[kup](https://github.com/runtimeverification/k/blob/master/k-distribution/INSTALL.md)
```
bash <(curl https://kframework.org/install)
kup install k
or 
kup install kevm 
or 
kup install kontrol
```

# Tool Suggestions
When working with RV tools for audits at times requires making custom changes. Check these tools out separately apart from this project. 
Utilize a tool called `git worktree` to work on multiple branches of the same repo at the same time. 

Source Doc Reading for [git worktree](https://git-scm.com/docs/git-worktree)
These repositories will track and test the associated deps main releases and run tests against new releases as they update. 

# Reproduce builds locally 
Instructions to install Docker are here: [Docker Install](https://docs.docker.com/engine/install/ubuntu/) 
Do not install Docker Desktop. For businesses it is not free and a violation of their user policy. Be sure to install Docker Engine. 

## Local CI Setup to reproduce issues in CI
This is intended to setup a local environment that mimic what is done in a CI environment. 
```bash
      docker run                          \
        --name kontrol-test               \
        --rm                              \
        --interactive                     \
        --tty                             \
        --detach                          \
        --user root                       \
        --workdir /home/user/workspace            \
        runtimeverificationinc/kontrol:ubuntu-jammy-0.1.12

      # Copy the current Checkout direcotry into the container
      # Run the following again if you need to update scripts or proofs to run tests again
      docker cp . kontrol-test:/home/user/workspace
      docker exec kontrol-test chown -R user:user /home/user
      # Get a shell in the environment 
      docker exec -it --user user kontrol-test bash
      # Now with a shell go to the scripts you wish to run. 
```
