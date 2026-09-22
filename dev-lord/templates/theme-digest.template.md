# Theme Digest: [Theme Name & Version]

> **Purpose & Contract**: This document defines the standardized interface specification for a UI theme. When introducing a new theme to **Road U**, fill out this template based on the theme's local documentation and files. Road U will read this profile to generate authentic, working, self-contained HTML prototypes without guessing or hallucinating markup.

---

## 1. Metadata & Source Coordinates
- **Theme Identity:** [e.g., Metronic / AdminLTE / Tabler / Tailwind UI Admin]
- **Origin / Author:** [e.g., Coderthemes / KeenThemes / Colorlib]
- **Version:** [e.g., 8.2.0]
- **Foundation Stack:** [e.g., Bootstrap 5.3 / Tailwind 3.4 / Vanilla Modern JS / SCSS]
- **Core Dependencies:** [e.g., Vanilla Modern JS (no jQuery) / jQuery required for legacy plugins]
- **Default Local Source Path:** [e.g., d:/path/to/theme/directory/ or ./theme/]
  *(Note: Road U checks this path first. If found, it reads assets and pages automatically without asking the user).*
- **Theme Asset Root:** [e.g., assets/ or dist/]
- **Output Prototype Default:** `<project-root>/ui-prototype/`

---

## 2. Directory Structure & Key Files
```text
[theme-root]/
├── *.html                                 # Production HTML Pages / Demonstrators
└── [assets-folder]/
    ├── css/
    │   ├── [app.min.css]                  # Master compiled stylesheet (token engine)
    │   └── [vendors.min.css]              # Bundled vendor CSS
    ├── js/
    │   ├── [config.js]                    # Pre-render bootstrap script (sets <html> attributes; MUST LOAD FIRST)
    │   ├── [app.js]                       # Main theme controller (nav, customizer, portlets)
    │   ├── [vendors.min.js]               # Core vendor bundles (Bootstrap, Popper, etc.)
    │   └── pages/                         # Page-specific modular controllers (load on-demand)
    ├── fonts/                             # Icon webfonts and custom typography
    ├── data/
    │   └── translations/                  # Multilingual JSON language dictionaries
    ├── plugins/                           # Bundled third-party vendor plugins
    └── images/                            # Stock images, avatars, flags, illustrations
```

---

## 3. Configuration Dimensions (Root `<html>` or `<body>` Attributes)

Specify the exact HTML attributes used to control layout, theme skins, and display modes.

### 3.1 Available Theme Skins / Palettes
- **Skin Activation Attribute:** [e.g., `data-skin="[identifier]"` or `data-theme="[identifier]"` or class on `<html>`]

| Skin Identifier | Primary Accent (Hex) | Google Font Family | Recommended Tone / Architectural Mood |
| :--- | :--- | :--- | :--- |
| `[skin-1]` | `#[hex]` | [Font Name] | [e.g., Corporate, balanced, refined enterprise] |
| `[skin-2]` | `#[hex]` | [Font Name] | [e.g., Modern SaaS, high-energy, bold brand] |
| `[skin-3]` | `#[hex]` | [Font Name] | [e.g., Minimal, clean, tech-focused, documentation] |
| `[skin-4]` | `#[hex]` | [Font Name] | [e.g., Dark futuristic, deep indigo/violet] |

### 3.2 Global Layout Dimensions
- **Detected Architecture Pattern:** `[Attribute-Driven (data-*) | Class-Driven (Tailwind/BEM) | Token-Driven (CSS vars)]`
- **Theme Color Mode:** `[e.g., data-bs-theme="light|dark|system" OR class="dark" OR CSS token]`
- **Top Navigation Bar:** `[e.g., data-topbar-color="light|dark|gray" OR header utility classes]`
- **Sidebar / Menu Color:** `[e.g., data-menu-color="light|dark" OR sidebar utility classes]`
- **Sidebar Sizing / Mode:** `[e.g., data-sidenav-size="default|compact|offcanvas" OR sidebar toggle class]`
- **Container Width:** `[e.g., data-layout-width="fluid|boxed" OR container max-width class]`
- **Scroll Positioning:** `[e.g., data-layout-position="fixed|scrollable" OR sticky header class]`
- **Layout Direction & RTL:** `[e.g., dir="ltr|rtl"]`

### 3.3 Default Recommended Lockset
Define the optimal combination of attributes or classes to apply identically across all prototype screens:
```html
<!-- Attribute-driven example: -->
<html lang="en" data-skin="[default-skin]" data-bs-theme="light" data-topbar-color="dark" data-menu-color="light" data-sidenav-size="default" data-layout-width="fluid" data-layout-position="fixed" dir="ltr">

<!-- Class-driven / Tailwind example: -->
<html lang="en" class="light layout-fluid" dir="ltr">
```

---

## 4. Icon Systems & Conventions

Define the distinct icon systems provided by this theme and their exact syntactic rules:

1. **Primary UI & Navigation Icons:**
   - Library: [e.g., Lucide SVG / Feather / Bootstrap Icons]
   - Markup Syntax: `[e.g., <i data-lucide="home"></i> or <i class="bi bi-house"></i>]`
   - Showcase Reference Page: `[e.g., icons-lucide.html]`
2. **Domain & Metric Indicator Icons:**
   - Library: [e.g., Tabler Icons / FontAwesome / Material Icons]
   - Markup Syntax: `[e.g., <i class="ti ti-chart-bar"></i>]`
   - Showcase Reference Page: `[e.g., icons-tabler.html]`
3. **National Flags & Badges:**
   - Markup Syntax: `[e.g., <img src="assets/images/flags/us.svg" class="avatar-xs">]`
   - Showcase Reference Page: `[e.g., icons-flags.html]`

---

## 5. Internationalization (i18n) & RTL Engine

- **Engine Mechanism:** [e.g., Managed by I18nManager in app.js / Static HTML / None]
- **Dictionary Location:** `[e.g., assets/data/translations/*.json]`
- **Supported Languages:** `[e.g., en, ar, de, es, fr, zh]`
- **Markup Translation Key:** `[e.g., data-lang="nav.dashboard"]`
- **Language Switcher Trigger:** `[e.g., <a href="javascript:void(0);" data-translator-lang="ar">Arabic</a>]`
- **Native RTL Behavior:** [e.g., Selecting Arabic automatically sets dir="rtl" and mirrors the layout]

---

## 6. Proprietary Working Features (Use As-Is)

List interactive widgets built into this theme that Road U must preserve with full working JavaScript:

1. **Interactive Data Tables:**
   - Controller Script: `[e.g., assets/js/pages/custom-table.js or datatables.net]`
   - Live Search Input: `[e.g., <input type="search" data-table-search placeholder="Search...">]`
   - Column Sort Header: `[e.g., <th data-table-sort="string|number|date">]`
   - Filter Syntax: `[e.g., data-table-filter="category"]`
   - Batch Actions: `[e.g., data-table-delete-selected]`
2. **Card / Portlet Actions (`data-action`):**
   - Collapse Card: `[e.g., data-action="card-toggle"]`
   - Refresh Spinner: `[e.g., data-action="card-refresh"]`
   - Close / Dismiss: `[e.g., data-action="card-close"]`
3. **Numeric Counter Animations:**
   - Markup Syntax: `[e.g., <span data-target="1500">0</span>]`
4. **Settings Customizer Drawer:**
   - Selector / Offcanvas ID: `[e.g., #theme-settings-offcanvas]`
   - Note: Keep this drawer in every prototype page as a permanent product feature.
5. **Form Wizards & Steppers:**
   - Reference File: `[e.g., form-wizard.html]`
   - Step Navigation Markup: `[e.g., .wizard-step, .nav-pills with data-bs-toggle="pill"]`
6. **Date & Time Pickers:**
   - Library & Init Class: `[e.g., Flatpickr: <input type="text" data-provider="flatpickr" class="form-control">]`
7. **Pre-Built Widget Catalog:**
   - Reference File: `[e.g., widgets.html]`
   - Available Patterns: KPI metric tiles, target progress bars, contact cards, embedded chat stream.

---

## 7. Complete Page Showcase & Application Inventory

Road U consults this index to pick the exact matching page or component for each requirement:

### 7.1 Dashboards & Marketing
| File Name | Role & Primary Features |
| :--- | :--- |
| `[e.g., index.html]` | Main / Analytics Dashboard (KPIs, revenue charts, maps) |
| `[e.g., dashboard-projects.html]` | Project / Operations Dashboard (sprints, tasks, workloads) |
| `[e.g., landing.html]` | Public SaaS Marketing Landing Page (hero, CTA, pricing, FAQ) |

### 7.2 Full Applications Suite (`apps-*`)
| Domain | Production HTML Files | Features & Key Components |
| :--- | :--- | :--- |
| **CRM / Sales** | `[e.g., apps-crm-pipeline.html, apps-crm-leads.html, apps-crm-deals.html]` | Drag-and-drop Kanban pipeline, lead status tables, deals |
| **eCommerce** | `[e.g., apps-ecommerce-products.html, apps-ecommerce-orders.html, apps-ecommerce-cart.html]` | Product catalog (grid & list), product editor, orders, checkout |
| **Email & Chat** | `[e.g., apps-email.html, apps-chat.html]` | Webmail / Outlook multi-pane client, real-time chat bubbles |
| **File Management** | `[e.g., apps-file-manager.html]` | Folder navigation tree, file type badges, storage breakdown |
| **Billing & Invoices** | `[e.g., apps-invoice-list.html, apps-invoice-create.html]` | Printable itemized invoices, invoice generator |
| **Help Desk & Support** | `[e.g., apps-ticket-list.html, apps-ticket-details.html]` | Ticketing system with priority filters, agent threads |
| **User & Role Admin (RBAC)** | `[e.g., apps-users-permissions.html, apps-api-keys.html]` | Matrix permissions grid, API key generation, account settings |
| **Productivity & Social** | `[e.g., apps-calendar.html, apps-social-feed.html]` | Event calendar, social activity feed with media uploads |

### 7.3 Authentication Library (`auth-*`)
List available layouts for authentication screens (e.g., Centered-Card vs. Split-Screen with visual hero):

| Auth Flow | Centered Card Layout | Split-Screen Hero Layout |
| :--- | :--- | :--- |
| **Sign In** | `[e.g., auth-sign-in.html]` | `[e.g., auth-split-sign-in.html]` |
| **Sign Up / Register** | `[e.g., auth-sign-up.html]` | `[e.g., auth-split-sign-up.html]` |
| **Password Reset** | `[e.g., auth-reset-pass.html]` | `[e.g., auth-split-reset-pass.html]` |
| **Set New Password** | `[e.g., auth-new-pass.html]` | `[e.g., auth-split-new-pass.html]` |
| **Two-Factor Auth (2FA)** | `[e.g., auth-two-factor.html]` | `[e.g., auth-split-two-factor.html]` |
| **Lock Screen** | `[e.g., auth-lock-screen.html]` | `[e.g., auth-split-lock-screen.html]` |
| **Success / Confirm Mail** | `[e.g., auth-success-mail.html]` | `[e.g., auth-split-success-mail.html]` |

### 7.4 UI Component Showcases (`ui-*`)
- Accordions & Collapses: `[e.g., ui-accordions.html]`
- Alerts & Badges: `[e.g., ui-alerts.html, ui-badges.html]`
- Cards & Portlets: `[e.g., ui-cards.html, ui-portlets.html]`
- Modals & Dialogs: `[e.g., ui-modals.html]`
- Tabs & Navs: `[e.g., ui-tabs.html]`
- Notifications & Toasts: `[e.g., ui-notifications.html, ui-toasts.html]`

### 7.5 Forms & Input Controls (`form-*`)
- Standard & Floating Inputs: `[e.g., form-elements.html]`
- Advanced Pickers & Masks: `[e.g., form-advanced.html]`
- Multi-Step Wizard: `[e.g., form-wizard.html]`
- Form Validation States: `[e.g., form-validation.html]`
- Rich Text Editors: `[e.g., form-editors.html (Quill / Summernote / TinyMCE)]`
- File Upload Zones: `[e.g., form-file-uploads.html (Dropzone / FilePond)]`

### 7.6 Data Tables (`tables-*`)
- Basic & Responsive Tables: `[e.g., tables-basic.html]`
- Advanced DataTables: `[e.g., tables-datatables-basic.html, tables-datatables-advanced.html]`
- Export Actions (Excel/PDF/Print): `[e.g., tables-datatables-buttons.html]`

### 7.7 Layout Demonstrators & Utility Pages (`layouts-*`, `pages-*`, `error-*`)
- Horizontal Topnav Layout: `[e.g., layouts-horizontal.html]`
- Boxed / Scrollable Layouts: `[e.g., layouts-boxed.html, layouts-scrollable.html]`
- Pricing Table: `[e.g., pages-pricing.html]`
- Timeline & FAQ: `[e.g., pages-timeline.html, pages-faq.html]`
- Error Pages: `[e.g., error-404.html, error-500.html, error-maintenance.html]`
- Blank Starter: `[e.g., pages-empty.html]`

---

## 8. Bundled Plugins Inventory (`assets/plugins/`)

| Plugin Name | Directory Location | Primary Purpose & Usage Context |
| :--- | :--- | :--- |
| `[e.g., ApexCharts]` | `plugins/apexcharts/` | SVG charting engine |
| `[e.g., Flatpickr]` | `plugins/flatpickr/` | Date & datetime pickers |
| `[e.g., Quill.js]` | `plugins/quill/` | WYSIWYG rich text editor |
| `[e.g., Dropzone]` | `plugins/dropzone/` | Drag-and-drop file uploads |
| `[e.g., SweetAlert2]` | `plugins/sweetalert2/` | Modal dialogs and alerts |
| `[e.g., Choices.js]` | `plugins/choices/` | Searchable select dropdowns |
| `[e.g., FullCalendar]`| `plugins/fullcalendar/`| Event calendars |

---

## 9. Data Visualization & Charting Catalog

- **Primary Chart Engine:** [e.g., ApexCharts / Chart.js / ECharts]
- **Theme Color Binding:** [e.g., `data-colors="chart-primary, chart-secondary"`]
- **Available Chart Types & Controllers:**
  - Area / Line Charts: `[e.g., charts-apex-line.html, charts-apex-area.html]`
  - Bar / Column Charts: `[e.g., charts-apex-bar.html, charts-apex-column.html]`
  - Pie / Donut Charts: `[e.g., charts-apex-pie.html]`
  - Financial / Candlestick: `[e.g., charts-apex-candlestick.html]`
  - Maps (Vector / Leaflet): `[e.g., maps-vector.html, maps-leaflet.html]`

---

## 10. Standard Page Shell & Boilerplate Structure

Provide the exact boilerplate skeleton for screens in this theme:

```html
<!doctype html>
<html lang="en" data-skin="[default-skin]" data-bs-theme="light" data-topbar-color="dark" data-menu-color="light" data-sidenav-size="default" data-layout-width="fluid" data-layout-position="fixed" dir="ltr">
<head>
    <meta charset="utf-8" />
    <title>Page Title | [Theme Name]</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="shortcut icon" href="assets/images/favicon.ico" />

    <!-- 1. Theme Configuration Script (MUST run synchronously in head to prevent FOUC) -->
    <script src="assets/js/config.js"></script>

    <!-- 2. Vendor CSS -->
    <link href="assets/css/vendors.min.css" rel="stylesheet" type="text/css" />

    <!-- 3. Optional Page-Specific Plugin CSS -->
    <!-- <link href="assets/plugins/..." rel="stylesheet" type="text/css" /> -->

    <!-- 4. Master App CSS -->
    <link id="app-style" href="assets/css/app.min.css" rel="stylesheet" type="text/css" />
</head>

<body>
    <div class="wrapper">
        <!-- Topbar Component -->
        <header class="app-topbar">...</header>

        <!-- Sidenav Component -->
        <div class="sidenav-menu">
            <div class="scrollbar" data-simplebar>
                <!-- Native Sidebar Navigation Links -->
            </div>
        </div>

        <!-- Main Content Area -->
        <div class="content-page">
            <div class="container-fluid">
                <!-- Page Title Head & Breadcrumbs -->
                <div class="page-title-head">...</div>

                <!-- Page Content Body Grid -->
                <div class="row">...</div>
            </div>

            <!-- Global Footer -->
            <footer class="footer">...</footer>
        </div>
    </div>

    <!-- Vendor Scripts -->
    <script src="assets/js/vendors.min.js"></script>

    <!-- Core App Controller -->
    <script src="assets/js/app.js"></script>
</body>
</html>
```
