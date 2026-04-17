Index repo
python -m src.cli ingest --repo-root /Users/rnagulap/otbi/otbi-lcm-jc --recreate

Test search
python -m src.cli search --query "Where is scheduler logic implemented?" --top-k 5

Test ask
python -m src.cli ask --question "Where is vector store initialized and used?" --top-k 6


Tasks:

  1. First we will create AST out of input repository
  2. Then we will create Knowledge Graph using AST. KG contains Call graph builder, Import graph builder
  3. Call/import graphs = higher-level relationship graphs extracted from AST across files/modules
  4. Build semantic chunks from AST nodes (function/class/block + doc/comments).  
  5. Generate embeddings for chunks and store in vector DB (Qdrant).
  6. Build symbolic index (identifiers, file paths, APIs, tech terms).  
  7. Build behavior signatures (optional first pass): test traces/log-derived execution links.  
  8. Create query intent compiler (NL query -> slots: action/object/phase/tech/control-flow).  
  9. Run parallel retrieval per intent across semantic, graph, symbolic (and behavior if present).  
  10. Fuse candidates with evidence ranker (semantic score + graph coherence + behavior match + optional recency).  
  11. Compose answer bundle: top snippets + dependency path + “why matched” explanation + confidence.  
  12. Evaluate against baseline (keyword and plain vector search) using Recall@k, MRR, groundedness.  
  13. Add incremental indexing and refresh (git-diff based) for production readiness.

