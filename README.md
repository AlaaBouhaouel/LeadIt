# LeadIt — AI Orientation Platform for Tunisian Entrepreneurs

> **Diagnostic, score et feuille de route pour l'entrepreneur tunisien.**  
> *Know where your project stands. Understand why. Know what to do next.*

---

## The Problem

Tunisia produces over 100,000 university graduates every year. A growing share turn to entrepreneurship — not always by choice, but because the formal job market leaves them without a path. They arrive with degrees, ideas, and motivation, but almost no structured way to know whether their project is ready, what's missing, and where to go next.

The result is a predictable failure pattern: a graduate overestimates their readiness, applies to a financing program they aren't eligible for, gets rejected without explanation, and exits the ecosystem — sometimes permanently. **The gap isn't talent. It's orientation.**

Existing support structures (ANETI, BTS, incubators) are fragmented, Tunis-centric, and reactive. A first-time founder in Gafsa or Sidi Bouzid has no reliable way to self-assess before showing up to an institution and being told they're not ready.

---

## Who This Is For

**Primary user:** A recent graduate — typically 22 to 30, holding a Licence or Master's, located anywhere in Tunisia — who has a project idea at some stage between concept and early validation. They are likely applying to public support programs for the first time, likely underestimating their structuration gaps, and likely without a mentor or ecosystem contact who can give them honest, calibrated feedback.

This is not a niche. ANETI alone processes tens of thousands of dossiers annually. The *diplômé-chômeur* entrepreneur is the modal user of Tunisian public startup support — and the one most consistently failed by it.

**Secondary user:** Support-program officers at institutions like APII and ANETI who triage applicants and need a structured, explainable basis for orientation decisions.

---

## What LeadIt Does

LeadIt runs an adaptive diagnostic on a founder's project. It surfaces the gap between self-perception and actual readiness, scores the project across five explainable dimensions, and generates a prioritized roadmap pointing to real Tunisian programs and financing devices matched to the project's actual profile.

Every output is traceable — to a criterion, a score breakdown, or a real KB source. No black box. No vague encouragement.

---

## Architecture

```
Founder Input
      ↓
Profile Builder
      ↓
Shared Project Profile
      ↓
Diagnostic Agent          ← Classifies maturity stage + detects perception gap
      ↓
5 Parallel Scoring Agents
  ├── Market Understanding
  ├── Commercial Viability
  ├── Innovation & Differentiation
  ├── Scalability
  └── Green / Sustainability Score
      ↓
Gap Analyzer              ← Identifies weaknesses and structural blockers
      ↓
Roadmap Agent             ← Matches project to Tunisian programs & resources
      ↓
Explainable Report
```

**Key design principles:**

- All agents work on the **same shared project profile** — no information loss between stages
- The **Diagnostic** tells us *where the project is* (maturity stage)
- The **Scoring layer** tells us *how strong it is* (five independent dimensions)
- The **Gap Analyzer** tells us *what is missing* (weaknesses and blockers)
- The **Roadmap** tells us *what to do next* (matched, real resources)
- Every result is **traceable** to evidence, rules, or KB sources

---

## The Five Scoring Dimensions

| Dimension | What It Measures |
|-----------|-----------------|
| **Market Understanding** | Does the founder know who their customer is, what the market size looks like, and whether demand has been validated? |
| **Commercial Viability** | Is there a credible revenue model? Are unit economics understood? Is the pricing defensible? |
| **Innovation & Differentiation** | What makes this project distinct? Is there a moat — IP, process, network effect, or unique positioning? |
| **Scalability** | Can this grow beyond the founder? Is the model replicable across geographies or segments? |
| **Green Score** | Does the project align with sustainability criteria relevant to Tunisian green economy programs (GEWEET, Think Green, GGJAP)? |

Each dimension is scored with an explanation tied to the founder's responses — not a black-box number.

---

## Perception Gap Detection

A core feature of the diagnostic is the **perception gap**: the distance between how ready a founder *thinks* they are and how ready they *actually* are based on their responses. First-time founders — particularly diplômés-chômeurs — systematically overestimate financial readiness and underestimate structuration requirements.

LeadIt surfaces this gap explicitly, not as a rejection, but as a calibration: *here is what you said, here is what your answers show, here is the delta*.

---

## Project Maturity Stages

The Diagnostic Agent classifies each project into one of six stages:

| Stage | Description |
|-------|-------------|
| **1. Idéation** | The idea exists but hasn't been validated |
| **2. Validation marché** | Real customer tests underway, MVP in progress |
| **3. Structuration** | Business model defined, legal creation in progress |
| **4. Levée de fonds** | Seeking external financing |
| **5. Lancement** | Product or service live on the market |
| **6. Croissance** | Scaling, expansion, internationalisation |

Roadmap recommendations are always stage-appropriate. A project at Idéation is not recommended a BFPME credit application.

---

## Knowledge Base

LeadIt's Roadmap Agent draws on a curated knowledge base of **23 verified Tunisian programs and institutions**, covering:

- **Accompaniment:** APII Pépinières, APII Think Green (PNUD), ANPE, Flat6Labs Tunis, Wiki Startup, ODC Orange
- **Financing:** BFPME (credit + zero-interest startup lines), BTS microcredit, Startup Act Fonds de fonds, UNDP programs, AFD facility, AMC microfinance associations
- **Administrative:** RNE registration, APII guichet unique, CEPEX export support, Startup Act label
- **Ecosystem:** CONECT, IEEE Tunisia, AINS 4.0
- **Validation:** APII Concours National de l'Invention

Each KB entry includes eligibility criteria, compatible project stages, priority sectors, expected benefit, and application pathway. Recommendations are matched against the project profile — not served as a generic list.

---

## Explainability Commitment

Every output LeadIt produces is grounded:

- **Scores** link back to specific diagnostic questions and the criteria applied
- **Gap flags** explain *why* something is identified as a weakness, not just *that* it is
- **Roadmap entries** specify which KB source was matched, on what basis, and what the application pathway is
- **Perception gap** shows the delta between self-reported and response-implied readiness, dimension by dimension

This is intentional. Founders who receive a score without explanation cannot act on it. Institutions that receive a recommendation without traceable reasoning cannot trust it.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Multi-agent orchestration | Claude API (Anthropic) — multi-agent pipeline |
| Knowledge base | Structured JSON/XLSX — 23 Tunisian programs |
| Frontend | React / Next.js |
| Backend | Node.js / Python |
| Deployment | TBD |

---

## Status

This project was developed in the context of the **AIESEC x PNUD / GEWEET hackathon**. The knowledge base, diagnostic logic, scoring rubrics, and agent architecture are defined. Implementation is in progress.

---

## Contributing

Contributions to the knowledge base are especially welcome — new programs, updated eligibility criteria, corrected URLs, or regional institutions not yet covered. Open an issue or submit a PR against `kb/base_connaissances.xlsx`.

---

## License

MIT

---

*Built in Tunisia. For Tunisia.*
