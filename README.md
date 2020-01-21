# OpenFF's dangerbot

Dangerbot is the host of Open Force Field Reviewer Roulette. This was inspired by [GitLab's reviewer roulette system](https://about.gitlab.com/blog/2019/10/23/reviewer-roulette-one-year-on/)

The code that powers this bot is a pared-down version of [GitLab's roulette Dangerfile](https://gitlab.com/gitlab-org/gitlab-foss/blob/master/danger/roulette/Dangerfile). 


### Getting off-dangerbot to run in your repo 
To add this Dangerbot to your repo, 

1) Add the following to your `.travis.yml`:

* In the `matrix` section, add 
```
  - os: linux
    language: ruby
    env: DANGERBOT=true
```
* In the `before_install`, add
```
# If this is just a "Dangerbot" PR-checking build, install+run the bot, then exit
- if [ "$DANGERBOT" == true ]; then 
    gem install danger --version '~> 5.0' && 
    danger --version && 
    danger && 
    exit ;  fi;
```

2) Add a new file to the top of your repo, called `Dangerfile`, containing:
```
# Run the shared Dangerfile with these settings
danger.import_dangerfile(github: "openforcefield/dangerbot") 
```

3) Finally, add `openforcefield-dangerbot`'s limited API token to your travis environment. Mine is https://travis-ci.org/openforcefield/openforcefield/settings. Add the token by creating a new environment variable, `DANGER_GITHUB_API_TOKEN` which equals `caa3a4a51664dd050a016b7a215f183f2b7ef60a`, and enable `DISPLAY VALUE IN BUILD LOG`

### Bot account

The account that runs this bot is https://github.com/openff-dangerbot. Per their [recommended setup instructions](https://danger.systems/guides/getting_started.html#tokens-for-oss-projects), Travis CI has that bot's API token stored as a NON-SECRET value, so that Travis can still use it in CI for forks. As such, I think someone could hijack it, so **do not give this account write access to anything** (it's still allowed to comment). Contact Jeff or Karmen if you need the login info for this account.

### Setup details

We use the Ruby version of Danger. In case we need to change anything, [here are original setup instructions for Danger](https://danger.systems/guides/getting_started.html#setting-up-danger-to-run-on-your-ci), [here are some example Dangerfiles](https://danger.systems/reference.html), and [here's a helpful example JSON of a GitHub PR](https://raw.githubusercontent.com/danger/danger/master/spec/fixtures/github_api/pr_response.json).
