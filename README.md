Index repo
python -m src.cli ingest --repo-root /path/to/repo --recreate

Web UI
uvicorn src.api:app --host 127.0.0.1 --port 8080
http://127.0.0.1:8080/ui/

Ask
python -m src.cli ask --question "Where is scheduler logic implemented?" --top-k 6
python -m src.cli ask --question "Where is scheduler logic implemented?" --top-k 6 --explain


Import Graph

Shows file/module dependencies.
Node: file or module.
Edge: A -> B means A imports B.

Example:

service.py imports db.py and utils.py
Edges:
service.py -> db.py
service.py -> utils.py
Why useful:

Find dependency impact: “If I change db.py, what can break?”
Understand architecture/layers.
Detect tight coupling or circular dependencies.


Call Graph

Shows runtime-like function call relationships.
Node: function/method.
Edge: f -> g means function f calls function g.
Example:

handle_request() calls validate_user() and save_order()
Edge:
handle_request -> validate_user
handle_request -> save_order
Why useful:

Trace logic flow.
Debug quickly (“who calls this function?”).
Understand entrypoint-to-deep-function paths.

Simple difference

Import graph = module/file-level dependency graph.
Call graph = function/method-level execution flow graph.

How they help your semantic code finder

Query: “Where is scheduler logic?”
Semantic search finds candidate files/functions.
Import graph shows related modules around scheduler.
Call graph shows actual execution path around scheduler methods.
Result becomes more accurate and explainable.


Knowledge Graph:

Next we will create Knowledge Graph based on import Graph + call Graph
why again we need Knowledge Graph in addition to import Graph and call Graph

    Import graph is file/module-level only.
    Call graph is function-level only.
    But real code understanding requires inheritance relationships, interfaces, configs, external APIS, environmnet configuration etc
    
What KG gives you:

Single connected model across levels (repo -> file -> class -> method -> external API).
Better retrieval context and explainability path.
Multi-hop reasoning (“find where Qdrant client is configured, created, and used in pipeline”)


Import/call graphs are subgraphs.
Knowledge graph is the unified graph that includes them + additional semantic/structural edges needed for production-quality retrieval


Tasks:

  1. First we will create AST out of input repository
  2. Then we will create Knowledge Graph using AST. KG contains Call graph builder, Import graph builder
  3. Build the Code Knowledge Graph
     Combine nodes (file/module/class/function) + edges (defines, imports, calls, inherits, etc.).

  4. Build semantic chunks
     Create chunk records from function/class/code blocks + doc/comments.

  5. Generate embeddings and index in Qdrant
     Store vectors + metadata (path, symbol, lines, language).
  6. Add query intent compiler
     Convert user question into intents (functional, lifecycle, technology, control-flow).
  7. Run hybrid retrieval
     Retrieve from vector index + symbolic index + graph neighborhood.
  8. Run evidence fusion ranking
     Combine semantic score + graph coherence (+ behavior signal later).
  9. Compose explainable answer bundle
     Return snippets, file/line citations, dependency/call path, and “why matched”.
  10. Add evaluation + hardening
      Benchmark vs keyword/plain-vector baselines, then add incremental indexing, observability, and security controls.





