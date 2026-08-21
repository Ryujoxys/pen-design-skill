# States and Accessibility

Apply this reference to interactive components, forms, multi-screen flows, and
production handoff. Scope the matrix to real requirements; a static concept does
not need every possible state.

## Component states

Consider the states relevant to each control:

- default, hover, pressed, focus-visible
- disabled and read-only
- loading or pending
- validation success, warning, and error
- selected, expanded, checked, or active

Keep state variants connected to the same component system. Do not save the
reusable master itself in a non-default state. When state differences affect
behavior rather than appearance, capture the intent in `context` or grounded
`metadata`.

## Screen states

For data-dependent screens, determine whether the scope needs:

- initial loading and background refresh
- empty-first-use and empty-after-filtering
- partial failure and full-page failure
- offline, reconnecting, and stale-data states
- permission denied or gated content
- confirmation, undo, and optimistic-update recovery

Empty states should explain what the area is for and offer the appropriate next
action. Error states should say what happened, why when known, and how to recover.
Do not represent loading with a blank frame or error with color alone.

For flows, make validation timing, modal/page/sheet choice, back behavior, and
preserved input explicit. Screenshot the highest-risk state, not only the happy
path.

## Baseline accessibility

- Preserve a logical reading order that can also become DOM and keyboard focus
  order.
- Provide a visible focus treatment for keyboard-operable controls.
- Do not use color as the only carrier of status or meaning; pair it with text,
  iconography, shape, or pattern.
- Aim for WCAG AA contrast: 4.5:1 for normal text and 3:1 for large text and
  meaningful UI graphics, unless the project's governing standard is stricter.
- Use practical touch targets, commonly at least 44 by 44 CSS pixels, while
  respecting the target platform's own guidance.
- Ensure icon-only controls have an accessible name in implementation intent.
- Let text containers tolerate localization and text scaling; avoid fixed-height
  text boxes unless truncation is deliberate.
- Check RTL-sensitive order, icon direction, alignment, and spacing when the
  product supports RTL languages.
- Respect reduced-motion, increased-contrast, and similar preferences in the
  implementation handoff when relevant.

## Content quality

- Use specific action labels such as `Save changes` or `Send invite` instead of
  ambiguous labels such as `OK` or `Submit` when the action is known.
- Use plausible content lengths and data shapes; placeholder copy can conceal
  wrapping, density, and localization defects.
- Keep units with their values and use tabular numerals for aligned numeric data
  when the target typeface supports them.
- Do not invent policy, legal, financial, or product claims to make a mockup look
  complete.

## Verification

For each required state:

1. Inspect the component or screen hierarchy.
2. Query the subtree for `c.problems`.
3. Screenshot the state and assess focus visibility, contrast, target size,
   reading order, wrapping, and status redundancy.
4. Compare related states for layout stability and component reuse.
5. Record anything intentionally deferred in the handoff.
