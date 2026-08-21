# Platform Patterns

Use this file to route substantial new designs. Small edits should preserve the
existing pattern without running a broad mode-selection exercise.

## Intake

Resolve only information that can materially change the design:

- target platform and canonical viewport or canvas size
- audience, role, task, and primary success action
- required content, data fields, states, and permissions
- brand, component library, codebase, and reference assets
- delivery format and implementation target

Ask when a missing answer blocks a correct structure. Otherwise proceed with a
clearly stated, reversible assumption.

## Enterprise admin and SaaS

- Start from business objects, user roles, permissions, data fields, and common
  actions rather than a decorative dashboard template.
- Choose the correct page type: list/table, detail, create/edit form, dashboard,
  settings, or workflow queue.
- Design density, filters, sorting, pagination, selection, bulk actions, and
  destructive confirmations around realistic data volume.
- Include loading, empty, error, and permission states where the workflow needs
  them. Preserve scanability and numeric alignment.

## Responsive web and landing pages

- Establish audience, value proposition, primary CTA, proof, and objection
  handling before choosing sections.
- Give the hero one dominant message and one clear primary action.
- Vary section rhythm and composition; do not produce a sequence of identical
  card grids.
- Design mobile navigation, content order, image crops, CTA persistence, and
  wrapping intentionally instead of shrinking desktop.

## Mobile apps

- Identify iOS, Android, or a shared product language and respect its navigation
  and system conventions.
- Account for safe areas, keyboard behavior, touch targets, bottom navigation,
  sheets, back behavior, and scroll boundaries.
- Keep primary actions reachable and avoid desktop-style information density.
- Represent permission, offline, loading, empty, error, and interrupted-flow
  states when relevant.

## WeChat mini programs

- Respect platform navigation, tab-bar, title-bar, authorization, sharing, and
  webview limitations rather than treating the surface as a generic mobile web
  page.
- Minimize deep navigation and heavy interaction; keep frequent actions close to
  the current context.
- Verify Chinese copy length, platform-native controls, loading feedback, and
  network degradation.

## Data visualization and big screens

- Begin with the monitoring question, decision cadence, viewing distance, data
  freshness, thresholds, and alert priority.
- Use chart types that match the comparison: trend, composition, distribution,
  relationship, geography, or status.
- Provide units, time range, source, last-updated state, legends, and empty/error
  behavior. Decorative charts without a decision purpose should be removed.
- Design color scales for contrast and color-vision deficiency; do not encode
  status with red/green alone.

## E-commerce and campaigns

- Establish product, audience, channel, offer, proof, required legal copy, and
  conversion action.
- Order content by selling logic: value, evidence, product detail, comparison,
  offer, trust, and action.
- Preserve real product imagery and brand assets. Do not fabricate claims,
  discounts, reviews, certifications, or scarcity.
- Adapt product cards, price treatment, promotions, sticky actions, and image
  density to the channel rather than reusing a generic landing page.

## Design systems

- Separate foundations, variables, primitives, components, patterns, states,
  examples, and deprecated material with real hierarchy.
- Define component anatomy, slots, valid variants, default state, responsive
  behavior, and accessibility expectations.
- Build gallery instances that exercise states and content extremes. A reusable
  master without a verified instance is difficult to trust.
- Keep live `.pen` variables and components authoritative unless the project
  explicitly defines code as the source of truth.

## Cross-platform families

- Share product concepts, vocabulary, brand tokens, and component intent while
  preserving platform-specific navigation and interaction conventions.
- Document which elements are invariant, adapted, or platform-only.
- Do not force pixel-identical layouts across desktop, web, mobile, and mini
  program surfaces.

## Presentation decks

- Establish audience, decision goal, story arc, slide sequence, per-slide
  takeaway, visual system, and export constraints before drawing.
- Use one communicative idea per slide; charts and diagrams must support that
  idea rather than decorate it.
- Keep slides as named, ordered top-level frames and components in separate
  groups so exports do not mix them.
- Verify first, last, and visually dense slides, then validate PDF page count and
  ordering. Use [Export and handoff](export-handoff.md) for delivery.
