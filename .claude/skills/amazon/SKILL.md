---
name: amazon
description: Fetches and extracts Amazon customer ratings for a tech product review. Use this skill whenever an Amazon source rating is needed — whether for a standalone lookup or as part of a multi-source reviewer workflow. Triggers on any request to check Amazon reviews, get Amazon star ratings, or pull customer feedback from Amazon. ALWAYS use this skill when the reviewer rubric requires an Amazon source rating, even if the user only names the product.
---

# Amazon subagent

Your job is to find and return the Amazon customer rating for a given product. This feeds the **Source Ratings** section of the product review rubric.

## Trigger

This subagent is triggered whenever the reviewer rubric requires an Amazon source rating. Even if the user only names the product, you should automatically fetch the Amazon rating as part of the review process.

## Workflow

1. **Search** using: `site:amazon.com "[product name]"`
   - If that returns nothing, try: `amazon.com "[product name]" [brand]`
2. **Identify the correct listing** — prefer the listing sold directly by the brand or fulfilled by Amazon. Avoid third-party resellers, bundles, or accessories
3. **Fetch the product page** — extract:
   - The star rating (e.g., `4.6 out of 5 stars`)
   - The total number of global ratings (e.g., `2,847 global ratings`)

## Rules

- Use the exact star rating shown (e.g., `4.6 / 5`, not `4.5` or `5`)
- Use the total rating count, not the number of written reviews
- If multiple Amazon listings exist for the same product (different colors, variants), use the highest-volume listing (most ratings), as it's most representative

## Edge cases

- If the product has fewer than 50 ratings, note: `Rating: X.X / 5 (based on X ratings — low sample size)`
- If the page is inaccessible or blocked, mark `[NO DATA] — page inaccessible`
- If the search yields no Amazon listing at all, mark `[NO DATA] — not listed on Amazon`
- Never guess or round the rating — report only what the page explicitly shows

## Output

```
**Amazon**
Rating: X.X / 5 (based on X,XXX ratings)
Source: [URL]
```

If no data is found: `Rating: [NO DATA]`