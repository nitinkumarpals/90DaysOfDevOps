# Day 39 -- What is CI/CD?

## Task 1 -- The Problem

### What Can Go Wrong?

-   Code conflicts between developers
-   Bugs reaching production
-   Manual deployment mistakes
-   Environment mismatch issues
-   No automated testing before release

### What Does "It Works on My Machine" Mean?

"It works on my machine" means the application runs correctly on the
developer's local system but fails in staging or production due to
differences in environment, dependencies, or configuration.

This is a real problem because production environments must be
consistent and reproducible.

### How Many Times Can a Team Safely Deploy Manually?

Usually 1--2 times per day at most. Manual deployments are slow, risky,
and error-prone.

------------------------------------------------------------------------

## Task 2 -- CI vs CD

### Continuous Integration (CI)

Developers frequently merge code into a shared repository. Each merge
triggers automated builds and tests to catch bugs early.

**Example:** A GitHub Action runs tests automatically every time code is
pushed.

------------------------------------------------------------------------

### Continuous Delivery

Code is automatically built and tested, and kept ready for release.
Deployment to production requires manual approval.

**Example:** After tests pass, the app is automatically prepared for
release but requires a button click to deploy.

------------------------------------------------------------------------

### Continuous Deployment

Every change that passes tests is automatically deployed to production
without human intervention.

**Example:** A SaaS startup automatically deploys every successful
commit to production.

------------------------------------------------------------------------

## Task 3 -- Pipeline Anatomy

-   **Trigger** -- Event that starts the pipeline (push, pull request,
    schedule)
-   **Stage** -- Logical phase (build, test, deploy)
-   **Job** -- Unit of work inside a stage
-   **Step** -- Single command or action inside a job
-   **Runner** -- Machine that executes the job
-   **Artifact** -- Output produced (Docker image, build file, logs)

------------------------------------------------------------------------

## Task 4 -- Pipeline Diagram

Text-Based Pipeline:

Developer Pushes Code │ ▼ Trigger: GitHub Push Event │ ▼ Stage 1:
Build - Install dependencies - Build application │ ▼ Stage 2: Test - Run
unit tests - Run lint checks │ ▼ Stage 3: Deploy (Staging) - Build
Docker image - Push to registry - Deploy to staging server

------------------------------------------------------------------------

## Task 5 -- Explore in the Wild

Repository Observed: Kubernetes

What triggers it? - Push events - Pull requests

How many jobs? - Multiple jobs (build, test, lint, security checks)

What does it do? - Builds components - Runs automated tests - Performs
validation checks - Ensures code quality before merge

------------------------------------------------------------------------

## Key Learnings

1.  CI/CD is a practice, not just a tool.
2.  Automation reduces deployment risk.
3.  Pipeline failures help catch problems early.
4.  CI catches bugs early; CD automates release processes.

