| :exclamation:  This is a work in progress.   |
|----------------------------------------------|


# Github Actions Cheat Sheet

## Introduction

GitHub Actions serve as a cornerstone in modern software development, offering a dynamic and automated approach to building, testing, and deploying code. However, as development workflows become more interconnected and reliant on automation, the importance of securing these processes, especially in the context of the software supply chain, cannot be overstated. This cheat sheet is designed to provide developers with essential guidance on securing GitHub Actions, addressing critical aspects like secrets management and workflow configuration. By implementing these security best practices, developers contribute to a robust defense against potential supply chain attacks, ensuring the integrity and trustworthiness of their software delivery pipeline.

POSSIBLE CATEGORIES
- Workflow Structure and Configuration
- Secrets and Authentication
- Workflow Events and Triggers
- Dependency Management
- Security Best Practices
- Code and Configuration Security
- Workflow Access and Approval

Recommendations:

* Restrict Github Actions that can be used. "Allow actions created by Github", "Allow Marketpace actions by verified creators", "Allow specified actions and reusable workflows" (whitelist at max 1000). 
* Use same kinds of best practices you would use with other source code repositories for your repositories for custom Github Actions.
* Do not use self-hosted runners with public repositories.
* Manage network access to self-hosted runners. (For instance, with a Github enterprise or organization account, you can use an allowed list for IP addresses or a proxy server.)
* Do not use id property if not needed. (it can be used to retrieve information about the run)
* Restrict GITHUB_TOKEN permissions
* Use custom deployment protection rules.
* Do not use set-output. (it is deemed unsafe)
* Configure the artifact retention policy
* Do not store any sensitive information in the cache. 
* Do not always rely on always().
* Use Github secrets for secrets (do not print them, use a credential rolling process)
    - Never use structured data as a secret (https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)
    - Register generated secrets within the workflow as secrets (https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)
    - Consider requiring review for access to secrets (https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)
* Use pull_request instead of pull_request_target (https://securitylab.github.com/research/github-actions-preventing-pwn-requests/)
    * If a workflow need access, consider splitting the workflow into multiple pieces.
    * Consider using separate workflows for trusted and untrusted events to minimize risk.
    * Also do not use workflow_run
    (Workflows triggered with pull_request_target/workflow_run will have write permission to the target repository and access to target repository secrets. PR authors may compromise the repository by using build scripts controlled by the author of the PR or reading token in memory.)
* Use dependabot to keep your actions up-to-date (https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions#using-dependabot-version-updates-to-keep-actions-up-to-date)
* Prefer actions from reputable sources, use scorecard
* Minimize the permissions granted to Github Actions. use read-only permissions where possible. (https://securitylab.github.com/research/github-actions-building-blocks/)
* Prevent injection attacks (https://securitylab.github.com/research/github-actions-untrusted-input/) (https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions#understanding-the-risk-of-script-injections)
    - do not use issue title, PR title, etc.
    - 
* Use commit hash of stable versions (do not use master tag) (https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions#using-third-party-actions)
* Do not use the add::mask command to prevent sensitive values from appearing as plaintext in workflow logs.
* Do not overwrite Github default variables
* Do not dump Github context data into logs as it might contain sensitive information
* Use CODEOWNERS file to require approval from a designated reviewer for changes on .github/workflows
* Prevent workflow runs from pull requests originating from repository forks, in Settings -> Actions tab, ensure "Fork pull request workflows" is disabled.
* Enable "Require approval for all outside collaborators" in Settings -> Actions tab
* Use OpenID Connect (OIDC) instead of long-lived secrets to allow your workflows to interact with your cloud provider. (https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions#using-openid-connect-to-access-cloud-resources)
* Use starter workflows for code scanning (https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions#using-starter-workflows-for-code-scanning)
* Prevent Github Actions from creating or approving pull requests (https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions#preventing-github-actions-from-creating-or-approving-pull-requests)
* Harden your self-hosted runners (https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions#hardening-for-self-hosted-runners)

## References

Any useful references to other useful resources that aren't linked inline elsewhere in the cheat sheet.
