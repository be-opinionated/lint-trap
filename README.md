# lint-trap
CI actions to automatically fix linting issues

## Concept

This project is a place for actions and workflows for various CI systems
specifically for automatically fixing linting issues. It's not meant to do
any of that fixing itself, but rather install and run the appropriate tools
to do so for the user and commit those changes back into the repository.

## Development

This is in very early development. If you want to use it, make sure you
get from a specific commit.

## Usage

The `test-trap.yaml` workflow contains an up to date sample. A subset is
replicated here with explanation of what's important.

```yaml
# Wherever you want this to run...
on:
  pull_request:

# You should always only be running one job that commits changes at a time.
# Using the git ref as a concurrency flag allows this even with multiple 
# workflows that commit.
concurrency: ${{ github.ref }}

# In order to push the changes, the workflow needs permission to write to
# the repository contents.
permissions:
  contents: write

jobs:
  run-action:
    steps:
      # You must check out the appropriate repository before running Lint Trap
      - uses: actions/checkout@v3
      - uses: be-opinionated/lint-trap
        with:
          # The commands here are just examples of the kinds of auto-formatters
          # you might want to run. They don't work on their own.
          fix-commands: | 
            black
            ruff --fix
            isort
            gofmt
            rustfmt
          commit-message: "Format with a bunch of autoformatters."
```
