1. What is a GitHub Actions workflow?

Answer: A GitHub Actions workflow is an automated process that runs one or more jobs defined in a YAML file. Workflows are used for continuous integration and continuous delivery (CI/CD) pipelines.

Example:
```
name: CI

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Build
        run: make build
```

2. How do you define secrets in GitHub Actions and use them in workflows?

Answer: Secrets are defined in the repository settings and can be used in workflows by referencing them with the secrets context. They are injected as environment variables.

Example:
```
env:
  MY_SECRET: ${{ secrets.MY_SECRET }}
```


3. What are reusable workflows and how can you use them?

Answer: Reusable workflows allow you to define a workflow in one repository and reuse it in other workflows. They are defined using the workflow_call event.

Example: In workflow.yml:
```
name: Reusable Workflow

on:
  workflow_call:

jobs:
  reuse:
    runs-on: ubuntu-latest
    steps:
      - name: Echo message
        run: echo "This is a reusable workflow"


In another repository:


name: Use Reusable Workflow

on: [push]

jobs:
  use:
    uses: user/repo/.github/workflows/workflow.yml@main

```



4. How can you cache dependencies in GitHub Actions to speed up builds?

Answer: Caching can be done using the actions/cache action to store and restore dependencies. You specify a cache key and path.
```
steps:
  - name: Cache Node modules
    uses: actions/cache@v3
    with:
      path: node_modules
      key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
      restore-keys: |
        ${{ runner.os }}-node-

```



5. Explain the use of matrix builds in GitHub Actions.

Answer: Matrix builds allow you to run a job across multiple versions of a language or OS. This is useful for testing across different environments.
```

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node: [12, 14, 16]
    steps:
      - uses: actions/checkout@v2
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: ${{ matrix.node }}
      - run: npm install


```




6. How do you handle conditional execution of jobs or steps in GitHub Actions?

Answer: You can use if conditions to run jobs or steps based on certain criteria, such as the branch name or success of a previous step.
```
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run tests
        if: github.event_name == 'push'
        run: npm test
```


7. What is the purpose of the jobs.<job_id>.needs property?

Answer: The needs property specifies that a job depends on the completion of one or more other jobs. This creates a job dependency chain.
```

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - run: make build

  test:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - uses: actions/checkout@v2
      - run: make test


```



8. How can you use GitHub Actions to deploy to multiple environments (e.g., staging and production)?

Answer: You can use separate jobs or workflows for different environments and control deployment using environment-specific secrets and conditions.
```
jobs:
  deploy-staging:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - run: deploy-to-staging.sh
    env:
      STAGING_SECRET: ${{ secrets.STAGING_SECRET }}

  deploy-production:
    runs-on: ubuntu-latest
    needs: deploy-staging
    steps:
      - uses: actions/checkout@v2
      - run: deploy-to-production.sh
    env:
      PRODUCTION_SECRET: ${{ secrets.PRODUCTION_SECRET }}

```


9. What is the difference between run and uses in GitHub Actions?

Answer: run is used to execute shell commands directly, while uses is used to invoke an action defined in a repository or in the GitHub Marketplace.
```
steps:
  - name: Checkout code
    uses: actions/checkout@v2

  - name: Install dependencies
    run: npm install
```



10. How do you handle versioning of GitHub Actions used in workflows?

Answer: You should specify the exact version or tag of an action to ensure consistent behavior. Using a specific version tag or commit hash is recommended over using latest.
```
- uses: actions/checkout@v2
```



11. How can you trigger workflows on a schedule using GitHub Actions?

Answer: You can use the schedule event to define workflows that run on a cron schedule.
```
on:
  schedule:
    - cron: '0 0 * * *' # Runs at midnight every day

```



2. What are GitHub Actions artifacts and how do you use them?

Answer: Artifacts are files or directories created by a workflow that can be uploaded and downloaded later. They are useful for sharing build results or logs.
```
steps:
  - name: Build
    run: make build

  - name: Upload artifact
    uses: actions/upload-artifact@v3
    with:
      name: build-results
      path: build/

```



13. Explain how to use concurrency in GitHub Actions workflows.

Answer: The concurrency feature ensures that only one job or workflow runs at a time for a given concurrency group. It helps in avoiding conflicts and redundant builds.
```
concurrency:
  group: ${{ github.ref }}
  cancel-in-progress: true
```



14. How can you integrate GitHub Actions with external systems or APIs?

Answer: You can use custom scripts or actions to interact with external systems or APIs. Typically, you would use the run step with curl or a custom action.
```
steps:
  - name: Call external API
    run: |
      curl -X POST -H "Authorization: Bearer ${{ secrets.API_TOKEN }}" \
      -d '{"key":"value"}' \
      https://api.example.com/endpoint
```



15. What are self-hosted runners and how do you configure them?

Answer: Self-hosted runners are machines you configure to run GitHub Actions workflows. They can be used to run workflows on custom hardware or in specific environments.

Example:

    Go to the GitHub repository settings and select "Actions" > "Runners".
    Follow the instructions to download and configure the runner on your machine.
    Start the runner service to make it available for workflows.
```
    ./config.sh --url https://github.com/OWNER/REPO --token YOUR_TOKEN
```


