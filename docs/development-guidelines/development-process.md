# Development process

Use GitHub flow by default when building or updating an Azure Developer CLI (`azd`) template.

Repositories may adopt additional release branches or packaging steps, but the day-to-day contribution path should stay simple, reviewable, and easy to automate.

## Recommended flow

1. **Define the scenario**
   - Capture the user problem, target architecture, and success criteria.
   - Identify the minimum `azd` assets required for the template: `azure.yaml`, infrastructure, and deployable app or configuration content.
2. **Track the work**
   - Create issues for meaningful changes.
   - Group related work with milestones or GitHub Projects when the effort spans multiple contributors.
3. **Build in a short-lived branch**
   - Branch from the default branch.
   - Keep changes focused enough for practical review and validation.
4. **Open a pull request**
   - Link the related issue.
   - Include evidence for validation, tests, and documentation updates.
   - Request the reviewers needed for the technologies involved.
5. **Merge through review**
   - Require pull requests for changes to the default branch.
   - Merge only after required checks and reviews pass.
6. **Prepare for release or submission**
   - Recheck the [definition of done](definition-of-done.md).
   - Follow [publishing-guidelines.md](../../publishing-guidelines.md) when the change affects public submission readiness.

## Branch protection and rulesets

- The default branch **SHOULD** be protected with GitHub branch protection or, where available, [rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets).
- The default branch **SHOULD** require pull requests and the checks that matter for your repository.
- Force pushes and direct pushes to the default branch **SHOULD** be limited to explicit administrative bypass cases only.

Rulesets are a good default because they are visible to readers, composable, and can be managed at repository or organization scope.

## Release models

GitHub flow is the default recommendation:

- work in short-lived branches,
- review through pull requests,
- merge into a protected default branch.

Repositories **MAY** add release branches, tags, or scheduled release trains if their maintainers need them. A long-lived `develop` branch is not required by this guidance set.
