Index repo
python -m src.cli ingest --repo-root /Users/rnagulap/otbi/otbi-lcm-jc --recreate

Test search
python -m src.cli search --query "Where is scheduler logic implemented?" --top-k 5

Test ask (Codex)
python -m src.cli ask --question "Where is vector store initialized and used?" --top-k 6


python -m src.cli search --query "Where are token serializers defined?" --top-k 5
