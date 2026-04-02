# All-In-One (AIO) EmDash Template

Welcome to the **AIO EmDash Template**! This template includes everything you need to get started with an EmDash project, featuring pre-configured Blog, Portfolio, Ecommerce, and Marketing sections.

This guide will walk you through the very basics of running the site locally and deploying it to production on Cloudflare.

## Local Setup

To run this project on your own computer, you will need to install Node.js (**version 22 or higher is required**).

### 1. Install Node.js

**For Windows Users:**
1. Go to the [Node.js official website](https://nodejs.org/).
2. Download the **LTS (Long Term Support)** installer for Windows.
3. Open the downloaded `.msi` file and follow the installation wizard (you can keep all the default settings).

**For Mac Users:**
1. Go to the [Node.js official website](https://nodejs.org/).
2. Download the **LTS** installer for macOS.
3. Open the downloaded `.pkg` file and follow the installation wizard.

*(Alternatively, Mac users can use Homebrew: `brew install node`)*

### 2. Start the Project

Once Node.js is installed, follow these steps:

1. Open your terminal (Command Prompt/PowerShell on Windows, Terminal on Mac).
2. Navigate to this project folder.
3. Install the project dependencies by running:
   ```bash
   npm install
   ```
4. Start the EmDash development server:
   ```bash
   npx emdash dev
   ```

That's it! Your site will now be running locally. 
- You can view the site at `http://localhost:4321`
- You can access the CMS admin UI at `http://localhost:4321/_emdash/admin`

---

## Production Deployment (Cloudflare)

This template is configured to deploy seamlessly to **Cloudflare Pages**. 

### Deploying via GitHub (Recommended)

1. **Push your code to GitHub**: Create a repository on GitHub and push this project's code there.
2. **Connect to Cloudflare**: Log in to your [Cloudflare Dashboard](https://dash.cloudflare.com/).
3. **Create Pages Project**: Go to **Workers & Pages** -> **Create application** -> **Pages** -> **Connect to Git**.
4. **Select Repository**: choose your GitHub repository.
5. **Configure Build Settings**:
   - **Framework preset**: Astro
   - **Build command**: `npm run build`
   - **Build output directory**: `dist`
6. **Deploy**: Click "Save and Deploy".

Cloudflare will automatically build and deploy your site every time you push changes to your GitHub repository!

### Deploying via CLI (Wrangler)

If you prefer to deploy directly from your terminal, you can use Cloudflare's **Wrangler** CLI:

1. **Log in to Cloudflare** (if you haven't already):
   ```bash
   npx wrangler login
   ```
2. **Build and Deploy**:
   ```bash
   npm run build
   npx wrangler deploy
   ```

*Note: Make sure your D1 databases and R2 buckets are appropriately configured in your Cloudflare dashboard and match the bindings in your `wrangler.jsonc` file before deploying.*
