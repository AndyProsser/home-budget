# Design System Guidelines

## Foundation tokens

Define shared design tokens for:

- color roles (surface, text, accent, warning, success)
- spacing scale
- typography scale
- border radius and elevation
- motion timings

Use token names, not hard-coded values, in components.

## Theming

- Support light and dark themes with system default.
- Ensure semantic role parity between themes.
- Validate chart colors for contrast and category distinction.

## Component priorities

Core components to standardize first:

- App shell and navigation
- Data table with inline edit states
- Summary cards and KPI strips
- Checklist rows with status controls
- Form controls for amount, frequency, due pattern
- Progress bars for debt payoff
- Empty-state and error-state callouts

## Data visualization guidance

- Use consistent category color mapping across pages.
- Always label totals and units clearly.
- Avoid animations that obscure numeric interpretation.
- Provide text fallback summaries for charts.

## Interaction consistency

- Status controls use a single interaction pattern across screens.
- Inline edits should keep row context visible.
- Primary actions appear in predictable locations.

## Responsive behavior

Desktop:

- Two-column layouts where helpful for context and detail.

Mobile:

- Single-column priority flow.
- Sticky summary and action bars for long checklists.
- Minimize horizontal scrolling in tables by using stacked row details.
