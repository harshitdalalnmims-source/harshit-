# Baseline — Developer Handoff

**Version 1.0.0 · the contract between design and engineering**

This document tells a developer how to read the Baseline Figma file, what the tokens mean, and how a Figma component turns into a coded one. If the Figma file and this document ever disagree, `tokens.json` is the source of truth — it's the only file both design tooling and code build from.

---

## 1. Mental model: everything is a token

A token is a named decision made once. There are three tiers, and the rule that makes the whole system work is that **you only reference the tier above you, never below.**

```
PRIMITIVE            SEMANTIC                 COMPONENT
raw value       →    carries intent      →    component-specific
neutral-900          text-primary             button-height-md
iris-600             action-primary           input-radius
#4F52D6              border-focus             (only where semantic
                                               can't express it)
```

- **Primitives** are the palette and raw scales: `neutral-900`, `iris-600`, `space-5`, `radius-md`. They have no meaning. **Application code and components must never reference a primitive directly.**
- **Semantic tokens** are what you actually use: `text-primary`, `action-primary`, `border-focus`, `feedback-danger-bg`. They say *why*, not *what*. Reskinning a client project means remapping semantic tokens, which is why components must not skip past them to a hex or a primitive.
- **Component tokens** exist only where a component needs a value the semantic layer doesn't express, e.g. `button-height-md: 40`. Keep this tier small; most components need none.

**The one rule to enforce in code review:** no raw hex values, no primitive references, and no off-scale pixel numbers inside a component. If you're reaching for `#4F52D6` or `13px`, a token is missing — raise it, don't inline it.

---

## 2. Naming convention

All token names are lowercase, hyphen-delimited, and read **category → concept → variant → state**, most general to most specific:

```
category   concept    variant   state
--------   -------    -------   -----
action  -  primary            -  hover     →  action-primary-hover
text    -  secondary                       →  text-secondary
feedback - danger   -  bg                   →  feedback-danger-bg
border  -  focus                            →  border-focus
space              -  5                     →  space-5
```

Rules:
- **State is always a suffix**, never a prefix: `action-primary-hover`, not `hover-action-primary`. This keeps every state of one token adjacent when sorted.
- **Spacing and radius are numeric steps**, not t-shirt sizes for spacing (`space-5`) but named for radius (`radius-md`) — spacing has too many steps for names to stay memorable; radius has five, so names read better.
- **Feedback families always ship the same four parts**: `-fg`, `-bg`, `-border`, `-solid`. Don't invent a `-text` on one and `-fg` on another.
- Figma layer/style names use `/` where token names use `-`, because Figma groups styles by slash. `action/primary/hover` in Figma **is** `action-primary-hover` in code. The build step does that swap; treat them as the same name.

---

## 3. Token structure in the file

`tokens.json` is grouped by tier. Semantic entries include a `value` (the primitive they resolve to, written `{iris.600}`) and a `use` string describing when to reach for them. Your build tooling (Style Dictionary, Tokens Studio, or the framework's own theming) resolves `{iris.600}` → `#4F52D6` at build time and emits whatever your stack needs: CSS custom properties, a JS/TS theme object, SCSS variables.

The reference page (`baseline-design-system.html`) is that resolution made visible: the colour section shows each semantic token with an arrow to the primitive it points at. Read it as the human-facing view of the JSON.

**Consuming tokens in code** (CSS-variable output shown; adapt to your stack):

```css
/* generated from tokens.json — do not edit by hand */
.button--primary { background: var(--action-primary); color: var(--action-primary-fg); }
.button--primary:hover { background: var(--action-primary-hover); }
.card { box-shadow: var(--elev-1); border-radius: var(--radius-lg); padding: var(--space-5); }
```

You never type a hex. You type intent, and the token resolves.

---

## 4. How to read the Figma file

The file is organised so its structure matches the code structure one-to-one:

- **Page: Foundations** — one frame each for Type, Colour, Spacing, Radius, Elevation. These are published as Figma **variables and styles**, so they show up in the asset panel of every client project that pulls Baseline as a team library. Designers pick a style; they don't eyedrop a colour.
- **Page: Components** — one component set per component (Button, Input, etc.). Each set uses **variant properties** that map exactly to the props you'll code.

**Variant properties → your props.** A Figma Button with properties `Variant = Primary | Secondary | Ghost | Danger`, `Size = sm | md | lg`, `State = Default | Hover | Focus | Disabled | Loading` becomes:

```tsx
type ButtonProps = {
  variant?: 'primary' | 'secondary' | 'ghost' | 'danger';  // Figma "Variant"
  size?: 'sm' | 'md' | 'lg';                                 // Figma "Size"
  loading?: boolean;                                         // Figma "State = Loading"
  disabled?: boolean;                                        // Figma "State = Disabled"
  // hover / focus are interaction states, not props — handled by CSS :hover / :focus-visible
};
```

Read it this way:
- **Enum variant properties → union-type props** (`variant`, `size`).
- **Boolean variant properties → boolean props** (`loading`, `disabled`, `error`).
- **Interaction states (hover, focus, active) are never props.** Figma draws them as separate variants so designers can see them; in code they're pseudo-classes (`:hover`, `:focus-visible`, `:active`). Don't create a `hover` prop.
- **Auto Layout → padding, gap, and direction.** A frame's Auto Layout spacing is a `space-*` token; reproduce it with the matching padding/gap token, not a hand-measured pixel value. If you measure `13px` off a frame, it's a mistake in the file — flag it, because everything should be on the 4px grid.
- **Slots.** Where a component has an instance-swap or text property (icon, label, avatar), that's a `children`/slot in code.

**Finding a token from a Figma layer:** select the layer, read its applied style or variable name, swap `/` for `-`, and that's the token to reference. `text/primary` → `var(--text-primary)`.

---

## 5. States every interactive component must implement

This is the accessibility and completeness floor. A component isn't done until all of these exist, whether or not the mock shows them:

| State | Trigger | Requirement |
|---|---|---|
| Default | resting | on-scale spacing, semantic colours only |
| Hover | pointer over | pointer devices only, never the sole signal |
| Focus | keyboard / tab | **visible ring via `border-focus`; never remove the outline** |
| Active | pressed | brief, uses the `-active` token |
| Disabled | `disabled` | `bg-muted` + `text-disabled`, `cursor:not-allowed`, not focusable |
| Error | invalid input | `feedback-danger` border + message, not colour alone |
| Loading | async | spinner, control stays same width, action blocked |

Focus is non-negotiable: the focus ring is a token (`border-focus`) precisely so nobody "cleans up" the outline. Colour is never the only carrier of meaning — errors get an icon or text, statuses get a label, not just a hue.

---

## 6. Versioning and change

- Baseline is **semver'd**. Token renames or removals are **major**; new tokens/components are **minor**; value tweaks that don't rename anything are **patch**.
- Tokens are **deprecated before deleted**: a removed token stays in the file for one minor version with a `deprecated` note pointing to its replacement, so projects have a window to migrate.
- Components ship a short changelog entry. If a prop changes name, the old prop warns for one version.

---

## 7. Quick checklist before you ship a screen

- [ ] Every colour is a semantic token — no hex, no primitives.
- [ ] Every space/pad/gap is a `space-*` step — nothing off the 4px grid.
- [ ] Type uses a named role, not a custom size.
- [ ] Every interactive element has a visible focus state.
- [ ] Disabled, error and loading states exist where relevant.
- [ ] Component props match the Figma variant properties by name.
