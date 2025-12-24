# GitHub Actions Composite Actions Practice

## Overview
This project demonstrates the use of **composite actions** in GitHub Actions to create reusable workflow components with conditional caching logic.

## Key Features

### Custom Composite Action: `cache-deps`
Located in `.github/actions/cache-deps/action.yaml`, this reusable action:
- **Conditionally caches** `node_modules` based on an input parameter
- **Installs dependencies** via `npm ci` when cache misses or caching is disabled
- **Outputs** whether dependencies were installed or restored from cache

### Matrix Strategy with Environments
The workflow uses a **matrix strategy** to test both caching scenarios:

1. **`cache-true-environment`** - Caching enabled (`secrets.caching = "true"`)
   - Uses actions/cache@v5 to cache node_modules
   - Restores from cache on subsequent runs
   - Only installs when cache misses

2. **`cache-false-environment`** - Caching disabled (`secrets.caching = "false"`)
   - Skips caching step entirely
   - Always runs fresh `npm ci` install

### Environment Secret Configuration
- Created two GitHub repository environments
- Each environment has a `caching` secret set to `"true"` or `"false"`
- The composite action receives this secret as input and adjusts behavior accordingly

## Workflow Jobs
1. **Lint** - Lints code, displays install status output
2. **Test** - Runs tests, uploads test reports on failure
3. **Build** - Builds the project, uploads dist artifacts
4. **Deploy** - Downloads artifacts and simulates deployment

Each job runs in parallel for both environments to validate caching vs. non-caching workflows.
