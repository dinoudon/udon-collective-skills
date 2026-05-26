---
name: funnel-builder
description: Design and optimize conversion funnels for SaaS, e-commerce, and lead generation. Includes funnel mapping, conversion rate optimization (CRO), A/B testing strategy, and analytics setup.
tags: [funnel, conversion, cro, marketing, analytics, optimization, growth]
version: 1.0.0
author: Enhanced from growth marketing best practices)
---

# Funnel Builder

## Overview

Design high-converting funnels that turn visitors into customers.

**A conversion funnel:**
- Maps the customer journey from awareness to purchase
- Identifies drop-off points and friction
- Optimizes each stage for maximum conversion
- Measures and improves over time

**Key concepts:**
- **AIDA Framework** - Awareness, Interest, Desire, Action
- **Pirate Metrics (AARRR)** - Acquisition, Activation, Retention, Revenue, Referral
- **Conversion Rate Optimization (CRO)** - Systematic improvement of conversion rates
- **A/B Testing** - Data-driven experimentation

## When to Use

**Use for:**
- SaaS signup flows
- E-commerce checkout
- Lead generation campaigns
- Webinar registrations
- Free trial conversions
- Onboarding flows

**Use when:**
- Launching a new product
- Conversion rates are low
- Drop-off is high at specific stages
- You need to scale customer acquisition

## The 5-Stage Funnel Framework

```
┌─────────────────────────────────────────────────────────┐
│              CONVERSION FUNNEL STAGES                    │
└─────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │   STAGE 1    │  Awareness (Top of Funnel)
    │  AWARENESS   │  - Traffic sources
    │              │  - Landing pages
    └──────┬───────┘  - First impression
           │
           ▼ 40% convert
    ┌──────────────┐
    │   STAGE 2    │  Interest (Middle of Funnel)
    │   INTEREST   │  - Value proposition
    │              │  - Feature exploration
    └──────┬───────┘  - Social proof
           │
           ▼ 25% convert
    ┌──────────────┐
    │   STAGE 3    │  Desire (Middle of Funnel)
    │    DESIRE    │  - Pricing page
    │              │  - Comparison
    └──────┬───────┘  - Testimonials
           │
           ▼ 50% convert
    ┌──────────────┐
    │   STAGE 4    │  Action (Bottom of Funnel)
    │    ACTION    │  - Signup/checkout
    │              │  - Payment
    └──────┬───────┘  - Confirmation
           │
           ▼ 80% convert
    ┌──────────────┐
    │   STAGE 5    │  Retention (Post-Purchase)
    │  RETENTION   │  - Onboarding
    │              │  - Engagement
    └──────────────┘  - Upsell

Overall Conversion: 1000 visitors → 40 customers (4%)
```

## Stage 1: Awareness (Top of Funnel)

### Goal: Get the right people to your site

**Traffic Sources:**
```markdown
## Traffic Channels

### Paid
- **Google Ads**: Search intent, high conversion
- **Facebook/Instagram Ads**: Targeting, retargeting
- **LinkedIn Ads**: B2B, professional audience
- **Display Ads**: Brand awareness, remarketing

### Organic
- **SEO**: Long-term, sustainable traffic
- **Content Marketing**: Blog, guides, resources
- **Social Media**: Community, engagement
- **PR/Media**: Credibility, backlinks

### Referral
- **Partnerships**: Co-marketing, integrations
- **Affiliates**: Performance-based
- **Word of Mouth**: Customer referrals
- **Communities**: Reddit, forums, Slack groups
```

**Landing Page Optimization:**

```markdown
## High-Converting Landing Page Structure

### Above the Fold (First 3 seconds)
1. **Headline**: Clear value proposition
   - "Ship code 10x faster with AI pair programming"
   - NOT: "Welcome to our platform"

2. **Subheadline**: Expand on the promise
   - "Join 50,000+ developers using AI to write, review, and deploy code"

3. **Hero Image/Video**: Show the product in action
   - Screenshot of IDE with AI suggestions
   - 30-second demo video

4. **Primary CTA**: One clear action
   - "Start Free Trial" (not "Learn More")
   - Contrasting color, large button

### Below the Fold
5. **Social Proof**: Build trust immediately
   - Customer logos (recognizable brands)
   - "Trusted by 50,000+ developers"
   - G2/Capterra ratings

6. **Benefits (Not Features)**: What they get
   - ✅ "Write code 10x faster" (not "AI autocomplete")
   - ✅ "Catch bugs before production" (not "Static analysis")
   - ✅ "Deploy with confidence" (not "CI/CD integration")

7. **How It Works**: 3-step process
   - Step 1: Install extension
   - Step 2: Write code with AI suggestions
   - Step 3: Ship faster

8. **Testimonials**: Real customer stories
   - Photo + name + company + quote
   - Specific results ("Reduced bugs by 40%")

9. **Pricing Preview**: Remove price anxiety
   - "Free for 14 days, no credit card required"
   - "Plans starting at $10/month"

10. **FAQ**: Address objections
    - "Is my code private?" → "Yes, end-to-end encrypted"
    - "What languages are supported?" → "20+ languages"

11. **Final CTA**: Repeat the ask
    - Same CTA as above the fold
    - Add urgency: "Join 50,000+ developers today"
```

**Conversion Rate Benchmarks:**
- Landing page → Signup: 2-5% (cold traffic)
- Landing page → Signup: 10-20% (warm traffic)
- Landing page → Signup: 30-50% (retargeting)

**Optimization Checklist:**
- [ ] Headline communicates value in 3 seconds
- [ ] One primary CTA (not multiple competing CTAs)
- [ ] Social proof above the fold
- [ ] Mobile-responsive (50%+ traffic is mobile)
- [ ] Page load time < 3 seconds
- [ ] No distractions (remove navigation, sidebars)
- [ ] Clear next step (what happens after clicking CTA)

---

## Stage 2: Interest (Middle of Funnel)

### Goal: Engage visitors and demonstrate value

**Value Proposition Canvas:**

```markdown
## Crafting Your Value Proposition

### Customer Jobs (What they're trying to do)
- Functional: "Write code faster"
- Social: "Look competent to my team"
- Emotional: "Feel confident in my code"

### Customer Pains (What frustrates them)
- "Spend too much time on boilerplate"
- "Miss bugs that reach production"
- "Context switching slows me down"

### Customer Gains (What they want)
- "Ship features faster"
- "Fewer bugs in production"
- "More time for creative work"

### Your Solution
**Pain Relievers:**
- AI writes boilerplate automatically
- Static analysis catches bugs before commit
- Inline suggestions reduce context switching

**Gain Creators:**
- 10x faster code writing
- 40% fewer production bugs
- 5 hours/week saved

### Value Proposition Statement:
"For developers who want to ship faster, [Product] is an AI pair programmer that writes, reviews, and tests code automatically, unlike GitHub Copilot which only suggests code, we catch bugs and deploy for you."
```

**Feature Exploration:**

```markdown
## Interactive Product Tour

### Step 1: Show the "Aha!" Moment
- Don't start with account setup
- Jump straight to the magic
- Example: "Watch AI write a React component in 10 seconds"

### Step 2: Let Them Try It
- Interactive demo (no signup required)
- Sandbox environment
- Real use case, not toy example

### Step 3: Highlight Key Features
- Feature 1: [Benefit] → [Demo]
- Feature 2: [Benefit] → [Demo]
- Feature 3: [Benefit] → [Demo]

### Step 4: Social Proof
- "10,000 developers used this feature today"
- Customer testimonial video
- Case study: "How Stripe reduced bugs by 40%"

### Step 5: CTA
- "Start your free trial"
- "See it in your codebase"
```

**Content Marketing:**

```markdown
## Content That Converts

### Blog Posts (SEO + Education)
- "How to [achieve goal] with [your product]"
- "10 [pain points] and how to solve them"
- "[Competitor] vs [Your Product]: Which is better?"

### Case Studies (Social Proof)
- Customer name + logo
- Problem they had
- How you solved it
- Quantified results (40% faster, $100K saved)

### Guides/Ebooks (Lead Magnets)
- "The Complete Guide to [topic]"
- Gated content (email required)
- Nurture sequence after download

### Webinars (High-Intent)
- Live demo + Q&A
- 30-40% conversion to trial
- Record and repurpose

### Comparison Pages (High-Intent)
- "[Your Product] vs [Competitor]"
- Honest comparison (builds trust)
- Highlight your unique advantages
```

**Conversion Rate Benchmarks:**
- Landing page → Product tour: 20-40%
- Product tour → Signup: 10-30%
- Blog post → Email signup: 2-5%

---

## Stage 3: Desire (Middle of Funnel)

### Goal: Build desire and overcome objections

**Pricing Page Optimization:**

```markdown
## High-Converting Pricing Page

### Structure

**1. Headline**
"Simple, transparent pricing"

**2. Billing Toggle**
[Monthly] [Annual (Save 20%)]

**3. Pricing Tiers (3 options)**

┌─────────────┬─────────────┬─────────────┐
│   STARTER   │     PRO     │  ENTERPRISE │
│   $10/mo    │   $50/mo    │   Custom    │
├─────────────┼─────────────┼─────────────┤
│ For         │ For         │ For         │
│ individuals │ small teams │ large orgs  │
│             │             │             │
│ ✓ Feature 1 │ ✓ Feature 1 │ ✓ Feature 1 │
│ ✓ Feature 2 │ ✓ Feature 2 │ ✓ Feature 2 │
│ ✗ Feature 3 │ ✓ Feature 3 │ ✓ Feature 3 │
│ ✗ Feature 4 │ ✓ Feature 4 │ ✓ Feature 4 │
│             │ ✓ Feature 5 │ ✓ Feature 5 │
│             │             │ ✓ Feature 6 │
│             │             │ ✓ Feature 7 │
│             │             │             │
│ [Start Free]│ [Start Free]│ [Contact Us]│
└─────────────┴─────────────┴─────────────┘

**4. FAQ**
- "Can I change plans later?" → Yes, anytime
- "What payment methods?" → Credit card, PayPal
- "Is there a free trial?" → Yes, 14 days, no CC required

**5. Social Proof**
"Trusted by 50,000+ developers at Google, Stripe, Airbnb"

**6. Money-Back Guarantee**
"30-day money-back guarantee, no questions asked"
```

**Pricing Psychology:**

```markdown
## Pricing Strategies

### Anchoring
- Show highest price first (anchors perception)
- "Was $100, now $50" (discount anchor)
- "Most popular" badge on target tier

### Decoy Effect
- Add expensive tier to make middle tier look reasonable
- Example: $10, $50, $500 → $50 looks like a deal

### Value-Based Pricing
- Price based on value delivered, not cost
- "Save 10 hours/week = $500/week value → $50/month is a steal"

### Freemium
- Free tier for acquisition
- Paid tier for power users
- Enterprise tier for large orgs

### Usage-Based
- Pay for what you use
- Aligns cost with value
- Scales with customer growth
```

**Objection Handling:**

```markdown
## Common Objections & Responses

### "It's too expensive"
**Response:**
- Show ROI: "Save 10 hours/week × $50/hour = $2,000/month value"
- Offer annual discount: "Save 20% with annual billing"
- Free trial: "Try it free for 14 days, see the value yourself"

### "I need to think about it"
**Response:**
- Create urgency: "20% off ends Friday"
- Risk reversal: "30-day money-back guarantee"
- Social proof: "10,000 developers already using it"

### "I need to ask my boss"
**Response:**
- Provide ROI calculator
- Offer to join the call
- Send case study with similar company

### "What if it doesn't work for me?"
**Response:**
- Free trial: "Try before you buy"
- Money-back guarantee: "Full refund within 30 days"
- Customer success: "We'll help you get set up"

### "How is this different from [competitor]?"
**Response:**
- Comparison page: "See side-by-side comparison"
- Unique features: "We're the only one that does X"
- Customer testimonials: "Why customers switched from [competitor]"
```

**Conversion Rate Benchmarks:**
- Pricing page → Signup: 5-15%
- Free trial → Paid: 10-25%

---

## Stage 4: Action (Bottom of Funnel)

### Goal: Remove friction and close the deal

**Signup Flow Optimization:**

```markdown
## Frictionless Signup

### Step 1: Minimize Fields
**Bad (7 fields):**
- First name
- Last name
- Email
- Password
- Confirm password
- Company
- Phone

**Good (2 fields):**
- Email
- Password

**Best (1 click):**
- "Sign up with Google"
- "Sign up with GitHub"

### Step 2: Progressive Profiling
- Collect minimum info upfront
- Ask for more during onboarding
- Example: Email → Name → Company → Use case

### Step 3: Clear Value
**Each step should answer:**
- "Why am I doing this?"
- "What happens next?"
- "How long will this take?"

**Example:**
"Step 1 of 3: Create your account (30 seconds)"

### Step 4: Trust Signals
- "We'll never spam you"
- "Your data is encrypted"
- "Cancel anytime, no questions asked"

### Step 5: Remove Distractions
- No navigation menu
- No external links
- One clear path forward
```

**Checkout Optimization (E-commerce):**

```markdown
## High-Converting Checkout

### Best Practices

**1. Guest Checkout**
- Don't force account creation
- "Checkout as guest" option
- Offer account creation AFTER purchase

**2. Progress Indicator**
- Show steps: Shipping → Payment → Review
- Show current step clearly
- Allow going back

**3. Autofill**
- Use browser autofill
- Save addresses for returning customers
- One-click checkout (Amazon-style)

**4. Multiple Payment Options**
- Credit cards (Visa, MC, Amex)
- PayPal, Apple Pay, Google Pay
- Buy now, pay later (Klarna, Affirm)

**5. Security Badges**
- SSL certificate icon
- "Secure checkout" badge
- Payment processor logos (Stripe, PayPal)

**6. Exit-Intent Popup**
- Trigger when user tries to leave
- Offer discount: "Wait! Get 10% off"
- Capture email: "Save your cart"

**7. Cart Abandonment Email**
- Send within 1 hour
- Remind what they left behind
- Offer incentive (free shipping, discount)
```

**Conversion Rate Benchmarks:**
- Signup page → Account created: 30-60%
- Checkout started → Purchase completed: 20-40%
- Cart abandonment rate: 60-80% (industry average)

**Optimization Checklist:**
- [ ] Minimize form fields (2-3 max)
- [ ] Social login options (Google, GitHub)
- [ ] Clear progress indicator
- [ ] Trust signals (security badges, guarantees)
- [ ] Mobile-optimized (50%+ of traffic)
- [ ] Autofill enabled
- [ ] Error messages are helpful
- [ ] Success state is clear

---

## Stage 5: Retention (Post-Purchase)

### Goal: Activate, engage, and retain customers

**Onboarding Flow:**

```markdown
## Effective Onboarding

### Day 0: Welcome Email (Immediate)
**Subject:** "Welcome to [Product]! Here's how to get started"

**Content:**
- Thank you for signing up
- Quick start guide (3 steps)
- Link to first action
- Support contact

### Day 1: First Value (Within 24 hours)
**Goal:** Get them to "aha!" moment

**Examples:**
- SaaS: Complete first project
- E-commerce: First purchase delivered
- Social: First connection made

**Tactics:**
- In-app tutorial
- Onboarding checklist
- Personal welcome video

### Day 3: Feature Discovery
**Goal:** Show additional value

**Content:**
- "Did you know you can [feature]?"
- Use case examples
- Customer success story

### Day 7: Engagement Check
**Goal:** Re-engage if inactive

**Content:**
- "We noticed you haven't [action]"
- Offer help: "Need assistance?"
- Incentive: "Here's a tip to get started"

### Day 14: Upgrade Prompt (Freemium)
**Goal:** Convert to paid

**Content:**
- "You've used [X] of your free [limit]"
- Show value delivered: "You've saved 10 hours"
- Upgrade CTA: "Unlock unlimited [feature]"

### Day 30: Feedback Request
**Goal:** Improve product

**Content:**
- "How are we doing?"
- NPS survey
- Feature request form
```

**Engagement Tactics:**

```markdown
## Keep Users Coming Back

### Email Campaigns
- **Weekly digest**: "Your weekly summary"
- **Feature announcements**: "New: [Feature]"
- **Tips & tricks**: "How to [achieve goal]"
- **Re-engagement**: "We miss you!"

### In-App Notifications
- **Achievements**: "You've completed 10 projects!"
- **Milestones**: "You've saved 100 hours"
- **Social**: "Your teammate commented"
- **Reminders**: "You have 3 pending tasks"

### Gamification
- **Progress bars**: "Profile 80% complete"
- **Badges**: "Early adopter" badge
- **Leaderboards**: "Top 10 users this week"
- **Streaks**: "7-day streak!"

### Community
- **User forum**: Peer support
- **Slack/Discord**: Real-time chat
- **Events**: Webinars, meetups
- **User-generated content**: Case studies, tutorials
```

**Retention Metrics:**

```markdown
## Key Retention Metrics

### Day 1 Retention
- % of users who return on Day 1
- Benchmark: 40-60% (good), 60%+ (great)

### Day 7 Retention
- % of users who return on Day 7
- Benchmark: 20-40% (good), 40%+ (great)

### Day 30 Retention
- % of users who return on Day 30
- Benchmark: 10-20% (good), 20%+ (great)

### Churn Rate
- % of customers who cancel per month
- Benchmark: <5% (good), <3% (great)

### Net Revenue Retention (NRR)
- Revenue from existing customers (including upsells)
- Benchmark: >100% (good), >120% (great)
```

---

## Funnel Analytics Setup

### Essential Metrics to Track

```markdown
## Funnel Metrics Dashboard

### Acquisition
- **Traffic**: Visitors per day/week/month
- **Traffic sources**: Organic, paid, referral, direct
- **Cost per visitor**: Ad spend / visitors

### Activation
- **Signup rate**: Signups / visitors
- **Time to first value**: Hours until "aha!" moment
- **Activation rate**: % who reach "aha!" moment

### Retention
- **Day 1/7/30 retention**: % who return
- **Churn rate**: % who cancel per month
- **Engagement**: DAU/MAU ratio

### Revenue
- **Conversion rate**: Paid customers / signups
- **Average revenue per user (ARPU)**: Revenue / customers
- **Customer lifetime value (LTV)**: ARPU × avg lifetime

### Referral
- **Referral rate**: % of customers who refer
- **Viral coefficient**: Invites sent × conversion rate
- **Net Promoter Score (NPS)**: Likelihood to recommend

### Efficiency
- **Customer acquisition cost (CAC)**: Marketing spend / new customers
- **LTV:CAC ratio**: LTV / CAC (should be >3:1)
- **Payback period**: Months to recover CAC
```

### Analytics Tools

```markdown
## Recommended Stack

### Web Analytics
- **Google Analytics 4**: Free, comprehensive
- **Plausible**: Privacy-friendly, simple
- **Mixpanel**: Event-based, user-centric

### Funnel Analysis
- **Amplitude**: Product analytics
- **Heap**: Auto-capture events
- **PostHog**: Open-source, self-hosted

### A/B Testing
- **Optimizely**: Enterprise-grade
- **VWO**: Visual editor
- **Google Optimize**: Free (deprecated, use GA4)

### Heatmaps
- **Hotjar**: Heatmaps + recordings
- **Crazy Egg**: Heatmaps + A/B testing
- **Microsoft Clarity**: Free, unlimited

### Session Recording
- **FullStory**: Session replay + analytics
- **LogRocket**: Session replay + error tracking
- **Hotjar**: Session recordings

### Email Analytics
- **Customer.io**: Behavioral emails
- **Klaviyo**: E-commerce focused
- **Mailchimp**: Simple, affordable
```

---

## A/B Testing Strategy

### What to Test

```markdown
## High-Impact Tests (Prioritized)

### Landing Page
1. **Headline**: Value proposition clarity
2. **CTA button**: Color, text, placement
3. **Hero image**: Product demo vs lifestyle
4. **Social proof**: Logos vs testimonials vs numbers

### Pricing Page
1. **Price points**: $10 vs $15 vs $20
2. **Billing frequency**: Monthly vs annual
3. **Feature list**: Benefits vs features
4. **Guarantee**: 30-day vs 60-day vs lifetime

### Signup Flow
1. **Form fields**: 2 fields vs 5 fields
2. **Social login**: With vs without
3. **Password requirements**: Strict vs lenient
4. **Progress indicator**: With vs without

### Checkout
1. **Guest checkout**: Required account vs optional
2. **Payment options**: Credit card only vs multiple
3. **Shipping options**: Free vs paid vs tiered
4. **Upsells**: With vs without
```

### Testing Framework

```markdown
## A/B Test Process

### 1. Hypothesis
"Changing [element] from [A] to [B] will increase [metric] by [X]% because [reason]"

**Example:**
"Changing CTA button from 'Learn More' to 'Start Free Trial' will increase signups by 20% because it's more action-oriented"

### 2. Design Test
- **Control (A)**: Current version
- **Variant (B)**: New version
- **Sample size**: Calculate required traffic
- **Duration**: Run until statistical significance

### 3. Run Test
- Split traffic 50/50
- Run for at least 1 week (capture weekly patterns)
- Don't peek early (wait for significance)

### 4. Analyze Results
- **Winner**: Variant with higher conversion
- **Confidence**: 95%+ statistical significance
- **Lift**: % improvement over control

### 5. Implement Winner
- Roll out to 100% of traffic
- Document learnings
- Plan next test
```

### Sample Size Calculator

```markdown
## How Long to Run Test

**Formula:**
Sample size = (Z² × p × (1-p)) / E²

Where:
- Z = 1.96 (for 95% confidence)
- p = baseline conversion rate
- E = minimum detectable effect

**Example:**
- Baseline conversion: 5%
- Minimum detectable effect: 1% (absolute)
- Required sample size: ~3,000 visitors per variant
- If you get 1,000 visitors/day → 6-day test
```

---

## Funnel Optimization Checklist

**Awareness:**
- [ ] Landing page loads in < 3 seconds
- [ ] Headline communicates value in 3 seconds
- [ ] One primary CTA above the fold
- [ ] Social proof visible immediately
- [ ] Mobile-responsive design

**Interest:**
- [ ] Value proposition is clear and compelling
- [ ] Product demo/tour available
- [ ] Social proof (testimonials, case studies)
- [ ] Content addresses customer pain points
- [ ] Clear next step at every stage

**Desire:**
- [ ] Pricing is transparent and easy to understand
- [ ] Multiple pricing tiers (good, better, best)
- [ ] FAQ addresses common objections
- [ ] Money-back guarantee or free trial
- [ ] Comparison with competitors

**Action:**
- [ ] Signup form has minimal fields (2-3 max)
- [ ] Social login options available
- [ ] Trust signals (security badges, guarantees)
- [ ] Clear error messages
- [ ] Success state is obvious

**Retention:**
- [ ] Welcome email sent immediately
- [ ] Onboarding guides user to "aha!" moment
- [ ] Regular engagement emails
- [ ] In-app notifications for key events
- [ ] Feedback loop (NPS, surveys)

---

## Common Funnel Mistakes

### Mistake 1: Too Many Steps
**Problem:** 10-step signup flow

**Solution:** Reduce to 2-3 steps, use progressive profiling

### Mistake 2: No Clear CTA
**Problem:** Multiple competing CTAs

**Solution:** One primary CTA per page

### Mistake 3: Ignoring Mobile
**Problem:** Desktop-only optimization

**Solution:** Mobile-first design (50%+ traffic is mobile)

### Mistake 4: No Social Proof
**Problem:** No trust signals

**Solution:** Add testimonials, logos, ratings above the fold

### Mistake 5: Asking for Too Much Info
**Problem:** 10-field signup form

**Solution:** Ask for email only, collect more later

### Mistake 6: No Follow-Up
**Problem:** User signs up, never hears from you

**Solution:** Automated onboarding email sequence

---

## Integration with Other Skills

**Use before building funnel:**
- `spec-driven-development-enhanced` - Define requirements
- `architecture-blueprint` - Design technical architecture

**Use during optimization:**
- `claude-design` - Create landing page designs
- `sketch` - Rapid prototyping

**Use for analytics:**
- `parallel-orchestration` - Analyze multiple funnels simultaneously

---

**Remember:** A funnel is never "done." Continuously test, measure, and optimize. Small improvements compound over time.
