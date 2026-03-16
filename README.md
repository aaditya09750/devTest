### NUFLOS ADVERTORIAL - PAGESPEED OPTIMIZATION SPRINT

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![SVG](https://img.shields.io/badge/SVG-Inline_Icons-FFB13B?style=for-the-badge&logo=svg&logoColor=black)
![Google Fonts](https://img.shields.io/badge/Google_Fonts-Self_Hosted-4285F4?style=for-the-badge&logo=googlefonts&logoColor=white)
![Lighthouse](https://img.shields.io/badge/Lighthouse-Optimized-F44B21?style=for-the-badge&logo=lighthouse&logoColor=white)
![Netlify](https://img.shields.io/badge/Netlify-Deployed-00C7B7?style=for-the-badge&logo=netlify&logoColor=white)

A production-grade PageSpeed optimization of the Nuflos Lymphatic Drainage Drops advertorial landing page. The page was audited for technical bottlenecks and rebuilt for maximum Lighthouse scores while maintaining pixel-identical visuals no layout, spacing, typography, or content changes.

**Developer:** Aaditya Gunjal

**Live Preview:** [https://ag-devtest.netlify.app](https://ag-devtest.netlify.app/)

**Original Reference:** [https://get.nuflos.com/pages/5-reason-why-swollen-legs](https://get.nuflos.com/pages/5-reason-why-swollen-legs)

## Lighthouse Score Comparison

### Before Optimization (Original)

| Metric | Mobile | Desktop |
|--------|--------|---------|
| Performance | 55 | 81 |
| Accessibility | ~80 | ~80 |
| Best Practices | ~90 | ~90 |
| SEO | ~90 | ~90 |

### After Optimization (Current)

| Metric | Mobile | Desktop |
|--------|--------|---------|
| Performance | **86** | **98** |
| Accessibility | **96** | **96** |
| Best Practices | **100** | **100** |
| SEO | **100** | **100** |

**PageSpeed Reports:**
- [Mobile Report](https://pagespeed.web.dev/analysis/https-ag-devtest-netlify-app/ic74ohe5aq?form_factor=mobile)
- [Desktop Report](https://pagespeed.web.dev/analysis/https-ag-devtest-netlify-app/ic74ohe5aq?form_factor=desktop)

## Optimization Summary

### Round 1 — HTML/CSS Cleanup (55/81 → 85/96)

**Added `<!DOCTYPE html>` and `lang="en"`** — Missing DOCTYPE triggered quirks mode; missing `lang` failed Lighthouse accessibility audit. +7 Accessibility points.

**Removed anti-caching meta tags** — `Cache-Control: no-cache, no-store`, `Pragma: no-cache`, and `Expires: 0` were forcing every asset to re-download on every visit.

**Fixed broken LCP preload** — Changed `<link rel="preload" as="style" href="...png">` to `as="image"`. The browser was ignoring the preload entirely.

**Removed duplicate Google Fonts stylesheet** — Same 50KB+ font CSS was loaded twice — once blocking, once non-blocking. Eliminated the blocking one.

**Eliminated Material Symbols icon font (~150KB+)** — Replaced 2 icon usages (`thumb_up` x17, `inventory_2` x1) with inline SVGs (~200 bytes total). Saved ~150KB + 1 font request.

**Inlined critical above-the-fold CSS** — Extracted ~2KB of critical CSS for nav, header, and first article into `<head>` `<style>` tag. Full CSS loaded async.

**Removed all inline `onerror`/`onload` handlers** — Removed ~8KB of repeated base64 SVG fallback JavaScript from 16+ images.

**Removed invalid HTML attributes** — Cleaned `href=""`, `onclick=""`, `target="_self"`, `align="center"`, `title=""`, and redundant `data-src` from all `<img>` tags. ~3KB savings.

**Added `decoding="async"` to all images** — Offloads image decoding from the main thread.

**Added `loading="lazy"` to below-fold images** — Prevents below-fold images from competing with LCP image for bandwidth.

**Added `content-visibility: auto`** — Applied to comments section (~1200 DOM nodes) and offer section to skip layout/paint for off-screen content.

**Added preconnect for image CDNs** — Preconnects to `img.funnelish.com` and `assets.checkoutchamp.com` eliminate ~100-300ms DNS+TCP+TLS latency.

**Removed unused CSS selectors** — Dead selectors like `.diagram-label`, `.section_wrapper_24939`, old footer classes, and Material Symbols font-face.

**Minified CSS** — Removed comments, collapsed whitespace, shortened hex colors. ~30% smaller.

**Fixed viewport meta tag** — Removed `user-scalable=no` and `maximum-scale=1.0` that blocked pinch-to-zoom. +5-10 Accessibility points.

### Round 2 — Font Self-Hosting & Full Inlining (85/96 → 86/98)

**Self-hosted Google Fonts** — Downloaded DM Sans and Space Grotesk WOFF2 files (latin subset, ~50KB total) to local `fonts/` directory. Replaced external Google Fonts `<link>` with inline `@font-face` declarations. Added `<link rel="preload">` for primary body font. Eliminated 4+ external network round trips (~500-1000ms on mobile).

**Inlined ALL CSS** — Merged complete `styles.css` into a single `<style>` block in `<head>`. Zero external CSS requests. Zero render-blocking resources. Total inline CSS ~5KB — well within the 14KB initial TCP window.

**Fixed accessibility issues** — Added `aria-label` to textarea, replaced `outline:none!important` with visible `:focus-visible` indicator on CTA button, removed misleading underline from non-interactive footer spans, fixed `cursor:pointer` on non-clickable comment names.

**Added Netlify `_headers` file** — Cache-Control headers: 1-year immutable for font files, 1-hour with revalidation for HTML.

**Removed dead HTML** — Removed ~1KB server-recommendation comment block and empty `<noscript>` element.

## Technology Stack

| Technology | Purpose |
|------------|---------|
| HTML5 | Semantic markup with accessibility attributes |
| CSS3 | Complete styles inlined in `<head>` for zero render-blocking |
| Inline SVGs | Replaced 150KB+ Material Symbols icon font |
| WOFF2 Fonts | Self-hosted DM Sans + Space Grotesk (latin subset) |
| Netlify | Static hosting with automatic Brotli compression + HTTP/2 |
| `_headers` | Netlify cache configuration for immutable font assets |

## Project Structure

```
devTest/
├── Lst-1.html                    # Original unoptimized HTML
├── Lst-1.css                     # Original unoptimized CSS
├── README.md                     # This file
└── optimized/                    # Production-ready optimized build
    ├── index.html                # Fully optimized HTML (all CSS inlined)
    ├── styles.css                # Standalone CSS reference (not used at runtime)
    ├── _headers                  # Netlify cache-control configuration
    ├── CHANGELOG.md              # Detailed changelog of every optimization
    └── fonts/                    # Self-hosted Google Fonts (WOFF2)
        ├── dm-sans-400-latin.woff2        # DM Sans variable font (~37KB)
        └── space-grotesk-700-latin.woff2  # Space Grotesk Bold (~13KB)
```

## Quick Start

### Local Preview

```bash
# Clone the repository
git clone https://github.com/aaditya09750/devTest.git
cd devTest/optimized

# Serve locally (any static server works)
npx serve .
# or
python -m http.server 8000
# or simply open index.html in your browser
```

### Deployment

```bash
# Deploy the optimized/ folder to any static hosting service
# Compatible with: Netlify, Vercel, GitHub Pages, Cloudflare Pages

# For Netlify (already deployed)
# Set publish directory to: optimized/
```

## File Size Comparison

| Asset | Original | Optimized | Savings |
|-------|----------|-----------|---------|
| HTML | ~73KB | ~40KB (CSS inlined) | **45% smaller** |
| CSS (external) | ~18KB | 0KB (inlined into HTML) | **100% eliminated** |
| Material Symbols font | ~150KB | 0KB (replaced with SVGs) | **100% eliminated** |
| Google Fonts requests | 4+ round trips | 0 (self-hosted WOFF2) | **100% eliminated** |
| Self-hosted fonts | — | ~50KB (cached immutably) | New, but cached forever |
| **Render-blocking resources** | **3+** | **0** | **100% eliminated** |

## Key Performance Techniques Used

### Eliminate Render-Blocking Resources
- All CSS inlined in `<head>` `<style>` block (~5KB total)
- Zero external stylesheets
- Zero external font CSS
- All JavaScript is non-blocking

### Optimize Font Loading
- Self-hosted WOFF2 with `font-display: swap` (no FOIT)
- `<link rel="preload">` for primary body font
- Unicode-range subsetting (latin only)
- Variable font for DM Sans (one file covers 400 + 700 weights)

### Optimize Images
- LCP image preloaded with `fetchpriority="high"`
- All below-fold images use `loading="lazy"` + `decoding="async"`
- Explicit `width`/`height` on all images (prevents CLS)
- Preconnect to image CDN origins

### Reduce DOM Processing
- `content-visibility: auto` on comments section (~1200 nodes)
- `content-visibility: auto` on offer section
- `contain-intrinsic-size` set for accurate scroll estimation

### Caching Strategy
- Fonts: `Cache-Control: public, max-age=31536000, immutable`
- HTML: `Cache-Control: public, max-age=3600, must-revalidate`
- Netlify auto-applies Brotli compression and HTTP/2

### Accessibility Fixes
- `<!DOCTYPE html>` + `lang="en"` for standards mode
- Viewport allows pinch-to-zoom (no `user-scalable=no`)
- All form elements have accessible labels
- Visible `:focus-visible` indicator on interactive elements
- Semantic HTML landmarks (`<nav>`, `<main>`, `<header>`, `<footer>`)

## Design Preservation

The optimization is purely technical — zero visual changes:

- Layout, spacing, and content are identical
- Typography (DM Sans, Space Grotesk) renders the same via self-hosted WOFF2
- All images, colors, and hover effects preserved
- CTA button functionality (`route(event)`) unchanged
- Comments section appearance identical
- Footer content and structure unchanged

## Browser Compatibility

![Chrome](https://img.shields.io/badge/Chrome-90+-4285F4?style=flat-square&logo=googlechrome&logoColor=white)
![Firefox](https://img.shields.io/badge/Firefox-88+-FF7139?style=flat-square&logo=firefox&logoColor=white)
![Safari](https://img.shields.io/badge/Safari-14+-000000?style=flat-square&logo=safari&logoColor=white)
![Edge](https://img.shields.io/badge/Edge-90+-0078D7?style=flat-square&logo=microsoftedge&logoColor=white)

**Full Support** — Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
**`content-visibility`** — Chrome 85+, Edge 85+ (graceful degradation in others)
**`:focus-visible`** — All modern browsers
**WOFF2** — All modern browsers (99%+ global support)

## Contact & Support

![Email](https://img.shields.io/badge/Email-aadigunjal0975%40gmail.com-D14836?style=for-the-badge&logo=gmail&logoColor=white)
![LinkedIn](https://img.shields.io/badge/LinkedIn-aadityagunjal0975-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)
![WhatsApp](https://img.shields.io/badge/WhatsApp-Contact-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)

**Get In Touch**

- **Email:** [aadigunjal0975@gmail.com](mailto:aadigunjal0975@gmail.com)
- **Phone:** +91 84335 09521
- **LinkedIn:** [aadityagunjal0975](https://www.linkedin.com/in/aadityagunjal0975/)
- **Location:** Dombivli, Maharashtra, India

## License

![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge&logo=opensourceinitiative&logoColor=white)

```
MIT License

Copyright (c) 2025 Aaditya Gunjal

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

---

**Nuflos Advertorial PageSpeed Optimization** — A demonstration of systematic performance engineering: auditing bottlenecks, eliminating render-blocking resources, self-hosting fonts, inlining critical CSS, and fixing accessibility issues to achieve near-perfect Lighthouse scores while preserving pixel-identical design fidelity.

**Star this repository** if you found it helpful!
