# SSL Configuration for GitHub Pages

This website is configured to use HTTPS/SSL through GitHub Pages. GitHub Pages automatically provides SSL certificates for custom domains using Let's Encrypt.

## Setup Steps

### 1. Configure DNS Records

Add the following DNS records at your domain registrar for `arewm.com`:

**A Records** (for apex domain):
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

**CNAME Record** (for www subdomain - optional):
```
www.arewm.com -> arewm.github.io
```

### 2. Enable HTTPS in GitHub Pages Settings

1. Go to repository Settings → Pages
2. Under "Custom domain", enter: `arewm.com`
3. Wait for DNS check to complete (may take a few minutes)
4. Check "Enforce HTTPS" once DNS is verified

### 3. Verification

Once configured:
- GitHub Pages will automatically provision an SSL certificate
- The certificate renews automatically
- All HTTP traffic will redirect to HTTPS
- The site will be accessible at `https://arewm.com`

## Notes

- SSL certificate provisioning may take up to 24 hours after DNS configuration
- The CNAME file in this repository tells GitHub Pages which custom domain to use
- GitHub Pages uses Let's Encrypt for free SSL certificates
- Certificates automatically renew before expiration

## Testing

After DNS propagation (can take up to 48 hours):
1. Visit `http://arewm.com` - should redirect to HTTPS
2. Visit `https://arewm.com` - should load with valid SSL certificate
3. Check certificate in browser - should show "Issued by: Let's Encrypt"

## Current Status

- ✅ CNAME file configured
- ⏳ DNS records (to be configured at domain registrar)
- ⏳ GitHub Pages custom domain setting
- ⏳ HTTPS enforcement (enable after DNS verification)
