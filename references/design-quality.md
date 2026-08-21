# Design Quality

## Structure

- Every visual section must be a real child of its frame or group.
- Use frames for layout and sizing; groups for logical grouping only.
- Use `layout: "vertical"|"horizontal"` and `gap` rather than manual spacing.
- Use `fill_container` and `fit_content` where the parent layout determines
  size.
- Do not set `x`/`y` on auto-layout children unless `layoutPosition` is
  `absolute`.
- Use `padding: number|[vertical,horizontal]|[top,right,bottom,left]`.

## Reuse

Before creating an element:

```javascript
Get(n => n.reusable && Print(n.id, n.name, n.type))
Print(GetVariables())
```

Reuse matching components as `ref` instances. Override descendants through
instance paths. Copy existing logos, icons, and image assets; generate a new
asset only when no suitable source exists.

## Tokens

Use `$variable-name` references in supported properties. Read variables before
calling `SetVariables`. Merge by default; replace the entire token set only when
the user explicitly requests it.

## Overflow and layout

- Give wrapping text a constrained width, usually `fill_container` inside a
  layout frame.
- Ensure the parent layout and height can contain its children.
- Use `c.bounds` for measured geometry and `c.problems` for clipping.
- Treat intentional overlap separately from accidental overlap; `c.problems`
  currently focuses on clipping, so screenshots remain mandatory.

## Responsive variants

Represent important breakpoints as separate top-level screen frames when the
user needs explicit responsive designs. Preserve shared components and tokens
across variants. Compare hierarchy, visibility, layout direction, type scale,
padding, and image treatment rather than merely scaling the desktop frame.

## Visual direction

- Decide whether the design is primarily a brand surface or a product surface.
  Brand work can use expressive composition and broader rhythm; product work
  should prioritize information hierarchy, task clarity, and controlled density.
- Choose a clear composition, type system, color hierarchy, and spacing rhythm.
- Avoid default-looking typography and interchangeable card grids.
- Reuse the file's established design system when one exists.
- Use a few purposeful visual or motion ideas rather than decorative noise.
- Establish a first, second, and third focal point. Decorative elements should
  not outrank the primary task or message.
- Use one coherent neutral family and a restrained accent strategy unless the
  brand system establishes otherwise.
- Balance headings to avoid weak one-word final lines and keep prose at a
  comfortable reading width.

## Precision and content

- Use semantic, plausible copy and data lengths so wrapping and density are
  exercised honestly.
- Prefer action-specific labels over generic `OK`, `Continue`, or `Submit` when
  the actual action is known.
- Treat nested radii, icon alignment, image crop, and numeric alignment as
  optical decisions, not only geometric ones.
- Avoid pure decoration that carries neither information nor atmosphere.
- Preserve established brand fonts and assets; do not replace them merely to
  satisfy a generic aesthetic rule.

## Verification loop

For each coherent section:

1. Query the subtree for `c.problems`.
2. Call `TakeScreenshot([sectionId])`.
3. Inspect hierarchy, clipping, alignment, spacing, text, contrast, assets, and
   completeness.
4. Fix issues and repeat.

Finish with a screenshot of the complete screen or smallest meaningful final
root.

Before sign-off, ask:

1. Could this design belong to any unrelated product?
2. Does the eye move through content in the intended priority?
3. What element is decorative without contributing meaning or atmosphere?
4. What single grounded change would most improve distinctiveness or clarity?

Make the supported correction rather than reporting it as a future suggestion.
After several unsuccessful visual iterations, stop compounding changes and ask
for direction or a stronger reference.
