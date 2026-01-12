# Sales-Lead-Scraper
Sales Lead Scraper is an agentic B2B prospecting pipeline that automatically discovers, enriches, and qualifies high-intent sales leads from public web sources and LinkedIn-style signals.

Instead of blindly scraping profiles, the system focuses on purposeful lead discovery: identifying real buyers based on role, company context, and verifiable buying signals. The output is structured, explainable, and directly usable for outbound sales workflows.

This project is built as a modular, automation-first pipeline using n8n, LLM-based reasoning, and entity-aware data normalization.
<img width="3485" height="1528" alt="image" src="https://github.com/user-attachments/assets/98d7665e-8c80-4e15-a91c-46308e99d0ec" />

Start
  │
  ▼
Schedule / Manual Trigger
  │
  ▼
Load Configuration
(Roles, Industries, Keywords)
  │
  ▼
Agentic Lead Discovery
(LLM + Web / Search Sources)
  │
  ▼
Entity Normalization
(Create entity keys)
  │
  ▼
Deduplication
(Check against existing leads)
  │
  ▼
Enrichment & Qualification
(Context, intent, buying signals)
  │
  ▼
Sales Pitch Generation
(Context-rich outreach narrative)
  │
  ▼
Scoring & Ranking
(Relative to existing leads)
  │
  ▼
Output
(Google Sheets / JSON)
  │
  ▼
End


## High-Level Workflow
**1. Trigger & Keyword Generation**
- A scheduled or manual trigger initiates the pipeline. The schedule is set to run on the first day of every month.
- Keywords are generated for people, companies, and events based on:
  - Target roles (e.g., Head of AI, VP Engineering)
  - Technical signals (GenAI, LLMs, governance, compliance)
  - Industry and use-case context
**2. Data Discovery & Scraping**
- LinkedIn and Web sources are queried using structured keyword sets.
- Raw results may include:
  - People profiles
  - Company pages
  - Event or conference listings
**3. Entity Normalization & Deduplication**
- Each record is assigned a stable entity_key:
  - li:in:* for individuals
  - li:co:* for companies
  - event:* for conferences or summits
- This prevents repeated analysis of the same entity across runs and sources.
**4. AI-Based Lead Qualification**
- LLM agents evaluate each entity using contextual reasoning:
  - Is this a decision-maker or influencer?
  - Does the company show real buying intent?
  - Are there credible AI, security, or platform signals?
- Leads are scored, tiered (Cold / Warm / Hot), and explained.
**5. Structured Output**
- Results are written to Google Sheets
- Each record includes:
  - Lead score and tier
  - Buying signals
  - Rationale for qualification
  - Recommended next action
  - Details about the lead

## Why This Is Purposeful
There are already many lead scrapers and prospecting tools available. Most of them focus on extracting large volumes of contacts and selling access through rigid, subscription-based platforms. This project was built to solve a different problem.

Sales Lead Scraper is designed to be customizable to the company and use case, not a one-size-fits-all database. Instead of just collecting leads, it uses LLMs to understand, evaluate, and contextualize them—reducing the manual effort typically required after scraping.

### Key ways this system is different:
- **LLM-powered lead vetting and pitching:** Leads are not only qualified, but also enriched with reasoning, buying signals, and a short, contextual pitch. This helps users move directly from discovery to outreach without manually figuring out “why this lead matters” or “what to say.”
- **Multi-source enrichment per lead:** Each lead is evaluated using information gathered from multiple sources (profiles, company context, events, public mentions). This creates a single snapshot view of the lead instead of fragmented data spread across tools.
- **Reduced manual research time:** The system is built to minimize time spent searching, validating, and cross-checking leads. Users can focus on connecting and selling, not on assembling information.
- **Higher-quality leads than service-based databases:** Unlike static CRM or lead-list providers, leads here are evaluated dynamically using context and intent. This typically results in fewer leads, but with higher relevance and conversion potential.
- **Cost-efficient by design:** This approach avoids expensive per-seat or per-lead subscriptions. Costs are primarily usage-based (LLM calls and automation runtime), making it significantly more affordable than traditional CRM-style prospecting tools.

## Design Principles

- **Entity-aware modeling:** People, companies, and events are treated as distinct entities, allowing more accurate analysis and deduplication.
- **Context over keywords:** Titles and keywords alone do not determine buying intent. Company maturity, role relevance, and technical signals matter more.
- **Explainability:** Every lead includes a clear explanation of why it was qualified, not just a numerical score.
- **Automation-ready output:** Outputs are structured and clean, requiring no manual cleanup before outreach or downstream automation.

## Cost Considerations

Costs are intentionally controlled and predictable.

The system is designed to minimize recurring subscription fees and instead rely on usage-based costs, while still producing higher-quality leads than traditional scraping or CRM tools.

### Primary Cost Drivers

#### 1. LLM Usage (Discovery, Enrichment, Vetting, and Pitching)
LLMs are used as **agentic search and reasoning layers**, not just for post-processing. This includes:
- Discovering leads from multiple web sources
- Aggregating and summarizing information per lead
- Vetting and scoring leads based on context and intent
- Extracting buying signals
- Generating short, contextual sales pitches
- Providing explainability ("why this is a lead")

**Token usage per lead (average):**
- ~6,000 tokens per lead (search + reasoning + output)

**Example cost calculation (OpenAI-style pricing):**
- Approximate cost: $0.01 per 1,000 tokens
- Cost per lead:  
  6,000 tokens × $0.01 / 1,000 ≈ **$0.06**
- Cost per run (400 leads):  
  400 × $0.06 ≈ **$24**

Prompt design favors structured, bounded outputs to keep costs stable and predictable.

#### 2. LinkUp Service (Search & Discovery Augmentation)
LinkUp is used as a complementary discovery source because it provides reliable search results at a low recurring cost.

- $30 per month
- 500 credits per month
- Credits are consumed per search, not per lead

As the pipeline runs repeatedly, stable entity keys are used to detect duplicates.  
This reduces the number of new leads processed per run, lowering both LinkUp credit usage and LLM costs over time.

#### 3. n8n Execution
- Can be run locally (free)
- Or on n8n Cloud
- No per-lead or per-action pricing

#### 4. Data Storage
- Google Sheets is used as the current database
- Free, simple, and sufficient for early-stage lead tracking
- Can be replaced later with a database or CRM if required

### Why Not Use Extruct AI (or Similar Tools)
Extruct AI offers scraping capabilities inside n8n, but was intentionally not used for the following reasons:
- **Cost**
  - $59 per month for 1,000 credits
  - Credits are quickly consumed with limited yield per source
- **Scope**
  - Primarily a scraper
  - Does not include:
    - Lead discovery via reasoning
    - Lead vetting or scoring
    - Buying-signal analysis
    - Sales pitch generation
    - Explainability
- **Lower information density**
  - Fewer sources per lead
  - No consolidated snapshot view

In contrast, this system:
- Uses agentic LLM search to discover leads across multiple sources
- Produces qualified, pitch-ready leads
- Consolidates discovery, vetting, and messaging into a single workflow

### Time Savings vs Manual Research
- Automated run time: ~35–45 minutes per batch
- Manual research time: ~45–60 minutes per lead

For ~400 leads:
- Manual effort: **300+ hours**
- Automated effort: **< 1 hour of compute time**

This eliminates the need for manual:
- Cross-platform searching
- Information aggregation
- Intent inference
- Outreach context writing

### Cost Comparison

| Solution                 | Monthly Cost          | What You Get                                           | Lead Quality |
|--------------------------|-----------------------|--------------------------------------------------------|--------------|
| This System              | ~$30 (LinkUp) + usage | Discovery + vetting + pitch + explainability           | High         |
| Extruct AI               | $59 / 1,000 credits   | Scraping only                                          | Medium       |
| Apollo / ZoomInfo        | $80–$300+             | Static databases, limited context                      | Medium       |
| LinkedIn Sales Navigator | $99+                  | Profile access, manual research required               | Medium       |
| Manual SDR Research      | $$$ (time cost)       | High quality, extremely time-intensive                 | High         |

### Typical Cost Profile Summary
- Low to moderate cost per run
- Primary variables:
  - Number of new entities discovered
  - LLM model choice
- Designed to scale horizontally, not exponentially

This system is intentionally cheaper than:
- Paid lead databases
- High-volume LinkedIn scraping tools
- Manual SDR research time

## Why These Design Choices
This project was built with a pragmatic product-engineering mindset: deliver a complete, usable system quickly under real-world constraints, while keeping costs and maintenance low.

### Why n8n
n8n was chosen as the orchestration layer to enable fast iteration on a multi-step, stateful pipeline without rebuilding infrastructure.

- Visual, auditable workflows that are easy to debug and modify
- Native support for branching, retries, scheduling, and failure handling
- Clean integrations with APIs, webhooks, and Google Sheets
- Low learning curve, enabling rapid end-to-end delivery

Given limited development time alongside a demanding full-time role, n8n made it possible to ship a complete pipeline significantly faster than a fully custom-coded approach.

### Why LLMs for Discovery and Qualification
Lead discovery and qualification require contextual reasoning, not just data extraction.

LLMs are used as agentic search and reasoning layers to:
- Discover leads across multiple web sources
- Aggregate and summarize fragmented information
- Evaluate intent, relevance, and role fit
- Extract buying signals and generate short outreach context

This produces fewer leads, but with significantly higher signal and far less manual research required.

### Why Entity Keys Matter
Stable entity keys prevent repeated analysis of the same person or company and keep the system efficient over time.
- Reduce duplicate processing and costs
- Enable reliable incremental runs
- Maintain consistency as the lead database grows

### Why Not Microsoft Copilot or a Fully Custom-Coded Pipeline
This system required orchestration, state, retries, scheduling, and multi-source coordination.
- Copilot assists with coding but does not orchestrate workflows
- A fully custom solution would require building and maintaining significant infrastructure
n8n provided the fastest path to a production-ready system while keeping the logic transparent and extensible.

### Trade-offs and Limitations
- LLM outputs are probabilistic; mitigated through structured prompts
- Optimized for lead quality, not massive scale
- Google Sheets is sufficient for early use but not a long-term database
- Workflow-first design favors speed over full code-level control

These are conscious trade-offs aligned with the goal of high-signal lead discovery with minimal manual effort.

## Next Steps / Roadmap

The current system is intentionally scoped to validate lead quality, cost efficiency, and end-to-end automation. The next phase focuses on improving robustness, decision quality, and scalability.

Planned improvements include:

- **Zero-lead handling and recovery**  
  Introduce explicit logic for scenarios where a run produces zero new leads, including:
  - Automatically widening search criteria
  - Falling back to adjacent roles, industries, or events
  - Logging and surfacing “no-op” runs as signals rather than failures

- **Dynamic re-ranking of leads**  
  Re-rank newly discovered leads against existing leads stored in Google Sheets, allowing:
  - Continuous prioritization as new information arrives
  - Relative scoring rather than isolated per-run scoring
  - Better focus on the highest-value opportunities at any point in time

- **Deeper event-level enrichment**  
  Improve event discovery and enrichment by:
  - Expanding speaker, sponsor, and attendee signal extraction
  - Linking events to companies and decision-makers already in the database
  - Using events as temporal buying-intent signals rather than standalone leads

- **Prompt optimization for scale**  
  Reduce prompt sizes and redundant context where possible to:
  - Lower token usage
  - Improve latency
  - Enable higher-volume runs without linear cost increases

- **Multi-agent validation (optional)**  
  Add lightweight cross-checking agents to validate lead decisions in high-stakes scenarios, without materially increasing cost.

- **CRM-native exports**  
  Enable direct exports to systems like HubSpot and Salesforce once lead schemas stabilize.

These improvements are focused on making the system more resilient, more cost-efficient, and more useful as lead volume and usage increase.

## How to Use This in n8n
### Prerequisites
- n8n (running locally or on n8n Cloud)
- OpenAI or compatible LLM API key
- Google account (optional, used for Google Sheets output)

### Setup
- Clone the repository
- git clone https://github.com/your-username/sales-lead-scraper.git
- Import the n8n workflow
- Open n8n
- Navigate to Workflows → Import
- Upload the workflow JSON provided in this repository
- Configure credentials
  - Add LLM API credentials
  - Add Google Sheets credentials if Sheets is used as the data store
  - Update configuration nodes
- Define target roles and industries
- Adjust keyword sets for discovery
- Configure output destinations (Sheets, JSON, etc.)
### Run or schedule
- Run the workflow manually for ad-hoc discovery
- Use a Schedule Trigger for recurring runs
- Review results directly in Google Sheets or as structured JSON output

Output Format
Each lead is emitted in a structured, automation-ready format:

{
  "entity_type": "company",
  "entity_key": "li:co:example",
  "lead_score": 82,
  "lead_tier": "Warm",
  "buying_signals": [
    "Active GenAI tooling",
    "Mentions AI governance initiatives"
  ],
  "why_this_is_a_lead": "Company shows active AI adoption with clear compliance concerns.",
  "recommended_next_step": "Research AI security ownership and outreach"
}

This structure is designed to be consumed directly by spreadsheets, CRMs, or downstream automation without additional cleanup.

**Disclaimer**
This project is intended for ethical, compliant lead research using publicly available information.
Users are responsible for complying with applicable platform terms and data privacy regulations.
