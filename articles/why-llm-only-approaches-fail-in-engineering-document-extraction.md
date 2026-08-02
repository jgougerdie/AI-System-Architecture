**Why** **LLM-Only** **Approaches** **Fail** **in** **Engineering**
**Document** **Extraction** **—** **and** **What** **Works** **Instead**

**Introduction**

Engineering teams working with complex documents such as HVAC
blueprints, architectural drawings, and CAD files often attempt to use
large language models (LLMs) as end-to-end extraction systems. At first
glance, this approach appears efficient: upload a PDF, ask the model to
extract structured data, and receive a usable output.

However, in real-world production environments, this approach
consistently breaks down.

The issue is not the capability of LLMs themselves, but the mismatch
between **probabilistic** **language** **models** **and**
**structurally** **precise** **engineering** **data.**

**The** **Core** **Problem:** **Documents** **Are** **Not** **Text**

HVAC blueprints and CAD files are fundamentally **geometric** **and**
**relational** **systems**, not linear text documents.

They contain:

> • Vector geometry (lines, arcs, polylines) • Layered organizational
> structures
>
> • Spatial relationships (adjacency, distance, alignment) • Symbolic
> representations (ducts, fittings, equipment) • Embedded annotations
> and measurements

When these files are converted into flat text or images for LLM
ingestion, a critical loss occurs:

The structural truth of the document is destroyed.

This leads to hallucinations, inconsistent parsing, and unstable
outputs.

**Why** **LLM-Only** **Pipelines** **Fail**

LLMs are optimized for reasoning over language, not geometry or
deterministic structure.

Common failure modes include:

> • Misinterpretation of spatial relationships (e.g. connecting
> unrelated ducts) • Loss of scale and coordinate integrity
>
> • Inconsistent entity classification across similar drawings •
> Non-reproducible outputs between runs

• Hidden ambiguity amplification (small OCR errors cascade into
structural errors) In short:

LLMs “understand language,” but HVAC systems are not language — they are
structured engineering graphs.

**The** **Correct** **Approach:** **Deterministic-First**
**Architecture** A reliable solution requires reversing the pipeline
logic. Instead of:

Document → LLM → Structured Output The correct architecture is:

Document → Deterministic Extraction → Structured Model → Controlled LLM
Reasoning → Output

**1.** **Deterministic** **Extraction** **Layer**

This stage ensures that no interpretation happens early in the pipeline.
It includes:

> • PDF structure parsing (text vs vector separation)
>
> • CAD extraction (DWG/DXF: geometry, layers, blocks) • Targeted OCR
> only where necessary

Output:

A raw but structured representation of all visible document components.

**2.** **Structural** **Decomposition** **Layer**

At this stage, the system begins organizing raw fragments into
meaningful groups: • Zones and spatial regions

> • System-level segmentation (HVAC networks) • Repeated component
> detection
>
> • Spatial relationship mapping

The goal is to convert a file into a **structured** **system**
**model**, not a visual artifact.

**3.** **Schema-Based** **Representation** **Layer**

Once structure is identified, entities are formalized: • Ducts →
structured flow paths

> •      Equipment → labeled system nodes •      Annotations → anchored
> references
>
> • Dimensions → validated measurements

All entities are mapped into strict schemas with traceability back to
source geometry. This ensures:

> • consistency • auditability
>
> • reproducibility

**4.** **Controlled** **LLM** **Integration**

Only at this stage are LLMs introduced.

However, they are not used as readers of raw documents. Instead, they
operate strictly on structured data for:

> • Ambiguity resolution
>
> • Semantic classification

• Edge-case interpretation Constraints:

> • No raw PDF or CAD input
>
> • Schema-validated input/output only • Task-specific, isolated
> reasoning

This transforms the LLM from a “primary engine” into a **controlled**
**reasoning** **component** **inside** **a** **deterministic**
**system.**

**5.** **Output** **Layer** **for** **Engineering** **Systems** Final
outputs are designed for real-world usage:

> • CSV / Excel for estimation workflows • JSON for APIs and databases

• BIM-compatible structured datasets Each output is:

> • traceable
>
> • reproducible
>
> • validation-ready

**Key** **Insight**

The fundamental shift is architectural:

Success in engineering document AI does not come from stronger prompts —
it comes from reducing ambiguity before the model is ever involved.

**Conclusion**

LLM-based document extraction fails when it attempts to interpret
structure that should already be known.

The reliable approach is to treat LLMs as **secondary** **reasoning**
**tools**, not primary extraction engines.

By separating:

> • deterministic extraction • structural modeling
>
> • controlled semantic reasoning

we achieve systems that are not only accurate, but also stable enough
for production engineering environments.

<img src="./wedy3waa.png" style="width:10in;height:5.45455in" />

> **Javad** **G.** **&** **Muhammad** **T.** Senior AI Systems &
> Integration Architects
>
> <https://architect-blog-33ef1269.viktor.space>
