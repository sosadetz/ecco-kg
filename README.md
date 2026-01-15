# An Eighteenth-Century Knowledge Graph (ECCO-KG)

This repository contains information about a proof‑of‑concept implementation of an **assertional knowledge graph** for large historical text corpora, built over Gale’s *Eighteenth Century Collections Online* (ECCO) dataset.

Instead of navigating only by bibliographic containers (titles, authors, dates), we want to navigate by what books actually *say* — their citations, concepts, events, and named agents — at the level of specific passages.

To that end, this project combines:

- **Passage‑level indexing** (1,000‑word segments)  
- A **cascading metadata model** inspired by BIBFRAME  
- A layer of **localized assertions** about what each passage contains  
- Assertions generated both by **machine (LLM‑assisted)** and by **human scholars**, with the possibility of validation and correction

The result is a graph in which citations, concepts and events become a primary means of navigating the archive.

**Status:** This is an active proof‑of‑concept. The goal of this repository is to make the conceptual model and basic tooling inspectable and reproducible for other digital‑humanities projects.

### Four‑Level Cascading & Assertional Model

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

This repository provides a small, inspectable **toy version** of that graph over a limited subset of passages, illustrating how such queries could be framed.

## Repository Contents

The repository is very minimal, in current development, and organized as follows:

- `data/`  
  - `volumes.csv` – toy bibliographic records (volume IDs, titles, authors, etc.).  
  - `structural_units.csv` – chapters / sermons / sections within volumes.  
  - `entities.csv` – entities used in assertions (persons, works, concepts, events).  
  - `assertions.csv` – assertion records (passage, entity, relation, source, confidence, etc.).

- `images/`  
  - `toy_graph.png` – Neo4j visualization of the toy graph, which is in development.  
  - `abolitionist_vols.png` – Shows the assertional relationships between three abolitionist texts: Equiano, Clarkson, and Wesley.  
