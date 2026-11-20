# Branching Strategy

The project history uses a GitFlow-style model to separate feature development, integration, and release-ready code. This document records the branch roles reflected in the repository history; branch protection is configured at repository-host level and is not defined by the source files.

## Long-Lived Branches

| Branch | Role |
| --- | --- |
| `main` | Release-ready project history |
| `develop` | Integration point for completed feature work and the target of the CI workflow |

## Feature Branches

| Branch | Implemented scope |
| --- | --- |
| `feature/project-setup` | Initial Node.js project structure and the five campus information pages |
| `feature/dev-tools-setup` | HTMLHint, Stylelint, Parcel, GitHub Actions, and Docker tooling |
| `feature/user-interface` | Responsive design system, animations, focus states, and reusable page components |

These branches preserve the development sequence and contributor attribution in Git history. Completed feature work was integrated into `develop` and subsequently merged into `main`.

## Integration Flow

```text
feature/*
    |
    v
develop  -- lint -> build -> docker validation
    |
    v
main
```

The workflow in `.github/workflows/ci.yml` is triggered by pushes to `develop` and pull requests targeting `develop`. It validates markup and styles, produces a Parcel build, and confirms that the Docker image can be built. Image publication and environment deployment are outside the current workflow.

## Recommended Contribution Flow

1. Create a focused `feature/*` branch from `develop`.
2. Commit the implementation with clear, scoped messages.
3. Open a pull request targeting `develop`.
4. Require the configured CI jobs and review policy to pass before merging.
5. Promote validated integration work from `develop` to `main` through a reviewed release merge.
