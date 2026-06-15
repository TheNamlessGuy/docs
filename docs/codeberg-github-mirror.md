# What?
Codeberg good, GitHub bad.

This is a step-by-step guide on how to set up a Codeberg repo that mirrors to a read-only GitHub version.

# Step by step guide
## Creating the repos
1) Create the repository on GitHub **with** a README.md
2) In GitHub, go to the [Personal Access Tokens](https://github.com/settings/personal-access-tokens) page
  * Top right -> Settings -> Developer settings -> Personal Access Tokens
3) Name the token `codeberg-rw_<repo name>`
4) Expiration => Never
5) Only selected repositories => `<repo name>`
6) Add permissions:
    * Artifact metadata - read-only
    * Commit statuses - read-only
    * Contents - read and write
    * Custom properties - read-only
    * Discussions - read-only
    * Issues - read-only
    * Metadata (required) - read-only
7) Create the token, and copy the value. Do NOT close the tab
8) Head over to the [Codeberg repo migration tool](https://codeberg.org/repo/migrate)
9) Select GitHub, then fill in:
    * Migrate from URL: The **HTTPS** clone URL from GitHub
    * Access token: the newly generated PAT
    * Check all the boxes for what to import
    * Let the autofill handle the rest
10) Migrate the repo

## Setting up mirroring
1) On Codeberg, head over to to the repo settings
2) Find the "Mirror settings" (on the first page)
3) Fill in:
    * Remote repository URL: the GitHub **HTTPS** clone URL
    * Branch filter: `main`
    * Username: `TheNamlessGuy`
    * Password: the PAT created earlier
    * Sync when commits are pushed: Yes
    * Mirror interval: whatever
4) Create the mirror

## Setting GitHub to readonly
1) In GitHub, head over to the settings for the repo
2) Head to the "Rulesets" setting
3) Create a new branch ruleset:
    * Name: readonly
    * Enforcement status: active
    * Bypass list: repo admin, always allow
    * Target branches: all branches
    * Branch rules:
        * Restrict creations: yes
        * Restrict updates: yes
        * Restrict deletions: yes
        * Require linear history: yes
        * Require deployments to succeed: no
        * Require signed commits: no
        * Require a pull request before merging: yes
            * Required approvals: 1
            * All other: default
        * Require status checks to pass: no
        * Block force pushes: yes
        * Require code scanning results: no
        * Require code quality results: no
        * Automatically request Copilot code review: no
4) Create the ruleset

## Final touches
1) Clone the repo from Codeberg (SSH)
2) Update the README.md to include:
  ```markdown
  ## Cross-hosted
  This repository is hosted both on [GitHub](https://github.com/TheNamlessGuy/<repo name>) and [Codeberg](https://codeberg.org/TheNamlessGuy/<repo name>).
  ```
3) Push changes, and see how they mirror to GitHub
