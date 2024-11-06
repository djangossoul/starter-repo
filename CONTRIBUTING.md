# Contribution Guidelines

Before filing a bug report or a feature request, be sure to read the contribution guidelines.

## How to use GitHub
We use GitHub exclusively for well-documented bugs, feature requests and code contributions. Communication is always done in English.

No free support here.

## Security issues

## I have found a bug
Before opening a new issue, please:
* update to the newest versions of WordPress and the plugin.
* search for duplicate issues to prevent opening a duplicate issue. If there is already an open existing issue, please comment on that issue.
* follow our _New issue_ template when creating a new issue.
* add as much information as possible. For example: add screenshots, relevant links, step by step guides etc.

## I have a feature request
Before opening a new issue, please:
* search for duplicate issues to prevent opening a duplicate feature request. If there is already an open existing request, please leave a comment there.
* add as much information as possible. For example: give us a clear explanation of why you think the feature request is something we should consider for the plugin.

## I want to create a patch
Community made patches, localizations, bug reports and contributions are very welcome and help us tremendously.

When contributing please ensure you follow the guidelines below so that we can keep on top of things.

#### Submitting an issue you found
Make sure your problem does not exist as a ticket already by searching through the existing issues. If you cannot find anything which resembles your problem, please create a new one.

#### Fixing an issue

* Fork the repository on GitHub (make sure to use the `develop` branch).
* Make the changes to your forked repository.
* Ensure you stick to the [WordPress Coding Standards](https://make.wordpress.org/core/handbook/best-practices/coding-standards/) and you properly document any new functions, actions and filters following the [documentation standards](https://make.wordpress.org/core/handbook/best-practices/inline-documentation-standards/php/).
* When committing, reference your issue and include a note about the fix.
* Push the changes to your fork and submit a pull request to the `develop` branch of the Accessibility Checker repository.

We will review your pull request and merge when everything is in order. We will help you to make sure the code complies with the standards described above.

#### 'Patch welcome' issues
Some issues are labeled 'patch-welcome'. This means we see the value in the particular enhancement being suggested but have decided for now not to prioritize it. If you however decide to write a patch for it, we'll gladly include it after some code review.

## Additional Resources

### Version Control with Git

We collaborate on code using Git. This is a powerful and standard version control system that allows parallel work and offers codebase history tracking. Our main platform for this is GitHub.

The following chapters describe the main guidelines about our standard Git management.

#### Branch management
##### Long-lived branches

Our standard repository configuration is to have two main branches: `trunk` and `develop`:

* `trunk` updates should correspond to releases. The current state of trunk should therefore be the codebase currently in production (for services) or the latest released version (for plugins). Pushes to trunk are related to a release process. For instance, a CI/CD process can be automated based on this action ; or releases can be tagged based on this branch.
* `develop` should be the main branch from which development branches will start from, and be merged into.
`develop` should always be strictly ahead of `trunk`. If it is not the case, `trunk` must be merged back into `develop`.

Additional long-lived branches might exist for specific needs, for instance for `transifex` ; but this must remain an exception.

##### Development branches
To contribute to a repository, developers should always create a new branch from develop and commit their changes there. Once the implementation is ready to be reviewed and validated, they should open a Pull Request to develop.

Those development branches should have a lifetime of up to a few days as we are working on small increments. In some cases, we need to implement a complex feature that requires more time, several contributors, and intermediate steps that should not be merged to develop before the whole feature is ready. In those cases, a `feature` branch must be created and used as the baseline for the small increments to start from it and to be merged into it.

During the development and review phases, it is the responsibility of the branch owner to regularly update it with develop (by merging develop into the branch) to stay up-to-date and identify and fix conflicts as early as possible.

##### Branch naming
Apart from the long-lived branches, branch names must be self-explanatory and follow a standard format: `branch_type/issue_number-short_description`. This is valuable for other teammates, reviewers and testers to quickly identify the context of code changes. The following values are recommended, but you can exceptionally use other values if you see fit, as long as the branch name remains self-standing.

* `branch_type` should be one of the following:
  * `feature` for branches introducing a new feature.
  * `fix` for branches that fix a bug.
  * `enhancement` for branches introducing an enhancement of an already-existing feature
  * `chore` for branches handling technical maintenance such as editing documentation, adding tests, cleaning up code, changing version numbers, updating dependencies, etc.
* `issue_number` should be the number identifying the GitHub issue this PR is addressing.
* `short_description` should be a few words, separated by `-`, that quickly explain the topic of the branch.
  
Here are a few examples:

* `feature/3700-remove-unused-css`
* `enhancement/2773-no-beta`
* `fix/3437-picture-lazyload-exclusion`

##### Branch protections
Branch protections are rules that can prevent some events on specific branches. They can be configured for each repository on GitHub in the Settings of the repository. Those rules enforce the peer-review process, ensuring the code we produce and released is at least validated by two developers, and that our quality standards are met (code style, automated testing, coverage, etc.).

Our standard configuration is to protect both the `trunk` and the `develop` branches with different settings. The configuration of specific repository can differ, based on the specific context and needs; the following guidelines are recommendations for mature repositories with trustable CI and automations.

On `develop`, we apply the following rules:

* Require a pull request before merging: all commits must be made to a non-protected branch and submitted via a pull request before they can be merged into a branch that matches this rule.
  * Require 1 approval: pull requests targeting a matching branch require a number of approvals and no changes requested before they can be merged.
  * Dismiss stale pull request approvals when new commits are pushed: New reviewable commits pushed to a matching branch will dismiss pull request review approvals.
* Require status checks to pass before merging: critical steps of the CI must be validated before merging, typically linter and tests.
  * Require branches to be up-to-date before merging: This ensures that the current state of develop always passes the CI. Those rules are quite restrictive to ensure that, under the nominal development flow, our quality standards are applied. When available (on public repositories), we can enable the GitHub merge queue to automate the update of branches, CI runs and merge. This is not available on private repositories.
  * 
On `trunk`, we apply less restrictive rules:

* Require a pull request before merging: all commits must be made to a non-protected branch and submitted via a pull request before they can be merged into a branch that matches this rule.
  * Dismiss stale pull request approvals when new commits are pushed: New reviewable commits pushed to a matching branch will dismiss pull request review approvals. Since `trunk` is used as the baseline for deployments, it is important to keep it easily accessible for eventual hotfixes. Engineering teammates are responsible for pushing to this branch responsibly.

#### Closing branches
Branch owners are responsible for ensuring their branches don’t end up stale. If they are relevant, they should be merged and deleted, otherwise they can be closed and deleted. To merge a branch, our review process must be followed.

Once a branch has been merged (or closed), it should be deleted. This ensures that we keep only branches for ongoing work, easing the identification of in progress development on a repository.

### Commits
Usually, a single developer is working on a branch. Therefore, it could seem sufficient to commit all changes once they are ready and working. However, this can cause troubles if one needs to circle back to the implementation to investigate a bug or understand what has been implemented. More numerous, small and well-scoped commits can help to navigate the changes, test intermediate versions for a bug, and eventually precisely rollback changes. Therefore, commits should rather be done frequently, and only contain a small, well-defined scope.

Commit messages are critical for teammates to navigate the history and quickly understand the reason of a change. The first line of a commit message is a brief summary of the commit, describing the expected result of the change or what is modified in the code. It should be a continuation of “This commit will…”. Then, a longer message can be added to provide more details if needed. This appended message must be separated from the first line by a blank line, to ensure the first line remains the highly visible description.

### Releases
Releases are linked to a push to the `trunk` branch typically for a merge from `develop`, and ideally automated through CI/CD (GitHub actions). The deployment process may vary depending on the repository, and is quite different for our plugins and our services for example. However, we have some common steps. All our releases should be tagged as such on their GitHub repository and an internal release note should be shared internally. Those steps should ideally be automated as they facilitate knowledge sharing and ease investigation in case of bugs or regressions.

Thanks to our branch management, branch protections and CIs, code must have been reviewed and must be passing the CI before getting into a release. Depending on the project, manual tests on the `develop` branch can be conducted to ensure the version is properly working.

#### Version name/numbers
It is important to clearly name each release to ease the discussion about them. Depending on the repository, we can have 2 version naming system.

* For releases deliverable to users (typically WordPress plugins), we use semantic versioning, which is also the recommended standard for PHP and WordPress. This syntax is well understood by most users and fits well with our release cycle of plugins.
* For service releases (websites, apps, back-ends, …), we use timestamp versioning with the following syntax: yyyy.MM.dd-hhmm. This approach better fits continuous delivery where version bumps are more of a burden than providing actual benefits. The timestamp is automatically picked during the automated delivery.

#### Release notes
Keeping all teammates up-to-date and providing them with a consistent way of knowing about the changes is key in ensuring all operations can smoothly be performed and continuously adapted company-wide: this is true for engineering teammates but also support, product, marketing, etc. Standardize release notes are a great way to achieve this goal.

Every release must have a corresponding release note in our corresponding Notion database. Internal release notes (apps, websites, back-ends, …) should be automated. Release notes can also be manually added or appended if needed.

To be useful and readable, releases notes must:

* Reference the corresponding version number, product and date;
* List all technical changes with a link to the corresponding GitHub issue or PR (for automated release notes, the automated release description from GitHub should be leveraged);
* Explain the impact and changes experienced from a user perspective.
        
## Pull Requests & Code Reviews
### Why do we need code reviews?
Crafting software is complex, and as good as they can be, developers are human beings: they can make mistakes, miss something, or simply not think about the best approach right away.

We are working in teams, where several developers will successively work on the same code. To ease this collaboration, we need to standardize our code style, the code quality and expectations: moving from one codebase to another must be as seamless as possible, and anyone should be able to pick up anyone else code seamlessly.

Finally, reviews are a great way to share knowledge, continuously learn and improve yourself and your team.

Therefore, code reaching production must be passing all automated checks, and be at least approved by one peer. To apply this process, we leverage Pull Requests (PR) on GitHub (or Merge Requests on GitLab).

## Opening a Pull Request
### When to open a Pull Request?
You don’t have to wait for the code to be completed to open a draft PR. It can be beneficial to share your work early on to ask for and get feedback. The sooner you get the feedback, the quicker you can improve your code.

Once you consider that your code is ready to be shipped into production, you should open a PR (or remove the draft status of the existing PR).

### Target branch
If your PR is part of a more complex code change and is not useful on its won, not meant to be delivered on its own, or depends on other code changes not merged yet, then the target branch must be a dedicated feature branch. Only when the feature is complete, this branch will be merged back into the `develop` branch.

If your PR covers a complete fix/enhancement/feature on its own, then the target branch should be the `develop` branch.

### Pull Request description
The Pull Request description is the document that will follow your code until it reaches production, through peer reviews, QA testing, release notes and documentation. For all teammates to perform those steps accurately, they need to quickly grasp the context of your PR, the reasons why the implementation has been done this way, what it changes, etc. While this is obvious to you after spending hours working on it, it is likely not the case for teammates ; so make sure to make the Pull Request as self-standing as possible!

To achieve this, a default Pull Request template is automatically applied on new Pull Requests of WP Media owned repositories on GitHub. This template is maintained here. Specific templates can override this generic one in some specific repositories.

Note: Our CI pipelines on GitHub should contain this GitHub action that validates proper filling of the Pull Request description.

### Getting reviews
As the PR owner (creator), you are responsible for getting reviews once the PR is ready. A PR is considered ready when the automated CI checks are passing and that you consider your implementation completed. Then, make sure to properly update your PR description, make it visible and, if needed, ask for reviews.

Many things happen every day within our teams: it is easy to miss an opened PR. To make sure your PR get reviews check that it is visible as “Ready for Review” on your team’s board, in the current sprint. Also assign the team tag as reviewer, so that teammates are notified.

Our teams SLA is to handle reviews within 24 hours. If you don’t get any reviews after 24 hours, kindly ask for one on Slack specifying what the PR is about and consider pinging relevant teammates. You should also leverage the daily stand-up meeting to find reviewers.

## CI - Automated Continuous Integration Pipeline
Many checks and validations can be done automatically on each PR, without the need for a human to perform any actions. The set of automated checks is called the Automated Continuous Integration Pipeline (CI). While it can differ between each repository, mostly depending on the language, we have baselines that should be common to all our CIs.

On GitHub, we leverage GitHub actions to implement our CIs. For each main task to be performed by the CI, it is preferred to use separate jobs so that they can fail independently, facilitating investigation and allowing to retry only the needed steps.

### Pull Request Description validation
The importance of the Pull Request description is emphasized above in this document. We have an automated check that ensures the standardized PR template is properly filled. This check can help quickly identify Pull Requests not up to standards so that the owner can fix it, and avoid reviewers to begin reviewing while some context is missing.

### Linting, Code Style and Code Standards
Developers have to work on many repositories. To minimize cost of context switching and feel at home in every codebase, it is important to share common style guidelines. Also, keeping code easy and natural to read is key for efficiency in the long run.

All our CI must include steps and tools to:

* Check code style. This ensures conventions and rules about formatting and code structure are applied (indentation, naming conventions, spacing, etc.)
* Check code standards. This ensures general best practices of the language are applied so that the implementation remains safe, organized and robust.
* Check linting. This ensures there are not syntax errors and identify potential bad patterns, useless complexity and potential bugs.
* 
For each language that we use, we leverage different tools. Refer to the section of each language for more details and examples.

### Running automated tests
Automated tests are a key asset of our codebases. They are designed to capture some behaviors of the codebase and check that they can consistently be reproduced across evolutions of the software.

Every time we alter the code (hence, for every PR), we must ensure that those behaviors are still reproducible. So all tests must run and pass as part of the CI.

If tests are not passing, it means a behavior changed:

* if this is expected as a result of the code change, the tests must be adapted to reflect this change;
* if this is not expected, then a regression has been introduced and the code change must be reviewed to fix the issue.
For each language that we use, we leverage different tools. Refer to the section of each language for more details and examples.

### Diff Coverage
To ensure that the changes we introduce will be stable over time and prevent them to be impacted by regressions, it is critical to cover those changes with automated tests. Adding tests to capture the behaviors we introduce is a great way to demonstrate how the code should behave, and to allow future maintainers to easily check they did not alter this expected behavior.

Diff coverage step identifies all lines being modified by your PR, and checks whether they are executed during test execution. This approach allows to only focus on what the current PR is changing, regardless of the overall coverage level of the repository.

When a line is not covered, it means that it could be removed or altered without any tests failing, which is a great risk for regression in the future. On the contrary, a covered line is not totally safe: a line being executed during a test does not mean its logic is fully tested and validated. Therefore, diff coverage must be taken as an indication of untested lines of code for developers to self-review and improve autonomously their coverage.

We globally aim at a minimum of 50% diff coverage. Below this, this step of the pipeline must fail. This should not be a mandatory step to validate, as it depends on the context of the PR and the impacted lines.

We have two ways of computing and reporting diff coverage, depending on the repository we are working on.

#### Diff coverage with Codacy
Codacy is a platform that analyses git repositories and additional reports such as code coverage to report metrics. We use it on our public repositories as those features are free for them.

To get diff coverage reported directly on the Pull Requests:

* Add the repository on the Codacy account and check its configuration, especially Integrations & Gates.
* Authorize Codacy app on GitHub to access the repository.
* Instruct the test tools to output a coverage report during the CI. For instance, with --coverage-php tests/report/unit.cov for PHPUnit, or --cov=. --cov-report=xml for pytest.
* Once the report is available, upload it to Codacy using the dedicated GitHub action. You can check the CI of apply-filters-typed as a good example. Here are the steps related to Codacy:

```
    - name: Unit/Integration tests
      run: composer run-tests

    - name: Merge Code Coverage Reports
      run: composer report-code-coverage

    - name: Run codacy-coverage-reporter
      uses: codacy/codacy-coverage-reporter-action@v1
      with:
        project-token: $
        coverage-reports: tests/report/coverage.clover

```

#### Diff coverage with diff-cover
diff-cover is an open-source library that computes diff coverage based on coverage reports and git diff, for any languages. We use it for our private repositories for which Codacy is not available.

To get diff coverage reported directly on the Pull Requests:

* Instruct the test tools to output a coverage report during the CI. For instance, with 	`--coverage-php tests/report/unit.cov` for PHPUnit, or `--cov=. --cov-report=xml` for pytest.
* Once the report is available, add steps in the CI to run diff-cover and send comments and annotations to the PR. As an example, check out the TB-TT CI.

```
    - name: Run pytest
      run: |
        pytest -m "not staging_env" --cov=. --cov-report=xml 
      shell: bash

      - name: Generate diff-coverage report
        if: github.event_name == 'pull_request'
        run: |
          diff-cover coverage.xml --compare-branch=origin/$ --markdown-report diff-cover-report.md --exclude test*.py --fail-under=50 --expand_coverage_report
          echo "DIFF_COVER_EXIT_STATUS=$?" >> $GITHUB_ENV
        shell: bash

      - name: Delete previous diff-cover reports
        uses: actions/github-script@v6
        with:
          script: |
            const { data: comments } = await github.rest.issues.listComments({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number
            });
            
            for (const comment of comments) {
              if (comment.user.login === 'github-actions[bot]' && comment.body.includes('# Diff Coverage')) {
                console.log(`Deleting comment with ID: ${comment.id}`);
                await github.rest.issues.deleteComment({
                  owner: context.repo.owner,
                  repo: context.repo.repo,
                  comment_id: comment.id
                });
              }
            }
        env:
          GITHUB_TOKEN: $ 

      - name: Post diff-cover report to PR
        if: github.event_name == 'pull_request'
        uses: actions/github-script@v6
        with:
          script: |
            const fs = require('fs');
            const comment = fs.readFileSync('diff-cover-report.md', 'utf8');
            await github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: comment,
            });
      - name: Fail job if coverage is below threshold
        if: github.event_name == 'pull_request'
        run: |
          if [[ "$" -ne 0 ]]; then
            echo "Coverage below threshold; failing the job."
            exit 1
          fi
        shell: bash
```
