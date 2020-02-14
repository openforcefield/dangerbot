# OpenFF's dangerbot

Dangerbot is the host of Open Force Field Reviewer Roulette. This was inspired by [GitLab's reviewer roulette system](https://about.gitlab.com/blog/2019/10/23/reviewer-roulette-one-year-on/)


### Getting off-dangerbot to run in your repo 
To add this Dangerbot to your repo, 

1) Create the GitHub Action for Dangerbot by creating a file at `.github/workflows/dangerbot.yml` in your project repo. 
  It should contain 

```
name: off-dangerbot

on:
  pull_request:
      types: assigned

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v1
    - name: Set up Ruby 2.6
      uses: actions/setup-ruby@v1
      with:
        ruby-version: 2.6.x
    - name: Assign reviewer if prompted
      # API token for off-dangerbot
      env:
          DANGER_GITHUB_API_TOKEN: ${{ secrets.DANGER_GITHUB_API_TOKEN }}
      run: |
        gem install danger --version '~> 5.0'
        danger --version 
        danger
```

2) Add a new file to the top of your repo called `Dangerfile`, containing:
```
# Run the shared Dangerfile with these settings
danger.import_dangerfile(github: "openforcefield/dangerbot") 
```

3) Add `openff-dangerbot`'s limited API token to your GitHub Secrets. Mine is at https://github.com/openforcefield/dangerbot/settings/secrets, and it must have the name `DANGER_GITHUB_API_TOKEN`. Contact Jeff once you get to this point and he will send you Dangerbot's API token.

4) Give `openff-dangerbot` "Triage" access to your repo if you'd like it to automatically assign the reviewer when it runs (otherwise it will just tag them in a comment). The place where I do that is https://github.com/openforcefield/dangerbot/settings/access. If the repo you're setting up is inside the `openforcefield` org, you should be able to just give the "Bots" team "Triage" permissions.

### Using openff-dangerbot

Once the above is complete and the changes are merged into your repo's `master` branch, you can trigger Dangerbot by selecting them in the "Assignees" section on the top right of your PR. 

### Bot account

The account that runs this bot is https://github.com/openff-dangerbot. Contact Jeff if you need the login info for the `openff-dangerbot` account.

### Setup details

We use the Ruby version of Danger. In case we need to change anything, [here are original setup instructions for Danger](https://danger.systems/guides/getting_started.html#setting-up-danger-to-run-on-your-ci), [here are some example Dangerfiles](https://danger.systems/reference.html), and [here's a helpful example JSON of a GitHub PR](https://raw.githubusercontent.com/danger/danger/master/spec/fixtures/github_api/pr_response.json). You can check out [Gitlab's roulette Dangerfile](https://gitlab.com/gitlab-org/gitlab-foss/blob/master/danger/roulette/Dangerfile). ) to see everything that's possible with Danger.
