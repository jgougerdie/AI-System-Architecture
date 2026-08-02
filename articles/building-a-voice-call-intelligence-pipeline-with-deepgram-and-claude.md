**Building** **a** **Voice** **Call** **Intelligence** **Pipeline**
**with** **Deepgram** **and** **Claude**

How to transform unstructured phone conversations into structured,
actionable business intelligence using a two-stage AI extraction
architecture.

Javad Gougerdie

CTO & AI Systems Architect
[<u>https://jgougerdie-8e2879f9.viktor.space/</u>](https://jgougerdie-8e2879f9.viktor.space/)

**The** **Problem:** **Business** **Intelligence** **Trapped** **in**
**Audio**

Every business runs on phone calls. Sales conversations, customer
support interactions, vendor negotiations, project check-ins — critical
decisions happen verbally and disappear the moment the call ends. Action
items get forgotten. Deadlines slip. Customer frustration goes
undetected until it's too late.

The standard fix — manual note-taking or post-call summaries — doesn't
scale. Human recall is unreliable, subjective, and inconsistent across
team members. What's needed is a deterministic system that converts raw
audio into structured, machine-readable business data — automatically,
every time.

*The* *goal* *is* *not* *transcription.* *Transcription* *is* *a*
*commodity.* *The* *goal* *is* *structured* *intelligence* *extraction*
*—* *turning* *messy* *human* *conversation* *into* *validated,*
*actionable* *data* *objects.*

**Architecture** **Overview:** **The** **Two-Stage** **Pipeline**

The solution is a two-stage extraction architecture. Each stage handles
a fundamentally different problem domain, and separating them is
critical to reliability:

> • Stage 1: Speech-to-Text — Deepgram Nova-2 converts raw audio into
> accurate text with smart formatting
>
> • Stage 2: AI Extraction — Claude 3.5 Sonnet parses the transcript and
> outputs a strictly validated JSON schema containing action items,
> customer sentiment, and key dates

This separation follows a core production AI principle: never ask one
component to solve two fundamentally different problems. Audio signal
processing and semantic reasoning require entirely different
architectures. Combining them into a single step introduces compounding
failure modes.

> 1 \| P a g e

<img src="./aq3ksxhd.png" style="width:6.5in;height:4.33333in" />

Voice Call Intelligence Pipeline — from raw audio through speech-to-text
transcription to schema-validated structured JSON output

**Stage** **1:** **Deepgram** **Nova-2** **—** **Speech-to-Text**
**Engine**

The first stage handles the signal processing problem: converting an
analog audio stream into clean, readable text. Deepgram's Nova-2 model
was selected for production use because of three specific properties:

> • Accuracy on conversational audio — Nova-2 handles overlapping
> speakers, background noise, and informal speech patterns that plague
> generic speech models
>
> • Smart formatting — automatic punctuation, capitalization, and number
> formatting reduce downstream preprocessing
>
> • Low latency — the API processes files quickly enough for
> near-real-time pipeline execution

The implementation reads audio files directly into a binary buffer and
sends them to Deepgram's RESTAPI with smart formatting enabled. The
transcript is extracted from the first channel's top alternative — a
deliberate architectural decision to take the highest-confidence path
rather than attempting multi-hypothesis reconciliation at the
transcription layer.

*The* *transcription* *layer* *is* *deliberately* *simple.* *Its* *only*
*job* *is* *faithful* *audio-to-text* *conversion.* *All*
*interpretation* *happens* *downstream.*

**Stage** **2:** **Claude** **3.5** **Sonnet** **—** **AI**
**Extraction** **Engine**

The second stage is where unstructured conversation becomes structured
business intelligence. Claude 3.5 Sonnet receives the raw transcript and
a strict extraction prompt, and returns a schema-validated JSON object
containing three distinct data categories:

> 2 \| P a g e

**Action** **Items**

Every task mentioned in the conversation is extracted with an assignee
and a clear task description. This captures both explicit commitments
('I'll send the invoice today') and implicit obligations ('the banners
need to be ready by Thursday'). Each item is attributed to the
responsible party — whether that's an internal team member, a customer,
or an unidentified speaker.

**Customer** **Sentiment**

A single classification label — Positive, Neutral, or Negative — with a
one-sentence justification grounded in specific transcript evidence.
This isn't vague sentiment scoring. It's a concrete, auditable
assessment that operations teams can act on immediately. A 'Negative'
classification with justification like 'The customer expressed
frustration regarding a previous late order' gives the account manager
everything they need before the next interaction.

**Key** **Dates** **and** **Commitments**

Every deadline, delivery window, follow-up date, or time-sensitive
commitment mentioned in the call is extracted with its surrounding
context. This feeds directly into project management systems, CRM
follow-up queues, and calendar automation — eliminating the gap between
verbal commitments and tracked obligations.

**Why** **Zero** **Temperature** **Matters**

The extraction engine runs at temperature 0. This is a deliberate
production architecture decision, not a default. In structured data
extraction pipelines, you need deterministic, reproducible outputs. The
same transcript processed twice should produce the same JSON. Creative
variation — useful in content generation — is catastrophic in data
extraction.

Combined with a strict system prompt that prohibits conversational text,
markdown wrappers, and introductions, this creates a controlled
extraction boundary. The LLM operates as a structured reasoning
component — not a chatbot.

*Temperature* *0* *+* *strict* *system* *prompt* *+* *schema*
*enforcement* *=* *deterministic* *extraction.* *The* *model* *is* *a*
*structured* *reasoning* *engine,* *not* *a* *conversational* *agent.*

**Production** **Resilience:** **Error** **Handling** **and**
**Fallbacks**

Production pipelines fail. The architecture accounts for this with
layered error handling at each stage:

> • Transcription failures — runtime errors from Deepgram (network
> issues, unsupported formats, corrupted audio) are caught and raised
> explicitly before the extraction stage is invoked
>
> • JSON parsing failures — if Claude's response cannot be parsed as
> valid JSON (a rare but possible edge case), the system captures the
> raw response for debugging rather than silently dropping data
>
> • API processing failures — connection timeouts, rate limits, and
> service outages are caught with descriptive error messages that enable
> automated retry logic upstream
>
> • File validation — the pipeline checks for file existence before
> processing, preventing silent failures on missing or moved audio files

Each failure mode produces a structured error object rather than
crashing the pipeline. This enables integration with alerting systems,
retry queues, and dead letter storage — standard production
infrastructure patterns.

> 3 \| P a g e

**Real-World** **Output**

Here is an actual extraction result from a vendor coordination call. The
pipeline processed a recorded phone conversation between a project
manager and a metal fabrication vendor, producing the following
structured output:

The system extracted three action items — two assigned to the vendor
(complete banner delivery and send a deposit invoice) and one assigned
to the customer (pay the deposit). It classified customer sentiment as
Negative, citing frustration about a previous late order while noting
the customer remains open to moving forward. It captured two key dates:
a hard delivery deadline and a same-day payment commitment.

*This* *is* *not* *a* *summary.* *It* *is* *a* *structured* *data*
*object* *that* *can* *be* *ingested* *directly* *by* *CRM* *systems,*
*project* *management* *tools,* *and* *automated* *follow-up*
*workflows* *—* *no* *human* *reformatting* *required.*

**Integration** **Patterns**

The pipeline's JSON output is designed for direct integration with
downstream business systems:

> • CRM ingestion — action items and sentiment feed directly into
> GoHighLevel, HubSpot, or Salesforce contact records
>
> • Project management — key dates and commitments sync to Asana,
> Monday.com, or Jira as tracked deliverables
>
> • Alerting — negative sentiment triggers immediate Slack notifications
> to account managers
>
> • Analytics — aggregated sentiment trends across hundreds of calls
> surface systemic customer experience issues
>
> • Compliance — full transcript + structured extraction creates an
> auditable record of verbal commitments

**Key** **Takeaway**

Voice data is the largest untapped source of business intelligence in
most organizations. The architecture presented here — deterministic
transcription followed by controlled LLM extraction — converts that
unstructured audio into production-grade structured data. The critical
insight is separation of concerns: let the speech model handle
acoustics, let the language model handle semantics, and enforce strict
schema validation at the boundary. The result is a pipeline that
operates reliably at scale, producing consistent, actionable output from
every call.

> 4 \| P a g e
