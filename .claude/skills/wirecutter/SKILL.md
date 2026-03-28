---
name: wirecutter
description: Fetches and extracts Wirecutter pick status for a tech product review. Use this skill whenever a Wirecutter source rating is needed — whether querying for a single product review or as part of a multi-source reviewer workflow. Triggers on any request to check Wirecutter, look up NYT Wirecutter recommendations, or find out if a product is a Wirecutter pick. ALWAYS use this skill when the reviewer rubric requires a Wirecutter source rating, even if the user only names the product.
---

# Wirecutter subagent

Your job is to find and return the Wirecutter pick status for a given product. This feeds the **Source Ratings** section of the product review rubric.

## Trigger

This subagent is triggered whenever the reviewer rubric requires a Wirecutter source rating. Even if the user only names the product, you should automatically fetch the Wirecutter pick status as part of the review process.

## Workflow

1. **Search** using: `site:nytimes.com/wirecutter "[product name]"`
   - If that returns nothing, try: `wirecutter "[product name]" best recommendation`
2. **Fetch the top result** — look for the category guide page (e.g., "Best Headphones"), not a news article
3. **Scan for the product** — find the section where the product is listed. Look for pick labels like "Our Pick", "Upgrade Pick", "Budget Pick", "Also Great"
4. **Extract the pick status and a brief rationale** — use the exact label Wirecutter uses. Pull 1 sentence of reasoning if present (e.g., why it earned that pick)

## Edge cases

- If the product appears in a list but has no explicit pick label, mark `[NO DATA]`
- If multiple Wirecutter pages mention the product, prefer the one where it's the primary pick
- If the page is paywalled or inaccessible, mark `[NO DATA] — page inaccessible`
- Never infer or guess the pick status — only report what the page explicitly says

## Output

A single structured result:

```
Wirecutter: [Pick Status] — [1-sentence reason if available]
Source: [URL]
```

Pick status must be one of:
- `Our Pick`
- `Upgrade Pick`
- `Budget Pick`
- `Also Great`
- `[NO DATA]` — if Wirecutter has not reviewed this product or the page is inaccessible