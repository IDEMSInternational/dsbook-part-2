# PreTeXt Deployment Guide

This repository contains a PreTeXt book project with multiple deployment options.

## Deployment Methods

### 1. Modern GitHub Pages Deployment (Automatic)

**Workflow:** `.github/workflows/pretext.yml`

This is the **primary deployment method** and runs automatically:
- Triggers: Push to `main` branch or manual workflow dispatch
- Method: Uses GitHub Pages Actions API (no gh-pages branch needed)
- Output: Deploys to GitHub Pages directly from artifacts
- Status: ✅ **Currently Active**

The site is automatically deployed when changes are pushed to the main branch.

### 2. Traditional gh-pages Branch Deployment (Manual)

**Workflow:** `.github/workflows/pretext-deploy.yml`

This is an **alternative deployment method** available for manual use:
- Triggers: Manual workflow dispatch only
- Method: Builds and force-pushes to `gh-pages` branch
- Output: Deploys by pushing HTML to gh-pages branch
- Status: ✅ **Available for manual use**

To use this method:
1. Go to Actions tab in GitHub
2. Select "Build and Deploy" workflow
3. Click "Run workflow"
4. The workflow will:
   - Build the PreTeXt project
   - Create/update the `gh-pages` branch
   - Force push to GitHub

**First Time Setup:**
The first time you run this workflow, it will create the `gh-pages` branch automatically.

## Build Configuration

The project is configured in `project.ptx`:
- **web** target: Configured for deployment (marked with `deploy="yes"`)
- **html** target: Alternative HTML build
- **pdf** target: PDF output

## GitHub Pages Settings

To view the deployed site:
1. Go to repository Settings → Pages
2. The site should be deployed from GitHub Pages Actions

## Local Development

To build and preview locally:

```bash
# Install dependencies
pip install -r requirements.txt

# Initialize system resources
pretext init --system

# Build the web target
pretext build web

# Preview the build
pretext view web
```

To deploy to gh-pages locally (for testing):

```bash
# Build the deploy targets
pretext build --deploys

# Deploy without pushing (creates gh-pages branch locally)
pretext deploy --no-push

# If you want to push
git push origin gh-pages --force
```

## Troubleshooting

### Runestone Services Error

If you encounter an error about Runestone services, create a cache file:

```bash
# Get your PreTeXt version
PTX_VERSION=$(pretext --version | grep -oE '[0-9]+\.[0-9]+\.[0-9]+' | head -1)

# Create the cache directory and file
mkdir -p ~/.ptx/${PTX_VERSION}/rs_cache
cat > ~/.ptx/${PTX_VERSION}/rs_cache/rs_services.xml << 'EOF'
<?xml version="1.0" ?>
<all>
  <js type="list">
    <item type="str">prefix-runtime.f91c1a4dc12163f2.bundle.js</item>
    <item type="str">prefix-723.3e6434f80549315a.bundle.js</item>
    <item type="str">prefix-runestone.fe35e59c546f8d19.bundle.js</item>
  </js>
  <css type="list">
    <item type="str">prefix-723.3bccd435914aa0ff.css</item>
    <item type="str">prefix-runestone.557d81b04b3ec0e4.css</item>
  </css>
  <cdn-url type="str">https://runestone.academy/cdn/runestone/</cdn-url>
  <version type="str">7.10.0</version>
</all>
EOF
```

This workaround is already included in the `.github/workflows/pretext.yml` and `.github/workflows/init-gh-pages.yml` workflows.

## More Information

For more information about PreTeXt deployment, see:
- [PreTeXt Guide](https://pretextbook.org/documentation.html)
- [PreTeXt CLI Documentation](https://github.com/PreTeXtBook/pretext-cli)
