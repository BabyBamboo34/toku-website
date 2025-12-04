# Cloudflare Pages Deployment with Password Protection

This guide will help you deploy your Toku Creative Systems website to Cloudflare Pages with password protection during development.

## Quick Overview

Your site now has **two modes**:

- **Development Mode**: Password-protected (current setup)
- **Production Mode**: Public and open to everyone

## Files Created for Password Protection

1. **`auth.html`** - The login page with logo, username, and password fields
2. **`_redirects`** - Forces all visitors to go to the auth page first
3. **`index.html`** - Modified to check for authentication

## Default Credentials

**Username:** `toku`  
**Password:** `dev2024`

> ⚠️ **IMPORTANT**: Change the password in `auth.html` (line 102) before deploying!

---

## Deployment Steps

### Step 1: Initialize Git Repository

```bash
cd /Users/paulbeckett/.gemini/antigravity/scratch/toku-website
git init
git add .
git commit -m "Initial commit: Toku website with password protection"
```

### Step 2: Create GitHub Repository

1. Go to <https://github.com/new>
2. Name it: `toku-website`
3. Keep it **private** (recommended for now)
4. **Do not** initialize with README or .gitignore
5. Click "Create repository"

### Step 3: Push to GitHub

```bash
git remote add origin https://github.com/YOUR-USERNAME/toku-website.git
git branch -M main
git push -u origin main
```

Replace `YOUR-USERNAME` with your GitHub username.

### Step 4: Deploy to Cloudflare Pages

1. **Log in to Cloudflare**
   - Go to <https://dash.cloudflare.com/>
   - Navigate to "Workers & Pages" in the sidebar

2. **Create New Pages Project**
   - Click "Create application"
   - Select "Pages" tab
   - Click "Connect to Git"

3. **Connect Repository**
   - Authorize Cloudflare to access GitHub
   - Select `toku-website` repository

4. **Configure Build Settings**
   - **Project name**: `tokucreativesystems` (or your preferred subdomain)
   - **Production branch**: `main`
   - **Build command**: Leave empty
   - **Build output directory**: `/`
   - Click "Save and Deploy"

5. **Wait for Deployment**
   - Takes about 1-2 minutes
   - Your site will be live at: `https://tokucreativesystems.pages.dev`

### Step 5: Add Custom Domain (<www.tokucreativesystems.com>)

1. In your Cloudflare Pages project, go to "Custom domains"
2. Click "Set up a custom domain"
3. Enter: `www.tokucreativesystems.com`
4. Also add: `tokucreativesystems.com` (root domain)
5. Follow DNS configuration instructions
6. SSL certificates are automatically provisioned

---

## How the Password Protection Works

### Development Mode (Current Setup)

1. **First visit**: User goes to `www.tokucreativesystems.com`
2. **Automatic redirect**: `_redirects` file forces them to `auth.html`
3. **Login page**: Shows logo, username, and password fields
4. **After login**: User is redirected to `index.html`
5. **Session check**: Each page checks for authentication using `sessionStorage`

### Going Live (Remove Password Protection)

When ready to make the site public:

1. **Delete or rename** `_redirects` file:

   ```bash
   git rm _redirects
   ```

2. **Comment out** the auth check in `index.html` (lines 20-27):

   ```html
   <!-- Development Authentication Check
   <script>
       if (!sessionStorage.getItem('authenticated')) {
           window.location.href = 'auth.html';
       }
   </script>
   -->
   ```

3. **Commit and push**:

   ```bash
   git add .
   git commit -m "Remove password protection - going live"
   git push
   ```

Cloudflare will automatically redeploy your site without password protection!

---

## Customizing the Login Page

### Change Password

Edit `auth.html` around line 102:

```javascript
const VALID_CREDENTIALS = {
    username: 'toku',
    password: 'YOUR-NEW-PASSWORD-HERE'  // Change this
};
```

### Change Logo

The login page uses `logo-wordmark.png`. To use a different logo, replace the file or update line 97 in `auth.html`.

### Styling

All styling is inline in `auth.html` (lines 8-153). You can customize:

- Colors (currently dark theme with blue gradients)
- Fonts
- Layout
- Animations

---

## Security Notes

### ⚠️ Important Limitations

This password protection is **client-side only** and suitable for:

- ✅ Development previews
- ✅ Hiding work-in-progress from search engines
- ✅ Basic privacy during testing

This is **NOT suitable** for:

- ❌ Protecting sensitive data
- ❌ Production authentication
- ❌ Preventing determined users from accessing content

### For Production-Grade Security

Consider these options:

1. **Cloudflare Access** (Enterprise solution)
   - True authentication service
   - Integrates with identity providers
   - Requires paid Cloudflare plan
   - <https://www.cloudflare.com/products/zero-trust/access/>

2. **Cloudflare Workers**
   - Server-side authentication
   - More secure than client-side
   - Requires writing custom Workers code

3. **Third-party Auth**
   - Auth0, Firebase Auth, etc.
   - Proper backend authentication

---

## Updating Your Website

After initial deployment, updates are automatic:

```bash
# Make your changes to any files
git add .
git commit -m "Description of changes"
git push
```

Cloudflare will automatically rebuild and deploy!

---

## Testing the Password Protection

### Test Locally First

1. Open `auth.html` in a browser
2. Try logging in with the credentials
3. Verify you're redirected to `index.html`
4. Close the browser and try again (session should end)

### Test on Cloudflare

1. Visit your Cloudflare Pages URL
2. Verify you're redirected to the login page
3. Test the login flow
4. Try accessing `index.html` directly without logging in

---

## Troubleshooting

### Redirect Loop

- Check that `_redirects` file is exactly as provided
- Make sure `auth.html` exists in the root directory

### Login Not Working

- Check browser console for errors
- Verify credentials in `auth.html`
- Clear browser cache and cookies

### Authentication Not Persisting

- This is expected! `sessionStorage` clears when browser is closed
- For persistent login, use `localStorage` instead

### Custom Domain Not Working

- DNS changes can take up to 24 hours
- Verify DNS settings in Cloudflare dashboard
- Check SSL certificate status

---

## Features

Your deployment includes:

✅ **Free SSL/HTTPS** - Automatic encryption  
✅ **Global CDN** - Fast worldwide  
✅ **Unlimited Bandwidth** - No traffic limits  
✅ **Auto Deployments** - Push to deploy  
✅ **Password Protection** - Simple dev access  
✅ **Custom Domain Support** - Use your own domain  

---

## Quick Commands Reference

```bash
# Deploy for the first time
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/YOUR-USERNAME/toku-website.git
git push -u origin main

# Update the site
git add .
git commit -m "Your changes"
git push

# Remove password protection
git rm _redirects
git add index.html  # After commenting out auth check
git commit -m "Going live - removed password protection"
git push

# Change credentials
# Edit auth.html, then:
git add auth.html
git commit -m "Update login credentials"
git push
```

---

## Next Steps

1. ✅ Change the default password in `auth.html`
2. ✅ Test the deployment locally
3. ✅ Deploy to Cloudflare Pages
4. ✅ Add your custom domain
5. ✅ Test the live site with password protection
6. ✅ When ready, remove password protection to go public

---

**Built with ❤️ for Toku Creative Systems Limited**

For support:

- Cloudflare Pages Docs: <https://developers.cloudflare.com/pages/>
- Cloudflare Community: <https://community.cloudflare.com/>
