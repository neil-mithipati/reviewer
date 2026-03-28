---
name: rtings
description: Fetches and extracts RTINGS.com scores for a tech product review. Use this skill whenever an RTINGS source rating is needed — whether for a standalone lookup or as part of a multi-source reviewer workflow. Triggers on any request to check RTINGS, find a product's RTINGS score, or pull ratings from rtings.com. ALWAYS use this skill when the reviewer rubric requires an RTINGS source rating, even if the user only names the product.
---

# RTINGS subagent

Your job is to find and return the RTINGS overall score for a given product. This feeds the **Source Ratings** section of the product review rubric.

## Trigger

This subagent is triggered whenever the reviewer rubric requires an RTINGS source rating. Even if the user only names the product, you should automatically fetch the RTINGS score as part of the review process.

## Workflow

1. **Search** using: `site:rtings.com "[product name]" review`
   - If that returns nothing, try: `rtings.com "[product name]"`
2. **Fetch the top result** — should be the product's dedicated review page (URL pattern: `rtings.com/[category]/reviews/[brand]/[model]`)
3. **Find the overall score** — RTINGS displays a prominent overall score out of 10 near the top of the page. It's labeled "Overall Score" or shown as a large number
4. **Record the score** exactly as shown (e.g., `8.4 / 10`)

## Edge cases

- RTINGS covers specific categories: TVs, monitors, headphones, soundbars, speakers, laptops, phones. If the product is outside these categories, mark `[NO DATA] — category not covered by RTINGS`
- If the page loads but no overall score is visible (e.g., test in progress), mark `[NO DATA] — review incomplete`
- If the page is inaccessible, mark `[NO DATA] — page inaccessible`
- Never estimate or interpolate a score — only report what the page explicitly states

## Output

```
**RTINGS**
Score: X.X / 10
Source: [URL]
```

If no data is found: `Score: [NO DATA]`