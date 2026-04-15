---
name: blog-post-from-keyword-cluster
description: Generates a fully SEO-optimized blog post from a keyword cluster. Use this when you have a topic or seed keyword and want a long-form post structured around related keyword variations, search intent, and on-page best practices.
category: seo
connectors:
  - name: Rampify
    url: https://rampify.dev
tools_used:
  - lookup_keywords
  - create_content_spec
  - optimize_content
usage_examples:
  - "Write a blog post targeting the keyword cluster about 'nextjs seo best practices'"
  - "Generate a blog post from my keyword cluster on 'automated testing'"
  - "Create a blog post for the keyword cluster around 'react performance optimization'"
---

You are generating a production-ready, SEO-optimized blog post from a keyword cluster. Follow each step in order.

## Step 1 — Discover and Expand the Keyword Cluster

Use `lookup_keywords` with the seed keyword or topic provided by the user. From the results, identify:
- The **primary keyword** (highest search volume, clearest intent)
- **Secondary keywords** (related terms, synonyms, long-tail variations) — aim for 5–10
- The dominant **search intent** (informational, navigational, commercial, transactional)
- Average monthly search volume and keyword difficulty for the primary keyword

## Step 2 — Create a Content Spec

Use `create_content_spec` with the primary keyword, secondary keywords, and search intent. Record the recommended:
- Target word count range
- Suggested title and H1
- Recommended heading structure (H2s and H3s)
- Target readability level
- Competitor content gaps to address

## Step 3 — Write the Blog Post

Draft the full blog post following the content spec. Apply these rules:

**Structure:**
- Title (H1): include the primary keyword naturally, keep under 65 characters
- Introduction (150–200 words): hook the reader, state the problem, preview what the post covers. Include the primary keyword in the first 100 words.
- Body sections (H2 headings): each section targets one secondary keyword or sub-topic. Aim for 200–350 words per section.
- Include at least one H3 sub-section within the two longest H2 sections.
- Conclusion (100–150 words): summarize key takeaways, include a clear CTA (subscribe, read related post, try a tool, etc.)

**On-page SEO:**
- Use the primary keyword in the title, first paragraph, one H2, and the conclusion.
- Distribute secondary keywords naturally across headings and body copy — never force them.
- Write meta title (50–60 characters) and meta description (140–155 characters) at the end.
- Suggest 2–3 internal link opportunities (placeholder anchors with [INTERNAL LINK: topic]).
- Suggest 1–2 authoritative external links (placeholder anchors with [EXTERNAL LINK: source]).

**Readability:**
- Sentences average under 20 words.
- Use bullet points or numbered lists where listing 3+ items.
- Avoid passive voice where possible.
- Use second person ("you") to address the reader directly.

## Step 4 — Optimize the Draft

Pass the draft to `optimize_content`. Apply any flagged improvements to keyword density, readability score, or missing semantic terms. Note any changes made.

## Output Format

Deliver the final blog post as clean Markdown:

1. `## Meta` section with: Title, Meta Description, Primary Keyword, Secondary Keywords, Word Count, Estimated Read Time
2. The full blog post body starting with the H1
3. `## Implementation Notes` section listing: suggested slug, recommended categories/tags, internal link placeholders to resolve, and any images to source (describe alt text)
