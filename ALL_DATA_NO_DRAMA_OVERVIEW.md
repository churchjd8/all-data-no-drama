# All Data No Drama Dashboard

**Live URL:** https://alldatanodrama.vercel.app
**GitHub Repo:** https://github.com/churchjd8/all-data-no-drama

---

## What It Is

A single-page business performance dashboard called "All Data No Drama" that gives you a top-level view of all key metrics across revenue, OKRs, sales, marketing, ads, and customer service -- all in one place.

Currently built as a static HTML mockup with sample data. The next step is wiring it up to live data sources.

---

## Tech Stack

- **Frontend:** Single `index.html` file (HTML + CSS + vanilla JS, no frameworks)
- **Font:** JetBrains Mono (Google Fonts)
- **Hosting:** Vercel (auto-deploys from GitHub on push to `main`)
- **Design Style:** Light, minimal, monospace -- inspired by [analysis.arkpartners.ai](https://analysis.arkpartners.ai/crosscourt). White background, 1px borders, no shadows, no rounded corners, grid-based stat cards.

---

## Features

### 1. Revenue Tracker (Top Section)
- Annual revenue target ($2.5M) with progress bar
- Shows current revenue, period revenue, projected annual, and % to target
- Time period selector lives here (since the annual target stays constant)

### 2. Time Period Selector
- **Options:** Today, This Week, Last Week, MTD, QTD, YTD, Last 90 Days
- Switching periods updates ALL numbers across the entire dashboard (revenue stats, KPI buckets, and all sub-metrics)
- Each period has its own unique dataset with realistic sample numbers

### 3. Quarterly OKRs (Expandable)
- Three objectives for Q2 2026, each expandable on click
- Each objective has 3-4 Key Results with:
  - Progress bar showing completion %
  - Dropdown to manually change status: **On Track** (green), **Off Track** (yellow), **Needs Attention** (red)
  - Status light indicator that updates in real time

### 4. KPI Buckets (Expandable)
Each bucket shows a summary on the collapsed row (headline value, growth %, status badge) and expands into a grid of detailed stat cards.

#### Total Sales
- DTC Website Sales, Amazon Sales, Affiliate Sales, Wholesale/B2B
- Average Order Value, Conversion Rate

#### Email Marketing
- Total Subscribers, Open Rate, Click-Through Rate, Conversion Rate
- Revenue per Email, Unsubscribe Rate

#### Social Marketing
- Instagram Followers, LinkedIn Followers, TikTok Followers
- Avg Engagement Rate, Reviews (All Platforms), Avg Review Rating

#### Ad Spend & ROAS
- Total Ad Spend, Google ROAS, Meta ROAS, TikTok ROAS
- Attributed ROAS, Cost Per Acquisition

#### Customer Service
- Customer Satisfaction (CSAT), Net Promoter Score (NPS)
- Avg Resolution Time, Avg First Response Time
- Tickets Resolved, Tickets Open

### 5. Status Indicators (Red/Yellow/Green)
Every metric has a status light:
- **Green** = On Track
- **Yellow** = Off Track
- **Red** = Needs Attention

Each bucket header also has a status badge showing the overall health of that category.

### 6. Growth Tracking
Every stat shows a `+/-` change vs the prior period, color-coded green (up), red (down), or yellow (flat).

### 7. ARK's Recommendations (AI Insights Section)
A dedicated section below the KPI buckets where ARK (the AI) surfaces actionable recommendations based on the dashboard data. Modeled after the analysis style at [analysis.arkpartners.ai/crosscourt](https://analysis.arkpartners.ai/crosscourt).

Each recommendation card includes:
- **Fix number and title** -- decisive, action-oriented language ("Kill underperforming Meta placements", "Reduce CAC by tightening Google targeting")
- **Priority tag** -- High Priority (red), Medium Priority (yellow), or Quick Win (green)
- **Detailed explanation** -- references specific metrics from the dashboard (e.g., "Meta ROAS dropped to 3.6x", "CAC is $24.80")
- **Estimated impact** -- quantified outcome (e.g., "+$1,200/mo in recovered ad spend")
- **"Want me to execute this?" button** -- clicking it queues the action (switches to "Queued for execution")

Sample recommendations included:
1. Kill underperforming Meta placements (High)
2. Reduce CAC by tightening Google audience targeting (High)
3. Launch a win-back flow for email conversion rate (Medium)
4. Scale TikTok spend -- ROAS is climbing (Medium)
5. Boost AOV with post-purchase upsell (Quick Win)
6. Reduce resolution time with auto-tagging in Gorgias (Quick Win)

### 8. Ask ARK Chat Widget (Floating AI Assistant)
A floating chat button ("A") in the bottom-right corner that opens a conversational AI panel.

- **Opening message:** ARK greets you with a contextual observation about your current data ("your Meta ROAS is trending down and your CAC is creeping up")
- **Conversational interface:** Type questions and get responses about any area of the dashboard
- **Keyword-matched responses** covering: ads/ROAS, email marketing, sales/revenue, social media, customer service, OKRs, budget reallocation, and prioritization
- **Typing indicator:** Simulated "Analyzing your data..." delay for realism
- **Tone:** Direct, specific, data-backed -- ARK references actual numbers and gives concrete next steps

Example prompts that work:
- "What should I focus on this week?"
- "How are my ads performing?"
- "What's going on with email?"
- "Where should I reallocate budget?"
- "How are my OKRs tracking?"

**Note:** Currently uses local keyword matching with sample responses. Future state would connect to a live AI model (Claude API) with access to the real dashboard data for dynamic, contextual answers.

---

## File Structure

```
all-data-no-drama/
  index.html      # The entire dashboard (HTML + CSS + JS in one file)
  .gitignore      # Ignores .env, .vercel, screenshots, personal files
  .env            # API tokens (gitignored, not in repo)
```

---

## Future Integrations (Not Yet Connected)

The dashboard is built with placeholder data. Here's what each bucket would eventually connect to:

| Bucket | Likely Data Source |
|---|---|
| Sales | Shopify, Amazon Seller Central, or a data warehouse |
| Email Marketing | Klaviyo, Mailchimp, or similar ESP |
| Social Marketing | Meta Graph API, LinkedIn API, TikTok API |
| Ad Spend & ROAS | Google Ads, Meta Ads Manager, TikTok Ads |
| Customer Service | Gorgias, Zendesk, or similar CS platform |
| Revenue Target | Manual config or Stripe/accounting system |
| ARK (Recommendations) | Claude API with dashboard data context for dynamic, real-time insights |
| ARK (Chat) | Claude API conversational agent with tool access to query live data sources |

---

## Deployment

- **Auto-deploy:** Any push to `main` on the GitHub repo triggers a Vercel production deploy
- **Manual deploy:** `npx vercel --prod` from the project directory
- **Domain:** `alldatanodrama.vercel.app` (can add a custom domain in Vercel settings)

---

## Sample Business

The mockup uses "JDC Adventures" as the placeholder business name (shown in the header above the dashboard title).
