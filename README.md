# A knowledge graph for eighteenth-century corpora

This repository documents an assertional knowledge graph built for large historical corpora, using public-domain eighteenth-century volumes from HathiTrust. Its premise is simple: humanists should be able to navigate an archive not only according to volume-level bibliographic metadata, but by what books actually _do on the page_: whom they invoke, what authorities they cite, what events they recount, what concepts they debate, and how they position their claims.

The graph is designed to support scholarly discovery at the scale of reading: using generative AI models as first-pass catalogers, it attaches structured, interoperable claims about passage-level content that can be aggregated and queried across a corpus.

### What the graph enables

Instead of asking only “Which books contain a keyword?”, you can ask questions that are more meaningful for qualitative humanistic inquiry, at the scale of the corpus:
- **Reception and citation networks**: Which authorities are repeatedly cited in particular debates or genres?
- **Conceptual history**: Where and how do concepts cluster and change across decades?
- **Reference-based timelines and geographies**: Which episodes recur, and where do texts locate their arguments?
- **Argumentation and stance**: How do passages frame their claims? In aggregate, do they treat particular ideas approvingly or critically? Do they tend to be polemical, didactic, or satirical?

### Pilot snapshot (completed)

Our completed proof-of-concept represents **31 volumes** (largely concerning abolition), **4,594 passage objects,** and **95,599 assertions** (≈ 20.8 assertions/passage). The pilot demonstrates that carefully constrained extraction can yield dense, structured signals across several assertion families, including people mentioned, places, works cited, concepts, and (with more caution) events and discursive stance.

### Repository contents

This repository contains small snippets of the assertion data for the major node types in our graph.

- data/
	- `works.csv` - works analyzed and discussed
	- `instances.csv` – bibliographic volume records for works analyzed
	- `passages.csv` - 1,000-word passage objects derived from instances
	- `agents.csv` – authors cited and people discussed
	- `concepts.csv` - concepts discussed
	- `events.csv` - events discussed
- assertions/
	- JSON example assertions drawn from Clarkson's _An Essay on Slavery_ and Addison and Steele's _The Spectator_.
- images/
	- `1_pilot_graph.png` - Neo4j visualization of the pilot graph, excluding leaf nodes
	- `2_kg_master_schema.png` - Illustrates the graph's schema, including nodes and edges
	- `3_smith_slavery_passages.png` - A segment of the graph showing passages from Smith's _Wealth of Nations_ that discuss concepts related to slavery.
    -  `4_agents_gallery.pdf` - People mentioned in our pilot corpus, sorted by birthdate. This gallery was developed using the Wikidata Query Service, so only persons with Wikidata Q-IDs and associated images are represented. More suitable as a teaching resource, this gallery is meant meerely to provide an illustrative example, indicating the sorts of questions that become newly possible to ask through this graph.

### Citation

If you use or adapt this project’s model, schema, or exports, please cite the repository (and the relevant release tag). A longer-form scholarly description of the pilot’s affordances and limitations is in preparation.
