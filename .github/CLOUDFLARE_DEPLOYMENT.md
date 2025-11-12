# Cloudflare Pages Deployment

This repository includes a GitHub Actions workflow to deploy the static site to Cloudflare Pages.

## About Cloudflare Pages

Cloudflare Pages is the recommended deployment method for static sites. It provides:
- Automatic deployments on every push
- Branch preview deployments
- Built-in analytics
- Global CDN
- Free SSL certificates

**Workflow file:** `.github/workflows/deploy-cloudflare-pages.yml`

## Setup Instructions

### Prerequisites

1. A Cloudflare account (sign up at https://dash.cloudflare.com/sign-up)
2. A GitHub repository with this code

### Step 1: Get Your Cloudflare API Token

1. Go to https://dash.cloudflare.com/profile/api-tokens
2. Click "Create Token"
3. Use the "Edit Cloudflare Workers" template or create a custom token with:
   - Permissions: `Account - Cloudflare Pages - Edit`
4. Copy the API token

### Step 2: Get Your Cloudflare Account ID

1. Go to https://dash.cloudflare.com
2. Select any website or go to Workers & Pages
3. Copy your Account ID from the right sidebar

### Step 3: Add Secrets and Variables to GitHub

1. Go to your GitHub repository
2. Navigate to Settings → Secrets and variables → Actions

#### Add Secrets:
3. Click "New repository secret" and add:
   - Name: `CLOUDFLARE_API_TOKEN`
   - Value: Your Cloudflare API token from Step 1
4. Click "New repository secret" again and add:
   - Name: `CLOUDFLARE_ACCOUNT_ID`
   - Value: Your Cloudflare Account ID from Step 2

#### Add Variables:
5. Click on the "Variables" tab
6. Click "New repository variable" and add:
   - Name: `CLOUDFLARE_PROJECT_NAME`
   - Value: Your desired project name (e.g., `my-devtool-site`)

### Step 4: Deploy

Once configured, the workflow will automatically deploy your site when you push to the `main` branch.

You can also manually trigger the workflow:
1. Go to Actions tab in your GitHub repository
2. Select "Deploy to Cloudflare Pages"
3. Click "Run workflow"

## Verifying Your Deployment

After a successful deployment:

1. Go to https://dash.cloudflare.com
2. Navigate to Workers & Pages
3. Find your project in the list
4. Click on it to see deployment details and your site URL

## Troubleshooting

### "API token not found" error

- Ensure you've added the `CLOUDFLARE_API_TOKEN` secret in GitHub repository settings
- Verify the API token has the correct permissions

### "Account ID not found" error

- Ensure you've added the `CLOUDFLARE_ACCOUNT_ID` secret in GitHub repository settings
- Double-check the Account ID from Cloudflare dashboard

### "Project name not found" error

- Ensure you've added the `CLOUDFLARE_PROJECT_NAME` variable in GitHub repository settings
- The variable should be under Settings → Secrets and variables → Actions → Variables tab

### Deployment fails

- Check the Actions logs in GitHub for detailed error messages
- Ensure your API token has not expired
- Verify the project name doesn't conflict with existing Cloudflare Pages projects

## Additional Resources

- [Cloudflare Pages Documentation](https://developers.cloudflare.com/pages/)
- [GitHub Actions - Cloudflare Pages](https://github.com/cloudflare/pages-action)
