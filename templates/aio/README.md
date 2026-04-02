# Emdash AIO Template

Welcome to the Emdash AIO (All-in-One) template. This is a complete CMS template built on Astro, featuring a full admin UI, marketing pages, block systems, and an integrated blog.

## Prerequisites

Before getting started, you need to have **Node.js (version 22 or higher)** installed on your computer.

### Installing Node.js

#### For Windows Users:
1. Go to the [official Node.js website](https://nodejs.org/).
2. Download the recommended **LTS (Long Term Support)** installer for Windows.
3. Run the installer and follow the setup instructions. The default settings are recommended.

#### For Mac Users:
1. Go to the [official Node.js website](https://nodejs.org/).
2. Download the recommended **LTS (Long Term Support)** installer for macOS.
3. Run the installer and follow the prompts.
*(Alternatively, you can install Node.js using Homebrew by running: `brew install node` in your terminal)*

To verify that Node.js has been installed correctly, open up your terminal (Command Prompt or PowerShell on Windows, Terminal on Mac) and type:
```bash
node -v
```
It should return the version 22 or higher of the installed Node.js.

---

## Local Setup

Once you have Node.js installed, here are the basic steps to set up and run this project locally:

1. **Install dependencies:**
   Open your terminal, ensure you are in the project directory, and run:
   ```bash
   npm install
   ```
   *(If your project is configured with `pnpm` or `yarn`, use `pnpm install` or `yarn install` instead).*

2. **Run the development server:**
   Start the local development server by running:
   ```bash
   npx emdash dev
   ```
   *(This command starts the local server, runs database migrations, seeds demo content, and automatically generates required TypeScript types).*

3. **Open the site:**
   - The main frontend site will be accessible at: `http://localhost:4321`
   - The Emdash Admin UI is accessible at: `http://localhost:4321/_emdash/admin`

## Production Deployment

To run your site in a live, production environment, use the standard build and start scripts:

1. **Build the site:**
   To prepare the project assets and production builds, run:
   ```bash
   npm run build
   ```

2. **Start the server:**
   Once built, you can start the production server with:
   ```bash
   npm run start
   ```

### Hosting Options

Since this project relies on server-rendered routes for the CMS admin interface and data fetches, you must deploy it to a host capable of running a live Node.js process (rather than just static file hosting). Common deployment options include:

- **Managed Platform-as-a-Service (PaaS)**: Platforms like Render, Railway, Fly.io, or Heroku allow you to deploy seamlessly by linking your Git repository. They will automatically run the `build` and `start` commands for you.
- **Virtual Private Server (VPS)**: Providers like DigitalOcean, Linode, or AWS EC2 allow you to rent a linux server. You can install Node.js, clone your code, build it, run the server process with a manager like `pm2`, and secure it behind a reverse proxy (e.g., Nginx or Caddy).
- **Docker**: This application can easily be containerized with Docker, meaning you can deploy it to any cloud provider that supports container runtimes.

*Make sure to configure any necessary environment variables (such as secret keys or database connection strings) in your hosting provider's dashboard before making your application public.*
