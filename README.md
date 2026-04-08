How Rag worked here :
Corpus creation: 238 resumes were parsed from PDF to text, lightly cleaned, and split into section-aware, token-based chunks of 220 tokens with a 40-token overlap to preserve context across boundaries.

Embedding: Each chunk was converted into a dense vector using a transformer embedding model, enabling semantic matching beyond exact keywords.

Vector index: All vectors were stored in a local similarity index (cosine via normalized embeddings) so nearest-neighbor search can rapidly retrieve the most relevant chunks for any query.

Retrieval: At query time, the query is embedded with the same model and used to fetch top-k similar chunks; optional filtering by category or document id focuses results as needed.

Scoring: A simple scorer aggregates similarity across retrieved evidence to produce a 0–100 relevance score, while listing top evidence chunks to ground decisions.

Process flow completed
Kept only the two target categories (ENGINEERING and INFORMATION-TECHNOLOGY) and resolved each row to its PDF by ID, yielding 238 valid resumes.

Extracted text from PDFs (primary parser with a fallback), cleaned it, and produced overlapping chunks suitable for embedding.

Generated embeddings for 2,324 chunks (dimension 768) and built a cosine-based similarity index with aligned metadata.

Implemented retrieval for ad-hoc queries or JD bullets, returning scored evidence with ids, sections, and paths.

Added single-PDF ingestion: upload → extract → chunk (same settings) → embed → append vectors and metadata → optionally auto-assign a coarse category for quick tests.
