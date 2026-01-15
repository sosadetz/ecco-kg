# An Eighteenth-Century Knowledge Graph (ECCO-KG)

This repository contains information about a proof‑of‑concept implementation of an **assertional knowledge graph** for large historical text corpora, built over Gale’s *Eighteenth Century Collections Online* (ECCO) dataset.

The central idea is simple:

> Instead of navigating only by bibliographic containers (titles, authors, dates), we want to navigate by what books actually *say* — their citations, concepts, events, and named agents — at the level of specific passages.

To that end, this project combines:

- **Passage‑level indexing** (1,000‑word segments)  
- A **cascading metadata model** inspired by BIBFRAME  
- A layer of **localized assertions** about what each passage contains  
- Assertions generated both by **machine (LLM‑assisted)** and by **human scholars**, with the possibility of validation and correction

The result is a graph in which citations, concepts and events become a primary means of navigating the archive.

> **Status:** This is an active proof‑of‑concept. The goal of this repository is to make the conceptual model and basic tooling inspectable and reproducible for other digital‑humanities projects.

---

## 1. Conceptual Overview

### 1.1 Why a knowledge graph?

Most large digitized collections today are still organized like card catalogues:  
they describe **volumes**, not the **passages** and **claims** that actually matter for research.

The passage‑level search engine that underpins this project has shown that:

- treating passages as the unit of retrieval radically increases what can be found;  
- but it also breaks the alignment with traditional volume‑level metadata (MARC, BIBFRAME, etc.).

A knowledge graph is one way to “stitch together” these levels again:

- keeping the bibliographic record;  
- but adding a layer of passage‑level assertions that say, in structured form, *what is going on* in each segment of text.

### 1.2 Four‑Level Cascading & Assertional Model

The graph adopts a simple four‑layer model:

1. **Volume / Work–Instance (Bibliographic layer)**  
   - BIBFRAME‑style description of works and instances (title, author, imprint, etc.).

2. **Structural Units**  
   - Self‑contained parts within a volume (chapters, sermons, prefaces, articles, poems).

3. **Passage Objects**  
   - Fixed‑length segments (e.g. 1,000‑word passages) that are the unit of retrieval in the search engine.

4. **Assertion Records**  
   - Localized, structured statements that link spans within a passage to entities, via typed relations.  
   - Example assertions:
     - `passage_123` — *cites* → `Justinian`  
     - `passage_456` — *discusses* → `property in persons`  
     - `passage_789` — *mentions_event* → `Saint-Domingue slave revolt 1791`  

Assertions can be proposed by:

- **AI models** (first‑pass “reading”), and  
- **human scholars**, who validate, correct, or add more specific claims.

The graph thus records **what humans and machines say passages contain**, rather than trying to collapse them into a single “ground truth”.

---

## 2. What You Can Do with It

At scale, a graph like this supports new kinds of questions that are hard or impossible with volume‑level search alone, for example:

- **Implicit canons**  
  - Identify works that are rarely read today, but are *heavily cited* across other texts.
- **Citation networks and reception**  
  - “Where is Cicero quoted in abolitionist writing?”  
  - “Who cites Justinian, and in what genres (sermons, treatises, periodicals)?”
- **Conceptual and prosopographical history**  
  - Track concepts such as `liberty`, `natural equality`, or `the transatlantic slave trade` across decades, genres, and communities.
- **Focused case studies**  
  - For a set of 20 abolitionist authors:
    - Which **historical events** do they invoke (revolts, trials, parliamentary debates)?  
    - Which **biblical passages** (Paul, Exodus, the prophets) and which **classical authors** do they mobilize?  
    - How do their **reading networks** overlap or diverge?

This repository provides a small, inspectable **toy version** of that graph over a limited subset of passages, illustrating how such queries could be framed.

---

## 3. Repository Contents

The repository is very minimal, in current development, and organized as follows:

- `data/`  
  - `volumes.csv` – toy bibliographic records (volume IDs, titles, authors, etc.).  
  - `structural_units.csv` – chapters / sermons / sections within volumes.  
  - `entities.csv` – entities used in assertions (persons, works, concepts, events).  
  - `assertions.csv` – assertion records (passage, entity, relation, source, confidence, etc.).

- `images/`  
  - `toy_graph.png` – Neo4j visualization of the toy graph, which is in development.  
  - `abolitionist_vols.png` – Shows the assertional relationships between three abolitionist texts: Equiano, Clarkson, and Wesley.  
  
> **Note:** Due to licensing constraints, this repository does *not* contain the ECCO text corpus. All examples here are built from synthetic or public‑domain snippets.

---

## 4. AI Annotation Pipeline (High‑Level)

For the purposes of this proof‑of‑concept, AI is used to:

1. Take a **passage** (and minimal context) as input.  
2. Propose **candidate assertions** in a structured JSON‑like format:
   ```json
   {
     "passage_id": "p_00123",
     "assertions": [
       {"entity_id": "concept_property_in_persons", "relation": "discusses", "confidence": 0.82},
       {"entity_id": "person_justinian", "relation": "cites", "confidence": 0.77}
     ]
   }
