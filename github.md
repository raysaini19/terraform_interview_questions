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
