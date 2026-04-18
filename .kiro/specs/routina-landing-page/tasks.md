# Implementation Plan: Routina Landing Page

## Overview

Build a static marketing website for the Routina iOS app using vanilla HTML, CSS, and JavaScript. The site consists of three pages (landing, support, privacy), supports English/Thai localization via runtime JSON loading, light/dark theming via CSS custom properties, and responsive layouts from 320px to 2560px. All files are served directly from GitHub Pages with no build step.

## Tasks

- [ ] 1. Set up project structure, CSS design system, and shared layout
  - [ ] 1.1 Create directory structure and placeholder files
    - Create `index.html`, `docs/support.html`, `docs/privacy.html`, `css/styles.css`, `js/i18n.js`, `js/theme.js`, `js/main.js`, `locales/en.json`, `locales/th.json`, and `images/` directory
    - Add a `CNAME` placeholder file if custom domain is planned
    - _Requirements: 1.1, 1.2_

  - [ ] 1.2 Implement CSS design system in `css/styles.css`
    - Define CSS custom properties for light and dark color tokens on `:root` / `[data-theme="light"]` and `[data-theme="dark"]` selectors using the exact hex values from the design (Primary, Secondary, Accent, Success, Error, Background, Text, Surface, Border)
    - Add responsive breakpoints: mobile (320px–767px), tablet (768px–1023px), desktop (1024px+)
    - Set `max-width: 1200px; margin: 0 auto;` container utility
    - Set `min-height: 44px; min-width: 44px;` on all interactive elements (`button`, `a`, `input`) for touch targets
    - Add system font stack fallback with `font-display: swap`
    - Implement shared header styles (logo, nav links, controls, mobile hamburger menu)
    - Implement shared footer styles (disclaimer, nav, copyright)
    - Use CSS Grid for feature cards, Flexbox for header/footer layout
    - _Requirements: 11.1, 11.6, 11.7, 12.1, 12.2, 12.3, 14.4_

  - [ ] 1.3 Create shared HTML header and footer structure in `index.html`
    - Add `<!DOCTYPE html>`, `<html lang="en">`, charset, viewport meta
    - Add `<header class="site-header" role="banner">` with `<nav aria-label="Main navigation">` containing logo link, nav links (Home, Support, Privacy), Language_Switcher button, Color_Mode_Toggle button, and mobile menu toggle button
    - Add `<footer class="site-footer" role="contentinfo">` with medical disclaimer (`data-i18n="footer.disclaimer"`), footer nav links, and copyright notice
    - Use semantic HTML elements: `<header>`, `<nav>`, `<main>`, `<footer>`
    - Add ARIA labels on Language_Switcher, Color_Mode_Toggle, and mobile menu toggle
    - Link `css/styles.css` in `<head>` and `js/main.js` as `<script type="module">` before `</body>`
    - _Requirements: 7.1, 7.2, 13.1, 13.2, 13.3, 14.2_

- [ ] 2. Implement landing page content sections
  - [ ] 2.1 Build Hero section in `index.html`
    - Add `<section id="hero">` with `<h1>` for app name, tagline paragraph, App_Store_CTA link (`target="_blank" rel="noopener"`), and app icon/mockup `<img>` with descriptive `alt` text
    - All text elements get `data-i18n` attributes for localization
    - _Requirements: 2.1, 2.2, 2.3, 14.3_

  - [ ] 2.2 Build Features section in `index.html`
    - Add `<section id="features">` with a CSS Grid of at least 6 feature cards
    - Each card has an icon and description for: flexible scheduling, one-tap completion, streak tracking, CloudKit sync, Apple Watch, widgets
    - Include mention of voice search and quick add functionality
    - All text elements get `data-i18n` attributes
    - _Requirements: 3.1, 3.2, 3.3_

  - [ ] 2.3 Build Pricing section in `index.html`
    - Add `<section id="pricing">` displaying $3.99 one-time price, explicit "no ads, no subscriptions, no in-app purchases" statements, and an App_Store_CTA link
    - All text elements get `data-i18n` attributes
    - _Requirements: 4.1, 4.2, 4.3_

  - [ ] 2.4 Build Platform section in `index.html`
    - Add `<section id="platform">` listing iPhone, iPad, Apple Watch support and iOS 26.0+ requirement
    - All text elements get `data-i18n` attributes
    - _Requirements: 5.1, 5.2_

  - [ ] 2.5 Build Privacy Summary section in `index.html`
    - Add `<section id="privacy-summary">` stating no analytics, no ads, no crash SDKs, with a link to `/docs/privacy`
    - All text elements get `data-i18n` attributes
    - _Requirements: 6.1, 6.2_

  - [ ] 2.6 Add SEO meta tags and structured data to `index.html`
    - Add `<title>`, `<meta name="description">`, Open Graph `<meta>` tags (`og:title`, `og:description`, `og:image`, `og:url`, `og:type`)
    - Add `<link rel="canonical">` tag
    - Add `<script type="application/ld+json">` with SoftwareApplication schema (name, operatingSystem, applicationCategory, offers with price/currency, description, author)
    - _Requirements: 15.1, 15.2, 15.3_

- [ ] 3. Implement Support and Privacy pages
  - [ ] 3.1 Build Support page (`docs/support.html`)
    - Create full HTML page with shared header/footer structure (same nav, disclaimer, copyright)
    - Add `<main>` with support title, FAQ section containing at least 5 question/answer pairs about using Routina
    - Add contact email address for support inquiries
    - Add navigation back to Landing_Page in both header and footer
    - Add SEO meta tags (title, description, Open Graph, canonical URL)
    - All text elements get `data-i18n` attributes
    - _Requirements: 8.1, 8.2, 8.3, 8.4, 15.1, 15.2_

  - [ ] 3.2 Build Privacy page (`docs/privacy.html`)
    - Create full HTML page with shared header/footer structure
    - Add `<main>` with privacy policy content: what data is collected, how data is stored (private CloudKit container), no third-party sharing
    - State no third-party analytics, no advertising data, no crash reporting data
    - Describe user rights: data export (JSON) and right to delete all data
    - Reference GDPR and CCPA compliance
    - Add navigation back to Landing_Page in both header and footer
    - Add SEO meta tags (title, description, Open Graph, canonical URL)
    - All text elements get `data-i18n` attributes
    - _Requirements: 9.1, 9.2, 9.3, 9.4, 9.5, 9.6, 15.1, 15.2_

- [ ] 4. Implement localization engine and locale files
  - [ ] 4.1 Create English locale file (`locales/en.json`)
    - Populate all translation keys following the nested key structure from the design: `nav`, `hero`, `features` (all 7 features), `pricing`, `platform`, `privacySummary`, `footer`, `support` (title, contact, email, faqTitle, faq array with ≥5 items), `privacy` (full policy text sections)
    - Ensure every `data-i18n` key used in all three HTML pages has a corresponding entry
    - _Requirements: 10.1, 10.4_

  - [ ] 4.2 Create Thai locale file (`locales/th.json`)
    - Provide Thai translations for every key present in `en.json` with identical key structure
    - All values must be non-empty strings
    - _Requirements: 10.1_

  - [ ] 4.3 Implement localization module (`js/i18n.js`)
    - Export `initI18n()`: read saved language from `localStorage` (key `routina-lang`), fall back to `'en'`, fetch `/locales/{lang}.json`, apply translations to all `data-i18n` elements via `textContent`, handle `data-i18n-html` via `innerHTML`, update `<html lang>` attribute
    - Export `setLanguage(lang)`: validate against `SUPPORTED_LANGS`, fetch locale JSON if not cached, apply translations, update `<html lang>`, persist to `localStorage`
    - Export `getCurrentLanguage()`: return active language code
    - Cache loaded locale data in memory to avoid re-fetching on toggle
    - Handle fetch errors gracefully: log to console, keep current language
    - Handle missing translation keys: leave existing text unchanged, log warning
    - _Requirements: 10.3, 10.4, 10.5, 14.5_

- [ ] 5. Checkpoint
  - Ensure all HTML pages render correctly with English content, navigation works between pages, and CSS theming variables are applied. Ask the user if questions arise.

- [ ] 6. Implement theme manager and main controller
  - [ ] 6.1 Implement theme module (`js/theme.js`)
    - Export `initTheme()`: read saved mode from `localStorage` (key `routina-color-mode`), if none detect OS preference via `window.matchMedia('(prefers-color-scheme: dark)')`, apply by setting `data-theme` attribute on `<html>`
    - Export `toggleTheme()`: switch between `'light'` and `'dark'`, update `data-theme` attribute, persist to `localStorage`
    - Export `getCurrentTheme()`: return `'light'` or `'dark'`
    - Validate stored values against `['light', 'dark']`, fall back to OS preference if invalid
    - _Requirements: 11.1, 11.3, 11.4, 11.5_

  - [ ] 6.2 Implement main controller (`js/main.js`)
    - Import `initI18n`, `setLanguage` from `i18n.js` and `initTheme`, `toggleTheme` from `theme.js`
    - On `DOMContentLoaded`: call `initI18n()` and `initTheme()`
    - Bind click handler on `.lang-switcher` button to toggle language via `setLanguage()`
    - Bind click handler on `.theme-toggle` button to call `toggleTheme()`
    - Implement smooth scroll for anchor links on the landing page
    - Implement mobile hamburger menu toggle (set `aria-expanded`, show/hide nav links)
    - Set up FAQ accordion behavior on support page (if applicable)
    - _Requirements: 10.2, 10.3, 11.2, 11.3, 13.3_

- [ ] 7. Set up test infrastructure and write unit tests
  - [ ] 7.1 Set up Vitest with jsdom environment
    - Create `tests/vitest.config.js` configuring Vitest with jsdom environment
    - Install `vitest`, `jsdom`, and `fast-check` as dev dependencies (add `package.json` with dev dependencies)
    - Create test directory structure: `tests/unit/`, `tests/property/`
    - _Requirements: 1.1_

  - [ ]* 7.2 Write unit tests for landing page content
    - Create `tests/unit/landing-page.test.js`
    - Verify Hero section contains app name "Routina", tagline, and App Store CTA link with `target="_blank"`
    - Verify Features section has at least 6 feature cards with icons and descriptions
    - Verify Pricing section displays "$3.99", "no ads", "no subscriptions", "no in-app purchases"
    - Verify Platform section lists iPhone, iPad, Apple Watch, and iOS 26.0+
    - Verify Privacy Summary section links to `/docs/privacy`
    - Verify JSON-LD structured data is present with SoftwareApplication schema
    - Verify meta tags (title, description, Open Graph, canonical) are present
    - _Requirements: 2.1, 2.2, 2.3, 3.1, 3.2, 3.3, 4.1, 4.2, 5.1, 5.2, 6.1, 6.2, 15.1, 15.2, 15.3_

  - [ ]* 7.3 Write unit tests for support and privacy pages
    - Create `tests/unit/support-page.test.js`: verify FAQ section has ≥5 items, contact email present, navigation links to landing page
    - Create `tests/unit/privacy-page.test.js`: verify privacy policy content sections (data collection, storage, no third-party sharing, user rights, GDPR/CCPA), navigation links
    - Verify semantic HTML structure (header, nav, main, footer) on both pages
    - _Requirements: 8.1, 8.2, 8.3, 8.4, 9.1, 9.2, 9.3, 9.4, 9.5, 9.6, 14.2_

  - [ ]* 7.4 Write unit tests for navigation and shared elements
    - Create `tests/unit/navigation.test.js`
    - Verify header present on all pages with links to Home, Support, Privacy
    - Verify footer present on all pages with medical disclaimer, nav links, copyright
    - Verify Language_Switcher and Color_Mode_Toggle buttons present with ARIA labels
    - _Requirements: 7.1, 7.2, 13.1, 13.2, 14.2_

- [ ] 8. Write property-based tests
  - [ ]* 8.1 Write property test for locale key parity
    - Create `tests/property/locale-parity.property.test.js`
    - **Property 1: Locale key parity**
    - Load both `en.json` and `th.json`, enumerate all keys recursively, assert every key in one file exists in the other with a non-empty string value
    - Minimum 100 iterations
    - **Validates: Requirements 10.1**

  - [ ]* 8.2 Write property test for i18n application completeness
    - Create `tests/property/i18n.property.test.js`
    - **Property 2: i18n application completeness**
    - For randomly selected `data-i18n` elements and randomly selected languages, call `setLanguage(lang)` and verify `textContent` matches the locale JSON value and `document.documentElement.lang` equals the language code
    - Minimum 100 iterations
    - **Validates: Requirements 10.3, 14.5**

  - [ ]* 8.3 Write property test for language preference persistence
    - Add to `tests/property/i18n.property.test.js`
    - **Property 3: Language preference persistence round-trip**
    - For any supported language, call `setLanguage(lang)`, verify `localStorage.getItem('routina-lang')` returns that code, re-initialize i18n and verify same language is active
    - Minimum 100 iterations
    - **Validates: Requirements 10.5**

  - [ ]* 8.4 Write property test for theme toggle round-trip
    - Create `tests/property/theme.property.test.js`
    - **Property 4: Theme toggle round-trip and persistence**
    - For any initial mode, call `toggleTheme()` and verify opposite mode is active and persisted, call again and verify original mode is restored
    - Minimum 100 iterations
    - **Validates: Requirements 11.3, 11.5**

  - [ ]* 8.5 Write property test for touch target sizes
    - Create `tests/property/accessibility.property.test.js`
    - **Property 5: Interactive element touch target size**
    - For all interactive elements (`button`, `a`, `input`) at mobile viewport (≤767px), verify computed width and height are each ≥44 CSS pixels
    - Minimum 100 iterations
    - **Validates: Requirements 12.3**

  - [ ]* 8.6 Write property test for image accessibility
    - Add to `tests/property/accessibility.property.test.js`
    - **Property 6: Image accessibility**
    - For all `<img>` elements across all pages, verify each has a non-empty `alt` attribute
    - Minimum 100 iterations
    - **Validates: Requirements 14.3**

- [ ] 9. Final checkpoint
  - Ensure all tests pass, verify all three pages render correctly in both languages and both color modes, confirm responsive layout at mobile/tablet/desktop breakpoints, and validate navigation between pages. Ask the user if questions arise.

## Notes

- Tasks marked with `*` are optional and can be skipped for faster MVP
- Each task references specific requirements for traceability
- Checkpoints ensure incremental validation
- Property tests validate universal correctness properties from the design document
- Unit tests validate specific examples and edge cases
- The site ships English text as default HTML content, so pages are usable even if JavaScript fails to load
