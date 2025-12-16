# Life in Weeks - GitHub Pages Deployment (Option 2)

This document explains how the `life-in-weeks` Hugo site is deployed to GitHub Pages from the main repository.

## How It Works

The workflow (`.github/workflows/deploy-life-in-weeks.yml`) automatically:

1. **Checks out the repository** with submodules
2. **Restores the submodule** if it was previously replaced by built files
3. **Builds the Hugo site** from the submodule source
4. **Replaces the submodule directory** with the built static files
5. **Commits and pushes** the built files to the repository

## Accessing the Site

Once deployed, your Life in Weeks site will be available at:
**https://tonysusi.github.io/life-in-weeks/**

## Workflow Triggers

The workflow runs automatically when:
- Changes are pushed to files in the `life-in-weeks/` directory
- The workflow file itself is modified
- Manually triggered via "workflow_dispatch" in the Actions tab

## Important Notes

- **Submodule Replacement**: The workflow replaces the submodule directory with built static files. On the next run, it automatically restores the submodule to build from the source again.
- **Base URL**: The Hugo site is configured with `baseURL = 'https://tonysusi.github.io/life-in-weeks/'` in `life-in-weeks/hugo.toml`
- **Build Artifacts**: The built files are committed directly to your main repository, which means they'll be visible in your repo history.

## Manual Deployment

If you need to manually trigger a deployment:

1. Go to your repository on GitHub
2. Click on the "Actions" tab
3. Select "Build and Deploy Life in Weeks" workflow
4. Click "Run workflow" → "Run workflow"

## Troubleshooting

- **Workflow not running**: Check that the workflow file is in `.github/workflows/` and that changes are being pushed to the `main` branch
- **Submodule issues**: If the submodule fails to initialize, ensure the submodule repository (`tonysusi/life-in-weeks`) is accessible and the URL in `.gitmodules` is correct
- **Build failures**: Check the Actions logs for Hugo build errors. Common issues include missing themes or incorrect Hugo version

## Next Steps

1. Push this workflow to your repository
2. Make a change to the `life-in-weeks` submodule or trigger the workflow manually
3. Wait for the workflow to complete (usually 1-2 minutes)
4. Visit `https://tonysusi.github.io/life-in-weeks/` to see your deployed site

The workflow will continue to run automatically whenever you update the `life-in-weeks` submodule.

