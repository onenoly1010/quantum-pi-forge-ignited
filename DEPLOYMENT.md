# GitHub Pages Deployment Guide

## Overview

This project has been migrated from Netlify to GitHub Pages for free static site hosting.

## Configuration Steps

To enable GitHub Pages for this repository:

1. Go to **Settings** → **Pages**
2. Under **Build and deployment**:
   - **Source**: GitHub Actions
3. Click **Save**

## Automated Deployment

The repository includes a GitHub Actions workflow (`.github/workflows/deploy-github-pages.yml`) that automatically deploys the site when changes are pushed to the `main` branch.

### Workflow Features:
- ✅ Automatic deployment on push to `main`
- ✅ Manual deployment trigger via `workflow_dispatch`
- ✅ Proper permissions configured for GitHub Pages
- ✅ Concurrency control to prevent deployment conflicts

## Live URLs

- **Production URL**: https://onenoly1010.github.io/quantum-pi-forge-ignited/
- **Custom Domain**: (Can be configured in Settings → Pages → Custom domain)

## Files Deployed

The following files are deployed to GitHub Pages by the automated workflow:
- `index.html` - Main application
- `.nojekyll` - Prevents Jekyll processing
- `README.md` - Rendered as the main content on the deployed site

## Cleanup from Netlify

The following changes were made to remove Netlify dependencies:

✅ Removed `quantum-pi-forge-vb_fLJ3HjZuXHR/.github/workflows/netlify-guardian.yml`
✅ Updated documentation to remove Netlify references
✅ Added GitHub Pages deployment workflow

## Manual Deployment

To manually trigger a deployment:

1. Go to **Actions** tab
2. Select **Deploy to GitHub Pages** workflow
3. Click **Run workflow**
4. Select the `main` branch
5. Click **Run workflow**

## Troubleshooting

### Site not loading?
- Check that GitHub Pages is enabled in Settings → Pages
- Verify the workflow ran successfully in the Actions tab
- Ensure the `main` branch has the latest changes

### 404 errors?
- Clear browser cache
- Check that the `index.html` file exists in the repository root
- Verify the GitHub Pages source is set to GitHub Actions

### Workflow failing?
- Check the Actions tab for error messages
- Verify that Pages permissions are enabled in Settings → Actions → General
- Ensure the repository is public (or has GitHub Pages enabled for private repos)

## Cost

GitHub Pages is **completely free** for public repositories with these limits:
- 1 GB repository size
- 100 GB bandwidth per month
- 10 builds per hour

## Additional Resources

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Repository README](README.md)

---

**Last Updated**: 2026-01-06  
**Status**: ✅ Active
