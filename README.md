# KDS Components Library

> A terminal-interface inspired design system engineered for rich interactivity, high-density legibility, strict accessibility, and deterministic state transitions.

Version: `kds_v1.2` (Status: `alpha`)

---

## 1. Project Overview

The **KDS Components Library** provides a dual-layer implementation for building fast, accessible, and theme-aware web applications:

1. **Core Layer (Framework-Agnostic)**: Zero-dependency CSS custom properties (design tokens) and lightweight Vanilla JS modules.
2. **Astro Integration Layer**: Native `.astro` components designed for zero-JS-by-default or lightweight client hydrations.

---

## 2. Prerequisites

- **Node.js**: `^22.0.0` or `>=24.0.0`
- **Package Manager**: [`pnpm`](https://pnpm.io/) (`v9.0.0` or higher recommended)

---

## 3. Getting Started

### Installation

Clone the repository and install dependencies using `pnpm`:

```bash
# Install project dependencies
pnpm install
```

### Local Development

Start the development server to preview components and test interface interactions:

```bash
# Launch Astro dev server
pnpm dev
```

Build the library and project deliverables for production:

```bash
# Typecheck & build static site / outputs
pnpm build

# Preview production build locally
pnpm preview
```

---

## 4. Quick Start Usage

### Vanilla CSS & JS Integration

Include the core tokens and component styles in your application entry point:

```html
<!DOCTYPE html>
<html lang="en" data-theme="auto">
<head>
  <meta charset="UTF-8" />
  <title>KDS Terminal Interface</title>
  <link rel="stylesheet" href="/node_modules/kds-components-library/src/styles/tokens.css" />
  <link rel="stylesheet" href="/node_modules/kds-components-library/src/styles/components/button.css" />
</head>
<body>
  <button class="kds-btn kds-btn--primary">Execute Command</button>
  <script type="module" src="/node_modules/kds-components-library/src/scripts/kds-core.js"></script>
</body>
</html>
```

### Astro Component Usage

Import KDS components directly into your `.astro` pages or components:

```astro
---
import KdsButton from '../components/astro/KdsButton.astro';
import KdsInput from '../components/astro/KdsInput.astro';
import '../styles/tokens.css';
---

<html lang="en" data-theme="auto">
  <head>
    <title>KDS App</title>
  </head>
  <body>
    <main class="kds-container">
      <KdsInput id="search" label="Command Query" placeholder="type a command..." />
      <KdsButton variant="primary" type="submit">
        Run Action
      </KdsButton>
    </main>
  </body>
</html>
```

---

## 5. Theme Control

Themes are managed at runtime via the `data-theme` attribute on the root `<html>` element:

```html
<!-- Forced Dark Mode -->
<html data-theme="dark">

<!-- Forced Light Mode -->
<html data-theme="light">

<!-- Automatic System Synchronization -->
<html data-theme="auto">
```

---

## 6. Repository Architecture

```
kds-components-library/
├── src/
│   ├── styles/
│   │   ├── tokens.css        # Core design tokens (Primitives, Semantics, Overlays)
│   │   ├── utilities.css     # Layout containers, typography, motion utils
│   │   └── components/       # Component-specific CSS modules
│   ├── scripts/
│   │   ├── kds-core.js       # Core initialization & runtime state
│   │   └── components/       # Vanilla JavaScript behaviors
│   └── components/astro/     # Native Astro components (.astro)
├── AGENTS.md                 # Directives & guidelines for AI autonomous agents
├── DESIGN.md                 # Canonical Design Specifications (v1.2)
├── package.json
└── README.md
```

---

## 7. Documentation & Agent Guidelines

- Refer to **`DESIGN.md`** for full specification on design tokens, accessibility rules, color primitive values, and motion timing curves.
- AI Agents working on this project must consult **`AGENTS.md`** for coding standards, accessibility directives, and self-verification checklists.

---

```text
Author: Kolinn
Author URL: https://kolinn.dev/
Version: kds_v1.2
Status: alpha
```
