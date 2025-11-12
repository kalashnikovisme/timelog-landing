# Cloudflare Deployment

This repository includes GitHub Actions workflows to deploy the static site to Cloudflare.

## Deployment Options

### Option 1: Cloudflare Pages (Recommended)

Cloudflare Pages is the recommended deployment method for static sites. It provides:
- Automatic deployments on every push
- Branch preview deployments
- Built-in analytics
- Global CDN
- Free SSL certificates

**Workflow file:** `.github/workflows/deploy-cloudflare-pages.yml`

### Option 2: Cloudflare Workers

Cloudflare Workers can also serve static sites but requires additional configuration with Wrangler.

**Workflow file:** `.github/workflows/deploy-cloudflare-workers.yml` (manual trigger only)

## Setup Instructions

### Prerequisites

1. A Cloudflare account (sign up at https://dash.cloudflare.com/sign-up)
2. A GitHub repository with this code

### For Cloudflare Pages Deployment

#### Step 1: Get Your Cloudflare API Token

1. Go to https://dash.cloudflare.com/profile/api-tokens
2. Click "Create Token"
3. Use the "Edit Cloudflare Workers" template or create a custom token with:
   - Permissions: `Account - Cloudflare Pages - Edit`
4. Copy the API token

#### Step 2: Get Your Cloudflare Account ID

1. Go to https://dash.cloudflare.com
2. Select any website or go to Workers & Pages
3. Copy your Account ID from the right sidebar

#### Step 3: Add Secrets to GitHub

1. Go to your GitHub repository
2. Navigate to Settings → Secrets and variables → Actions
3. Click "New repository secret" and add:
   - Name: `CLOUDFLARE_API_TOKEN`
   - Value: Your Cloudflare API token from Step 1
4. Click "New repository secret" again and add:
   - Name: `CLOUDFLARE_ACCOUNT_ID`
   - Value: Your Cloudflare Account ID from Step 2

#### Step 4: Configure Project Name (Optional)

The default project name is `devtool-template`. To change it:

1. Open `.github/workflows/deploy-cloudflare-pages.yml`
2. Change the `projectName` value to your desired name
3. Commit and push the changes

#### Step 5: Deploy

Once configured, the workflow will automatically deploy your site when you push to the `main` branch.

You can also manually trigger the workflow:
1. Go to Actions tab in your GitHub repository
2. Select "Deploy to Cloudflare Pages"
3. Click "Run workflow"

### For Cloudflare Workers Deployment

If you prefer to use Cloudflare Workers instead:

#### Step 1: Create wrangler.toml

Create a `wrangler.toml` file in the root of your repository:

```toml
name = "devtool-template"
main = "src/index.js"
compatibility_date = "2024-01-01"

[site]
bucket = "."
```

#### Step 2: Create a Worker Script

Create `src/index.js`:

```javascript
export default {
  async fetch(request, env) {
    return await env.ASSETS.fetch(request);
  },
};
```

#### Step 3: Add Secrets (Same as Pages)

Follow steps 1-3 from the Cloudflare Pages setup above.

#### Step 4: Deploy

The Workers workflow is set to manual trigger only. To deploy:

1. Go to Actions tab in your GitHub repository
2. Select "Deploy to Cloudflare Workers"
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

### Deployment fails

- Check the Actions logs in GitHub for detailed error messages
- Ensure your API token has not expired
- Verify the project name doesn't conflict with existing Cloudflare Pages projects

## Additional Resources

- [Cloudflare Pages Documentation](https://developers.cloudflare.com/pages/)
- [Cloudflare Workers Documentation](https://developers.cloudflare.com/workers/)
- [GitHub Actions - Cloudflare Pages](https://github.com/cloudflare/pages-action)
