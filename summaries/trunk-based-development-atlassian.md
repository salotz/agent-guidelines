# Summary: Trunk-Based Development

Source: https://www.atlassian.com/continuous-delivery/continuous-integration/trunk-based-development

## Definition
Trunk-based development is a version control practice where developers merge small, frequent updates directly to a core “trunk” (main) branch.

## Key Characteristics
- Main branch is always assumed stable and ready to deploy.
- Short-lived feature branches (or direct commits to trunk).
- Frequent merges to main.
- Favors rebasing over long-lived merges.
- Enables continuous integration and faster delivery.

## Benefits
- Reduces integration friction.
- Enables continuous code review (small changes are easier to review).
- Supports CI/CD and frequent production releases.
- Main branch stays "green".

## Comparison
- Simpler than Gitflow (which uses multiple long-lived branches like develop, release, hotfix).
- All developers can commit to main (with appropriate review/automation).

## Recommendation in this project
Unless otherwise specified, assume trunk-based development with short-lived branches and heavy use of rebasing. Treat commits as logical "layers".