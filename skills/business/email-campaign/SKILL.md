---
name: email-campaign
description: Email marketing campaigns with drip sequences, segmentation, automation, and conversion optimization. Includes welcome series, nurture campaigns, re-engagement, and analytics.
tags: [email, marketing, automation, drip, campaigns, segmentation, conversion]
version: 1.0.0
author: Enhanced from email marketing best practices)
---

# Email Campaign

## Overview

Build and optimize email campaigns that nurture leads and drive conversions.

**Email marketing benefits:**
- **High ROI:** $36-$42 return for every $1 spent
- **Direct channel:** You own the list (not platform-dependent)
- **Personalization:** Segment and target specific audiences
- **Automation:** Set up once, runs forever
- **Measurable:** Track opens, clicks, conversions

**Key concepts:**
- Drip campaigns (automated sequences)
- Segmentation (targeted messaging)
- Personalization (dynamic content)
- A/B testing (optimize performance)
- Deliverability (avoid spam folder)

## When to Use

**Use for:**
- Welcome new subscribers
- Nurture leads to customers
- Onboard new users
- Re-engage inactive users
- Promote products/features
- Build relationships

**Critical for:**
- SaaS products (trial → paid conversion)
- E-commerce (abandoned cart, upsells)
- Content businesses (newsletter, courses)
- B2B lead generation
- Community building

## The 6-Campaign Framework

```
┌─────────────────────────────────────────────────────────┐
│              EMAIL CAMPAIGN TYPES                        │
└─────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │  CAMPAIGN 1  │  Welcome Series
    │   WELCOME    │  - Introduce brand
    │              │  - Set expectations
    └──────┬───────┘  - First value
           │
           ▼
    ┌──────────────┐
    │  CAMPAIGN 2  │  Lead Nurture
    │   NURTURE    │  - Educate
    │              │  - Build trust
    └──────┬───────┘  - Move to purchase
           │
           ▼
    ┌──────────────┐
    │  CAMPAIGN 3  │  Onboarding
    │  ONBOARDING  │  - Guide setup
    │              │  - Show features
    └──────┬───────┘  - Drive activation
           │
           ▼
    ┌──────────────┐
    │  CAMPAIGN 4  │  Engagement
    │ ENGAGEMENT   │  - Share content
    │              │  - Announce features
    └──────┬───────┘  - Build community
           │
           ▼
    ┌──────────────┐
    │  CAMPAIGN 5  │  Re-engagement
    │ RE-ENGAGE    │  - Win back inactive
    │              │  - Special offers
    └──────┬───────┘  - Last chance
           │
           ▼
    ┌──────────────┐
    │  CAMPAIGN 6  │  Conversion
    │ CONVERSION   │  - Trial → Paid
    │              │  - Upsell
    └──────────────┘  - Cross-sell
```

## Campaign 1: Welcome Series

### Goal: Make a great first impression and set expectations

**Welcome Series Structure (5 emails over 7 days):**

```markdown
## Email 1: Welcome (Immediate)

**Subject:** Welcome to [Product]! Here's what to expect

**Content:**
- Thank you for subscribing
- What you'll receive (content, frequency)
- Quick win (download, resource, tip)
- Set expectations (when to expect next email)
- Social proof (testimonial, customer count)

**Example:**
```
Subject: Welcome to DevTools Weekly! 🎉

Hi [First Name],

Thanks for subscribing to DevTools Weekly!

Every Monday, you'll get:
✅ 5 hand-picked developer tools
✅ Exclusive discounts and deals
✅ Tips to boost your productivity

**Your first gift:** Download our "50 Essential Dev Tools" guide
[Download Now]

Next email arrives Monday at 9 AM.

See you then!
[Your Name]

P.S. Join 50,000+ developers who read DevTools Weekly
```

## Email 2: Value Delivery (Day 1)

**Subject:** Here's your first [benefit]

**Content:**
- Deliver on promise from Email 1
- Provide immediate value
- No sales pitch yet
- Encourage engagement (reply, click)

**Example:**
```
Subject: 5 AI coding tools that will blow your mind

Hi [First Name],

As promised, here are this week's top developer tools:

1. **GitHub Copilot X** - AI pair programmer
   → Why it's cool: Writes entire functions from comments
   → Try it: [Link]

2. **Warp** - The terminal reimagined
   → Why it's cool: AI command suggestions
   → Try it: [Link]

[... 3 more tools]

Which one are you most excited to try? Hit reply and let me know!

[Your Name]
```

## Email 3: Social Proof (Day 3)

**Subject:** How [Customer] achieved [result] with [Product]

**Content:**
- Customer success story
- Specific results (numbers, metrics)
- How they did it
- Soft CTA (learn more, try it)

**Example:**
```
Subject: How Stripe reduced bugs by 40% with AI code review

Hi [First Name],

Meet Sarah, Senior Engineer at Stripe.

**Her challenge:** Too many bugs reaching production

**Her solution:** Automated AI code review

**Her results:**
- 40% fewer production bugs
- 5 hours/week saved per developer
- $100K+ saved in bug fixes

"The AI catches things I miss. It's like having a senior engineer review every commit."
- Sarah Chen, Stripe

Want similar results? [Start Free Trial]

[Your Name]
```

## Email 4: Education (Day 5)

**Subject:** How to [achieve goal] in [timeframe]

**Content:**
- Educational content (guide, tutorial)
- Actionable steps
- Positions your product as solution
- Medium CTA (try product)

**Example:**
```
Subject: How to ship code 10x faster (5-step guide)

Hi [First Name],

Want to ship features faster without sacrificing quality?

Here's how top developers do it:

**Step 1: Use AI code completion**
Stop writing boilerplate. Let AI handle it.
→ Saves 2 hours/day

**Step 2: Automate code review**
Catch bugs before they reach production.
→ Reduces bugs by 40%

**Step 3: Deploy automatically**
Push to main, deploy to production.
→ Ship 5x more frequently

[... Steps 4-5]

**Ready to try?** Start your free trial: [Link]

[Your Name]
```

## Email 5: Call to Action (Day 7)

**Subject:** Ready to [achieve goal]?

**Content:**
- Recap value provided
- Clear call to action
- Remove friction (free trial, no CC)
- Create urgency (limited time, bonus)

**Example:**
```
Subject: Your free trial is waiting (no credit card required)

Hi [First Name],

Over the past week, you've learned:
✅ Top AI coding tools
✅ How Stripe reduced bugs by 40%
✅ 5 steps to ship code 10x faster

**Now it's your turn.**

Start your 14-day free trial:
- No credit card required
- Cancel anytime
- Full access to all features

[Start Free Trial]

**Bonus:** Sign up today and get our "AI Coding Playbook" ($49 value, free)

Questions? Just hit reply.

[Your Name]

P.S. This offer expires in 48 hours
```
```

---

## Campaign 2: Lead Nurture

### Goal: Move leads from awareness to consideration to purchase

**Nurture Campaign Structure (10 emails over 30 days):**

```markdown
## Nurture Email Types

### Educational (40%)
**Goal:** Build trust, establish authority

**Topics:**
- Industry trends
- Best practices
- How-to guides
- Case studies

**Example subjects:**
- "The future of AI-assisted coding"
- "10 mistakes developers make (and how to avoid them)"
- "How to choose the right code assistant"

### Social Proof (30%)
**Goal:** Show others are succeeding

**Topics:**
- Customer testimonials
- Case studies
- User-generated content
- Awards/recognition

**Example subjects:**
- "How [Company] increased productivity by 10x"
- "What 50,000 developers are saying about us"
- "We won 'Best Developer Tool of 2026'"

### Product Education (20%)
**Goal:** Demonstrate value, address objections

**Topics:**
- Feature highlights
- Use cases
- Comparisons
- ROI calculators

**Example subjects:**
- "3 ways to use AI code review"
- "GitHub Copilot vs [Your Product]: Which is better?"
- "Calculate your ROI: How much time will you save?"

### Promotional (10%)
**Goal:** Drive conversions

**Topics:**
- Free trials
- Discounts
- Limited-time offers
- Webinars/demos

**Example subjects:**
- "Start your free trial (no credit card required)"
- "20% off for new customers (48 hours only)"
- "Join our live demo: AI coding in action"
```

**Nurture Campaign Timeline:**

```markdown
## 30-Day Nurture Sequence

**Week 1: Awareness**
- Day 1: Educational - "What is AI pair programming?"
- Day 3: Social proof - Customer success story
- Day 5: Educational - "5 benefits of AI code review"

**Week 2: Consideration**
- Day 8: Product education - "How [Product] works"
- Day 10: Comparison - "[Product] vs competitors"
- Day 12: Social proof - "10,000 developers trust us"

**Week 3: Decision**
- Day 15: Educational - "How to evaluate code assistants"
- Day 17: Product education - "See [Product] in action (video)"
- Day 19: Promotional - "Start your free trial"

**Week 4: Conversion**
- Day 22: Social proof - "Why developers love [Product]"
- Day 25: Promotional - "Limited offer: 20% off"
- Day 28: Last chance - "Your trial expires tomorrow"
- Day 30: Final CTA - "Ready to get started?"
```

---

## Campaign 3: Onboarding

### Goal: Get new users to "aha!" moment and first value

**Onboarding Campaign Structure (7 emails over 14 days):**

```markdown
## Email 1: Welcome & Setup (Immediate)

**Subject:** Welcome to [Product]! Let's get you set up

**Content:**
- Thank you for signing up
- Quick start guide (3 steps)
- Link to first action
- Support contact

**Example:**
```
Subject: Welcome to CodeAI! Let's get started 🚀

Hi [First Name],

Welcome to CodeAI! You're 3 steps away from shipping code 10x faster:

**Step 1:** Install the VS Code extension
[Install Now]

**Step 2:** Connect your GitHub account
[Connect GitHub]

**Step 3:** Write your first AI-assisted code
[Watch Tutorial]

Need help? Reply to this email or chat with us: [Chat Link]

Let's do this!
[Your Name]
```

## Email 2: First Value (Day 1)

**Subject:** Your first AI suggestion is waiting

**Content:**
- Guide to first action
- Show immediate value
- Celebrate small win
- Next step

**Example:**
```
Subject: Write your first function with AI (2 minutes)

Hi [First Name],

Ready to see AI in action?

**Try this:**
1. Open VS Code
2. Type: // Function to validate email
3. Press Tab
4. Watch AI write the entire function

**That's it!** You just saved 5 minutes of coding.

Now try writing a React component: [Tutorial]

[Your Name]
```

## Email 3: Feature Discovery (Day 3)

**Subject:** 3 features you might have missed

**Content:**
- Highlight key features
- Show use cases
- Encourage exploration

**Example:**
```
Subject: 3 CodeAI features that will blow your mind

Hi [First Name],

You've tried AI code completion. Here are 3 more features:

**1. Automated Code Review**
→ Catches bugs before you commit
→ Try it: [Link]

**2. Inline Documentation**
→ AI explains any code you select
→ Try it: [Link]

**3. Test Generation**
→ AI writes unit tests for you
→ Try it: [Link]

Which one will you try first?

[Your Name]
```

## Email 4: Best Practices (Day 5)

**Subject:** How to get the most out of [Product]

**Content:**
- Tips and tricks
- Common mistakes
- Pro tips

**Example:**
```
Subject: 5 tips to 10x your productivity with CodeAI

Hi [First Name],

Here's how top users get the most out of CodeAI:

**Tip 1:** Write detailed comments
→ Better comments = better AI suggestions

**Tip 2:** Use keyboard shortcuts
→ Tab (accept), Esc (reject), Ctrl+Space (trigger)

**Tip 3:** Review AI suggestions
→ AI is smart, but you're smarter. Always review.

[... Tips 4-5]

[Your Name]
```

## Email 5: Social Proof (Day 7)

**Subject:** See how [Customer] uses [Product]

**Content:**
- Customer story
- Specific workflow
- Results achieved

## Email 6: Upgrade Prompt (Day 10)

**Subject:** You've used [X] of your free [limit]

**Content:**
- Usage stats
- Value delivered
- Upgrade CTA

**Example:**
```
Subject: You've saved 10 hours this week! 🎉

Hi [First Name],

**Your CodeAI stats:**
- 500 AI suggestions used
- 10 hours saved
- 20 bugs caught

You're on the free plan (1,000 suggestions/month).

**Upgrade to Pro for:**
- Unlimited suggestions
- Priority support
- Advanced features

[Upgrade Now] - Just $10/month

[Your Name]
```

## Email 7: Feedback Request (Day 14)

**Subject:** How are we doing?

**Content:**
- Ask for feedback
- NPS survey
- Feature requests

**Example:**
```
Subject: Quick question: How's CodeAI working for you?

Hi [First Name],

You've been using CodeAI for 2 weeks. How's it going?

**Quick survey (1 minute):**
[Take Survey]

Your feedback helps us improve.

Thanks!
[Your Name]

P.S. Reply with any feature requests or questions
```
```

---

## Campaign 4: Engagement

### Goal: Keep users active and engaged

**Engagement Campaign Types:**

```markdown
## Newsletter (Weekly/Monthly)

**Structure:**
- Curated content
- Product updates
- Community highlights
- Tips and tricks

**Example:**
```
Subject: DevTools Weekly #42: AI coding tools special

Hi [First Name],

**This week's top picks:**

🔥 **Hot Tool:** Cursor - AI-first code editor
→ Why it's cool: [Description]
→ Try it: [Link]

📰 **News:** GitHub Copilot X launches
→ What's new: [Summary]
→ Read more: [Link]

💡 **Tip of the week:** Speed up your terminal with aliases
→ How to: [Tutorial]

🎉 **Community:** Meet Sarah, who built an AI chatbot in 1 hour
→ Her story: [Link]

See you next week!
[Your Name]
```

## Feature Announcement

**Subject:** Introducing [New Feature]

**Content:**
- What's new
- Why it matters
- How to use it
- CTA to try

## Content Digest

**Subject:** You might have missed these

**Content:**
- Recent blog posts
- Popular content
- Recommended reading

## Community Highlight

**Subject:** Meet [User] who [achievement]

**Content:**
- User story
- What they built
- How they did it
- Inspire others
```

---

## Campaign 5: Re-engagement

### Goal: Win back inactive users

**Re-engagement Campaign Structure (5 emails over 14 days):**

```markdown
## Email 1: We Miss You (Day 1)

**Subject:** We miss you, [First Name]

**Content:**
- Acknowledge absence
- Remind of value
- Soft CTA to return

**Example:**
```
Subject: We miss you! 😢

Hi [First Name],

We noticed you haven't logged in for a while.

**What you're missing:**
- New AI code review feature
- 10x faster code completion
- 50,000+ developers shipping faster

**Come back and see what's new:**
[Log In]

[Your Name]
```

## Email 2: What's New (Day 3)

**Subject:** You've been gone a while. Here's what's new

**Content:**
- Product updates
- New features
- Improvements

## Email 3: Special Offer (Day 7)

**Subject:** Come back and get [incentive]

**Content:**
- Exclusive offer
- Discount or bonus
- Limited time

**Example:**
```
Subject: Come back and get 50% off (just for you)

Hi [First Name],

We want you back!

**Special offer (just for you):**
- 50% off Pro plan (3 months)
- Exclusive access to new features
- Priority support

**Claim your offer:**
[Redeem Now]

Expires in 48 hours.

[Your Name]
```

## Email 4: Feedback Request (Day 10)

**Subject:** Why did you stop using [Product]?

**Content:**
- Ask for feedback
- What went wrong
- How to improve

**Example:**
```
Subject: Quick question: Why did you leave?

Hi [First Name],

We noticed you stopped using CodeAI.

**Can you help us improve?**

What made you stop?
- [ ] Too expensive
- [ ] Missing features
- [ ] Too complex
- [ ] Found alternative
- [ ] Other: _______

[Submit Feedback]

Your input helps us build a better product.

Thanks,
[Your Name]
```

## Email 5: Last Chance (Day 14)

**Subject:** Last chance to save your account

**Content:**
- Final reminder
- Account deletion warning
- Last CTA

**Example:**
```
Subject: Your account will be deleted in 7 days

Hi [First Name],

This is our last email.

Your CodeAI account will be deleted in 7 days due to inactivity.

**Want to keep your account?**
[Log In Now]

Otherwise, we'll delete your data on [Date].

We hope to see you again.

[Your Name]
```
```

---

## Campaign 6: Conversion

### Goal: Convert free users to paid customers

**Conversion Campaign Triggers:**

```markdown
## Trigger 1: Usage Limit Reached

**Subject:** You've used [X]% of your free [limit]

**Content:**
- Usage stats
- Value delivered
- Upgrade CTA

## Trigger 2: Trial Expiring

**Subject:** Your trial expires in [X] days

**Content:**
- Remind of trial end date
- Show value received
- Upgrade to continue

**Example:**
```
Subject: Your trial expires in 3 days

Hi [First Name],

Your 14-day trial ends on [Date].

**Your stats:**
- 1,000 AI suggestions used
- 20 hours saved
- 50 bugs caught

**Don't lose access!**
[Upgrade to Pro] - Just $10/month

Questions? Reply to this email.

[Your Name]
```

## Trigger 3: Feature Gated

**Subject:** Unlock [Feature] with Pro

**Content:**
- Feature they tried to use
- Why it's valuable
- Upgrade to access

## Trigger 4: Abandoned Cart

**Subject:** You left something behind

**Content:**
- Remind of cart
- Remove friction
- Incentive to complete

**Example:**
```
Subject: Complete your purchase and save 10%

Hi [First Name],

You started upgrading to Pro but didn't finish.

**Complete now and get 10% off:**
[Complete Purchase]

Questions? Chat with us: [Link]

[Your Name]
```
```

---

## Email Best Practices

### Subject Lines

```markdown
## Subject Line Formula

**Length:** 40-50 characters (mobile-friendly)

**Formulas:**
- **Curiosity:** "You won't believe what happened..."
- **Benefit:** "Save 10 hours/week with this trick"
- **Urgency:** "Last chance: Offer expires tonight"
- **Personalization:** "[First Name], your report is ready"
- **Question:** "Are you making this mistake?"
- **Number:** "5 ways to ship code faster"

**Examples:**

✅ Good:
- "Your free trial expires tomorrow"
- "How Stripe reduced bugs by 40%"
- "5 AI tools every developer needs"

❌ Bad:
- "Newsletter #42" (boring)
- "URGENT: READ THIS NOW!!!" (spammy)
- "You won't believe this amazing offer" (clickbait)

**A/B Test:**
- Test 2 subject lines per campaign
- Send to 10% of list each
- Winner goes to remaining 80%
```

### Email Copy

```markdown
## Copywriting Best Practices

**Structure:**
1. **Hook:** Grab attention (first sentence)
2. **Body:** Deliver value (2-3 paragraphs)
3. **CTA:** Clear next step (one primary CTA)
4. **P.S.:** Reinforce CTA or add urgency

**Tone:**
- Conversational (write like you talk)
- Personal (use "you" and "I")
- Concise (short sentences, short paragraphs)
- Scannable (bullet points, bold text)

**Example:**
```
Subject: Ship code 10x faster (here's how)

Hi [First Name],

Want to ship features faster? [HOOK]

Here's the secret: Let AI handle the boring stuff. [BODY]

Top developers use AI for:
- Boilerplate code (saves 2 hours/day)
- Code review (catches bugs early)
- Documentation (writes itself)

**Ready to try?** [CTA]
[Start Free Trial]

[Your Name]

P.S. No credit card required. Cancel anytime. [P.S.]
```

**Length:**
- Short emails (50-100 words): Quick tips, announcements
- Medium emails (100-300 words): Tutorials, case studies
- Long emails (300+ words): Comprehensive guides, stories

**Personalization:**
- Use first name
- Reference past behavior
- Segment by interests
- Dynamic content blocks
```

### Call-to-Action

```markdown
## CTA Best Practices

**Placement:**
- Above the fold (visible without scrolling)
- Repeat 2-3 times in long emails
- End with CTA

**Design:**
- Button (not just text link)
- Contrasting color
- Large enough to click (mobile)
- Whitespace around it

**Copy:**
- Action-oriented ("Start Free Trial" not "Click Here")
- Benefit-focused ("Save 10 Hours/Week" not "Sign Up")
- Urgent ("Start Today" not "Learn More")

**Examples:**

✅ Good:
- [Start Free Trial →]
- [Get 50% Off Now →]
- [Download Guide →]

❌ Bad:
- [Click Here]
- [Submit]
- [Learn More]
```

---

## Email Segmentation

```markdown
## Segmentation Strategies

### Behavioral Segmentation
**Based on actions:**
- Opened email (engaged)
- Clicked link (interested)
- Purchased (customer)
- Abandoned cart (almost converted)
- Inactive (re-engage)

**Example:**
- Segment: Opened last 3 emails but didn't click
- Campaign: Send more compelling CTAs

### Demographic Segmentation
**Based on attributes:**
- Job title (Developer, Manager, CTO)
- Company size (1-10, 10-50, 50+)
- Industry (Tech, Finance, Healthcare)
- Location (North America, Europe, Asia)

**Example:**
- Segment: CTOs at companies with 50+ employees
- Campaign: Enterprise features, ROI calculator

### Lifecycle Segmentation
**Based on stage:**
- Subscriber (not customer yet)
- Trial user (evaluating)
- Active customer (using product)
- Churned customer (stopped using)

**Example:**
- Segment: Trial users on day 10 of 14-day trial
- Campaign: Upgrade reminder, show value

### Engagement Segmentation
**Based on activity:**
- Highly engaged (opens every email)
- Moderately engaged (opens some emails)
- Disengaged (hasn't opened in 30 days)

**Example:**
- Segment: Disengaged (no opens in 30 days)
- Campaign: Re-engagement series or unsubscribe
```

---

## Email Deliverability

```markdown
## Avoid Spam Folder

### Technical Setup
- [ ] SPF record configured
- [ ] DKIM signature enabled
- [ ] DMARC policy set
- [ ] Dedicated IP address (for high volume)
- [ ] Warm up new IP gradually

### Content Best Practices
- [ ] Avoid spam trigger words (FREE, URGENT, BUY NOW)
- [ ] Don't use ALL CAPS
- [ ] Limit exclamation marks!!!
- [ ] Balance text and images (not image-only)
- [ ] Include plain text version
- [ ] Add unsubscribe link (required by law)

### List Hygiene
- [ ] Remove bounced emails
- [ ] Remove inactive subscribers (no opens in 6 months)
- [ ] Double opt-in for new subscribers
- [ ] Easy unsubscribe process
- [ ] Monitor spam complaints (<0.1%)

### Sender Reputation
- [ ] Consistent sending schedule
- [ ] Gradual volume increases
- [ ] High engagement rate (>20% opens)
- [ ] Low bounce rate (<2%)
- [ ] Low spam complaint rate (<0.1%)
```

---

## Email Analytics

```markdown
## Key Metrics

### Open Rate
**Formula:** Opens / Delivered × 100
**Benchmark:** 15-25% (varies by industry)
**Improve:** Better subject lines, send time optimization

### Click-Through Rate (CTR)
**Formula:** Clicks / Delivered × 100
**Benchmark:** 2-5%
**Improve:** Compelling CTAs, relevant content

### Click-to-Open Rate (CTOR)
**Formula:** Clicks / Opens × 100
**Benchmark:** 10-20%
**Improve:** Better email content, clearer CTAs

### Conversion Rate
**Formula:** Conversions / Delivered × 100
**Benchmark:** 1-5% (varies by goal)
**Improve:** Optimize landing page, reduce friction

### Bounce Rate
**Formula:** Bounces / Sent × 100
**Benchmark:** <2%
**Types:** Hard bounce (permanent), soft bounce (temporary)
**Improve:** Clean list, remove invalid emails

### Unsubscribe Rate
**Formula:** Unsubscribes / Delivered × 100
**Benchmark:** <0.5%
**Improve:** Better targeting, frequency capping

### List Growth Rate
**Formula:** (New subscribers - Unsubscribes) / Total subscribers × 100
**Benchmark:** 2-5% per month
**Improve:** Lead magnets, referral programs
```

---

## Email Campaign Checklist

**Before sending:**
- [ ] Subject line tested (A/B test)
- [ ] Preview text written
- [ ] From name and email set
- [ ] Email copy proofread
- [ ] Links tested (all work)
- [ ] Images optimized (load fast)
- [ ] Mobile preview checked
- [ ] Plain text version created
- [ ] Unsubscribe link included
- [ ] Segment selected
- [ ] Send time optimized

**After sending:**
- [ ] Monitor deliverability (bounce rate)
- [ ] Track opens and clicks
- [ ] Analyze conversions
- [ ] Review feedback/replies
- [ ] Document learnings
- [ ] Plan next campaign

---

## Common Email Mistakes

### Mistake 1: No Segmentation
**Problem:** Sending same email to everyone

**Solution:** Segment by behavior, demographics, lifecycle

### Mistake 2: Too Frequent
**Problem:** Emailing daily, annoying subscribers

**Solution:** Find optimal frequency (test 1x, 2x, 3x per week)

### Mistake 3: No Personalization
**Problem:** Generic "Dear Customer" emails

**Solution:** Use first name, reference past behavior

### Mistake 4: Weak Subject Lines
**Problem:** Boring, generic subject lines

**Solution:** Test curiosity, benefit, urgency formulas

### Mistake 5: No Clear CTA
**Problem:** Multiple CTAs, unclear next step

**Solution:** One primary CTA per email

### Mistake 6: Not Mobile-Friendly
**Problem:** Email looks bad on mobile (50%+ of opens)

**Solution:** Responsive design, test on mobile devices

---

## Integration with Other Skills

**Use before email campaigns:**
- `gtm-strategy` - Define target audience
- `funnel-builder` - Map customer journey

**Use during email campaigns:**
- `seo-optimizer` - Drive traffic to landing pages
- `user-research` - Understand subscriber needs

**Use after email campaigns:**
- `competitor-analysis` - Benchmark performance
- `funnel-builder` - Optimize conversion

---

**Remember:** Email marketing is about building relationships, not just selling. Provide value first, sell second. Test everything, optimize continuously.
