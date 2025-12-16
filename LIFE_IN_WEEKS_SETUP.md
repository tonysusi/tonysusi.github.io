# Setting up Life in Weeks on GitHub Pages

You have two options for deploying your `life-in-weeks` Hugo site to GitHub Pages:

## Option 1: Deploy from the Submodule Repository (Recommended)

This sets up GitHub Pages in the `tonysusi/life-in-weeks` repository itself, making it available at `https://tonysusi.github.io/life-in-weeks/`.

### Steps:

1. **Navigate to your submodule repository** (`tonysusi/life-in-weeks`) on GitHub
2. **Create a GitHub Actions workflow** in that repository:
   - Go to the repository → Actions → New workflow
   - Create a new file: `.github/workflows/hugo.yml`
   - Use the workflow content provided below
3. **Configure GitHub Pages** in the repository settings:
   - Go to Settings → Pages
   - Under "Source", select "GitHub Actions"
4. **Update the baseURL** in `hugo.toml` (already done - set to `https://tonysusi.github.io/life-in-weeks/`)

### Workflow for the submodule repository (`.github/workflows/hugo.yml`):

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.143.1'
          extended: false

      - name: Build
        run: hugo --gc --minify

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

## Option 2: Build and Deploy from Main Repository

This workflow (already created in `.github/workflows/deploy-life-in-weeks.yml`) builds the Hugo site from the submodule and commits the built files to your main repository. However, this approach has limitations:

- The built files will be committed to your main repo (clutters the repository)
- GitHub Pages serves from the root of a branch/folder, so serving a subdirectory requires additional configuration
- You may need to manually configure the publishing source to point to a specific folder

### Current Workflow

The workflow at `.github/workflows/deploy-life-in-weeks.yml` will:
1. Check out the repository with submodules
2. Build the Hugo site
3. Copy the built files to `life-in-weeks-build/` directory
4. Commit and push the built files

**Note**: To serve this from a subdirectory, you'll need to configure GitHub Pages to publish from a branch that contains the built files in the correct structure, or use a custom domain setup.

## Recommendation

**Use Option 1** - it's cleaner, follows GitHub Pages best practices, and keeps your main repository uncluttered. The submodule repository will have its own GitHub Pages site that you can access at `https://tonysusi.github.io/life-in-weeks/`.

## Next Steps

1. If choosing Option 1:
   - Go to `https://github.com/tonysusi/life-in-weeks`
   - Create the workflow file as described above
   - Configure Pages settings to use GitHub Actions
   - Push any changes to trigger the workflow

2. If choosing Option 2:
   - The workflow is already set up
   - Push changes to the `life-in-weeks` submodule to trigger the build
   - Configure GitHub Pages publishing source if needed

## References

- [GitHub Pages Documentation](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site)
- [Using submodules with GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/using-submodules-with-github-pages)

