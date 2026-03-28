---
name: reddit
description: Fetches and synthesizes Reddit community sentiment for a tech product review. Use this skill whenever a Reddit source rating is needed — whether for a standalone lookup or as part of a multi-source reviewer workflow. Triggers on any request to check Reddit opinions, find what Reddit thinks about a product, or pull community sentiment from Reddit threads. ALWAYS use this skill when the reviewer rubric requires a Reddit source rating, even if the user only names the product.
---

# Reddit subagent

Your job is to find, read, and synthesize Reddit community sentiment for a given product. This feeds the **Source Ratings** section of the product review rubric.

## Trigger

This subagent is triggered whenever the reviewer rubric requires a Reddit source rating. Even if the user only names the product, you should automatically fetch the Reddit sentiment as part of the review process.

## Workflow

1. **Search** using: `site:reddit.com "[product name]" review`
   - Also try: `reddit "[product name]" worth it` or `reddit "[product name]" impressions`
2. **Find 2–3 relevant threads** — prefer:
   - Threads from the past 1–2 years (more recent = more relevant)
   - Subreddits like r/headphones, r/audiophile, r/hometheater, r/Monitors, r/laptops, r/Android, r/apple, r/photography, r/buildapc, or product-specific subs
   - Threads with 20+ comments (higher signal)
3. **Read the top comments** in each thread — look for patterns:
   - What do most people agree on (positive or negative)?
   - Are there recurring complaints or recurring praises?
   - Are there notable dissenting voices?

## Rules

- The 1-sentence sentiment summary must reflect the **dominant pattern**, not an outlier
- If sentiment is split (e.g., audiophiles love it, casual users hate it), capture the split: "Split — enthusiasts praise X but mainstream users criticize Y"
- Do NOT paraphrase quotes. Pull the actual words from a real, highly-upvoted comment
- If you can't find a quote worth using, omit the "Notable quote" line rather than paraphrasing
- Sentiment should be grounded in specifics (e.g., "battery life" or "driver support"), not vague (e.g., "people seem happy")

## Edge cases

- If the product is too new and has minimal Reddit discussion (<3 threads with <10 comments each), mark `[NO DATA] — insufficient Reddit discussion`
- If Reddit threads exist but are all promotional or bot-heavy, mark `[NO DATA] — no credible organic discussion found`
- Do not fabricate or reconstruct quotes — only use exact text from the page

## Output

```
**Reddit**
Sentiment: [1-sentence summary drawn from dominant comment patterns]
Notable quote: "[exact quote from a highly-upvoted comment]"
Sources: [URL1], [URL2]
```

If no data is found: `Sentiment: [NO DATA]`