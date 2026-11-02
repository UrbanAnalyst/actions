# Actions

GitHub actions for UrbanAnalyst workflows.

## push-to-elsewhere

Composite action to mirror a branch (with tags) to non-GitHub remotes.
Mirrors are passed by the caller, so none are hard-coded here:

```yaml
name: push-to-elsewhere

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

concurrency:
  group: push-to-elsewhere
  cancel-in-progress: false

jobs:
  mirror:
    runs-on: ubuntu-latest
    steps:
      - uses: UrbanAnalyst/actions/push-to-elsewhere@main
        with:
          branch: main
          mirrors: |
            codeberg=https://${{ secrets.UNAME }}:${{ secrets.CODEBERG }}@codeberg.org/UrbanAnalyst/repo.git
            codefloe=https://${{ secrets.UNAME }}:${{ secrets.CODEFLOE }}@codefloe.com/UrbanAnalyst/repo.git
```

On pull requests the step completes as a no-op so a required check reads
green rather than skipped; mirroring only happens on pushes to `main`
(override with the `branch` input).
