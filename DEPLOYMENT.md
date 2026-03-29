# 🚀 BrandFolio Deployment Guide - GitHub Pages

This guide will help you deploy your portfolio to GitHub Pages and make it live on the internet with a free custom domain.

---

## 📋 Table of Contents

1. [Prerequisites](#prerequisites)
2. [Initial Setup](#initial-setup)
3. [Deployment Method 1: Static HTML/CSS/JS](#deployment-method-1-static-htmlcssjs)
4. [Deployment Method 2: React/Vue/Next.js](#deployment-method-2-reactvuenextjs)
5. [Enable GitHub Pages](#enable-github-pages)
6. [Verify Your Live Site](#verify-your-live-site)
7. [Custom Domain Setup](#custom-domain-setup)
8. [Automated Deployment with GitHub Actions](#automated-deployment-with-github-actions)
9. [Troubleshooting](#troubleshooting)
10. [Maintenance & Updates](#maintenance--updates)

---

## ✅ Prerequisites

Before you start, make sure you have:

- ✅ A GitHub account (free)
- ✅ Your portfolio files ready in a Git repository
- ✅ Git installed on your local machine
- ✅ Your repository pushed to GitHub
- ✅ Basic knowledge of Git commands

---

## 🔧 Initial Setup

### Step 1: Create or Verify Your Repository Name

**For GitHub Pages to work automatically:**

1. Go to your repository on GitHub
2. Click **Settings** (gear icon)
3. Check if your repository is named: `username.github.io`
   - Replace `username` with your actual GitHub username

**If your repository has a different name:**
- You can still deploy it, but the URL will be: `username.github.io/repository-name`
- Skip to "Enable GitHub Pages" section

### Step 2: Push Your Code to GitHub

If you haven't already:

```bash
# Navigate to your portfolio folder
cd path/to/your/portfolio

# Initialize git if not already done
git init

# Add all files
git add .

# Create initial commit
git commit -m "Initial portfolio commit"

# Add GitHub as remote
git remote add origin https://github.com/your-username/repository-name.git

# Push to GitHub
git branch -M main
git push -u origin main
```

---

## 📤 Deployment Method 1: Static HTML/CSS/JS

**Use this if your portfolio is plain HTML, CSS, and JavaScript (no build process).**

### Step 1: Organize Your Files

Ensure your project structure looks like this:

```
portfolio/
├── index.html
├── css/
│   └── style.css
├── js/
│   └── script.js
├── img/
│   └── (your images)
├── README.md
└── .gitignore
```

**Important**: Your main file MUST be named `index.html` at the root level.

### Step 2: Ensure index.html is in Root

Your `index.html` should be in the root directory of your repository:

```bash
# Check where your index.html is
ls -la index.html  # Linux/Mac
dir index.html     # Windows PowerShell
```

If it's in a subdirectory, move it to the root:

```bash
mv path/to/index.html ./
```

### Step 3: Update All Asset Paths (if needed)

In your `index.html`, make sure all paths are relative:

```html
<!-- ❌ DON'T use absolute paths -->
<link rel="stylesheet" href="/css/style.css">

<!-- ✅ DO use relative paths -->
<link rel="stylesheet" href="css/style.css">

<!-- ✅ Or for subdirectories -->
<img src="./img/profile.jpg" alt="Profile">
```

### Step 4: Commit and Push

```bash
git add .
git commit -m "Prepare for GitHub Pages deployment"
git push origin main
```

---

## ⚛️ Deployment Method 2: React/Vue/Next.js

**Use this if your portfolio is a React, Vue, or Next.js application.**

### For React (Vite, Create React App, or Webpack)

#### Step 1: Build Your Project

```bash
# For Vite
npm run build

# For Create React App
npm run build

# For Webpack (depends on your setup)
npm run build
```

#### Step 2: Configure for GitHub Pages

**Check your package.json** and add the homepage field if deploying to a subdirectory:

```json
{
  "name": "portfolio",
  "version": "0.1.0",
  "homepage": "https://username.github.io/repository-name",
  "private": true,
  "dependencies": { }
}
```

**If using subdirectory only** - Skip this if using `username.github.io` (no subdirectory).

#### Step 3: Install gh-pages Package

```bash
npm install --save-dev gh-pages
```

#### Step 4: Update package.json Scripts

Add these scripts to your `package.json`:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "predeploy": "npm run build",
    "deploy": "gh-pages -d dist"
  }
}
```

For Create React App, use `build` instead of `dist`:

```json
{
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "predeploy": "npm run build",
    "deploy": "gh-pages -d build"
  }
}
```

#### Step 5: Deploy

```bash
npm run deploy
```

This will automatically:
1. Build your project
2. Create a `gh-pages` branch
3. Push the build files to GitHub Pages

### For Next.js

#### Step 1: Configure next.config.js

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  basePath: process.env.NEXT_PUBLIC_BASE_PATH || '',
  exportTrailingSlash: true,
  images: {
    unoptimized: true,
  },
};

module.exports = nextConfig;
```

#### Step 2: Build and Export

```bash
npm run build
npm run export
```

#### Step 3: Deploy with gh-pages

```bash
npm install --save-dev gh-pages
```

Add to `package.json`:

```json
{
  "scripts": {
    "build": "next build && next export",
    "deploy": "gh-pages -d out"
  }
}
```

Deploy:

```bash
npm run deploy
```

### For Vue.js (Vite or Vue CLI)

#### Step 1: Configure vite.config.js (or vue.config.js)

**For Vite:**

```javascript
export default {
  base: '/repository-name/',  // Only if not using username.github.io
}
```

**For Vue CLI:**

```javascript
module.exports = {
  publicPath: process.env.NODE_ENV === 'production' ? '/repository-name/' : '/'
}
```

#### Step 2: Build

```bash
npm run build
```

#### Step 3: Deploy

```bash
npm install --save-dev gh-pages
```

Add to `package.json`:

```json
{
  "scripts": {
    "deploy": "gh-pages -d dist"
  }
}
```

Deploy:

```bash
npm run deploy
```

---

## 🔌 Enable GitHub Pages

### Step 1: Go to Repository Settings

1. Navigate to your GitHub repository
2. Click the **Settings** tab
3. Scroll to **Pages** section (on the left sidebar)

### Step 2: Configure GitHub Pages

In the **Pages** section:

1. **Source**: Select `Deploy from a branch`
2. **Branch**: Select `gh-pages` (if using gh-pages) or `main` (if static files in root)
3. **Folder**: Select `/ (root)` or `/docs` if applicable
4. Click **Save**

### Step 3: Wait for Deployment

GitHub will show you a message like:

> "Your site is live at `https://username.github.io`"

or

> "Your site is live at `https://username.github.io/repository-name`"

This usually takes 1-2 minutes.

---

## ✨ Verify Your Live Site

### Check Deployment Status

1. Go to your repository
2. Look for the **Deployments** section on the right sidebar
3. You should see "github-pages" with a checkmark ✅

### Visit Your Site

1. Click on the deployment or go to your GitHub Pages URL:
   - `https://username.github.io` (if repository is named `username.github.io`)
   - `https://username.github.io/repository-name` (if different repository name)

2. Test all pages and functionality:
   - Navigation links work
   - Images load correctly
   - Styles are applied
   - Forms function (if applicable)

### Troubleshooting Deployment

**Site shows 404?**
- Wait a few minutes, GitHub Pages can take time to build
- Clear browser cache (Ctrl+Shift+Delete)
- Check that index.html is in the deployment branch

**Styles/Images not loading?**
- Check file paths are relative (not absolute)
- Verify file names match exactly (case-sensitive on Linux/Mac)

---

## 🌐 Custom Domain Setup (Optional)

If you own a domain (e.g., `yourname.com`), you can use it for your portfolio.

### Step 1: Have Your Domain Ready

- You need to own a domain
- Know how to access your domain's DNS settings
- Can be purchased from GoDaddy, Namecheap, Google Domains, etc.

### Step 2: Update GitHub Pages Settings

1. Go to **Settings** → **Pages**
2. Under "Custom domain", enter your domain: `yourname.com`
3. Click **Save**

GitHub will show instructions for updating your DNS.

### Step 3: Update DNS Records

Log into your domain registrar and add these DNS records:

**For www subdomain** (recommended):

```
Type: CNAME
Name: www
Value: username.github.io
```

**For apex domain** (yourname.com):

Add these A records:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

Or add an ALIAS/ANAME record:

```
Name: @ (or leave blank)
Value: username.github.io
```

### Step 4: Wait for DNS Propagation

DNS changes take 15 minutes to 24 hours to propagate worldwide.

### Step 5: Verify Custom Domain

1. Go to your domain (e.g., https://yourname.com)
2. You should see your portfolio
3. GitHub will automatically enable HTTPS (free SSL certificate)

---

## 🤖 Automated Deployment with GitHub Actions

Deploy automatically every time you push code to main branch.

### Step 1: Create Workflow File

Create a new file: `.github/workflows/deploy.yml`

```yaml
name: Deploy Portfolio

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist  # or ./build for React CRA, or ./out for Next.js
          cname: yourname.com  # Only if using custom domain
```

### Step 2: For React/Vue with Build Process

Update the workflow:

```yaml
name: Deploy Portfolio

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm install
      
      - name: Build
        run: npm run build
      
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

### Step 3: Commit and Push

```bash
git add .github/workflows/deploy.yml
git commit -m "Add GitHub Actions deployment"
git push origin main
```

### Step 4: Monitor Deployment

1. Go to your repository
2. Click **Actions** tab
3. You'll see your workflow running
4. Wait for it to complete (green checkmark)
5. Your site is now live!

---

## 🔄 Maintenance & Updates

### Update Your Portfolio

1. Make changes to your code locally
2. Commit changes:

```bash
git add .
git commit -m "Update portfolio with new projects"
git push origin main
```

3. If using GitHub Actions, deployment happens automatically
4. If manual, run `npm run deploy` again

### Check Deployment History

1. Go to **Settings** → **Pages**
2. Scroll to "Deployments" section
3. See all past deployments and their status

### Monitor Site Performance

1. Set up Google Analytics (optional)
2. Check GitHub Pages for any deployment errors
3. Test your site regularly on different browsers and devices

---

## 🐛 Troubleshooting

### Issue: "File not found" or 404 error

**Solution:**
- Ensure `index.html` is at the root of deployment directory
- Check file paths in HTML are relative
- Verify branch is correctly selected in Pages settings
- Clear browser cache

### Issue: Styles/Images not loading

**Solution:**
```html
<!-- ❌ Wrong -->
<link href="/css/style.css">

<!-- ✅ Correct -->
<link href="css/style.css">
```

### Issue: Site not updating after push

**Solution:**
- Wait 2-5 minutes for GitHub Pages to rebuild
- Check Actions tab for deployment errors
- Hard refresh browser (Ctrl+F5)
- Clear browser cache completely

### Issue: Custom domain not working

**Solution:**
- Verify DNS records are correctly set
- Wait 24 hours for DNS propagation
- Check custom domain is entered correctly in Pages settings
- Ensure CNAME file exists in deployment branch

### Issue: HTTPS not working

**Solution:**
- In Pages settings, ensure "Enforce HTTPS" is enabled
- If using custom domain, wait 24 hours
- Check DNS is properly configured

### Issue: React Router or SPA not working

**Solution:**

Create a `404.html` file in your project root:

```html
<!DOCTYPE html>
<html>
  <head>
    <script>
      sessionStorage.redirect = location.href;
    </script>
    <meta http-equiv="refresh" content="0;URL='/'" />
  </head>
  <body>
    Redirecting...
  </body>
</html>
```

Include in your build output and ensure it's deployed.

---

## 📝 Quick Deployment Checklist

- [ ] Code pushed to GitHub main branch
- [ ] index.html exists at root (or build is configured)
- [ ] All file paths are relative, not absolute
- [ ] GitHub Pages enabled in Settings
- [ ] Deployment branch selected (main or gh-pages)
- [ ] Site accessible at GitHub Pages URL
- [ ] All pages and links work
- [ ] Images and styles load correctly
- [ ] Forms function properly (if applicable)
- [ ] Mobile responsive design works
- [ ] Custom domain configured (if using)
- [ ] HTTPS enabled
- [ ] DNS records verified (if custom domain)
- [ ] Google Analytics set up (optional)

---

## 🎉 Congratulations!

Your portfolio is now live on the internet! 🚀

### Next Steps:

1. **Test thoroughly**: Visit your site on different devices and browsers
2. **Share your portfolio**: Tell people about your live site
3. **Add SEO**: Ensure search engines can find you
4. **Monitor analytics**: Track who visits your portfolio
5. **Keep updating**: Add new projects and achievements regularly

### Share Your Live Portfolio:

Add your portfolio URL to:
- LinkedIn profile
- Job applications
- Resume/CV
- Email signature
- Social media profiles

---

## 📚 Additional Resources

- [GitHub Pages Official Documentation](https://docs.github.com/en/pages)
- [Configuring a custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
- [Deploying with gh-pages npm package](https://www.npmjs.com/package/gh-pages)
- [GitHub Actions Workflows](https://docs.github.com/en/actions)

---

**Questions? Check the Troubleshooting section or consult GitHub's official documentation.**

Good luck! Your portfolio is about to go live! 🌟
