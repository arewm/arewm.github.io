# PR Preview Setup Instructions

This repository includes GitHub Actions workflows to automatically deploy PR previews using Netlify. This allows you to see changes in a live environment before merging to production.

## Features

- **Automatic PR Previews**: Every pull request gets a unique preview URL
- **Auto-cleanup**: Preview deployments are automatically deleted when PRs are closed/merged
- **Works with Custom Domains**: Your production site (arewm.com) is unaffected; previews use temporary Netlify URLs
- **Comment Integration**: Preview URLs are automatically posted as PR comments

## How It Works

1. When a PR is opened or updated, the workflow builds the Jekyll site
2. The built site is deployed to Netlify with a unique URL: `https://pr-<number>--<your-site>.netlify.app`
3. A comment is added to the PR with the preview URL
4. When the PR is closed or merged, Netlify automatically marks the preview as inactive (note: the URL may remain for a period but will show as a past deployment)

## Setup Required

To enable PR previews, you need to:

### 1. Create a Netlify Account (Free)

1. Go to [netlify.com](https://www.netlify.com/) and sign up for a free account
2. You don't need to deploy anything manually - the GitHub Actions will handle it

### 2. Get Netlify Credentials

1. Go to Netlify → User Settings → Applications → Personal Access Tokens
2. Create a new token with "Sites" permission
3. Copy the token (you'll need it for the next step)

4. In Netlify, create a new site (you can use any method, even drag-and-drop a dummy file)
5. Go to Site Settings → General → Site Information
6. Copy the "Site ID" (usually looks like: `abc123def456-1234-5678-90ab-cdef12345678`)

### 3. Add Secrets to GitHub Repository

1. Go to your GitHub repository → Settings → Secrets and variables → Actions
2. Click "New repository secret" and add:
   - Name: `NETLIFY_AUTH_TOKEN`, Value: (the token from step 2)
   - Name: `NETLIFY_SITE_ID`, Value: (the site ID from step 2)

### 4. Test It

1. Create a new pull request with any change
2. Wait for the GitHub Action to complete (usually 1-2 minutes)
3. Check the PR comments for the preview URL
4. Visit the preview URL to see your changes live!

## Custom Domain Compatibility

**Yes, this works perfectly with your custom domain (arewm.com)!**

- **Production site** (arewm.com): Remains on GitHub Pages, unaffected by preview deployments
- **PR previews**: Use temporary Netlify URLs like `https://pr-123--yoursite.netlify.app`
- **No conflicts**: Preview deployments are completely separate from your production site
- **After merge**: Changes appear on your production site (arewm.com) via GitHub Pages, and the preview is deleted

## Cost

- **Netlify Free Tier**: Includes 300 build minutes/month and 100GB bandwidth/month
- **GitHub Actions**: Free for public repositories
- **Total cost**: $0 for typical usage

## Troubleshooting

If previews aren't deploying:
1. Check that both secrets (`NETLIFY_AUTH_TOKEN` and `NETLIFY_SITE_ID`) are correctly set
2. Verify the GitHub Action workflow ran successfully in the "Actions" tab
3. Make sure the Netlify site is active (not paused)

## Alternative Options

If you prefer not to use Netlify, alternatives include:
- **Vercel**: Similar service with GitHub integration
- **Cloudflare Pages**: Another free option with PR previews
- **GitHub Pages branches**: Deploy each PR to a separate branch (more manual, no auto-cleanup)

## Files

- `.github/workflows/pr-preview.yml`: Builds and deploys PR previews
- `.github/workflows/pr-preview-cleanup.yml`: Posts cleanup notification when PRs close

## Note on Cleanup

Netlify preview deployments are automatically marked as inactive when the source branch is deleted (which happens after PR merge). The cleanup workflow simply posts a notification comment. If you want to manually delete old deployments, you can do so from the Netlify dashboard under Deploys → Deploy Previews.
