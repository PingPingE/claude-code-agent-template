---
name: ux-check
description: Quick UX evaluation — heuristics, accessibility, and mobile review
user-invocable: true
context: fork
agent: ux-designer
allowed-tools:
  - Read
  - Grep
  - Glob
---

# UX Check

Run a UX evaluation on the specified target.

**Target:** $ARGUMENTS

## Instructions

### 1. Scope the Review

Read the target files and identify:
- What type of content (component, page, flow, full app)
- What user actions are involved
- What devices/viewports are relevant

### 2. Heuristic Evaluation

Check against Nielsen's 10 usability heuristics:

1. **Visibility of system status** — Loading states, feedback, progress indicators
2. **Match between system and real world** — Language, concepts, information order
3. **User control and freedom** — Undo, cancel, back navigation
4. **Consistency and standards** — Button styles, terminology, platform conventions
5. **Error prevention** — Confirmation dialogs, validation, disabled states
6. **Recognition rather than recall** — Visible options, autocomplete, recent items
7. **Flexibility and efficiency** — Shortcuts, defaults, customization
8. **Aesthetic and minimalist design** — Content hierarchy, unnecessary elements
9. **Help users recover from errors** — Clear messages, inline validation, recovery path
10. **Help and documentation** — Tooltips, onboarding, searchable docs

### 3. Accessibility Audit

Check WCAG 2.1 AA essentials:
- Color contrast (4.5:1 for text, 3:1 for large text/UI)
- Touch targets (44x44px minimum)
- Keyboard navigation (tab order, focus indicators, skip link)
- Semantic HTML (headings, labels, alt text, ARIA)
- Screen reader compatibility

### 4. Mobile Review

Check responsive design:
- No horizontal scroll on mobile (320px+)
- Touch-friendly target sizes
- Primary actions in thumb-friendly zones
- Text readable without zooming (16px+ body)

## Output Format

```markdown
## UX Review: [Target Name]

### Summary
[2-3 sentences: overall UX quality, key issues, top recommendation]

### Findings

| # | Issue | Category | Severity | Recommendation |
|---|-------|----------|----------|----------------|
| 1 | ... | Heuristic: Consistency | P1 | ... |
| 2 | ... | WCAG 1.4.3: Contrast | P0 | ... |

### Accessibility Checklist
- [x/] Color contrast meets AA
- [x/] Touch targets adequate
- [x/] Keyboard navigation works
- [x/] Focus indicators visible
- [x/] Images have alt text
- [x/] Form labels present

### Quick Wins
[Top 3 low-effort, high-impact fixes]
```

## Completion Criteria

- All 10 heuristics checked
- WCAG AA essentials audited
- Mobile responsiveness reviewed
- Findings prioritized by severity
- Specific recommendations with file paths
