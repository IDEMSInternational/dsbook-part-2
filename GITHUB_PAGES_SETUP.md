# GitHub Pages Setup for PreTeXt Book

## Important: GitHub Pages Source Configuration

This repository contains **two different books**:
1. **Quarto book** - Located in `/docs` folder (legacy)
2. **PreTeXt book** - Deployed via GitHub Actions (current/active)

### Required Configuration

For the PreTeXt book to display correctly on GitHub Pages, you **MUST** configure GitHub Pages to use "GitHub Actions" as the source.

#### Steps to Configure:

1. Go to your repository on GitHub
2. Navigate to **Settings** → **Pages** (in the left sidebar)
3. Under **Build and deployment** → **Source**, select **"GitHub Actions"**
4. **DO NOT** select "Deploy from a branch" with `/docs` folder

### Why This Matters

- The PreTeXt workflow (`.github/workflows/pretext.yml`) builds the book and deploys it using the GitHub Pages Actions API
- If GitHub Pages is configured to deploy from the `/docs` folder, it will serve the old Quarto book instead
- The screenshot in the issue shows the repository README being rendered, which indicates the wrong source is configured

### Verification

After changing the setting:
1. Wait a few minutes for the pages deployment to complete
2. Visit `https://idemsinternational.github.io/dsbook-part-2/`
3. You should see the PreTeXt book frontmatter, **not** the repository README

### Current Workflow Status

The PreTeXt build and deployment workflow is working correctly:
- ✅ Builds successfully
- ✅ Creates proper artifacts
- ✅ Deploys to GitHub Pages API

The issue is **only** with the GitHub Pages source configuration in Settings.

## Alternative: Using gh-pages Branch

If you prefer to use the traditional `gh-pages` branch method instead:

1. Run the "Build and Deploy" workflow manually (`.github/workflows/pretext-deploy.yml`)
2. Configure GitHub Pages source to deploy from `gh-pages` branch
3. This creates and pushes to a separate `gh-pages` branch

Note: This method is less recommended as it requires manual triggering.

## Troubleshooting

If you still see the wrong content after changing settings:
1. Clear your browser cache
2. Try visiting the site in an incognito/private window
3. Wait 5-10 minutes for GitHub's CDN to update
4. Check the Actions tab to verify the workflow ran successfully
