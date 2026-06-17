# LeadIt

**Intelligent Entrepreneurial Orientation Engine**

LeadIt is an AI orientation platform that tells a Tunisian entrepreneur where their project actually stands, why, and what to do next. It runs an adaptive diagnostic that classifies project maturity and flags the gap between self-perception and reality, scores the project across five explainable dimensions, and generates a personalised roadmap grounded in real Tunisian support programs and financing devices. Every output is traceable — to a criterion, a score breakdown, or a real source.

> AINS Hackathon 2026 · AI for Entrepreneurship
> First Submission — Concept & Prototype Foundation

---

## Team

**Tey bel Bondo9**

| Member | Role |
|---|---|
| Ala Bouhaouel | Technical conception & system design |
| Amin Hmida | Taxonomy criteria model |
| Adem Gaddour | Knowledge base construction |
| Ahmed Ellouze | Product & integration |

---

## The Problem

Entrepreneurs at the early and growth stages in Tunisia face compounding orientation failures. They misjudge their own maturity — a founder who believes they are financing-ready may have no validated business model. The information that exists is fragmented, generic, and never contextualised to a specific project profile. Existing tools answer questions but do not diagnose; they provide information but do not produce a structured, evidence-based assessment of where a project actually stands and what it actually needs.

The result is misaligned orientation: entrepreneurs are sent to programs they don't qualify for, offered resources that don't match their real stage, and left without a clear picture of what is genuinely blocking their progress.

## The Solution

LeadIt is a multi-agent AI platform built around three mandatory, interacting engines:

**1. Diagnose** — An adaptive diagnostic engine classifies the project into one of six maturity stages using factual, evidence-linked criteria. It detects the gap between the entrepreneur's self-assessed stage and the system's classification, and surfaces priority blockers.

**2. Score** — An explainable scoring engine evaluates the project across five composite dimensions (Market, Commercial Offer, Innovation, Scalability, Green), each decomposed into weighted sub-criteria with plain-language justification. Anomalies and inconsistencies are flagged.

**3. Guide** — A RAG-grounded roadmap engine retrieves relevant resources from a curated knowledge base of real Tunisian support programs, financing devices, and administrative procedures, and generates a personalised, ordered action plan. Every recommendation cites its source.

The integration is the differentiator: a diagnostic gap triggers resource retrieval; a low sub-score surfaces targeted roadmap actions. The three engines interact through a shared project profile — they don't just coexist as independent panels.

---

## Target Users

**Primary:** Young Tunisian founders and unemployed graduates exploring entrepreneurship as a career path — need an objective assessment of project maturity, readiness, and the concrete next steps required to grow.

**Secondary:** Support-program officers (APII, ANETI, incubators) who triage and orient applicants with evidence-based diagnosis instead of self-reported claims; ecosystem actors and investors who need a consistent, explainable scoring lens to compare projects.

---

## System Architecture

### Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js + Tailwind CSS |
| Backend | FastAPI (Python) |
| Orchestration | LangGraph |
| LLM | Qwen / Llama (open-source) |
| Database | PostgreSQL |
| Vector DB | Qdrant |
| RAG | LangChain |

### Multi-Agent Pipeline

```
User Input
    ↓
Profile Builder (adaptive intake, branching questionnaire)
    ↓
Shared Project Profile (PostgreSQL, persisted across sessions)
    ↓
Diagnostic Agent (rule-based classifier → stage + perception gap + blockers)
    ↓
┌──────────┬──────────────┬────────────┬──────────────┬─────────┐
│  Market  │  Commercial  │ Innovation │ Scalability  │  Green  │
│  Agent   │    Agent     │   Agent    │    Agent     │  Agent  │
└──────────┴──────────────┴────────────┴──────────────┴─────────┘
    ↓
Gap Analyzer + Rule Engine
    ↓
Roadmap Agent  ←── Tunisian Knowledge Base (Qdrant)
    ↓
Explainable Report (dashboard + Mon Parcours tracking view)
    ↓
Grounded Conversational Assistant (thin LLM layer, not the core product)
```

### Why Multi-Agent

Each agent has a single responsibility, making outputs explainable and traceable to specific rules or criteria. The five scoring agents run in parallel to reduce latency. The architecture maps directly to the hackathon's evaluation dimensions and is designed to extend (new scoring dimensions, new KB sources) without restructuring the pipeline.

---

## Taxonomy Criteria Model

**Status: Built** (`taxonomy.py`)

Six ordered maturity stages. A project sits at the highest stage whose factual criteria all pass. Each criterion is a function that returns one of three values:

| Return | Meaning |
|---|---|
| `True` | Criterion satisfied |
| `False` | Not satisfied — a real, identified gap |
| `None` | Data missing — surfaced as uncertainty, not hidden as failure |

This three-way separation is what allows the system to handle incomplete profiles gracefully and signal ambiguity rather than mask it.

### Stage criteria

| Stage | Criteria |
|---|---|
| 1 · Ideation | Default entry — no criteria required |
| 2 · Market Validation | `has_validated_problem` (interviews, pre-orders, or pilot) |
| 3 · Structuration | `business_model_documented` · `team_core_complete` · `legal_form_started` |
| 4 · Fundraising | `has_paying_customers` · `financial_docs_exist` · `legal_form_registered` |
| 5 · Launch Planning | `funding_or_self_financing` · `distribution_channel_tested` |
| 6 · Growth | `recurring_revenue` (3+ consecutive months) · `client_base_beyond_pilot` |

Each criterion is tagged with a blocker domain (`financier`, `legal`, `marche`, `organisationnel`, `technique`) to enable cross-module integration: a gap detected by the diagnostic feeds directly into the KB retrieval filter for the roadmap engine.

### Open decision

The rule for `None` propagation is still being finalised: when a higher-stage criterion returns `None` (missing data rather than failure), should the project be blocked at the prior stage or held at a provisional classification pending further information? This is the key design question for the "handles ambiguity gracefully" acceptance criterion.

---

## Scoring Dimensions

**Status: In progress**

Five composite scores, each decomposing into weighted sub-criteria:

| Score | Sub-dimensions |
|---|---|
| Market | Market share · customer validation · revenue model viability |
| Commercial Offer | Value-prop clarity · product readiness · pricing · offer-need fit |
| Innovation | Local novelty · tech intensity · barrier to entry · departure from existing offerings |
| Scalability | Replicability · manual dependency · deployment cost · addressable reach |
| Green | Environmental impact · SDG alignment · resource efficiency · circular economy |

Weights and aggregation logic are being defined. Composite scores will not be simple averages — a weak score on a fundamental dimension must not be masked by strong scores elsewhere.

---

## Knowledge Base

**Status: In construction** — 23 entries structured, target ≥ 30

A curated catalogue of real Tunisian entrepreneurship support resources, structured for retrieval. Each entry includes: name, provider, category, description, eligibility, amount/terms, applicable maturity stage, how to apply, and source URL.

### Categories covered

| Category | Institutions |
|---|---|
| Support & accompaniment | APII pépinières, ANPE, ODC, incubators |
| Financing | BFPME, BTS, Startup Act funds, Flat6Labs |
| Administrative & regulatory | Startup Label, RNE, guichet unique, CEPEX |
| Ecosystem actors | CONECT, IEEE, Wiki Startup, AINS 4.0 |

### Remaining work

- Reach 30+ entries with verified source URLs on every row
- Re-verify financial amounts against official sources (BTS ceilings, BFPME rates)
- Add a `gap_domain` column to enable join with the diagnostic engine's blocker detection
- Cover the Technical blocker domain (technopoles, R&D, tech-transfer programmes)

---


---

## Evaluation Plan

**Status: Planned**

At least one measurable metric will be defined and run on a labelled test set before the final submission:

- **Classification accuracy** — a small labelled set of synthetic entrepreneur profiles with ground-truth maturity stages, scored against the diagnostic engine's output.
- **Retrieval relevance** — for a given diagnostic output, do the top-K KB results match the identified gaps and stage? Measured by precision@K on a hand-labelled evaluation set.

The test protocol and results will be documented in `evaluation/`.

---

## Roadmap to Final Submission

| Status | Milestone |
|---|---|
| ✅ | Problem framing & target users |
| ✅ | 6-stage taxonomy criteria model (True/False/None) |
| ✅ | 5 scoring dimensions defined |
| ✅ | Knowledge base started — 23 real entries |
| ✅ | Architecture & integration plan |
| ⬜ | Finalise scoring weights & aggregation logic |
| ⬜ | Build diagnostic classifier + None-handling rule |
| ⬜ | Complete KB to 30+ verified entries |
| ⬜ | Implement RAG roadmap pipeline |
| ⬜ | Build dashboard + Mon Parcours tracking view |
| ⬜ | Evaluate on a labelled test set |

---

---

## Constraints & Non-Functional Requirements

- **Responsiveness:** User-facing queries return results within a few seconds on realistic dataset sizes.
- **Reliability:** The system handles missing, dirty, or incomplete data without crashing. The `None` return in the taxonomy model is the primary mechanism — uncertainty is surfaced, not hidden.
- **Privacy:** All sensitive data (personal, financial, project-specific) will be masked or anonymised. No real entrepreneur data is used without consent.
- **Language:** User-facing components in French and/or Arabic for the primary Tunisian user population.
- **Scalability:** PostgreSQL + Qdrant are chosen for their ability to scale to larger datasets and concurrent users. The multi-agent architecture allows horizontal scaling of scoring agents.

---

## License

This project was built for the AINS Hackathon 2026. License terms to be determined.

---

*LeadIt — Diagnostic, score et feuille de route pour l'entrepreneur tunisien.*
