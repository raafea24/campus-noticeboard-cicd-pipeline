# Campus Noticeboard CI/CD Pipeline

A responsive, static campus information portal backed by an automated quality-gated delivery workflow. The project centralizes notices, examination information, admissions guidance, and contact details while demonstrating repeatable validation, bundling, and container build practices.

## What the Project Solves

Campus information is often distributed across disconnected channels. This project provides a single browser-based noticeboard with dedicated pages for announcements, examinations, admissions, and contact information. Its automation pipeline reduces manual validation by enforcing HTML and CSS standards before accepting a successful application build or container build.

## Implementation

- Five semantic HTML pages with a shared responsive CSS design system
- Parcel development server and optimized static-site build
- HTMLHint and Stylelint quality gates
- GitHub Actions jobs ordered as `lint -> build -> docker`
- Reproducible dependency installation through `npm ci` and `package-lock.json`
- Docker image based on `nginx:alpine` for serving the static source on port 80
- GitFlow-oriented development history using `main`, `develop`, and feature branches

The workflow runs for pushes to `develop` and pull requests targeting `develop`. The Docker job validates that the web application can be packaged as an nginx image; the current workflow does not publish the image to a registry or deploy it to an environment.

## Technology Stack

| Area | Technology | Role |
| --- | --- | --- |
| Front end | HTML5, CSS3 | Static content and responsive presentation |
| Runtime | Node.js 20 | CI and local tooling environment |
| Build | Parcel 2 | Bundling and production asset optimization |
| Quality | HTMLHint, Stylelint | Markup and stylesheet validation |
| Automation | GitHub Actions | Ordered CI quality gates |
| Container | Docker, Nginx Alpine | Portable static-site serving image |
| Version control | Git, GitFlow | Feature integration and release history |

## Project Structure

```text
.
|-- .github/workflows/ci.yml  # Lint, build, and Docker validation jobs
|-- src/                      # Five HTML pages
|-- styles/style.css          # Shared responsive design system
|-- .htmlhintrc               # HTML quality rules
|-- .stylelintrc.json         # CSS quality rules
|-- Dockerfile                # Nginx image definition
|-- package.json              # Scripts and development dependencies
|-- package-lock.json         # Reproducible dependency resolution
`-- BRANCH_STRUCTURE.md       # Historical branching model
```

Generated directories such as `node_modules/`, `dist/`, and `.parcel-cache/` are intentionally excluded from version control.

## Local Development

### Prerequisites

- Git
- Node.js 20 or a compatible supported version
- Docker, only when building or running the container

```bash
git clone https://github.com/raafea24/campus-noticeboard-cicd-pipeline.git
cd campus-noticeboard-cicd-pipeline
npm ci
npm run dev
```

Parcel opens the local development server at `http://localhost:1234` by default.

## Quality and Build Commands

```bash
npm run lint:html
npm run lint:css
npm run build
```

`npm run build` writes generated assets to `dist/`. The CI workflow requires both linters to pass before the Parcel build begins, and requires the build job to pass before validating the Docker image.

## Container Usage

The Dockerfile serves the tracked HTML and CSS directly through nginx:

```bash
docker build -t campus-noticeboard:latest .
docker run --rm -p 8080:80 campus-noticeboard:latest
```

The site is then available at `http://localhost:8080`.

## CI Workflow

The workflow at `.github/workflows/ci.yml` contains three dependent jobs:

1. **Lint** — checks all HTML with HTMLHint and the shared stylesheet with Stylelint.
2. **Build** — runs the Parcel production build after linting succeeds.
3. **Docker** — builds the nginx image after the application build succeeds.

Each Node.js job uses the lockfile-aware npm cache and installs dependencies with `npm ci`. The workflow deliberately stops at container build validation; registry publishing and environment deployment require a separate, credential-aware release process.

## Branching Model

The repository history follows a GitFlow-style integration path: feature branches merge into `develop`, and validated integration work is promoted to `main`. See [BRANCH_STRUCTURE.md](BRANCH_STRUCTURE.md) for the roles reflected in the project history.
