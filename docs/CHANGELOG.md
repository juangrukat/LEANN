## 2026-05-15: Standardize local unified retrieval profile

- Made the default local LEANN setup use `Qwen/Qwen3-Embedding-0.6B`, DiskANN, 768-token document chunks with 160-token overlap, 1024-token code and AST chunks with 180-token overlap, build complexity 128, search complexity 64, top-k 35, graph degree 64, compact storage, recomputation, and AST-aware chunking.
- Added CLI aliases so the documented profile works directly with `--build-complexity`, `--search-complexity`, and `leann search --query`.
- Added metadata headers to text passed through `LeannBuilder.add_text`, so embedded chunks can include source, type, chapter, path, section, and file name context when metadata is available.
- Updated the README and configuration guide to describe the new standard profile and its intended use for mixed programming documentation and English literary corpora.

## 2026-05-16: Fix local DiskANN build and snapshot blockers

- Added a local embedding batch-size option and made `Qwen/Qwen3-Embedding-0.6B` use a safer MPS batch size by default to avoid Apple Silicon out-of-memory failures.
- Applied local tokenizer max sequence length in the sentence-transformers encode path and length-sorted embedding batches before restoring original order, reducing Qwen3 memory spikes on long mixed-length chunks.
- Fixed DiskANN native builds on macOS by moving the child-process build target to module scope so multiprocessing spawn can pickle it.
- Fixed DiskANN graph partition setup to build the required `partitioner` and `index_relayout` executables directly, with CMake available as a DiskANN backend dependency.
- Changed DiskANN graph partitioning to use one native partition thread by default because the upstream partitioner crashes with multi-threaded LDG partitioning on tested local indexes.
- Fixed single-file build snapshots so LEANN hashes only explicitly requested files instead of scanning unrelated files in the same parent directory.
