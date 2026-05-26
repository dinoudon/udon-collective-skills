---
name: seo-optimizer
description: Technical SEO, content strategy, and link building for organic traffic growth. Includes on-page optimization, keyword research, site architecture, Core Web Vitals, and ranking strategies.
tags: [seo, search, optimization, content, keywords, ranking, traffic]
version: 1.0.0
author: Enhanced from SEO best practices 2026)
---

# SEO Optimizer

## Overview

Drive organic traffic through comprehensive search engine optimization.

**SEO pillars:**
- **Technical SEO** - Site speed, mobile-friendliness, crawlability
- **On-Page SEO** - Content, keywords, meta tags, structure
- **Off-Page SEO** - Backlinks, authority, trust signals
- **Content Strategy** - High-quality, relevant, valuable content

**Benefits:**
- Sustainable traffic (not dependent on ads)
- High-intent visitors (searching for solutions)
- Compound growth (rankings improve over time)
- Cost-effective (vs paid ads)

## When to Use

**Use for:**
- Blog content optimization
- Product/landing page SEO
- Technical site audits
- Keyword research
- Link building campaigns

**Critical for:**
- Content-driven businesses
- SaaS products
- E-commerce sites
- Local businesses
- B2B lead generation

## The 5-Pillar SEO Framework

```
┌─────────────────────────────────────────────────────────┐
│                 SEO OPTIMIZATION PILLARS                 │
└─────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │   PILLAR 1   │  Technical SEO
    │  TECHNICAL   │  - Site speed
    │              │  - Mobile-friendly
    └──────┬───────┘  - Crawlability
           │
           ▼
    ┌──────────────┐
    │   PILLAR 2   │  Keyword Research
    │  KEYWORDS    │  - Search intent
    │              │  - Competition
    └──────┬───────┘  - Opportunity
           │
           ▼
    ┌──────────────┐
    │   PILLAR 3   │  On-Page SEO
    │   ON-PAGE    │  - Content quality
    │              │  - Meta tags
    └──────┬───────┘  - Internal links
           │
           ▼
    ┌──────────────┐
    │   PILLAR 4   │  Content Strategy
    │   CONTENT    │  - Topic clusters
    │              │  - Content calendar
    └──────┬───────┘  - User intent
           │
           ▼
    ┌──────────────┐
    │   PILLAR 5   │  Link Building
    │  OFF-PAGE    │  - Backlinks
    │              │  - Authority
    └──────────────┘  - Trust signals
```

## Pillar 1: Technical SEO

### Goal: Make your site fast, crawlable, and mobile-friendly

**Core Web Vitals (Google's ranking factors):**

```markdown
## Core Web Vitals Targets

### 1. Largest Contentful Paint (LCP)
**What:** Time until largest content element loads
**Target:** < 2.5 seconds
**How to improve:**
- Optimize images (WebP, lazy loading)
- Use CDN for static assets
- Minimize render-blocking resources
- Implement server-side rendering (SSR)

### 2. First Input Delay (FID)
**What:** Time until page responds to user interaction
**Target:** < 100 milliseconds
**How to improve:**
- Minimize JavaScript execution
- Code splitting (load only what's needed)
- Use web workers for heavy tasks
- Defer non-critical JavaScript

### 3. Cumulative Layout Shift (CLS)
**What:** Visual stability (elements don't jump around)
**Target:** < 0.1
**How to improve:**
- Set explicit width/height for images
- Reserve space for ads/embeds
- Avoid inserting content above existing content
- Use CSS transforms instead of layout properties

### 4. Interaction to Next Paint (INP) [New in 2024]
**What:** Responsiveness to all user interactions
**Target:** < 200 milliseconds
**How to improve:**
- Optimize event handlers
- Reduce main thread work
- Use requestIdleCallback for non-urgent tasks
```

**Technical SEO Checklist:**

```markdown
## Technical SEO Audit

### Crawlability
- [ ] robots.txt allows search engines
- [ ] XML sitemap exists and submitted to Google Search Console
- [ ] No orphan pages (all pages linked from somewhere)
- [ ] No broken links (404 errors)
- [ ] Proper use of noindex/nofollow tags
- [ ] Canonical tags prevent duplicate content

### Site Speed
- [ ] Page load time < 3 seconds
- [ ] Time to First Byte (TTFB) < 600ms
- [ ] Images optimized (WebP, compressed)
- [ ] CSS/JS minified and compressed
- [ ] Browser caching enabled
- [ ] CDN for static assets

### Mobile-Friendliness
- [ ] Responsive design (works on all screen sizes)
- [ ] Touch targets are large enough (48×48px minimum)
- [ ] Text is readable without zooming (16px minimum)
- [ ] No horizontal scrolling
- [ ] Mobile-friendly navigation

### HTTPS & Security
- [ ] SSL certificate installed (HTTPS)
- [ ] All resources loaded over HTTPS
- [ ] HSTS header enabled
- [ ] No mixed content warnings

### Structured Data
- [ ] Schema.org markup for rich snippets
- [ ] Organization schema
- [ ] Article schema for blog posts
- [ ] Product schema for e-commerce
- [ ] FAQ schema for Q&A content
- [ ] Breadcrumb schema for navigation

### URL Structure
- [ ] Clean, descriptive URLs (no ?id=123)
- [ ] Hyphens separate words (not underscores)
- [ ] Lowercase URLs
- [ ] No unnecessary parameters
- [ ] Proper redirects (301 for permanent, 302 for temporary)
```

**Tools for Technical SEO:**

```bash
# Google PageSpeed Insights
curl "https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url=https://example.com"

# Lighthouse CLI
npm install -g lighthouse
lighthouse https://example.com --output html --output-path ./report.html

# Check Core Web Vitals
# Use Chrome DevTools → Lighthouse → Performance

# Validate structured data
# https://search.google.com/test/rich-results

# Check mobile-friendliness
# https://search.google.com/test/mobile-friendly
```

---

## Pillar 2: Keyword Research

### Goal: Find keywords with high traffic potential and low competition

**Keyword Research Process:**

```markdown
## Step 1: Brainstorm Seed Keywords

**Your product:** AI code assistant

**Seed keywords:**
- AI code assistant
- AI pair programmer
- code completion tool
- automated code review
- AI coding tool

## Step 2: Expand with Keyword Tools

**Tools:**
- Google Keyword Planner (free)
- Ahrefs Keywords Explorer (paid)
- SEMrush Keyword Magic Tool (paid)
- Ubersuggest (freemium)

**Expanded keywords:**
- best AI code assistant (1,200 searches/month, KD: 45)
- AI code completion (800 searches/month, KD: 30)
- GitHub Copilot alternative (600 searches/month, KD: 25)
- free AI coding tool (500 searches/month, KD: 35)
- AI code review tool (400 searches/month, KD: 40)

## Step 3: Analyze Search Intent

**Intent types:**
- **Informational:** "what is AI code assistant" (top of funnel)
- **Navigational:** "GitHub Copilot login" (brand search)
- **Commercial:** "best AI code assistant" (comparison)
- **Transactional:** "buy AI code assistant" (bottom of funnel)

**Match content to intent:**
- Informational → Blog post, guide
- Commercial → Comparison page, reviews
- Transactional → Product page, pricing

## Step 4: Evaluate Keyword Difficulty

**Metrics:**
- **Search volume:** Monthly searches (higher = more traffic potential)
- **Keyword difficulty (KD):** 0-100 (lower = easier to rank)
- **Cost per click (CPC):** Paid ad cost (higher = more valuable)
- **SERP features:** Featured snippets, People Also Ask, etc.

**Prioritization matrix:**

| Keyword | Volume | KD | CPC | Priority |
|---------|--------|----|----|----------|
| AI code assistant | 2,000 | 60 | $8 | Medium |
| best AI code assistant | 1,200 | 45 | $6 | High |
| AI code completion | 800 | 30 | $5 | High |
| free AI coding tool | 500 | 35 | $3 | High |
| AI code review | 400 | 40 | $7 | Medium |

**Priority formula:**
Priority = (Volume × CPC) / KD

**Target keywords:**
- High priority: Volume >500, KD <40
- Medium priority: Volume >200, KD <60
- Low priority: Everything else (future targets)
```

**Long-Tail Keywords:**

```markdown
## Long-Tail Strategy

**Why long-tail:**
- Lower competition (easier to rank)
- Higher intent (more likely to convert)
- More specific (better match for content)

**Examples:**

**Short-tail (competitive):**
- "AI code assistant" (2,000 searches, KD: 60)

**Long-tail (easier):**
- "best AI code assistant for Python" (150 searches, KD: 25)
- "AI code assistant vs GitHub Copilot" (100 searches, KD: 20)
- "free AI code assistant for VS Code" (80 searches, KD: 15)

**Strategy:**
- Target 1 short-tail keyword per page (main target)
- Include 5-10 long-tail variations (secondary targets)
- Naturally incorporate in content
```

---

## Pillar 3: On-Page SEO

### Goal: Optimize individual pages for target keywords

**On-Page SEO Checklist:**

```markdown
## Page Optimization

### Title Tag
- [ ] Includes primary keyword (near the beginning)
- [ ] 50-60 characters (displays fully in search results)
- [ ] Compelling and click-worthy
- [ ] Unique for each page

**Examples:**
- ✅ "Best AI Code Assistant for Python Developers (2026)"
- ❌ "Home | AI Code Assistant | Welcome"

### Meta Description
- [ ] Includes primary keyword
- [ ] 150-160 characters
- [ ] Compelling call-to-action
- [ ] Unique for each page

**Examples:**
- ✅ "Discover the top 10 AI code assistants for Python. Compare features, pricing, and reviews. Find the perfect tool to boost your productivity."
- ❌ "Welcome to our website. We offer AI code assistants."

### URL Structure
- [ ] Includes primary keyword
- [ ] Short and descriptive
- [ ] Hyphens separate words
- [ ] No unnecessary parameters

**Examples:**
- ✅ `/best-ai-code-assistant-python`
- ❌ `/page?id=123&category=tools`

### Headings (H1-H6)
- [ ] One H1 per page (includes primary keyword)
- [ ] H2s for main sections (include secondary keywords)
- [ ] H3s for subsections
- [ ] Logical hierarchy (H1 → H2 → H3)

**Example structure:**
```html
<h1>Best AI Code Assistant for Python Developers</h1>

<h2>What is an AI Code Assistant?</h2>
<p>...</p>

<h2>Top 10 AI Code Assistants for Python</h2>

<h3>1. GitHub Copilot</h3>
<p>...</p>

<h3>2. Tabnine</h3>
<p>...</p>

<h2>How to Choose the Right AI Code Assistant</h2>
<p>...</p>
```

### Content
- [ ] 1,500+ words (comprehensive coverage)
- [ ] Primary keyword in first 100 words
- [ ] Keyword density 1-2% (natural, not stuffed)
- [ ] Includes related keywords and synonyms
- [ ] Answers user's search intent
- [ ] Original, high-quality content
- [ ] Proper grammar and spelling

### Images
- [ ] Descriptive file names (ai-code-assistant.jpg, not IMG_1234.jpg)
- [ ] Alt text includes keywords (naturally)
- [ ] Compressed and optimized (WebP format)
- [ ] Responsive (srcset for different sizes)

**Example:**
```html
<img 
  src="/images/ai-code-assistant-comparison.webp"
  alt="Comparison of top AI code assistants for Python developers"
  width="800"
  height="600"
  loading="lazy"
/>
```

### Internal Links
- [ ] Link to related content (3-5 internal links per page)
- [ ] Descriptive anchor text (not "click here")
- [ ] Links to high-priority pages
- [ ] Logical site structure

**Example:**
```markdown
Learn more about [AI code completion tools](/ai-code-completion) 
or read our [GitHub Copilot review](/github-copilot-review).
```

### External Links
- [ ] Link to authoritative sources (Wikipedia, official docs)
- [ ] Open in new tab (target="_blank")
- [ ] Use rel="nofollow" for untrusted links
- [ ] Verify links are not broken

### User Experience
- [ ] Easy to read (short paragraphs, bullet points)
- [ ] Scannable (headings, bold text, lists)
- [ ] Visual hierarchy (whitespace, typography)
- [ ] Clear call-to-action
- [ ] Mobile-friendly
```

**Content Optimization Template:**

```markdown
# [Primary Keyword] - [Benefit/Year]

[Introduction paragraph with primary keyword in first sentence]

## Table of Contents
- [Section 1]
- [Section 2]
- [Section 3]

## What is [Primary Keyword]?

[Definition, explanation, context]

## Why [Primary Keyword] Matters

[Benefits, use cases, problems it solves]

## [Main Content Section 1]

[Detailed content with examples, data, visuals]

### [Subsection 1.1]
[Content]

### [Subsection 1.2]
[Content]

## [Main Content Section 2]

[More detailed content]

## [Main Content Section 3]

[Even more content]

## Frequently Asked Questions

### [Question 1]?
[Answer]

### [Question 2]?
[Answer]

## Conclusion

[Summary, key takeaways, call-to-action]

---

**Related Articles:**
- [Internal link 1]
- [Internal link 2]
- [Internal link 3]
```

---

## Pillar 4: Content Strategy

### Goal: Create valuable content that ranks and converts

**Topic Cluster Model:**

```markdown
## Topic Cluster Structure

### Pillar Page (Main topic)
**URL:** `/ai-code-assistants`
**Target keyword:** "AI code assistants"
**Content:** Comprehensive guide (3,000+ words)

### Cluster Pages (Subtopics)
1. `/ai-code-assistants/python` - "AI code assistant for Python"
2. `/ai-code-assistants/javascript` - "AI code assistant for JavaScript"
3. `/ai-code-assistants/vs-code` - "AI code assistant for VS Code"
4. `/ai-code-assistants/github-copilot-alternative` - "GitHub Copilot alternatives"
5. `/ai-code-assistants/free` - "Free AI code assistants"

### Internal Linking
- Pillar page links to all cluster pages
- Cluster pages link back to pillar page
- Cluster pages link to related cluster pages

**Benefits:**
- Establishes topical authority
- Improves internal linking
- Easier to rank for competitive keywords
- Better user experience
```

**Content Calendar:**

```markdown
## Monthly Content Plan

### Week 1
- **Monday:** Publish pillar page "Complete Guide to AI Code Assistants"
- **Wednesday:** Publish cluster page "Best AI Code Assistant for Python"
- **Friday:** Update old post "GitHub Copilot Review" with 2026 updates

### Week 2
- **Monday:** Publish cluster page "AI Code Assistant for JavaScript"
- **Wednesday:** Publish comparison "GitHub Copilot vs Tabnine"
- **Friday:** Publish tutorial "How to Set Up AI Code Assistant in VS Code"

### Week 3
- **Monday:** Publish cluster page "Free AI Code Assistants"
- **Wednesday:** Publish case study "How We Reduced Bugs by 40% with AI"
- **Friday:** Publish listicle "10 AI Coding Tools Every Developer Should Know"

### Week 4
- **Monday:** Publish cluster page "AI Code Review Tools"
- **Wednesday:** Publish opinion piece "The Future of AI-Assisted Coding"
- **Friday:** Publish roundup "Best AI Tools for Developers in 2026"

**Content mix:**
- 40% Educational (guides, tutorials)
- 30% Commercial (comparisons, reviews)
- 20% Thought leadership (opinions, predictions)
- 10% Company (case studies, updates)
```

**Content Types by Funnel Stage:**

```markdown
## Content Funnel

### Top of Funnel (Awareness)
**Goal:** Attract visitors, build awareness

**Content types:**
- "What is [topic]" guides
- Industry trends and statistics
- Beginner tutorials
- Listicles ("10 best...")

**Example:**
- "What is AI Pair Programming? A Beginner's Guide"
- "10 AI Coding Trends to Watch in 2026"

### Middle of Funnel (Consideration)
**Goal:** Educate, build trust

**Content types:**
- Comparison articles
- In-depth guides
- Case studies
- How-to tutorials

**Example:**
- "GitHub Copilot vs Tabnine: Which is Better?"
- "How to Choose the Right AI Code Assistant"

### Bottom of Funnel (Decision)
**Goal:** Convert to customers

**Content types:**
- Product pages
- Pricing comparisons
- Free trials/demos
- Customer testimonials

**Example:**
- "Try Our AI Code Assistant Free for 14 Days"
- "See How [Company] Increased Productivity by 10x"
```

---

## Pillar 5: Link Building

### Goal: Earn high-quality backlinks to boost authority

**Link Building Strategies:**

```markdown
## 1. Content-Driven Link Building

### Create Linkable Assets
**Types:**
- Original research/data
- Comprehensive guides
- Tools/calculators
- Infographics
- Industry reports

**Example:**
"State of AI Coding in 2026" (annual report)
→ Gets linked by tech blogs, news sites

### Skyscraper Technique
**Process:**
1. Find popular content in your niche
2. Create something 10x better
3. Reach out to sites linking to original
4. Ask them to link to your improved version

**Example:**
- Find: "10 AI Coding Tools" (100 backlinks)
- Create: "50 AI Coding Tools Compared (2026)" (more comprehensive)
- Outreach: "Hey, I noticed you linked to [old article]. I created a more comprehensive version..."

## 2. Guest Posting

**Target sites:**
- Industry blogs (Dev.to, Hashnode, Medium)
- Tech publications (TechCrunch, The Verge)
- Company blogs (Stripe, Vercel, Netlify)

**Pitch template:**
```
Subject: Guest post idea: [Title]

Hi [Name],

I'm [Your Name], [Your Role] at [Company]. I've been following [Their Blog] and love your content on [Topic].

I'd like to contribute a guest post: "[Proposed Title]"

Outline:
- [Section 1]
- [Section 2]
- [Section 3]

This would provide value to your readers by [Benefit].

I've previously written for [Other Publications]. Here are some samples:
- [Link 1]
- [Link 2]

Would this be a good fit?

Thanks,
[Your Name]
```

## 3. Broken Link Building

**Process:**
1. Find broken links on relevant sites
2. Create content to replace broken link
3. Reach out to site owner

**Tools:**
- Ahrefs Site Explorer (find broken backlinks)
- Check My Links (Chrome extension)

**Outreach template:**
```
Subject: Broken link on [Page Title]

Hi [Name],

I was reading your article "[Title]" and noticed a broken link in the [Section] section.

The link to [Broken URL] returns a 404 error.

I recently published a similar resource that might be a good replacement:
[Your URL]

It covers [Topics] and includes [Unique Value].

Would you consider updating the link?

Thanks,
[Your Name]
```

## 4. Digital PR

**Tactics:**
- Press releases (product launches, funding)
- Expert quotes (HARO, SourceBottle)
- Podcast interviews
- Conference speaking
- Awards and recognition

**Example:**
- Submit to "Best AI Tools of 2026" lists
- Respond to journalist queries on HARO
- Speak at developer conferences

## 5. Partnership Link Building

**Tactics:**
- Integration partners (link exchange)
- Affiliate programs
- Co-marketing campaigns
- Sponsorships

**Example:**
- Partner with VS Code → Get listed in their marketplace
- Sponsor a developer conference → Get backlink from event site
```

**Link Quality Metrics:**

```markdown
## Evaluating Backlink Quality

### High-Quality Backlinks
- **Domain Authority (DA):** 50+ (Ahrefs DR, Moz DA)
- **Relevance:** Same industry/niche
- **Traffic:** Site gets organic traffic
- **Editorial:** Naturally placed in content
- **Dofollow:** Passes link equity

### Low-Quality Backlinks (Avoid)
- **Spam sites:** Low DA, no traffic
- **Irrelevant:** Unrelated industry
- **Paid links:** Violates Google guidelines
- **Link farms:** Exist only for links
- **Sitewide links:** Footer/sidebar links

### Disavow Toxic Links
```bash
# Create disavow file
cat > disavow.txt << 'EOF'
# Spam site
domain:spamsite.com

# Specific bad link
http://badsite.com/page
EOF

# Submit to Google Search Console
# Search Console → Links → Disavow Links
```
```

---

## SEO Tools & Resources

```markdown
## Essential SEO Tools

### Free Tools
- **Google Search Console:** Monitor search performance
- **Google Analytics:** Track traffic and conversions
- **Google PageSpeed Insights:** Check site speed
- **Google Keyword Planner:** Keyword research
- **Bing Webmaster Tools:** Bing search data
- **Screaming Frog (free tier):** Site audits (500 URLs)

### Paid Tools
- **Ahrefs ($99+/mo):** Comprehensive SEO suite
- **SEMrush ($119+/mo):** Keyword research, competitor analysis
- **Moz Pro ($99+/mo):** Rank tracking, site audits
- **Surfer SEO ($59+/mo):** Content optimization
- **Clearscope ($170+/mo):** Content briefs

### Browser Extensions
- **MozBar:** See DA/PA on any page
- **Ahrefs SEO Toolbar:** Quick SEO metrics
- **Keywords Everywhere:** Keyword data in search results
- **Check My Links:** Find broken links
```

---

## SEO Metrics to Track

```markdown
## Key Performance Indicators

### Traffic Metrics
- **Organic traffic:** Visitors from search engines
- **Organic sessions:** Number of visits
- **Pages per session:** Engagement depth
- **Bounce rate:** % who leave immediately (target <50%)
- **Average session duration:** Time on site (target >2 min)

### Ranking Metrics
- **Keyword rankings:** Position in search results (target top 10)
- **Featured snippets:** Position 0 rankings
- **SERP visibility:** % of keywords ranking
- **Click-through rate (CTR):** % who click your result (target >5%)

### Conversion Metrics
- **Organic conversions:** Signups/purchases from organic traffic
- **Conversion rate:** % of visitors who convert (target >2%)
- **Goal completions:** Specific actions (downloads, trials)
- **Revenue from organic:** $ attributed to SEO

### Authority Metrics
- **Domain Authority (DA):** Moz's 0-100 score
- **Domain Rating (DR):** Ahrefs' 0-100 score
- **Backlinks:** Number of referring domains
- **Referring domains:** Unique sites linking to you

### Technical Metrics
- **Core Web Vitals:** LCP, FID, CLS, INP
- **Crawl errors:** Issues found by search engines
- **Index coverage:** % of pages indexed
- **Mobile usability:** Mobile-friendly issues
```

---

## SEO Checklist

**Technical SEO:**
- [ ] Site speed < 3 seconds
- [ ] Mobile-friendly (responsive design)
- [ ] HTTPS enabled
- [ ] XML sitemap submitted
- [ ] robots.txt configured
- [ ] Structured data implemented
- [ ] Core Web Vitals passing

**Keyword Research:**
- [ ] Target keywords identified (10-20)
- [ ] Search intent analyzed
- [ ] Keyword difficulty evaluated
- [ ] Long-tail variations found

**On-Page SEO:**
- [ ] Title tags optimized (50-60 chars)
- [ ] Meta descriptions written (150-160 chars)
- [ ] URLs clean and descriptive
- [ ] H1-H6 hierarchy proper
- [ ] Content 1,500+ words
- [ ] Images optimized (WebP, alt text)
- [ ] Internal links added (3-5 per page)

**Content Strategy:**
- [ ] Topic clusters planned
- [ ] Content calendar created (4 weeks ahead)
- [ ] Content mix balanced (educational, commercial, thought leadership)
- [ ] Pillar pages published

**Link Building:**
- [ ] Linkable assets created
- [ ] Guest post targets identified (10-20 sites)
- [ ] Outreach templates prepared
- [ ] Backlink profile monitored

**Tracking:**
- [ ] Google Search Console set up
- [ ] Google Analytics configured
- [ ] Rank tracking enabled
- [ ] Monthly reports scheduled

---

## Common SEO Mistakes

### Mistake 1: Keyword Stuffing
**Problem:** Repeating keywords unnaturally

**Solution:** Use keywords naturally, focus on user experience

### Mistake 2: Thin Content
**Problem:** Short, low-value pages (<300 words)

**Solution:** Create comprehensive content (1,500+ words)

### Mistake 3: Duplicate Content
**Problem:** Same content on multiple pages

**Solution:** Use canonical tags, consolidate pages

### Mistake 4: Ignoring Mobile
**Problem:** Site not mobile-friendly

**Solution:** Responsive design, test on mobile devices

### Mistake 5: Slow Site Speed
**Problem:** Pages load slowly (>3 seconds)

**Solution:** Optimize images, use CDN, minify code

### Mistake 6: No Internal Linking
**Problem:** Pages are isolated, no link structure

**Solution:** Link related content, create topic clusters

---

## Integration with Other Skills

**Use before SEO:**
- `gtm-strategy` - Understand target audience
- `competitor-analysis` - Analyze competitor SEO

**Use during SEO:**
- `funnel-builder` - Optimize conversion from organic traffic
- `email-campaign` - Capture and nurture organic visitors

**Use after SEO:**
- `user-research` - Understand visitor behavior
- `web-performance` - Improve Core Web Vitals

---

**Remember:** SEO is a long-term game. Results take 3-6 months. Focus on creating valuable content, building authority, and providing great user experience. Rankings will follow.
