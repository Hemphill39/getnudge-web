# Nudge Website

Static website for Nudge app, including landing page, privacy policy, terms of service, and deep linking configuration.

## 📁 What's Included

```
getnudge-web/
├── index.html              # Landing page
├── privacy.html            # Privacy policy (required for App Store)
├── terms.html              # Terms of service (required for App Store)
├── css/
│   └── style.css          # Shared styles (Nudge brand colors)
├── .well-known/
│   ├── apple-app-site-association    # iOS Universal Links
│   └── assetlinks.json              # Android App Links
└── README.md              # This file
```

## 🚀 Quick Deploy to GitHub Pages

### 1. Create GitHub Repository

```bash
cd getnudge-web
git init
git add .
git commit -m "Initial website commit"

# Create repo (requires GitHub CLI)
gh repo create getnudge-web --public --push --source=.

# Or manually:
# 1. Create repo on github.com
# 2. git remote add origin https://github.com/YOUR-USERNAME/getnudge-web.git
# 3. git push -u origin main
```

### 2. Enable GitHub Pages

1. Go to your repo on GitHub
2. Settings → Pages (left sidebar)
3. Source: Deploy from a branch
4. Branch: `main` / `root`
5. Click Save

Your site will be live at: `https://YOUR-USERNAME.github.io/getnudge-web/`

### 3. Add Custom Domain

#### Buy Domain
- Recommended: [Namecheap](https://www.namecheap.com) or [Cloudflare](https://www.cloudflare.com)
- Buy `getnudge.dev` (or your preferred domain)

#### Configure DNS
In your domain registrar's DNS settings, add these records:

```
Type    Name    Value
A       @       185.199.108.153
A       @       185.199.109.153
A       @       185.199.110.153
A       @       185.199.111.153
CNAME   www     YOUR-USERNAME.github.io
```

#### Add Custom Domain to GitHub Pages
1. In your repo: Settings → Pages
2. Custom domain: `getnudge.dev`
3. Click Save
4. Wait for DNS check to pass (can take 24-48 hours)
5. Enable "Enforce HTTPS" once DNS propagates

## 🔧 Configure Deep Linking

### Before You Deploy

You need to update the `.well-known` files with your actual app credentials.

### iOS Universal Links

Edit `.well-known/apple-app-site-association`:

1. **Find your Team ID:**
   - Go to [Apple Developer Account](https://developer.apple.com/account)
   - Membership Details → Team ID

2. **Find your Bundle ID:**
   - In your `app.json`: `ios.bundleIdentifier`
   - Or create one: `com.nudge.app` (or `com.YOURNAME.nudge`)

3. **Update the file:**
   ```json
   {
     "applinks": {
       "apps": [],
       "details": [
         {
           "appID": "ABC123XYZ.com.nudge.app",
           "paths": ["/invite/*", "/contact/*", "/schedule/*"]
         }
       ]
     },
     "webcredentials": {
       "apps": ["ABC123XYZ.com.nudge.app"]
     }
   }
   ```
   Replace `ABC123XYZ` with your Team ID and `com.nudge.app` with your Bundle ID.

### Android App Links

Edit `.well-known/assetlinks.json`:

1. **Find your package name:**
   - In your `app.json`: `android.package`
   - Or use: `com.nudge.app`

2. **Get your SHA-256 certificate fingerprint:**

   For debug builds:
   ```bash
   cd ../nudge
   keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android
   ```

   For production builds (after running `eas build`):
   ```bash
   # Download your keystore from EAS
   eas credentials
   # Follow prompts to download keystore
   # Then run keytool on the downloaded file
   keytool -list -v -keystore path/to/keystore.jks
   ```

3. **Update the file:**
   ```json
   [
     {
       "relation": ["delegate_permission/common.handle_all_urls"],
       "target": {
         "namespace": "android_app",
         "package_name": "com.nudge.app",
         "sha256_cert_fingerprints": [
           "AA:BB:CC:DD:EE:FF:00:11:22:33:44:55:66:77:88:99:AA:BB:CC:DD:EE:FF:00:11:22:33:44:55:66:77:88:99"
         ]
       }
     }
   ]
   ```

## 📱 Update App Configuration

After deploying the website, update your Nudge app's `app.json`:

```json
{
  "expo": {
    "scheme": "nudge",
    "ios": {
      "bundleIdentifier": "com.nudge.app",
      "associatedDomains": ["applinks:getnudge.dev"]
    },
    "android": {
      "package": "com.nudge.app",
      "intentFilters": [
        {
          "action": "VIEW",
          "autoVerify": true,
          "data": [
            {
              "scheme": "https",
              "host": "getnudge.dev",
              "pathPrefix": "/invite"
            }
          ],
          "category": ["BROWSABLE", "DEFAULT"]
        }
      ]
    }
  }
}
```

## ✅ Testing Deep Links

### iOS
1. Build your app with EAS: `eas build --platform ios`
2. Install on a physical device (simulators don't support Universal Links properly)
3. Send yourself a link via Messages: `https://getnudge.dev/invite/test123`
4. Long press the link → should show "Open in Nudge"
5. Tap the link → should open the app directly

### Android
1. Build your app with EAS: `eas build --platform android`
2. Install on a physical device
3. Send yourself a link via Messages or Email
4. Tap the link → should show "Open with Nudge" dialog
5. Select "Always" to make it default

### Debugging
- Verify `.well-known` files are accessible:
  - iOS: `https://getnudge.dev/.well-known/apple-app-site-association`
  - Android: `https://getnudge.dev/.well-known/assetlinks.json`
- Both should return JSON (not 404)
- Both should be served with `Content-Type: application/json` header

## 🎨 Customization

### Update Content
- Edit `index.html` to change the landing page
- Edit `privacy.html` for privacy policy updates
- Edit `terms.html` for terms of service updates

### Update Email Addresses
Replace placeholder emails throughout the site:
- `hello@getnudge.dev` (general inquiries)
- `privacy@getnudge.dev` (privacy-related)
- `support@getnudge.dev` (support)

Set up email forwarding for your domain or use:
- [ImprovMX](https://improvmx.com) (free email forwarding)
- Google Workspace ($6/month)
- Cloudflare Email Routing (free)

### Update Styles
All styling is in `css/style.css`. Colors match the Nudge app brand:
- Coral: `#FF8A6C`
- Sage: `#87A878`
- Cream: `#FDF8F3`

### Add App Store Links
When your app is live, update `index.html`:

```html
<!-- Replace the "Coming Soon" button with: -->
<p style="margin-top: 32px;">
  <a href="https://apps.apple.com/app/YOUR-APP-ID" class="cta">
    Download on iOS
  </a>
  <a href="https://play.google.com/store/apps/details?id=com.nudge.app" class="cta" style="margin-left: 16px;">
    Download on Android
  </a>
</p>
```

## 📊 Optional: Add Analytics

If you want to track visitors:

### Google Analytics (Free)
1. Create account at [analytics.google.com](https://analytics.google.com)
2. Add tracking code before `</head>` in all HTML files:
   ```html
   <!-- Google Analytics -->
   <script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
   <script>
     window.dataLayer = window.dataLayer || [];
     function gtag(){dataLayer.push(arguments);}
     gtag('js', new Date());
     gtag('config', 'G-XXXXXXXXXX');
   </script>
   ```

### Plausible Analytics (Privacy-friendly)
- Self-hosted or paid ($9/month)
- No cookies, GDPR compliant
- Lighter weight than Google Analytics

## 🔒 Security Headers

Consider adding a `_headers` file (if using Netlify) or configuring security headers:

```
/*
  X-Frame-Options: DENY
  X-Content-Type-Options: nosniff
  X-XSS-Protection: 1; mode=block
  Referrer-Policy: strict-origin-when-cross-origin
```

For GitHub Pages, these are automatically set.

## 📝 TODO Before Launch

- [ ] Update `.well-known/apple-app-site-association` with real Team ID and Bundle ID
- [ ] Update `.well-known/assetlinks.json` with real package name and SHA-256
- [ ] Set up email forwarding for @getnudge.dev addresses
- [ ] Test deep links on real iOS device
- [ ] Test deep links on real Android device
- [ ] Update `index.html` with App Store links when app is live
- [ ] Add favicon.png (1024x1024)
- [ ] Verify HTTPS is enforced
- [ ] Test all pages on mobile browsers

## 🆘 Troubleshooting

### Deep links not working on iOS
- Verify Team ID and Bundle ID are correct
- Make sure domain is using HTTPS
- Test on a real device (not simulator)
- Try clearing Safari cache
- Check Apple's validator: [search.developer.apple.com/appsearch-validation-tool](https://search.developer.apple.com/appsearch-validation-tool)

### Deep links not working on Android
- Verify SHA-256 fingerprint matches your keystore
- Check `autoVerify: true` is set in app.json
- Test the link: `adb shell am start -a android.intent.action.VIEW -d "https://getnudge.dev/invite/test"`
- Verify assetlinks.json is accessible and valid JSON

### DNS not propagating
- Can take 24-48 hours
- Check status: [whatsmydns.net](https://www.whatsmydns.net)
- Try flushing DNS: `sudo dscacheutil -flushcache` (macOS)

## 📚 Resources

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [iOS Universal Links Guide](https://developer.apple.com/ios/universal-links/)
- [Android App Links Guide](https://developer.android.com/training/app-links)
- [Expo Linking Documentation](https://docs.expo.dev/guides/linking/)

## 📞 Support

Questions? Open an issue or contact:
- Email: support@getnudge.dev (once set up)
- Repo: [github.com/YOUR-USERNAME/getnudge-web](https://github.com/YOUR-USERNAME/getnudge-web)
