# Nudge Website

Official website for [Nudge](https://hemphill39.github.io/getnudge-web/) - a mobile app that helps you stay connected with the people who matter through gentle reminders and easy scheduling.

**Live Site:** https://hemphill39.github.io/getnudge-web/

## About

This repository contains the static website for Nudge, including:

- Landing page with app information and branding
- Privacy Policy (App Store compliance)
- Terms of Service (App Store compliance)
- Deep linking configuration for iOS Universal Links and Android App Links

## Structure

```
getnudge-web/
├── index.html              # Landing page
├── privacy.html            # Privacy policy
├── terms.html              # Terms of service
├── css/style.css           # Styles (Nudge brand colors)
└── .well-known/            # Deep linking configuration
    ├── apple-app-site-association
    └── assetlinks.json
```

## Technology

- **Hosting:** GitHub Pages
- **Fonts:** Fraunces (serif), Nunito (sans-serif)
- **Design:** Mobile-responsive, accessible HTML/CSS
- **Deep Linking:** iOS Universal Links + Android App Links

## Deployment

Deployed via GitHub Pages. Site updates automatically on push to `main` branch.

## Deep Linking

The `.well-known` directory contains configuration for mobile app deep linking:

- **iOS Universal Links:** `apple-app-site-association`
- **Android App Links:** `assetlinks.json`

These files enable `https://` URLs to open directly in the Nudge mobile app when installed.

## Design

- **Colors:** Warm palette (coral `#FF8A6C`, sage `#87A878`, cream `#FDF8F3`)
- **Typography:** Fraunces (headings), Nunito (body)
- **Responsive:** Mobile-first, accessible design

## Contact

For questions or feedback about the website:
- Open an issue in this repository
- Email: support@getnudge.dev

## License

© 2026 Nudge. All rights reserved.
