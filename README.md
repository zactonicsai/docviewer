# docviewer
# SecureDoc Portal — Comprehensive Line-by-Line Code Review

> **Document Type:** Technical Code Review & Explanation  
> **File Under Review:** `index.html` (718 lines)  
> **Application:** SecureDoc Portal — A single-page document management interface with map-based geospatial search  
> **Stack:** HTML5, Tailwind CSS (CDN), Vanilla JavaScript (ES6+), OpenLayers (map library), Google Fonts  
> **Target Audience:** Code reviewers, developers onboarding to the project, and technical auditors

---

## Table of Contents

1. [Architecture Overview](#1-architecture-overview)
2. [Document Head — Lines 1–165](#2-document-head--lines-1165)
   - 2.1 [DOCTYPE & Language Declaration](#21-doctype--language-declaration-lines-12)
   - 2.2 [Meta Tags](#22-meta-tags-lines-35)
   - 2.3 [Page Title](#23-page-title-line-6)
   - 2.4 [Tailwind CSS CDN & Configuration](#24-tailwind-css-cdn--configuration-lines-823)
   - 2.5 [Google Fonts](#25-google-fonts-lines-2528)
   - 2.6 [OpenLayers CDN](#26-openlayers-cdn-lines-3032)
   - 2.7 [Custom CSS Styles](#27-custom-css-styles-lines-34164)
3. [Body & Header — Lines 167–208](#3-body--header--lines-167208)
   - 3.1 [Body Element](#31-body-element-line-167)
   - 3.2 [Sticky Glass-Morphism Header](#32-sticky-glass-morphism-header-lines-169208)
   - 3.3 [Brand Identity Block](#33-brand-identity-block-lines-173178)
   - 3.4 [Search Bar with Integrated Actions](#34-search-bar-with-integrated-actions-lines-180191)
   - 3.5 [Theme Toggle & Navigation](#35-theme-toggle--navigation-lines-193207)
4. [Main Content Area — Lines 210–315](#4-main-content-area--lines-210315)
   - 4.1 [Page Title & Details Button](#41-page-title--details-button-lines-212221)
   - 4.2 [Grid Layout System](#42-grid-layout-system-lines-223314)
   - 4.3 [Left Pane — Document List](#43-left-pane--document-list-lines-225282)
   - 4.4 [Filter Bar](#44-filter-bar-lines-242258)
   - 4.5 [Data Table](#45-data-table-lines-260275)
   - 4.6 [Right Pane — Document Details (Desktop)](#46-right-pane--document-details-desktop-lines-284313)
5. [Popup Modals — Lines 317–371](#5-popup-modals--lines-317371)
   - 5.1 [Map Search Popup](#51-map-search-popup-lines-318344)
   - 5.2 [Advanced Search Popup](#52-advanced-search-popup-lines-346371)
6. [JavaScript Application Logic — Lines 373–716](#6-javascript-application-logic--lines-373716)
   - 6.1 [Section 1: Data Layer](#61-section-1-data-layer-lines-375382)
   - 6.2 [Section 2: SVG Icon Factory](#62-section-2-svg-icon-factory-lines-384385)
   - 6.3 [Section 3: Header Renderer](#63-section-3-header-renderer-lines-387388)
   - 6.4 [Section 4: Dark/Light Theme System](#64-section-4-darklight-theme-system-lines-390393)
   - 6.5 [Section 5: Column Sorting](#65-section-5-column-sorting-lines-395399)
   - 6.6 [Section 6: Filtering Engine](#66-section-6-filtering-engine-lines-401407)
   - 6.7 [Section 7: Responsive Helper](#67-section-7-responsive-helper-lines-409411)
   - 6.8 [Section 8: Inline Mobile Detail Panel](#68-section-8-inline-mobile-detail-panel-lines-413533)
   - 6.9 [Section 9: Table Renderer](#69-section-9-table-renderer-lines-535583)
   - 6.10 [Section 10: Right Pane Toggle (Desktop)](#610-section-10-right-pane-toggle-desktop-lines-585589)
   - 6.11 [Sections 11–12: OpenLayers Map Integration](#611-sections-1112-openlayers-map-integration-lines-591600)
   - 6.12 [Sections 13–14: Detail Rendering & Responsive Routing](#612-sections-1314-detail-rendering--responsive-routing-lines-602642)
   - 6.13 [Section 15: Window Resize Handler](#613-section-15-window-resize-handler-lines-644654)
   - 6.14 [Section 16: Map Search Logic](#614-section-16-map-search-logic-lines-656678)
   - 6.15 [Section 17: Advanced Search Logic](#615-section-17-advanced-search-logic-lines-680696)
   - 6.16 [Sections 18–20: Mobile Menu, Keyboard Shortcuts, Initialization](#616-sections-1820-mobile-menu-keyboard-shortcuts-initialization-lines-698716)
7. [Design Decisions & Rationale](#7-design-decisions--rationale)
8. [External References & Official Documentation](#8-external-references--official-documentation)

---

## 1. Architecture Overview

This application is a **single-file, single-page application (SPA)** that functions as a document management portal called "SecureDoc Portal." The entire application — markup, styles, and behavior — lives inside a single `index.html` file. This design choice prioritizes zero-build-step deployability: the file can be opened directly in a browser or dropped onto any static host with no server, bundler, or compilation required.

**Core Features:**

- A sortable, filterable data table of documents
- A split-pane layout with document details and an interactive map on desktop
- An inline accordion-style detail panel on mobile
- A map-based geospatial search using OpenLayers with radius filtering (haversine formula)
- An advanced multi-field search modal
- A dark/light theme toggle persisted to `localStorage`
- Keyboard shortcuts (Escape to close popups, Ctrl+K to focus search)
- Responsive design that adapts behavior (not just layout) between mobile and desktop

**Technology Choices:**

| Technology | Role | Why Chosen |
|---|---|---|
| Tailwind CSS (CDN) | Utility-first styling | Rapid prototyping without build step; consistent design tokens |
| OpenLayers | Interactive map rendering | Open-source, no API key, OSM tile support, rich vector drawing |
| Google Fonts | Typography | Free, high-quality variable fonts with CDN delivery |
| Vanilla JS | Application logic | No framework overhead; full control; small scope doesn't justify React/Vue |

---

## 2. Document Head — Lines 1–165

The `<head>` section establishes the document type, loads all external dependencies, configures Tailwind, and defines custom CSS that goes beyond what utility classes alone can express.

### 2.1 DOCTYPE & Language Declaration (Lines 1–2)

```html
<!doctype html>
<html lang="en">
```

**Line 1 — `<!doctype html>`:** This is the HTML5 document type declaration. It is not a tag but an instruction to the browser telling it to render the page in "standards mode" rather than "quirks mode." Quirks mode emulates legacy browser behaviors from the early 2000s and can cause unpredictable rendering of CSS box models, table layouts, and inline elements. By including this single line, the browser uses the modern W3C specification for layout and rendering.

The lowercase `<!doctype html>` is valid and equivalent to `<!DOCTYPE html>`. HTML5 is case-insensitive for this declaration, and the lowercase form is a common convention in modern tooling.

**Line 2 — `<html lang="en">`:** The root element of the document. The `lang="en"` attribute serves three purposes:

1. **Accessibility:** Screen readers such as NVDA, JAWS, and VoiceOver use this attribute to select the correct pronunciation engine. Without it, a screen reader might attempt to pronounce English words with French or German phonetics.
2. **SEO:** Search engines use the language tag to serve the page to users searching in the appropriate language.
3. **CSS `:lang()` pseudo-class:** The language attribute enables CSS selectors like `:lang(en)` for language-specific styling, such as adjusting quotation mark styles or hyphenation rules.

This element is also the target for the dark mode class toggle. When dark mode is activated, JavaScript adds `class="dark"` to the `<html>` element, which cascades through Tailwind's `dark:` variant prefix system.

### 2.2 Meta Tags (Lines 3–5)

```html
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

**Line 3 — `<head>`:** Opens the document's metadata container. Everything inside `<head>` is invisible to the user but critical for the browser's rendering engine, search engine crawlers, and assistive technologies.

**Line 4 — `<meta charset="UTF-8" />`:** Declares the character encoding as UTF-8 (Unicode Transformation Format, 8-bit). UTF-8 can represent every character in the Unicode standard (over 149,000 characters across 159 scripts) using variable-width encoding of 1–4 bytes per character. This is essential because the application contains:

- Standard ASCII text
- Special characters like `✓`, `⚠`, `↑`, `↓`, `•`
- Potential user data that could include international names or addresses

Without this declaration, browsers would fall back to system-dependent encodings (often ISO-8859-1 on Western systems), which would corrupt non-ASCII characters. The `charset` meta tag must appear within the first 1024 bytes of the document for the browser to detect it in time.

**Line 5 — `<meta name="viewport" ...>`:** This is the responsive design enablement tag. It configures the browser's virtual viewport on mobile devices. Without it, mobile browsers would render the page at a virtual width of approximately 980px and then scale it down to fit the physical screen, making text unreadably small.

The attributes:

- `width=device-width` — Sets the viewport width equal to the device's physical screen width in CSS pixels (e.g., 390px on an iPhone 14 rather than the default 980px).
- `initial-scale=1.0` — Sets the initial zoom level to 100%, preventing automatic zoom-in or zoom-out when the page loads.

This tag is a prerequisite for all Tailwind CSS responsive breakpoints (`sm:`, `md:`, `lg:`, `xl:`, `2xl:`) to function correctly. Without it, media queries would trigger against the virtual 980px width rather than the actual device width.

### 2.3 Page Title (Line 6)

```html
  <title>SecureDoc Portal</title>
```

The `<title>` element defines the text displayed in the browser tab, the bookmark name when saved, and the default heading in search engine results. Screen readers announce this title when a user navigates between tabs or windows, making it functionally important for accessibility. The title "SecureDoc Portal" succinctly identifies the application's purpose.

### 2.4 Tailwind CSS CDN & Configuration (Lines 8–23)

```html
  <!-- Tailwind (CDN) -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          fontFamily: {
            display: ['"DM Sans"', 'sans-serif'],
            body: ['"DM Sans"', 'sans-serif'],
            mono: ['"JetBrains Mono"', 'monospace']
          }
        }
      }
    }
  </script>
```

**Line 9 — Tailwind CDN Script:** Loads the Tailwind CSS Play CDN. Unlike the production build of Tailwind (which uses PostCSS to scan source files and generate only the classes you use), the CDN version is a JavaScript runtime that:

1. Scans the DOM for Tailwind class names
2. Dynamically generates the corresponding CSS rules
3. Injects them into a `<style>` element in the document head
4. Observes DOM mutations and generates new classes as elements are added

This is explicitly designed for development, prototyping, and single-file deployments like this one. The trade-off is a slightly larger payload (~350KB of JavaScript) versus a typical production Tailwind build (~10–30KB of CSS). For a prototype or internal tool, this trade-off is acceptable.

**Lines 10–23 — Custom Tailwind Configuration:** The `tailwind.config` object is the CDN equivalent of a `tailwind.config.js` file in a build-step project. Each property is explained below:

- **`darkMode: 'class'`** (Line 12): Tells Tailwind to activate dark-mode variants (`dark:bg-slate-900`, `dark:text-white`, etc.) when the `dark` CSS class is present on a parent element (in this application, the `<html>` element). The alternative is `'media'`, which would use the operating system's `prefers-color-scheme` media query. The `'class'` strategy was chosen because it gives the user explicit control through the toggle button, with the OS preference used only as the default (see Section 6.4).

- **`theme.extend.fontFamily`** (Lines 14–19): Extends (not replaces) Tailwind's default font-family tokens. Three custom families are defined:
  - `display` and `body` both map to **DM Sans** — a geometric sans-serif typeface from Google Fonts. Using the same font for both display and body creates visual consistency while the variable-weight axis (300–700) provides hierarchy through weight alone.
  - `mono` maps to **JetBrains Mono** — a monospaced font designed specifically for code readability. It is used for document IDs and coordinate displays where each character should occupy equal width for alignment.

  The double-quoted names (`'"DM Sans"'`) are necessary because CSS font names with spaces must be quoted, and within a JavaScript string, the outer quotes belong to JS while the inner quotes are preserved in the CSS output.

### 2.5 Google Fonts (Lines 25–28)

```html
  <!-- Google Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;0,9..40,700;1,9..40,400&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
```

**Lines 26–27 — `<link rel="preconnect">`:** These establish early connections to the Google Fonts servers *before* the browser has even parsed the font stylesheet. A network connection involves three steps: DNS lookup, TCP handshake, and TLS negotiation. `preconnect` performs all three proactively, saving 100–300ms when the font files are eventually requested.

Two separate preconnects are used because Google Fonts uses two domains:
- `fonts.googleapis.com` — Serves the CSS stylesheet that contains `@font-face` declarations
- `fonts.gstatic.com` — Serves the actual binary font files (`.woff2`)

The `crossorigin` attribute on the second link is required because font files are fetched using CORS (Cross-Origin Resource Sharing) anonymous mode per the browser's font-loading specification. Without `crossorigin`, the preconnection would be made without CORS headers, and the browser would discard it and open a new connection when the font file is actually requested, negating the preconnect benefit entirely.

**Line 28 — Font Stylesheet:** This is a Google Fonts CSS2 API URL that requests two font families with specific configurations:

- **DM Sans:** Requested as a variable font with:
  - `ital` axis: 0 (upright) and 1 (italic)
  - `opsz` axis: 9..40 (optical size range — the font adapts its drawing slightly for small vs. large rendering)
  - `wght` axis: 300 (light), 400 (regular), 500 (medium), 600 (semibold), 700 (bold)
  - Italic variant only at weight 400

- **JetBrains Mono:** Requested at weights 400 (regular) and 500 (medium)

- `display=swap`: This is the font-display strategy. It tells the browser to immediately show text using a fallback system font (like Arial or Helvetica) and then swap in the custom font once it has loaded. This prevents the "Flash of Invisible Text" (FOIT) where text disappears until the font downloads. The trade-off is a brief "Flash of Unstyled Text" (FOUT), which is generally preferred for content-heavy applications because it keeps the page usable during loading.

### 2.6 OpenLayers CDN (Lines 30–32)

```html
  <!-- OpenLayers (CDN) -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/ol@latest/ol.css">
  <script src="https://cdn.jsdelivr.net/npm/ol@latest/dist/ol.js"></script>
```

**Line 31 — OpenLayers CSS:** Loads the OpenLayers stylesheet, which is required for the map's viewport, controls (zoom buttons, attribution), and overlay elements to render correctly. Without this CSS, the map container would have no defined layout and controls would be unstyled or invisible.

**Line 32 — OpenLayers JavaScript:** Loads the full OpenLayers library. OpenLayers is an open-source JavaScript library for displaying interactive maps. It was chosen over alternatives for specific reasons:

| Library | API Key Required | License | Why Not Chosen / Why Chosen |
|---|---|---|---|
| **OpenLayers** | No | BSD 2-Clause | **Chosen** — Free, no API key, rich vector drawing for radius circles |
| Google Maps JS API | Yes (billable) | Proprietary | Requires billing account; cost scales with usage |
| Mapbox GL JS | Yes (free tier) | Proprietary (v2+) | License changed from open-source; token required |
| Leaflet | No | BSD 2-Clause | Viable alternative but OpenLayers has stronger built-in vector support for the radius drawing feature |

The `@latest` version tag on the CDN URL means the application always loads the newest release. For production, this should be pinned to a specific version (e.g., `@9.2.4`) to prevent breaking changes from unexpected updates.

### 2.7 Custom CSS Styles (Lines 34–164)

This `<style>` block contains approximately 130 lines of custom CSS that handle visual effects, animations, and component-specific styling that cannot be easily expressed with Tailwind utility classes alone. Each section is explained below.

#### 2.7.1 — Global Font Reset (Line 35)

```css
* { font-family: 'DM Sans', sans-serif; }
```

The universal selector `*` applies DM Sans to every element on the page. This is a belt-and-suspenders approach layered on top of Tailwind's `font-body` class. It ensures that dynamically generated elements (like OpenLayers controls or browser-native elements like `<select>` options) inherit the correct font even if they are outside Tailwind's class scanning scope.

#### 2.7.2 — Smooth Theme Transition (Lines 37–40)

```css
html, body, header, main, aside, section, div, table, thead, tbody, tr, th, td,
button, input, select, nav, p, span, h1, h2, h3, label {
  transition: background-color 0.3s ease, color 0.3s ease,
              border-color 0.3s ease, box-shadow 0.3s ease;
}
```

This is a large selector list that applies CSS transitions to all structural and interactive elements. When dark mode is toggled, instead of all colors snapping instantaneously (which looks jarring), they smoothly interpolate over 300 milliseconds using an `ease` timing function (slow start, fast middle, slow end).

The four properties targeted — `background-color`, `color`, `border-color`, and `box-shadow` — are the primary visual properties that change between light and dark themes. Other properties like `width` or `height` are intentionally excluded to avoid unintended animation side effects.

**Design Rationale:** The selector list is explicit rather than using `* { transition: all 0.3s; }` because animating *all* properties on *every* element would be computationally expensive (the browser would have to check and potentially interpolate every animatable CSS property on every DOM node) and could produce unwanted effects like animated padding, margin, or transform changes.

#### 2.7.3 — Map Container Heights (Lines 42–45)

```css
#map { height: 260px; }
#mapSearchMap { height: 340px; }
.inline-map-container { height: 220px; width: 100%; }
```

OpenLayers requires its target container to have an explicit height. Unlike most HTML elements, a map tile canvas cannot compute its own intrinsic height — without a set value, it collapses to 0px and becomes invisible. Three separate map containers are used:

- `#map` — The detail panel map on desktop (compact at 260px)
- `#mapSearchMap` — The map search popup map (taller at 340px for better interaction)
- `.inline-map-container` — Mobile inline detail map (smallest at 220px to conserve vertical space)

#### 2.7.4 — Custom Scrollbar Styling (Lines 47–51)

```css
::-webkit-scrollbar { width: 6px; height: 6px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: #94a3b8; border-radius: 99px; }
.dark ::-webkit-scrollbar-thumb { background: #475569; }
```

These pseudo-elements customize scrollbars in WebKit/Blink browsers (Chrome, Edge, Safari, Opera). The default browser scrollbar is replaced with a slim 6px-wide pill-shaped track. The colors use Tailwind's Slate palette (400 for light mode, 600 for dark mode) to blend with the UI.

**Browser Support Note:** The `::-webkit-scrollbar` pseudo-elements are non-standard. Firefox uses `scrollbar-width` and `scrollbar-color` properties instead. This application targets only WebKit/Blink scrollbar customization. In Firefox and other non-WebKit browsers, the default system scrollbar will display, which is an acceptable graceful degradation.

#### 2.7.5 — Sort Icon Animation (Line 54)

```css
.sort-icon { transition: transform 0.2s ease, opacity 0.2s ease; }
```

Applied to the small arrow icons in sortable table column headers. When a user clicks a column to sort, the icon rotates 180° (ascending vs. descending) and its opacity changes from 30% (inactive) to 100% (active). The transition makes this rotation smooth rather than instantaneous.

#### 2.7.6 — Popup Backdrop (Lines 56–62)

```css
.popup-backdrop {
  background: rgba(15, 23, 42, 0.5);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
}
.dark .popup-backdrop { background: rgba(0, 0, 0, 0.6); }
```

The backdrop is the semi-transparent overlay behind modal popups (map search and advanced search). It uses two layered effects:

1. **`rgba(15, 23, 42, 0.5)`** — A 50%-opaque dark slate background. The RGB values `(15, 23, 42)` correspond to Tailwind's `slate-900`, ensuring the overlay's tint matches the application's color palette.
2. **`backdrop-filter: blur(8px)`** — Applies a Gaussian blur to everything *behind* the backdrop element. This creates a "frosted glass" effect where the underlying page content is visible but defocused, providing depth cues to the user.

The `-webkit-backdrop-filter` prefix is included for Safari compatibility, which requires the vendor prefix for this property.

In dark mode, the background opacity increases to 60% because dark backgrounds show through more obviously at lower opacities.

#### 2.7.7 — Popup Entrance Animation (Lines 64–69)

```css
.popup-enter { animation: popupSlideIn 0.3s cubic-bezier(0.16, 1, 0.3, 1); }
@keyframes popupSlideIn {
  from { opacity: 0; transform: translateY(16px) scale(0.97); }
  to   { opacity: 1; transform: translateY(0) scale(1); }
}
```

When a popup opens, it doesn't just appear — it fades in while sliding upward from 16px below its final position and scaling from 97% to 100% size. The `cubic-bezier(0.16, 1, 0.3, 1)` timing function is an "ease-out-expo" curve: the animation starts fast and decelerates very smoothly. This mimics the physical motion of an object that has been tossed upward and is coming to rest, creating a natural, satisfying feel.

The combination of simultaneous opacity, translate, and scale animations creates what UI designers call a "material reveal" — a pattern popularized by Material Design and iOS that communicates to the user where the element came from and draws their attention.

#### 2.7.8 — Table Row Highlight (Lines 71–73)

```css
.row-active { background: linear-gradient(90deg, #eff6ff 0%, #f8fafc 100%) !important; }
.dark .row-active { background: linear-gradient(90deg, #1e293b 0%, #0f172a 100%) !important; }
```

Instead of a flat background color, the active table row uses a horizontal gradient. The gradient goes from a very light blue (`#eff6ff`, Tailwind `blue-50`) on the left to near-white (`#f8fafc`, Tailwind `slate-50`) on the right. This subtle directional gradient creates a sense of depth and makes the active row feel like it's on a slightly elevated surface catching light from the left side.

The `!important` flag is necessary to override Tailwind's `hover:bg-*` utilities, which have the same specificity level.

#### 2.7.9 — Header Glass Effect (Lines 75–81)

```css
.header-glass {
  background: rgba(255,255,255,0.85);
  backdrop-filter: blur(16px) saturate(180%);
  -webkit-backdrop-filter: blur(16px) saturate(180%);
}
.dark .header-glass { background: rgba(15,23,42,0.85); }
```

This creates an Apple-inspired "vibrancy" effect on the sticky header. As the user scrolls, the page content slides behind the header, which is:

- 85% opaque white (not fully opaque, so content peeks through)
- Blurred at 16px (stronger than the popup backdrop for a more solid feel)
- Saturated at 180% (colors behind the glass appear more vivid, preventing the washed-out look that plain blur creates)

This technique, commonly called "glassmorphism," was popularized by macOS Big Sur and iOS 14. The saturation boost is the key differentiator from a simple blurred overlay.

#### 2.7.10 — Search Focus Glow (Lines 83–85)

```css
.search-glow:focus-within { box-shadow: 0 0 0 3px rgba(59,130,246,0.2); }
.dark .search-glow:focus-within { box-shadow: 0 0 0 3px rgba(96,165,250,0.15); }
```

The `:focus-within` pseudo-class activates when any child element inside `.search-glow` receives focus — in this case, the search `<input>`. A soft blue ring (3px spread, 20% opacity) appears around the entire search bar container (not just the input). This is better UX than a standard input focus ring because the search bar is a composite element (input + buttons + divider) and the glow encompasses the whole group.

The `rgba(59,130,246,...)` color is Tailwind's `blue-500`, maintaining palette consistency.

#### 2.7.11 — Column Header Hover (Lines 87–90)

```css
th.sortable { cursor: pointer; user-select: none; }
th.sortable:hover { background: rgba(59,130,246,0.06); }
.dark th.sortable:hover { background: rgba(96,165,250,0.08); }
```

Sortable column headers get a pointer cursor (signaling clickability) and `user-select: none` (preventing accidental text selection during rapid clicking). On hover, a nearly-invisible blue tint (6% opacity) appears, providing just enough feedback to communicate interactivity without being distracting. In dark mode, the tint is slightly stronger (8%) to compensate for the darker background absorbing more of the color.

#### 2.7.12 — Filter Input Styling (Lines 92–99)

```css
.filter-input {
  background: white; border: 1px solid #e2e8f0; border-radius: 12px;
  padding: 6px 12px; font-size: 13px; outline: none; transition: all 0.2s ease;
}
.filter-input:focus { border-color: #3b82f6; box-shadow: 0 0 0 3px rgba(59,130,246,0.1); }
.dark .filter-input { background: #1e293b; border-color: #334155; color: #e2e8f0; }
.dark .filter-input:focus { border-color: #60a5fa; box-shadow: 0 0 0 3px rgba(96,165,250,0.1); }
```

A custom input style class used across the filter bar, map search popup, and advanced search popup. The design uses "pill-shaped" inputs (12px border-radius) rather than the default rectangular browser inputs. On focus, the border turns blue and a subtle focus ring appears. This is a reusable component pattern — defined once in CSS rather than repeated across dozens of Tailwind utility class strings on each `<input>` element.

**Why not pure Tailwind classes?** While Tailwind can express all of this (`rounded-xl border border-slate-200 focus:border-blue-500 focus:ring-2 focus:ring-blue-500/10 ...`), the class string would be extremely long and repeated on approximately 15 input elements. A CSS class is more maintainable and DRY (Don't Repeat Yourself) for this cross-cutting concern.

#### 2.7.13 — Button Styles (Lines 101–115)

```css
.btn-primary {
  background: linear-gradient(135deg, #3b82f6 0%, #2563eb 100%); color: white;
  border: none; border-radius: 12px; padding: 10px 20px; font-weight: 600;
  font-size: 14px; cursor: pointer; transition: all 0.2s ease;
  box-shadow: 0 2px 8px rgba(59,130,246,0.25);
}
.btn-primary:hover { transform: translateY(-1px); box-shadow: 0 4px 16px rgba(59,130,246,0.35); }
```

The primary button uses a 135° diagonal gradient from `blue-500` to `blue-600`, creating visual richness. On hover, two microinteractions occur simultaneously:

1. `translateY(-1px)` — The button lifts 1px upward, simulating a physical button being pushed upward before being pressed
2. The shadow deepens and spreads (from 8px/25% to 16px/35%), as if the button's elevation from the page surface has increased

This "floating button" pattern creates a tactile, material-like feel that communicates interactivity. The secondary button (`.btn-secondary`) uses a flat, subdued style with no gradient — establishing a clear visual hierarchy where primary actions are prominent and secondary actions are understated.

#### 2.7.14 — Badge Component (Lines 117–121)

```css
.badge {
  display: inline-flex; align-items: center; gap: 4px; padding: 2px 10px;
  border-radius: 99px; font-size: 11px; font-weight: 600; letter-spacing: 0.02em;
}
```

A utility class for document type labels (e.g., "Incident Report", "Audit Summary"). The `99px` border-radius ensures the shape is always a perfect pill regardless of content width. The small `letter-spacing: 0.02em` adds subtle character spacing that improves readability at the small 11px font size.

#### 2.7.15 — Map Crosshair Cursor (Line 124)

```css
.map-pick-mode .ol-viewport { cursor: crosshair !important; }
```

When the map search popup is open, the map container has the `.map-pick-mode` class. This overrides OpenLayers' default grab cursor with a crosshair, signaling to users that they should click to place a pin. The `.ol-viewport` is an internal OpenLayers class that wraps the map canvas.

#### 2.7.16 — Mobile Inline Detail Panel (Lines 126–163)

```css
.inline-detail-outer {
  overflow: hidden; max-height: 0;
  transition: max-height 0.45s cubic-bezier(0.16, 1, 0.3, 1);
}
.inline-detail-outer.open { max-height: 900px; }
```

This is the CSS-driven accordion animation for mobile. The technique:

1. Start with `max-height: 0` and `overflow: hidden` — the content is present in the DOM but invisible
2. When `.open` is added, `max-height` transitions to `900px` over 0.45 seconds
3. The `cubic-bezier(0.16, 1, 0.3, 1)` timing function provides a smooth "ease-out" deceleration

**Why `max-height` instead of `height`?** CSS cannot animate from `height: 0` to `height: auto` because `auto` is a keyword, not a numeric value — the browser cannot compute intermediate values between `0` and `auto`. The `max-height` technique works around this by transitioning to a value larger than the content will ever need (`900px`). The content naturally stops at its intrinsic height, and the remaining transition time (from intrinsic height to 900px) runs invisibly.

The inline detail panel also features a blue top border, a left-side accent border, and a gradient background that creates a clear visual connection between the parent row and its expanded details.

The **pulse ring animation** (`@keyframes pulseRing`, lines 152–163) creates a brief expanding ring of blue color when a mobile row is activated, drawing the user's attention to the row that has just opened its details. This uses a box-shadow animation that expands from 0px to 6px and fades to transparent over 1 second.

---

## 3. Body & Header — Lines 167–208

### 3.1 Body Element (Line 167)

```html
<body class="bg-slate-50 text-slate-900 dark:bg-slate-950 dark:text-slate-100 font-body">
```

The body sets the base visual context for the entire page:

- `bg-slate-50` — Light gray background in light mode (not pure white, which can cause eye strain)
- `text-slate-900` — Near-black text for high contrast
- `dark:bg-slate-950` — Very dark blue-gray in dark mode (not pure black, which looks harsh on LCD screens)
- `dark:text-slate-100` — Off-white text in dark mode
- `font-body` — Applies the DM Sans font family defined in the Tailwind config

The Slate color palette was chosen over neutral gray because it has a cool blue undertone that feels more modern and professional than a purely neutral gray.

### 3.2 Sticky Glass-Morphism Header (Lines 169–208)

```html
<header class="sticky top-0 z-50 header-glass border-b border-slate-200 dark:border-slate-800">
```

The header uses Tailwind's `sticky top-0` utilities to implement CSS `position: sticky`. The element behaves as `position: relative` within its normal document flow until the user scrolls past it, at which point it becomes `position: fixed` at the top of the viewport. This provides persistent access to the search bar and navigation without the layout-shifting problems of a permanently fixed header.

- `z-50` — Sets `z-index: 50`, ensuring the header stacks above all main content (which defaults to `z-index: auto` / `0`)
- `header-glass` — Applies the frosted-glass effect from the custom CSS (Section 2.7.9)
- `border-b border-slate-200` — A subtle bottom border that visually separates the header from the content below

### 3.3 Brand Identity Block (Lines 173–178)

```html
<div class="flex items-center gap-3 min-w-0">
  <div id="brandIcon" class="h-9 w-9 flex items-center justify-center rounded-xl
       bg-gradient-to-br from-blue-600 to-indigo-700 text-white text-sm font-bold shadow-md">
  </div>
  <div class="min-w-0 hidden sm:block">
    <div id="brandTitle" class="font-semibold truncate dark:text-white"></div>
    <div id="brandSubtitle" class="text-[11px] text-slate-500 dark:text-slate-400 truncate"></div>
  </div>
</div>
```

The brand block contains a square icon (36×36px with rounded corners) and the application title. The icon has a blue-to-indigo gradient background (`bg-gradient-to-br from-blue-600 to-indigo-700`), creating a distinctive brand mark. The text content ("SD") is populated by JavaScript from the `headerConfig` object.

Key Tailwind patterns:

- `min-w-0` — This is critical when using Flexbox with `truncate`. Without it, flex children have an implicit `min-width: auto` that prevents them from shrinking below their content width, causing text to overflow rather than truncate. Setting `min-w-0` allows the flex item to shrink to zero, enabling `truncate` (which applies `text-overflow: ellipsis`) to function.
- `hidden sm:block` — The brand text is hidden on very small screens (below 640px) to save horizontal space, showing only the icon.
- `text-[11px]` — Tailwind's arbitrary value syntax, used when no predefined size class matches the desired value exactly.

### 3.4 Search Bar with Integrated Actions (Lines 180–191)

```html
<div class="flex-1 max-w-xl mx-4">
  <div class="search-glow flex items-center gap-1 bg-white dark:bg-slate-900
       border border-slate-200 dark:border-slate-700 rounded-2xl px-3 h-10 transition-all">
    <svg ...><!-- magnifying glass icon --></svg>
    <input id="headerSearchInput" type="text" placeholder="Search documents…"
           class="flex-1 bg-transparent border-none outline-none text-sm px-2 py-1 ..." />
    <div class="w-px h-5 bg-slate-200 dark:bg-slate-700 mx-1"></div>
    <button id="openMapSearchBtn" ...><!-- map pin icon --></button>
    <button id="openAdvSearchBtn" ...><!-- filter lines icon --></button>
  </div>
</div>
```

The search bar is a composite element containing:

1. A search icon (inline SVG magnifying glass)
2. A text input that fills the remaining space (`flex-1`)
3. A 1px-wide vertical divider (`w-px h-5`)
4. Two action buttons: map search and advanced search

The `flex-1 max-w-xl mx-4` on the outer div allows the search bar to grow and fill available space but caps at 576px (`max-w-xl`), preventing it from becoming unreasonably wide on large screens.

The input has `bg-transparent border-none outline-none` to visually merge it into the container div rather than looking like a separate element. The parent div's border and background serve as the input's visual boundary.

The inline SVG icons are used rather than an icon library (like Font Awesome or Lucide) to eliminate an external dependency and keep the application fully self-contained. Each SVG is minimal (2–3 path elements) and uses `currentColor` for the `stroke` attribute, allowing the icon color to be controlled by Tailwind text color classes on the parent.

### 3.5 Theme Toggle & Navigation (Lines 193–207)

```html
<button id="themeToggle" ...>
  <svg id="sunIcon" class="h-5 w-5 text-amber-500 hidden" ...><!-- sun --></svg>
  <svg id="moonIcon" class="h-5 w-5 text-indigo-600" ...><!-- moon --></svg>
</button>
<nav id="desktopMenu" class="hidden md:flex items-center gap-1"></nav>
<button id="mobileMenuBtn" class="md:hidden ..." aria-label="Open menu">
  <svg ...><!-- hamburger icon --></svg>
</button>
```

The theme toggle contains both a sun icon (shown in dark mode) and a moon icon (shown in light mode). JavaScript toggles the `hidden` class on each to show the appropriate icon.

The navigation uses a responsive pattern:

- `desktopMenu` with `hidden md:flex` — Hidden by default, becomes a flex container at `md` (768px+)
- `mobileMenuBtn` with `md:hidden` — Visible by default, hidden at `md` (768px+)

The `aria-label="Open menu"` attribute is critical for accessibility. The hamburger button has no visible text label, so without this attribute, screen readers would announce it as an unlabeled button.

The mobile menu panel (`id="mobileMenuPanel"`, line 205) uses `md:hidden hidden` — it's always hidden above 768px, and the second `hidden` class keeps it initially collapsed on mobile until the hamburger button toggles it.

---

## 4. Main Content Area — Lines 210–315

### 4.1 Page Title & Details Button (Lines 212–221)

```html
<main class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 py-6">
  <div class="flex items-center justify-between gap-3 mb-5">
    <div>
      <h1 class="text-xl font-semibold dark:text-white">Documents</h1>
      <p class="text-sm text-slate-500 dark:text-slate-400">
        Click a row to view details, location, and map.
      </p>
    </div>
    <button id="openDetailsBtn" class="hidden lg:inline-flex ..." title="Open details panel">
      ...Details
    </button>
  </div>
```

The `<main>` element uses Tailwind's responsive container pattern:

- `mx-auto` — Centers the content horizontally
- `max-w-7xl` — Caps width at 1280px (prevents content from stretching too wide on 4K monitors)
- `px-4 sm:px-6 lg:px-8` — Progressive padding: 16px on mobile → 24px on tablet → 32px on desktop

This creates a comfortable reading width with proportional gutters at every screen size.

The "Details" button (`openDetailsBtn`) is `hidden lg:inline-flex` — it only appears on large screens (1024px+) where the split-pane detail panel is available. On mobile, there is no separate detail panel to open (details appear inline), so the button is unnecessary.

### 4.2 Grid Layout System (Lines 223–314)

```html
<div id="layout" class="grid grid-cols-1 lg:grid-cols-12 gap-4">
  <section id="leftPane" class="lg:col-span-12">
    ...
  </section>
  <aside id="rightPane" class="hidden lg:col-span-5">
    ...
  </aside>
</div>
```

The layout uses CSS Grid with a 12-column system (a common convention from Bootstrap and similar frameworks):

- **Default state:** The left pane spans all 12 columns (`lg:col-span-12`), taking the full width. The right pane is `hidden`.
- **When details open (via JS):** JavaScript removes `lg:col-span-12`, adds `lg:col-span-7` to the left pane, removes `hidden` from the right pane, and adds `lg:col-span-5`. This creates a 7:5 split (approximately 58%:42%).

This approach is superior to a percentage-based CSS layout because the 12-column grid provides a clean mathematical division and aligns with common design systems. The `gap-4` (16px) utility creates consistent spacing between panes.

On mobile (`grid-cols-1`), the grid collapses to a single column, stacking elements vertically. The right pane is hidden on mobile; instead, the inline accordion panel (Section 6.8) is used.

### 4.3 Left Pane — Document List (Lines 225–282)

The left pane is a card component with:

1. A header bar with an icon, title ("Document List"), and document count
2. A filter bar
3. A data table
4. A footer bar with result counts and sort info

The card uses `rounded-2xl border ... bg-white dark:bg-slate-900 shadow-sm overflow-hidden` — rounded corners with a border, white background, and a subtle shadow that lifts it off the page background. The `overflow-hidden` ensures child elements with backgrounds (like the filter bar) respect the parent's rounded corners rather than poking out.

### 4.4 Filter Bar (Lines 242–258)

```html
<div id="filterBar" class="px-4 py-3 border-b border-slate-100 dark:border-slate-800
     bg-slate-50/60 dark:bg-slate-800/40">
```

The filter bar is inset within the card with a slightly different background (`bg-slate-50/60` — 60% opacity over slate-50) to visually distinguish it from the table content. It contains five filter inputs:

- **Name** — Text input, searches document names and IDs
- **Owner** — Text input, filters by owner name
- **Type** — `<select>` dropdown, populated dynamically from the data
- **Date From / Date To** — Native date pickers for range filtering
- **Clear** — Resets all filters

Each input uses the `.filter-input` CSS class (Section 2.7.12) and has event listeners for real-time filtering (Section 6.6).

### 4.5 Data Table (Lines 260–275)

```html
<table class="min-w-full text-sm" id="docTable">
  <thead class="bg-slate-50 dark:bg-slate-800/60 text-slate-600 dark:text-slate-300">
    <tr class="text-left">
      <th class="px-4 py-3 w-28 ...">Actions</th>
      <th class="px-4 py-3 w-36 sortable ..." data-sort="date">
        <span class="flex items-center gap-1.5">
          Date
          <span class="sort-icon opacity-30"><!-- arrow SVG --></span>
        </span>
      </th>
      <!-- ... more columns ... -->
    </tr>
  </thead>
  <tbody id="docTableBody" class="divide-y divide-slate-100 dark:divide-slate-800"></tbody>
</table>
```

The table uses semantic HTML (`<table>`, `<thead>`, `<tbody>`, `<th>`, `<tr>`, `<td>`) rather than divs styled as a grid. This is the correct choice for tabular data because:

1. Screen readers can announce row and column headers to visually impaired users
2. The browser's native table layout algorithm handles column width distribution
3. The semantic structure is preserved if CSS fails to load

Sortable columns have `data-sort` attributes that JavaScript reads to determine which field to sort. The sort icon is an inline SVG arrow that rotates 180° when the sort direction changes.

The `min-w-full` class ensures the table is at least as wide as its container, preventing it from being narrower than the card. The `overflow-x-auto` on the parent div adds horizontal scrolling on small screens where the table would otherwise overflow.

`divide-y divide-slate-100` on the tbody adds 1px horizontal borders between rows without requiring a bottom border on every row (which would double up at the boundary between rows).

### 4.6 Right Pane — Document Details (Desktop) (Lines 284–313)

The right pane (inside an `<aside>` element for semantic correctness, since it's supplementary content) mirrors the left pane's card structure with:

1. A header with an icon and title ("Document Details")
2. A metadata card showing title, owner, type, and date
3. An OpenLayers map with coordinates
4. A notes section

The map container (`#map`) is wrapped in a bordered, rounded container to blend with the card UI. The notes section uses `leading-relaxed` (line-height: 1.625) for comfortable reading of longer text.

---

## 5. Popup Modals — Lines 317–371

### 5.1 Map Search Popup (Lines 318–344)

```html
<div id="mapSearchPopup" class="fixed inset-0 z-[100] hidden">
  <div class="popup-backdrop absolute inset-0" id="mapSearchBackdrop"></div>
  <div class="absolute inset-0 flex items-center justify-center p-4 pointer-events-none">
    <div class="popup-enter pointer-events-auto w-full max-w-2xl bg-white ...">
```

The popup uses a three-layer structure:

1. **Container** (`fixed inset-0 z-[100]`) — Fills the entire viewport and sits above everything (`z-index: 100`, using Tailwind's arbitrary value syntax since the default scale stops at 50).
2. **Backdrop** (`popup-backdrop absolute inset-0`) — The blurred, semi-transparent overlay. It has its own ID for click-to-close functionality.
3. **Centering wrapper** (`pointer-events-none`) — A flex container that centers the dialog. It's set to `pointer-events-none` so clicks pass through to the backdrop.
4. **Dialog card** (`pointer-events-auto`) — Re-enables pointer events for the actual dialog, so clicking the card doesn't close the popup.

This `pointer-events-none` / `pointer-events-auto` pattern is an elegant solution for distinguishing clicks on the backdrop from clicks on the dialog without requiring JavaScript click coordinate checking.

The map search popup contains:

- An OpenLayers map with crosshair cursor
- Read-only lat/lng inputs that display the clicked coordinates
- A radius input (1–500 miles, default 50)
- "Find Documents" and "Clear & Reset" buttons
- A status message area

### 5.2 Advanced Search Popup (Lines 346–371)

The advanced search popup follows the identical three-layer pattern but with a form containing:

- Document name text input
- Date range (from/to) date pickers
- Location text input
- Owner text input
- Search and Clear buttons

The popup's gradient icon uses `from-violet-500 to-purple-600` (purple) versus the map popup's `from-emerald-500 to-teal-600` (green), providing a quick visual distinction between the two popups.

---

## 6. JavaScript Application Logic — Lines 373–716

All JavaScript is contained in a single `<script>` block at the bottom of `<body>`. Placing scripts at the end ensures the entire DOM is parsed and available before the JavaScript executes, eliminating the need for `DOMContentLoaded` event listeners.

The code is organized into 20 numbered sections, each handling a specific concern.

### 6.1 Section 1: Data Layer (Lines 375–382)

```javascript
const headerConfig = {
  brand: { title: "SecureDoc Portal", subtitle: "Search & review documents securely", iconText: "SD" },
  menu: [
    { label: "Home", href: "#home", icon: "home" },
    { label: "Documents", href: "#docs", icon: "file" },
    { label: "Analytics", href: "#analytics", icon: "chart" },
    { label: "Settings", href: "#settings", icon: "gear" }
  ]
};
```

The `headerConfig` object is a configuration-driven pattern that separates data from rendering logic. The header's brand text and menu items are defined as data, and the `renderHeader()` function (Section 6.3) reads this config to build the DOM. This makes it trivial to change the app name, add/remove menu items, or reuse the header component in another context.

```javascript
const documents = [
  { id: "DOC-1001", name: "Incident Report - Warehouse 7", date: "2026-01-18",
    owner: "A. Johnson", type: "Incident Report",
    locationText: "Montgomery, AL — 1225 Air Base Blvd (Building D)",
    lat: 32.3517, lng: -86.2660,
    notes: "Initial report created during field assessment...", favorite: true },
  // ... 3 more documents
];
```

The `documents` array is the application's data store — a simple in-memory array of plain objects. Each document has:

- `id` — A unique string identifier following the pattern `DOC-XXXX`
- `name`, `owner`, `type`, `notes` — Human-readable text fields
- `date` — An ISO 8601 date string (`YYYY-MM-DD`), chosen because it sorts lexicographically (alphabetical sort = chronological sort)
- `lat`, `lng` — Decimal coordinates for geospatial features
- `favorite` — A boolean flag toggled by the user

In a production application, this array would be replaced by an API fetch. The array structure allows the same filtering, sorting, and rendering code to work unchanged with API-sourced data.

### 6.2 Section 2: SVG Icon Factory (Lines 384–385)

```javascript
function iconSvg(n) {
  const c = 'class="h-5 w-5" viewBox="0 0 24 24" fill="none" stroke="currentColor"';
  const s = 'stroke-linecap="round" stroke-width="2"';
  const i = {
    home: `<svg ${c}><path ${s} d="M3 10.5l9-7 9 7V20..."/></svg>`,
    file: `<svg ${c}>...</svg>`,
    chart: `<svg ${c}>...</svg>`,
    gear: `<svg ${c}>...</svg>`,
    star: `<svg ${c}>...</svg>`,
    download: `<svg ${c}>...</svg>`,
    info: `<svg ${c}>...</svg>`
  };
  return i[n] || i.file;
}
```

This function is a lightweight icon system. Instead of loading an entire icon font library (which could add 100KB+), it stores only the 7 icons the application needs as SVG template strings. The common SVG attributes are extracted into shared variables (`c` and `s`) to reduce duplication.

The fallback `|| i.file` ensures that an unrecognized icon name returns the file icon rather than `undefined`, preventing broken markup.

All icons use a consistent 24×24 viewBox, 2px stroke width, and `currentColor` as the stroke color. This makes them inherit the text color of their parent element, integrating seamlessly with Tailwind's color utilities.

### 6.3 Section 3: Header Renderer (Lines 387–388)

```javascript
function renderHeader(cfg) {
  document.getElementById("brandIcon").textContent = cfg.brand.iconText;
  document.getElementById("brandTitle").textContent = cfg.brand.title;
  document.getElementById("brandSubtitle").textContent = cfg.brand.subtitle;

  const dm = document.getElementById("desktopMenu");
  dm.innerHTML = "";
  const mm = document.getElementById("mobileMenu");
  mm.innerHTML = "";

  cfg.menu.forEach(item => {
    // Create desktop menu link
    const a = document.createElement("a");
    a.href = item.href;
    a.className = "inline-flex items-center gap-2 h-10 px-3 rounded-xl text-sm ...";
    a.innerHTML = `<span ...>${iconSvg(item.icon)}</span><span>${item.label}</span>`;
    dm.appendChild(a);

    // Create mobile menu link (same data, different layout)
    const m = document.createElement("a");
    m.href = item.href;
    m.className = "flex items-center gap-2 px-3 py-2 rounded-xl ...";
    m.innerHTML = `<span ...>${iconSvg(item.icon)}</span><span>${item.label}</span>`;
    mm.appendChild(m);
  });
}
```

This function takes the `headerConfig` object and populates both the desktop navigation bar and the mobile dropdown menu. Each menu item becomes an anchor (`<a>`) with an icon and text label.

The desktop items use `inline-flex items-center` (horizontal layout, vertically centered), while mobile items use `flex items-center` with additional padding for touch targets. Both use hover state styles with border and background transitions.

Using `document.createElement()` and setting `innerHTML` for the inner content is a hybrid approach — the outer element is created programmatically (safe from XSS since `href` and `className` are set directly), while the inner HTML is a template string. In a production environment with user-supplied data, the inner HTML should also be built with `createElement` or sanitized to prevent XSS.

### 6.4 Section 4: Dark/Light Theme System (Lines 390–393)

```javascript
function initTheme() {
  const s = localStorage.getItem('theme');
  if (s === 'dark' || (!s && window.matchMedia('(prefers-color-scheme: dark)').matches))
    document.documentElement.classList.add('dark');
  updateThemeIcons();
}

function updateThemeIcons() {
  const d = document.documentElement.classList.contains('dark');
  document.getElementById('sunIcon').classList.toggle('hidden', !d);
  document.getElementById('moonIcon').classList.toggle('hidden', d);
}

document.getElementById('themeToggle').addEventListener('click', () => {
  document.documentElement.classList.toggle('dark');
  localStorage.setItem('theme',
    document.documentElement.classList.contains('dark') ? 'dark' : 'light');
  updateThemeIcons();
});
```

The theme system implements a three-tier preference cascade:

1. **Explicit user choice** — If `localStorage.getItem('theme')` returns `'dark'` or `'light'`, that is used.
2. **Operating system preference** — If no localStorage value exists (`!s`), the `prefers-color-scheme: dark` media query is checked. This detects the OS-level dark mode setting.
3. **Default** — If neither is set, light mode is the implicit default (the `dark` class is simply not added).

The toggle button inverts the current state and persists the new choice to `localStorage`, ensuring the preference survives page reloads and browser restarts.

`document.documentElement` refers to the `<html>` element. Adding or removing the `dark` class here causes all Tailwind `dark:` variants throughout the entire page to activate or deactivate simultaneously, and the CSS transitions (Section 2.7.2) smooth the visual change.

### 6.5 Section 5: Column Sorting (Lines 395–399)

```javascript
let sortField = null, sortDir = 'asc';

function sortDocuments(docs) {
  if (!sortField) return docs;
  const s = [...docs];
  s.sort((a, b) => {
    let vA, vB;
    switch (sortField) {
      case 'date': vA = a.date; vB = b.date; break;
      case 'name': vA = a.name.toLowerCase(); vB = b.name.toLowerCase(); break;
      case 'owner': vA = a.owner.toLowerCase(); vB = b.owner.toLowerCase(); break;
      case 'type': vA = a.type.toLowerCase(); vB = b.type.toLowerCase(); break;
      default: return 0;
    }
    if (vA < vB) return sortDir === 'asc' ? -1 : 1;
    if (vA > vB) return sortDir === 'asc' ? 1 : -1;
    return 0;
  });
  return s;
}
```

The sorting system uses module-level state variables (`sortField` and `sortDir`) to track the current sort column and direction.

Key implementation details:

- `[...docs]` — The spread operator creates a shallow copy of the array so the original `documents` array is never mutated. This preserves the original data order for future operations.
- String comparison for dates works correctly because the `YYYY-MM-DD` ISO format is lexicographically ordered (e.g., `"2025-12-29" < "2026-01-10"` evaluates to `true`). This is a key reason ISO date strings were chosen for the data format.
- `.toLowerCase()` is called for name, owner, and type comparisons to ensure case-insensitive sorting (e.g., "apple" sorts near "Apricot" rather than being separated by uppercase/lowercase ASCII code boundaries).

The `updateSortUI()` function updates the visual state of sort icons — the active column's icon becomes fully opaque and blue, with rotation indicating direction (180° for ascending, 0° for descending).

Column header click listeners toggle between ascending/descending on re-click, or switch to ascending on a new column:

```javascript
document.querySelectorAll('th.sortable').forEach(th => {
  th.addEventListener('click', () => {
    const f = th.dataset.sort;
    if (sortField === f) sortDir = sortDir === 'asc' ? 'desc' : 'asc';
    else { sortField = f; sortDir = 'asc'; }
    updateSortUI();
    applyFiltersAndRender();
  });
});
```

### 6.6 Section 6: Filtering Engine (Lines 401–407)

```javascript
function getFilteredDocuments() {
  const n = document.getElementById('filterName').value.toLowerCase().trim();
  const o = document.getElementById('filterOwner').value.toLowerCase().trim();
  const t = document.getElementById('filterType').value;
  const df = document.getElementById('filterDateFrom').value;
  const dt = document.getElementById('filterDateTo').value;

  return documents.filter(d => {
    if (n && !d.name.toLowerCase().includes(n) && !d.id.toLowerCase().includes(n)) return false;
    if (o && !d.owner.toLowerCase().includes(o)) return false;
    if (t && d.type !== t) return false;
    if (df && d.date < df) return false;
    if (dt && d.date > dt) return false;
    return true;
  });
}
```

The filter function applies a chain of conditions using short-circuit evaluation. Each condition is only checked if the corresponding filter field is non-empty (the `n &&` prefix). If a filter is empty, its condition is skipped entirely. This means an empty filter = no filter for that field, and all non-empty filters are combined with AND logic.

The name filter searches both `d.name` and `d.id`, allowing users to search by document ID ("DOC-1001") or by title text.

The date comparison works because ISO date strings sort lexicographically, so `d.date < df` correctly determines whether the document's date is before the "from" date.

The `applyFiltersAndRender()` composition function chains filtering → sorting → rendering:

```javascript
function applyFiltersAndRender() {
  renderDocumentsTable(sortDocuments(getFilteredDocuments()));
}
```

This functional pipeline pattern ensures filters and sorts always produce a consistent result.

Real-time filtering is enabled by listening for `input` and `change` events on all filter elements:

```javascript
['filterName', 'filterOwner', 'filterType', 'filterDateFrom', 'filterDateTo'].forEach(id => {
  const el = document.getElementById(id);
  el.addEventListener('input', applyFiltersAndRender);
  el.addEventListener('change', applyFiltersAndRender);
});
```

Both `input` and `change` events are bound because:

- `input` fires on every keystroke in text fields (for instant feedback)
- `change` fires when the date picker or select dropdown value is committed (some browsers don't fire `input` for these)

### 6.7 Section 7: Responsive Helper (Lines 409–411)

```javascript
const LG_BP = 1024;
function isMobile() { return window.innerWidth < LG_BP; }
```

A simple breakpoint check that mirrors Tailwind's `lg:` breakpoint (1024px). This function is called throughout the JavaScript to decide between mobile and desktop behavior paths. Using a named constant (`LG_BP`) rather than a magic number ensures the breakpoint value is defined once and matches the Tailwind configuration.

### 6.8 Section 8: Inline Mobile Detail Panel (Lines 413–533)

This is the most complex section of the application. On mobile, clicking a table row does not open a side panel (there's no room). Instead, a detail panel is injected as a new table row immediately below the clicked row, creating an accordion-like expansion.

**State Management:**

```javascript
let inlineMap = null, inlineMarkerLayer = null, inlineDetailRow = null, inlineDocId = null;
```

Four state variables track the current inline panel. Only one can be open at a time. `inlineMap` stores the OpenLayers map instance so it can be properly destroyed when the panel closes (preventing memory leaks from orphaned map instances).

**Type Color Mapping:**

```javascript
const TYPE_COLORS = {
  'Incident Report': 'bg-red-50 dark:bg-red-950 text-red-700 dark:text-red-300 border ...',
  'Audit Summary': 'bg-blue-50 dark:bg-blue-950 text-blue-700 dark:text-blue-300 ...',
  'Maintenance Log': 'bg-amber-50 dark:bg-amber-950 text-amber-700 dark:text-amber-300 ...',
  'Training Record': 'bg-emerald-50 dark:bg-emerald-950 text-emerald-700 ...'
};
```

Each document type maps to a set of Tailwind classes that create a color-coded badge. The pattern uses:

- Very light tint backgrounds (`-50` in light, `-950` in dark)
- Medium-intensity text (`-700` in light, `-300` in dark)
- Matching borders (`-200` in light, `-800` in dark)

This creates an accessible, visually distinct badge for each type while maintaining the overall color scheme.

**Panel Construction:**

The `buildInlinePanelHTML()` function returns a complete HTML string containing the detail panel layout. It uses template literals (backtick strings) with `${expression}` interpolation to embed document data. The panel contains:

- Document title and ID
- A 4-column grid of metadata cards (Owner, Type, Date, Coordinates)
- A location description
- An OpenLayers map placeholder
- A notes section

**Panel Lifecycle:**

```javascript
function createInlinePanel(doc, afterRow) {
  destroyInlinePanel(); // Clean up any existing panel

  // Create a new table row with a single cell spanning all columns
  const colCount = afterRow.querySelectorAll('td').length;
  inlineDetailRow = document.createElement('tr');
  const td = document.createElement('td');
  td.setAttribute('colspan', colCount);
  td.innerHTML = buildInlinePanelHTML(doc);
  inlineDetailRow.appendChild(td);
  afterRow.insertAdjacentElement('afterend', inlineDetailRow);

  // Trigger CSS animation (double rAF ensures the browser has painted the initial state)
  requestAnimationFrame(() => requestAnimationFrame(() => {
    document.getElementById('inlineOuter_' + doc.id)?.classList.add('open');
  }));

  // Initialize map after DOM insertion (setTimeout for rendering pipeline)
  setTimeout(() => {
    inlineMap = new ol.Map({
      target: mapId,
      layers: [new ol.layer.Tile({ source: new ol.source.OSM() }), inlineMarkerLayer],
      view: new ol.View({ center: ol.proj.fromLonLat([doc.lng, doc.lat]), zoom: 12 })
    });
    // Add marker...
  }, 120);

  // Smooth scroll to make the panel visible
  setTimeout(() => inlineDetailRow?.scrollIntoView({ behavior: 'smooth', block: 'nearest' }), 250);
}
```

The **double `requestAnimationFrame`** pattern on line 501 is a crucial technique. The CSS accordion animation (Section 2.7.16) relies on transitioning `max-height` from `0` to `900px`. For the transition to occur, the browser must:

1. First paint the element with `max-height: 0`
2. Then change to `max-height: 900px` in a subsequent frame

A single `requestAnimationFrame` runs before the *next* repaint, but the initial state might not have been painted yet. The double-rAF pattern guarantees the browser has completed at least one paint cycle before the class change, ensuring the transition always plays.

The `destroyInlinePanel()` function properly cleans up by:

1. Detaching the OpenLayers map from its target (`setTarget(null)`) — this stops tile loading and event listeners
2. Removing the table row from the DOM
3. Nullifying all state variables

The closing animation (`closeMobileInline()`) removes the `open` class first, waits 420ms for the CSS transition (450ms with a small buffer), then destroys the panel. This ensures the accordion collapses smoothly before the DOM element is removed.

### 6.9 Section 9: Table Renderer (Lines 535–583)

```javascript
function formatDate(iso) {
  return new Date(iso + "T00:00:00").toLocaleDateString(undefined, {
    year: "numeric", month: "short", day: "2-digit"
  });
}
```

The `"T00:00:00"` suffix is appended to prevent timezone shifting. When JavaScript parses a date-only string like `"2026-01-18"`, it treats it as UTC midnight. Depending on the user's timezone, this could display as the previous day (e.g., January 17th in US time zones). By appending `T00:00:00`, the date is treated as midnight local time, preventing the off-by-one-day bug.

The `toLocaleDateString()` options produce output like "Jan 18, 2026" in English locales. The `undefined` first argument tells the browser to use the user's system locale.

```javascript
function renderDocumentsTable(docs) {
  // ... cleanup existing inline panel ...

  const tbody = document.getElementById("docTableBody");
  tbody.innerHTML = "";
  // Update counts...

  docs.forEach(doc => {
    const tr = document.createElement("tr");
    tr.dataset.docId = doc.id; // data-doc-id attribute for later lookup

    // Create cells: Actions, Date, Name, Owner, Type, Location
    // ... (each cell is created with document.createElement and populated)

    tr.addEventListener("click", e => {
      if (e.target.closest("button")) return; // Don't open details on button clicks
      openDetails(doc.id);
    });
    tbody.appendChild(tr);
  });

  // Event delegation for action buttons
  tbody.addEventListener("click", e => {
    const b = e.target.closest("button");
    if (!b) return;
    const a = b.getAttribute("data-action");
    const id = b.getAttribute("data-id");
    if (a === "favorite") toggleFavorite(id);
    if (a === "download") fakeDownload(id);
    if (a === "details") openDetails(id);
    e.stopPropagation();
  });
}
```

The table renderer uses **event delegation** for action buttons. Instead of attaching individual click listeners to each button in each row (which could be hundreds of listeners), a single listener on the `<tbody>` element uses `e.target.closest("button")` to detect which button was clicked and reads `data-action` and `data-id` attributes to determine the action. This is more memory-efficient and automatically works for dynamically added rows.

The `e.stopPropagation()` call prevents the button click from bubbling up to the `<tr>` click handler (which opens details). Without this, clicking the star/download button would both perform the button action and open the details panel.

**The favorite toggle** (`toggleFavorite`) flips the `favorite` boolean on the document object and re-renders the table. Since the data is mutated in place and re-rendered from the same array, the state change is automatically reflected.

**The download function** (`fakeDownload`) creates a text file with the document's data using the Blob API and triggers a download via a temporary `<a>` element with a `download` attribute. The `URL.createObjectURL()` creates a temporary URL pointing to the in-memory Blob, and `URL.revokeObjectURL()` releases it after the download starts.

### 6.10 Section 10: Right Pane Toggle (Desktop) (Lines 585–589)

```javascript
const leftPane = document.getElementById("leftPane");
const rightPane = document.getElementById("rightPane");
let currentDocId = null;

function showRightPane() {
  rightPane.classList.remove("hidden");
  leftPane.classList.remove("lg:col-span-12");
  leftPane.classList.add("lg:col-span-7");
  rightPane.classList.add("lg:col-span-5");
}

function hideRightPane() {
  rightPane.classList.add("hidden");
  leftPane.classList.remove("lg:col-span-7");
  leftPane.classList.add("lg:col-span-12");
  currentDocId = null;
  closeMobileInline();
  applyFiltersAndRender();
}
```

These functions toggle the 12-column → 7:5 grid layout. The `showRightPane` function creates the split view, while `hideRightPane` restores the full-width layout and clears the selection state.

### 6.11 Sections 11–12: OpenLayers Map Integration (Lines 591–600)

**Detail Map (Section 11):**

```javascript
let detailMap, detailMarkerLayer;

function initDetailMap() {
  detailMarkerLayer = new ol.layer.Vector({ source: new ol.source.Vector() });
  detailMap = new ol.Map({
    target: "map",
    layers: [
      new ol.layer.Tile({ source: new ol.source.OSM() }),
      detailMarkerLayer
    ],
    view: new ol.View({
      center: ol.proj.fromLonLat([-86.2660, 32.3517]),
      zoom: 10
    })
  });
}
```

Each OpenLayers map has the same three-layer architecture:

1. **Tile layer** with `ol.source.OSM()` — Loads map tiles from OpenStreetMap's free tile server. No API key required.
2. **Vector layer** — A programmatic drawing layer used for markers (and for the search map, the radius circle).
3. **View** — Defines the center point and zoom level. `ol.proj.fromLonLat()` converts longitude/latitude (EPSG:4326) to the map's internal Web Mercator projection (EPSG:3857).

The marker is created as an `ol.Feature` with a `Point` geometry and styled as a filled circle:

```javascript
f.setStyle(new ol.style.Style({
  image: new ol.style.Circle({
    radius: 8,
    fill: new ol.style.Fill({ color: "#3b82f6" }),
    stroke: new ol.style.Stroke({ color: "#fff", width: 2.5 })
  })
}));
```

This creates an 8px blue circle with a 2.5px white border — resembling a standard map pin without requiring an image resource.

**Search Map (Section 12):**

The search map adds click-to-place interactivity. When the user clicks the map:

1. The click coordinate is converted from EPSG:3857 to EPSG:4326 (lon/lat)
2. A green SVG pin marker is placed using `ol.style.Icon` with an inline SVG data URI
3. The radius circle is drawn using `drawSearchRadius()`

The `drawSearchRadius()` function:

```javascript
function drawSearchRadius(lat, lng, mi) {
  // Remove existing radius circle
  src.getFeatures().forEach(f => { if (f.get('isRadius')) src.removeFeature(f); });

  // Convert miles to meters
  const r = mi * 1609.34;

  // Create a geometric circle and convert to polygon (for rendering)
  const circ = new ol.geom.Circle(ol.proj.fromLonLat([lng, lat]), r);
  const pg = ol.geom.Polygon.fromCircle(circ, 64); // 64-sided polygon approximation

  // Style with transparent fill and dashed stroke
  f.setStyle(new ol.style.Style({
    fill: new ol.style.Fill({ color: 'rgba(16,185,129,0.1)' }),
    stroke: new ol.style.Stroke({ color: '#10b981', width: 2, lineDash: [6, 4] })
  }));
}
```

The radius circle is a 64-sided polygon approximation of a true circle. OpenLayers doesn't render `Circle` geometries directly in vector layers — it must be converted to a `Polygon`. The 64 sides make the polygon visually indistinguishable from a true circle at any reasonable zoom level.

The `isRadius` flag on the feature allows the code to distinguish the radius circle from the pin marker, so only the circle is replaced when the radius input changes.

### 6.12 Sections 13–14: Detail Rendering & Responsive Routing (Lines 602–642)

**`renderDetails(doc)`** populates the right pane's detail fields by setting `textContent` on each element. Using `textContent` instead of `innerHTML` is a deliberate security choice — `textContent` treats the input as plain text, preventing XSS even if a document name contained `<script>` tags.

**`openDetails(id)`** is the routing function that decides how to display details based on screen size:

```javascript
function openDetails(id) {
  const doc = documents.find(d => d.id === id);
  if (!doc) return;

  if (isMobile()) {
    // Toggle: clicking the same row again collapses it
    if (inlineDocId === id) { closeMobileInline(); return; }

    // Hide desktop pane, create inline panel after the clicked row
    rightPane.classList.add("hidden");
    currentDocId = id;
    createInlinePanel(doc, targetRow);
  } else {
    // DESKTOP: open side panel, render details, highlight row
    closeMobileInline();
    showRightPane();
    renderDetails(doc);
    setTimeout(() => detailMap && detailMap.updateSize(), 50);
  }
}
```

The `setTimeout` on the desktop path gives the CSS grid transition 50ms to complete before telling OpenLayers to recalculate its viewport size. Without this, the map might render at the old container dimensions.

### 6.13 Section 15: Window Resize Handler (Lines 644–654)

```javascript
let lastWasMobile = isMobile();
window.addEventListener('resize', () => {
  const now = isMobile();
  if (lastWasMobile !== now && currentDocId) {
    const doc = documents.find(d => d.id === currentDocId);
    if (now) {
      // Transitioning desktop → mobile: close side panel, open inline
      hideRightPane();
      if (doc) setTimeout(() => openDetails(doc.id), 100);
    } else {
      // Transitioning mobile → desktop: close inline, open side panel
      destroyInlinePanel();
      if (doc) { showRightPane(); renderDetails(doc); ... }
    }
  }
  lastWasMobile = now;
});
```

This handler detects when the user resizes their browser window across the mobile/desktop breakpoint boundary (1024px). If a document is currently selected, it seamlessly transitions the detail view from one presentation mode to the other. Without this, the user would lose their selection when resizing.

The `lastWasMobile` flag prevents the transition logic from running on every resize event — it only fires when the breakpoint is actually crossed.

### 6.14 Section 16: Map Search Logic (Lines 656–678)

**Haversine Formula:**

```javascript
function haversine(lat1, lng1, lat2, lng2) {
  const R = 3958.8; // Earth's radius in miles
  const dLat = (lat2 - lat1) * Math.PI / 180;
  const dLng = (lng2 - lng1) * Math.PI / 180;
  const a = Math.sin(dLat/2)**2 +
            Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) *
            Math.sin(dLng/2)**2;
  return R * 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
}
```

The haversine formula calculates the great-circle distance between two points on a sphere. It accounts for the Earth's curvature, unlike simple Euclidean distance which would give incorrect results over large distances. The formula:

1. Converts degree differences to radians
2. Computes the central angle using the haversine of the angular differences
3. Multiplies by Earth's radius (3,958.8 miles) to get the arc length

The result is the "as the crow flies" distance in miles between two geographic coordinates. This is used to filter documents within the user-specified radius from the clicked map point.

**Search Execution:**

```javascript
document.getElementById('mapSearchBtn').addEventListener('click', () => {
  if (!searchPickedCoords) { /* show warning */ return; }
  const r = parseFloat(document.getElementById('mapSearchRadius').value) || 50;
  const res = documents.filter(d =>
    haversine(searchPickedCoords.lat, searchPickedCoords.lng, d.lat, d.lng) <= r
  );
  // Clear text filters, render results, close popup
  renderDocumentsTable(sortDocuments(res));
  closeMapSearch();
});
```

The search filters the documents array to only those within `r` miles of the clicked coordinate, renders the filtered results, and closes the popup.

### 6.15 Section 17: Advanced Search Logic (Lines 680–696)

The advanced search popup collects five criteria (name, date from, date to, location text, owner) and filters the documents array using the same pattern as `getFilteredDocuments()`. After finding results, it also syncs the filter bar inputs to match the popup's values, so the user can see and adjust the active filters after the popup closes.

### 6.16 Sections 18–20: Mobile Menu, Keyboard Shortcuts, Initialization (Lines 698–716)

**Mobile Menu (Section 18):**

```javascript
function setupMobileMenu() {
  document.getElementById("mobileMenuBtn").addEventListener("click", () =>
    document.getElementById("mobileMenuPanel").classList.toggle("hidden")
  );
  document.getElementById("mobileMenu").addEventListener("click", e => {
    if (e.target.closest("a"))
      document.getElementById("mobileMenuPanel").classList.add("hidden");
  });
}
```

The mobile menu uses a simple `hidden` class toggle. Clicking any link within the menu automatically closes it (event delegation again).

**Keyboard Shortcuts (Section 19):**

```javascript
document.addEventListener('keydown', e => {
  if (e.key === 'Escape') {
    // Close popups in priority order
    if (!mapSearchPopup.classList.contains('hidden')) closeMapSearch();
    else if (!advSearchPopup.classList.contains('hidden')) closeAdvSearch();
    else if (inlineDocId) closeMobileInline();
  }
  if ((e.ctrlKey || e.metaKey) && e.key === 'k') {
    e.preventDefault();
    document.getElementById('headerSearchInput').focus();
  }
});
```

Escape closes overlays in a prioritized order (popups first, then inline panels). `Ctrl+K` (or `Cmd+K` on Mac) focuses the search bar — this is a widely recognized keyboard shortcut used by Spotlight, VS Code, Slack, and many web applications.

`e.preventDefault()` on Ctrl+K prevents the browser's default behavior (which in some browsers opens the address bar).

**Initialization (Section 20):**

```javascript
initTheme();
renderHeader(headerConfig);
populateTypeFilter();
applyFiltersAndRender();
updateSortUI();
setupMobileMenu();
initDetailMap();
```

The initialization sequence follows a logical order:

1. Apply theme (prevent flash of wrong theme)
2. Render the header (fill in brand text and menu items)
3. Populate the type filter dropdown from the data
4. Render the table with all documents (no filters active)
5. Set the sort UI to its default state
6. Set up the mobile menu toggle
7. Initialize the detail map (even though the panel is hidden — this prevents a rendering delay when first opened)

---

## 7. Design Decisions & Rationale

| Decision | Alternative Considered | Rationale for Chosen Approach |
|---|---|---|
| Single HTML file | Multi-file with build step | Zero-dependency deployment; opens directly in browser; ideal for internal tools and prototypes |
| Tailwind CDN | Bootstrap, custom CSS, CSS Modules | Utility-first approach enables rapid iteration; CDN eliminates build step; consistent design tokens |
| Vanilla JavaScript | React, Vue, Svelte | Application scope is small (4 documents, ~20 interactions); framework would add complexity without proportional benefit |
| OpenLayers | Google Maps, Leaflet, Mapbox | No API key required; free OSM tiles; strong vector drawing for radius circles; permissive BSD license |
| Class-based dark mode | Media-query dark mode | User control > OS preference; supports explicit toggle; falls back to OS preference |
| CSS animations | JS animation libraries (GSAP, Framer) | Lightweight keyframe animations sufficient for this UI; no external dependency needed |
| Event delegation | Per-element listeners | Single listener per container vs. N listeners per row; scales better; works with dynamic DOM |
| `textContent` over `innerHTML` | `innerHTML` everywhere | Prevents XSS where user data is displayed; `innerHTML` used only for trusted template strings |
| ISO 8601 dates | Timestamps, Date objects | Lexicographic sorting matches chronological sorting; human-readable; timezone-agnostic |
| Haversine formula | Euclidean distance, PostGIS | Client-side geospatial filtering without a database; accurate for distances < 1000 miles |

---

## 8. External References & Official Documentation

### Core Technologies

- **HTML5 Specification:** https://html.spec.whatwg.org/
- **MDN Web Docs — HTML:** https://developer.mozilla.org/en-US/docs/Web/HTML
- **MDN Web Docs — CSS:** https://developer.mozilla.org/en-US/docs/Web/CSS
- **MDN Web Docs — JavaScript:** https://developer.mozilla.org/en-US/docs/Web/JavaScript

### Tailwind CSS

- **Tailwind CSS Official Docs:** https://tailwindcss.com/docs
- **Tailwind CDN (Play CDN):** https://tailwindcss.com/docs/installation/play-cdn
- **Dark Mode Configuration:** https://tailwindcss.com/docs/dark-mode
- **Responsive Design Breakpoints:** https://tailwindcss.com/docs/responsive-design
- **Customizing Font Families:** https://tailwindcss.com/docs/font-family
- **Gradient Background Utilities:** https://tailwindcss.com/docs/background-image
- **Arbitrary Values (Bracket Notation):** https://tailwindcss.com/docs/adding-custom-styles#using-arbitrary-values

### OpenLayers

- **OpenLayers Official Docs:** https://openlayers.org/en/latest/doc/
- **API Reference:** https://openlayers.org/en/latest/apidoc/
- **ol.Map:** https://openlayers.org/en/latest/apidoc/module-ol_Map-Map.html
- **ol.View:** https://openlayers.org/en/latest/apidoc/module-ol_View-View.html
- **ol.layer.Tile:** https://openlayers.org/en/latest/apidoc/module-ol_layer_Tile-TileLayer.html
- **ol.layer.Vector:** https://openlayers.org/en/latest/apidoc/module-ol_layer_Vector-VectorLayer.html
- **ol.source.OSM:** https://openlayers.org/en/latest/apidoc/module-ol_source_OSM-OSM.html
- **ol.Feature & Geometry:** https://openlayers.org/en/latest/apidoc/module-ol_Feature-Feature.html
- **ol.proj.fromLonLat:** https://openlayers.org/en/latest/apidoc/module-ol_proj.html#.fromLonLat
- **Styling (Circle, Fill, Stroke, Icon):** https://openlayers.org/en/latest/apidoc/module-ol_style_Style-Style.html

### Google Fonts

- **Google Fonts:** https://fonts.google.com/
- **DM Sans:** https://fonts.google.com/specimen/DM+Sans
- **JetBrains Mono:** https://fonts.google.com/specimen/JetBrains+Mono
- **CSS2 API Documentation:** https://developers.google.com/fonts/docs/css2
- **Font Display (`swap`):** https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-display

### CSS Techniques

- **Backdrop Filter (Glassmorphism):** https://developer.mozilla.org/en-US/docs/Web/CSS/backdrop-filter
- **CSS Transitions:** https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_transitions/Using_CSS_transitions
- **CSS Animations (@keyframes):** https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animations/Using_CSS_animations
- **Cubic Bezier Timing Functions:** https://developer.mozilla.org/en-US/docs/Web/CSS/easing-function
- **`:focus-within` Pseudo-class:** https://developer.mozilla.org/en-US/docs/Web/CSS/:focus-within
- **`user-select` Property:** https://developer.mozilla.org/en-US/docs/Web/CSS/user-select
- **Custom Scrollbar Styling:** https://developer.mozilla.org/en-US/docs/Web/CSS/::-webkit-scrollbar
- **`max-height` Accordion Technique:** https://css-tricks.com/using-css-transitions-auto-dimensions/

### JavaScript APIs

- **DOM Manipulation:** https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model
- **`localStorage`:** https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage
- **`matchMedia` (prefers-color-scheme):** https://developer.mozilla.org/en-US/docs/Web/API/Window/matchMedia
- **`requestAnimationFrame`:** https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame
- **`Element.closest()`:** https://developer.mozilla.org/en-US/docs/Web/API/Element/closest
- **`insertAdjacentElement()`:** https://developer.mozilla.org/en-US/docs/Web/API/Element/insertAdjacentElement
- **`classList` API:** https://developer.mozilla.org/en-US/docs/Web/API/Element/classList
- **`scrollIntoView()`:** https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollIntoView
- **`toLocaleDateString()`:** https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleDateString
- **Blob API:** https://developer.mozilla.org/en-US/docs/Web/API/Blob
- **`URL.createObjectURL()`:** https://developer.mozilla.org/en-US/docs/Web/API/URL/createObjectURL_static
- **Event Delegation Pattern:** https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Event_bubbling
- **Keyboard Events:** https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent

### Geospatial

- **Haversine Formula (Wikipedia):** https://en.wikipedia.org/wiki/Haversine_formula
- **EPSG:4326 (WGS 84):** https://epsg.io/4326
- **EPSG:3857 (Web Mercator):** https://epsg.io/3857
- **OpenStreetMap Tile Usage Policy:** https://operations.osmfoundation.org/policies/tiles/

### Accessibility

- **ARIA Labels:** https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-label
- **`lang` Attribute:** https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/lang
- **Semantic HTML Elements:** https://developer.mozilla.org/en-US/docs/Glossary/Semantics#semantics_in_html

### Design Patterns

- **CSS Grid Layout:** https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout
- **Flexbox:** https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout
- **Responsive Design:** https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive_Design
- **Glassmorphism (CSS Tricks):** https://css-tricks.com/almanac/properties/b/backdrop-filter/
- **Dark Mode Best Practices:** https://web.dev/articles/prefers-color-scheme

---

*Document generated for code review purposes. All line numbers reference the original `index.html` file (718 lines total).*