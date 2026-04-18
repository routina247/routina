# Requirements Document

## Introduction

This document defines the requirements for the Routina marketing landing page — a static website hosted on GitHub Pages that promotes the Routina iOS app, drives App Store downloads, and provides support and privacy documentation. The site must support English (en-US) and Thai (th-TH) localization, light and dark color modes, and be fully responsive across desktop, tablet, and mobile viewports.

## Glossary

- **Landing_Page**: The main marketing page at the site root that presents the app's value proposition, features, pricing, and an App Store download link.
- **Support_Page**: A page under the `/docs/` path that provides user support information including FAQs, contact details, and troubleshooting guidance.
- **Privacy_Page**: A page under the `/docs/` path that presents the app's privacy policy, data handling practices, and user rights.
- **Site**: The complete static website comprising the Landing_Page, Support_Page, and Privacy_Page.
- **Color_Mode_Toggle**: A UI control that allows the user to switch between light and dark color themes.
- **Language_Switcher**: A UI control that allows the user to switch between English (en-US) and Thai (th-TH) localized content.
- **App_Store_CTA**: A call-to-action button or badge that links to the Routina app listing on the Apple App Store.
- **Hero_Section**: The prominent top section of the Landing_Page that displays the app name, tagline, and primary call-to-action.
- **Feature_Section**: A section of the Landing_Page that highlights the app's key features with icons and descriptions.
- **Pricing_Section**: A section of the Landing_Page that communicates the one-time purchase price of $3.99 USD.
- **Viewport**: The visible area of the web page on a user's device screen.

## Requirements

### Requirement 1: Static Site Compatibility with GitHub Pages

**User Story:** As a developer, I want the landing page to be a static site compatible with GitHub Pages, so that I can host it without a server or build pipeline.

#### Acceptance Criteria

1. THE Site SHALL consist exclusively of static HTML, CSS, and JavaScript files that require no server-side rendering or build step to serve.
2. THE Site SHALL be deployable to GitHub Pages by pushing files to a repository branch without additional CI/CD configuration.
3. THE Site SHALL load all assets (fonts, images, icons) from local files or public CDNs, with no dependency on private servers.

### Requirement 2: Landing Page Hero Section

**User Story:** As a potential customer, I want to immediately understand what Routina is and how to get it, so that I can decide whether to download the app.

#### Acceptance Criteria

1. THE Hero_Section SHALL display the app name "Routina", a tagline describing the app as a smart routine management app for iOS, and an App_Store_CTA.
2. THE Hero_Section SHALL display the app icon or a device mockup image to visually represent the app.
3. WHEN a user activates the App_Store_CTA, THE Landing_Page SHALL navigate the user to the Routina listing on the Apple App Store in a new browser tab.

### Requirement 3: Landing Page Feature Highlights

**User Story:** As a potential customer, I want to see the key features of Routina, so that I can understand the value the app provides.

#### Acceptance Criteria

1. THE Feature_Section SHALL present at least six key features of the app, each with an icon and a short description.
2. THE Feature_Section SHALL include the following features: flexible scheduling, one-tap completion, streak tracking, CloudKit sync across devices, Apple Watch companion, and widgets.
3. THE Feature_Section SHALL include a mention that the app supports voice search and quick add functionality.

### Requirement 4: Landing Page Pricing Display

**User Story:** As a potential customer, I want to clearly see the pricing model, so that I know exactly what I am paying.

#### Acceptance Criteria

1. THE Pricing_Section SHALL display the one-time purchase price of $3.99 USD.
2. THE Pricing_Section SHALL explicitly state that there are no ads, no subscriptions, and no in-app purchases.
3. THE Pricing_Section SHALL include an App_Store_CTA linking to the Apple App Store listing.

### Requirement 5: Landing Page Platform and Compatibility Information

**User Story:** As a potential customer, I want to know which devices the app supports, so that I can confirm compatibility before purchasing.

#### Acceptance Criteria

1. THE Landing_Page SHALL display the supported platforms: iPhone, iPad, and Apple Watch.
2. THE Landing_Page SHALL state the minimum operating system requirement of iOS 26.0 or later.

### Requirement 6: Landing Page Privacy Summary

**User Story:** As a privacy-conscious user, I want to see a summary of the app's privacy stance on the landing page, so that I feel confident about my data safety.

#### Acceptance Criteria

1. THE Landing_Page SHALL include a privacy summary section stating that Routina collects no third-party analytics, displays no ads, and includes no crash reporting SDKs.
2. THE Landing_Page SHALL include a link from the privacy summary to the full Privacy_Page.

### Requirement 7: Landing Page Medical Disclaimer

**User Story:** As a user, I want to see a medical disclaimer, so that I understand the app's limitations regarding health advice.

#### Acceptance Criteria

1. THE Landing_Page SHALL display the disclaimer: "Routina is not a medical app and provides reminders only."
2. THE Landing_Page SHALL place the medical disclaimer in the footer area, visible on every page.

### Requirement 8: Support Page

**User Story:** As an existing user, I want a support page where I can find help and contact information, so that I can resolve issues with the app.

#### Acceptance Criteria

1. THE Support_Page SHALL be accessible at the path `/docs/support` relative to the site root.
2. THE Support_Page SHALL include a Frequently Asked Questions section with at least five common questions and answers about using Routina.
3. THE Support_Page SHALL provide a contact email address for user support inquiries.
4. THE Support_Page SHALL include navigation back to the Landing_Page.

### Requirement 9: Privacy Page

**User Story:** As a user, I want a detailed privacy policy page, so that I understand how my data is handled.

#### Acceptance Criteria

1. THE Privacy_Page SHALL be accessible at the path `/docs/privacy` relative to the site root.
2. THE Privacy_Page SHALL describe what data the app collects, how data is stored (private CloudKit container), and that no data is shared with third parties.
3. THE Privacy_Page SHALL state that the app collects no third-party analytics, no advertising data, and no crash reporting data.
4. THE Privacy_Page SHALL describe user rights including data export (JSON) and the right to delete all data.
5. THE Privacy_Page SHALL reference compliance with GDPR and CCPA regulations.
6. THE Privacy_Page SHALL include navigation back to the Landing_Page.

### Requirement 10: Localization Support

**User Story:** As a Thai-speaking user, I want to view the site in Thai, so that I can understand the content in my native language.

#### Acceptance Criteria

1. THE Site SHALL provide all user-facing text content in both English (en-US) and Thai (th-TH).
2. THE Language_Switcher SHALL be visible on every page of the Site.
3. WHEN a user selects a language via the Language_Switcher, THE Site SHALL display all text content in the selected language without a full page reload.
4. THE Site SHALL default to English (en-US) when the user has not made a language selection.
5. WHEN a user selects a language, THE Site SHALL persist the language preference in the browser's local storage so that subsequent visits use the same language.

### Requirement 11: Dark and Light Color Mode

**User Story:** As a user, I want to switch between dark and light modes, so that I can view the site comfortably in different lighting conditions.

#### Acceptance Criteria

1. THE Site SHALL support both a light color mode and a dark color mode.
2. THE Color_Mode_Toggle SHALL be visible on every page of the Site.
3. WHEN a user activates the Color_Mode_Toggle, THE Site SHALL switch between light and dark color modes without a full page reload.
4. THE Site SHALL default to the user's operating system color mode preference on first visit.
5. WHEN a user selects a color mode, THE Site SHALL persist the preference in the browser's local storage so that subsequent visits use the same mode.
6. THE Site SHALL use the following color palette for light mode: Primary #6366F1, Secondary #34D399, Accent #F59E0B, Success #22C55E, Error #EF4444, Background #F9FAFB.
7. THE Site SHALL use the following color palette for dark mode: Primary #818CF8, Secondary #6EE7B7, Accent #FCD34D, Success #4ADE80, Error #F87171, Background #0F172A.

### Requirement 12: Responsive Design

**User Story:** As a user, I want the site to look good on any device, so that I can browse it on my phone, tablet, or desktop.

#### Acceptance Criteria

1. THE Site SHALL render correctly on viewports from 320px to 2560px wide.
2. THE Site SHALL use a responsive layout that adapts content arrangement for mobile (320px–767px), tablet (768px–1023px), and desktop (1024px and above) viewports.
3. THE Site SHALL ensure all interactive elements have a minimum touch target size of 44×44 CSS pixels on mobile viewports.

### Requirement 13: Navigation and Page Structure

**User Story:** As a user, I want consistent navigation across all pages, so that I can easily move between the landing page, support page, and privacy page.

#### Acceptance Criteria

1. THE Site SHALL include a navigation header on every page containing links to the Landing_Page, Support_Page, and Privacy_Page.
2. THE Site SHALL include a footer on every page containing the medical disclaimer, links to the Support_Page and Privacy_Page, and a copyright notice.
3. WHEN a user activates a navigation link, THE Site SHALL navigate to the corresponding page.

### Requirement 14: Performance and Accessibility

**User Story:** As a user, I want the site to load fast and be accessible, so that I have a good browsing experience regardless of my abilities or connection speed.

#### Acceptance Criteria

1. THE Site SHALL achieve a Lighthouse performance score of 90 or above on mobile.
2. THE Site SHALL use semantic HTML elements (header, nav, main, section, footer) for page structure.
3. THE Site SHALL provide alt text for all images.
4. THE Site SHALL ensure a minimum color contrast ratio of 4.5:1 for all text against its background in both light and dark modes.
5. THE Site SHALL set the `lang` attribute on the HTML element to match the currently selected language.

### Requirement 15: SEO and Social Sharing

**User Story:** As a marketer, I want the site to be discoverable by search engines and shareable on social media, so that the app reaches more potential customers.

#### Acceptance Criteria

1. THE Site SHALL include appropriate meta tags for title, description, and Open Graph properties on every page.
2. THE Site SHALL include a canonical URL meta tag on every page.
3. THE Site SHALL include structured data (JSON-LD) for the SoftwareApplication schema on the Landing_Page.
