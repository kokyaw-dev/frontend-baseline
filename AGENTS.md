# AGENTS.md — AI Agent Directives for KDS Components Library

This document outlines the architecture, engineering rules, component design guidelines, and code standards for AI autonomous agents tasked with building or extending the **Kolinn Design System (KDS)** component library.

---

## 1. Project Overview & Scope

The **KDS Components Library** provides dual implementation layers:
1. **Framework-agnostic Core**: Pure CSS (using custom properties/design tokens) and lightweight Vanilla JavaScript modules.
2. **Astro Integration Layer**: Native `.astro` components wrapping the core CSS/JS logic for static-first, highly-optimized web applications.

All code must strictly adhere to the `DESIGN.md` specification (**Version: kds_v1.2, Status: alpha**).

---

## 2. Global Guidelines & Design Tokens Adherence

### 2.1 CSS Custom Properties (Tokens)
- **Do NOT hardcode colors, dimensions, transitions, or radii.** Always use KDS design tokens defined in `DESIGN.md`.
- **Theme Parity**: Ensure every component works seamlessly across Light, Dark, and Auto mode (`data-theme="light|dark|auto"`).
- **Key Tokens Reference**:
  - Borders: `var(--kds-border-default)` (Uses 72% opacity translucency `--kds-polar-3`).
  - Dropdowns & Flyouts: Background `var(--kds-dropdown-bg)` (Opaque surface).
  - Transition Easing: `var(--kds-transition-timing)` (`cubic-bezier(0.16, 1, 0.3, 1)`).
  - Max Container Widths: Global `var(--kds-container-max)` (`90rem`), Content `var(--kds-content-max)` (`80rem`).

### 2.2 Mathematical & Scientific Notation Rules
- All inline math notation in documentation or component specs must use `$...$`.
- Display formulas must use `$$...$$`.
- **Never** use Unicode characters for mathematical symbols or equations.

---

## 3. Engineering & Behavioral Directives for Agents

### 3.1 Accessibility (`a11y`) First
1. **Focus Rings**:
   - Apply explicit focus rings during `:focus-visible` using `var(--kds-frost-2)`.
   - Syntax: `outline: 2px solid var(--kds-frost-2); outline-offset: 2px;`
2. **Focus Containment & `inert`**:
   - Hidden overlays, drawers, and closed mega menu panels **MUST** set the HTML `inert` attribute alongside `aria-hidden="true"`.
3. **Touch Targets**:
   - All interactive triggers must have a minimum hit area of `44px x 44px` on mobile/touch contexts.
4. **No Auto-Focus**:
   - Never auto-focus inputs or triggers upon opening drawers/flyouts to avoid forced keypress overrides or mobile keyboard jumps.

### 3.2 Motion Comfort (`prefers-reduced-motion`)
All animated CSS/JS components must respect user motion preferences:
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

---

## 4. Component Development Guidelines

### 4.1 Vanilla CSS/JS Components
- Components must be zero-dependency, standard ES Modules (`.js`).
- State modifications must leverage HTML attributes (`data-state="open|closed"`, `aria-expanded="true|false"`, `inert`).

#### Example: Button Component (`src/styles/components/button.css`)
```css
.kds-btn {
  font-family: var(--kds-font-mono);
  font-size: var(--kds-small-14);
  font-weight: 600;
  line-height: 1;
  padding: 11px var(--kds-space-2);
  border-radius: var(--kds-radius-sm);
  border: 1px solid transparent;
  cursor: pointer;
  transition: background-color var(--kds-transition-fast) var(--kds-transition-timing),
              border-color var(--kds-transition-fast) var(--kds-transition-timing);
}

.kds-btn--primary {
  background-color: var(--kds-action-primary);
  color: var(--kds-on-action);
}

.kds-btn--primary:hover {
  background-color: var(--kds-action-hover);
}

.kds-btn--primary:active {
  background-color: var(--kds-action-active);
}

.kds-btn:focus-visible {
  outline: 2px solid var(--kds-frost-2);
  outline-offset: 2px;
}
```

---

### 4.2 Astro Component Guidelines
- Use 'pnpm' as default package manager. Use pnpm version 12+ and node.js version 24 or higher. 
- Astro components must import their relative CSS module or share global token references.
- Always expose standard HTML attributes via `Astro.props`.
- Slot usage must support default and named content structures.

#### Example: Astro Button (`src/components/astro/KdsButton.astro`)
```astro
---
interface Props extends Astro.HTMLAttributes<'button'> {
  variant?: 'primary' | 'icon';
  type?: 'button' | 'submit' | 'reset';
  class?: string;
}

const { 
  variant = 'primary', 
  type = 'button', 
  class: className = '', 
  ...rest 
} = Astro.props;
---

<button 
  type={type} 
  class:list={['kds-btn', `kds-btn--${variant}`, className]} 
  {...rest}
>
  <slot />
</button>

<style>
  /* Local component extensions if necessary */
</style>
```

#### Example: Astro Search Input (`src/components/astro/KdsInput.astro`)
```astro
---
interface Props extends Astro.HTMLAttributes<'input'> {
  label?: string;
  error?: string;
}

const { label, error, id, class: className = '', ...rest } = Astro.props;
---

<div class="kds-input-wrapper">
  {label && <label for={id} class="kds-input-label">{label}</label>}
  <div class:list={['kds-input-container', { 'has-error': Boolean(error) }]}>
    <span class="kds-prompt-symbol" aria-hidden="true">$</span>
    <input 
      id={id} 
      class:list={['kds-input', className]} 
      {...rest} 
    />
  </div>
  {error && <span class="kds-input-error-msg">{error}</span>}
</div>

<style>
  .kds-input-container {
    display: flex;
    align-items: center;
    background-color: var(--kds-surface-raised);
    border: 1px solid var(--kds-border-default);
    border-radius: var(--kds-radius-md);
    padding: 0 var(--kds-space-2);
  }
  .kds-input-container:focus-within {
    border-color: var(--kds-frost-2);
    box-shadow: 0 0 0 4px color-mix(in srgb, var(--kds-frost-2) 22%, transparent);
  }
  .kds-input-container.has-error {
    border-color: var(--kds-status-error);
    box-shadow: 0 0 0 4px color-mix(in srgb, var(--kds-status-error) 18%, transparent);
  }
  .kds-prompt-symbol {
    font-family: var(--kds-font-mono);
    color: var(--kds-action-primary);
    margin-right: var(--kds-space-1);
  }
  .kds-input {
    background: transparent;
    border: none;
    color: var(--kds-text-primary);
    font-family: var(--kds-font-mono);
    font-size: max(1rem, 16px);
    width: 100%;
    outline: none;
    padding: var(--kds-space-1) 0;
  }
</style>
```

---

## 5. Verification & Quality Assurance Checklist

Agents generating code must self-verify deliverables against this checklist:

- [ ] **Token Integrity**: Are all colors, margins, fonts, and radii referencing `var(--kds-*)` variables?
- [ ] **Design Spec v1.2 Verification**:
  - [ ] Default border opacity is 72% (`rgba(..., 0.72)`).
  - [ ] Dropdowns use opaque backgrounds (`#e5e9f0` / `#3b4252`).
  - [ ] Transitions use `cubic-bezier(0.16, 1, 0.3, 1)`.
  - [ ] Container widths enforce `90rem` (Global) and `80rem` (Content).
- [ ] **Accessibility**: Is `inert` applied when elements are hidden? Are focus rings visible on keyboard nav? Is touch target height $\ge 44\text{px}$?
- [ ] **Zero Hardcoded CSS Values**: No standalone hex values or static px radii.
- [ ] **Clean Modular Structure**: Separated CSS styles, JS handlers, and Astro views.
