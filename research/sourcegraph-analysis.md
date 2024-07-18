---
filepath: /research/sourcegraph-analysis.md
---

# Sourcegraph Open Source Analysis

## Overview

This document outlines our research into Sourcegraph's open-source components, focusing on their AST and/or LSP based indexing of source code. Our goal is to identify any elements that could be leveraged for our AST-KG-RAG Code Generation Project.

## Research Questions

1. How does Sourcegraph implement AST-based indexing?
  - Sourcegraph uses their SCIP library to parse code into ASTs and extract semantic information
2. What open-source components does Sourcegraph provide that could be relevant to our project?
  - Sourcegraph's SCIP library for parsing code into ASTs and extracting semantic information
  - Their SCIP library and how they keep it at rest
  - How do they query their LSIP-indexed code once it's parsed?
3. How does Sourcegraph handle large-scale codebases, and can we apply similar techniques?


## Initial Findings

1. How does Sourcegraph implement AST-based indexing?
  - Sourcegraph uses their SCIP library to parse code into ASTs and extract semantic information
2. What open-source components does Sourcegraph provide that could be relevant to our project?
  - Sourcegraph's SCIP library for parsing code into ASTs and extracting semantic information
  - Their SCIP library and how they keep it at rest
  - How do they query their LSIP-indexed code once it's parsed?
3. How does Sourcegraph handle large-scale codebases, and can we apply similar techniques?

## Potential Applications

Open Source SCIP Implentations:
- [`scip-python`](https://github.com/sourcegraph/scip-python) Python implementation of the SCIP protocol 
- [`scip-go`](https://github.com/sourcegraph/scip-go) Go implementation of the SCIP protocol
- [`scip-typescript`](https://github.com/sourcegraph/scip-typescript) TypeScript implementation of the SCIP protocol

- [SCIP Language Bindings](https://github.com/sourcegraph/scip/tree/main/bindings)
  
  Contains a mix of generated and hand-written bindings for different languages.
   
   -   The TypeScript, Rust and Haskell bindings are auto-generated.
   -   The Go bindings include protoc-generated code as well as extra functionality, such as for converting a SCIP index into an LSIF index. This is used by the CLI below as well as the [Sourcegraph CLI](https://github.com/sourcegraph/src-cli).

## Open Source Components of Interest

(This section will list and describe any open source components from Sourcegraph that we might be able to use)

### [`SCIP`](https://github.com/sourcegraph/scip) 
SCIP (pronunciation: "skip") is a language-agnostic protocol for indexing source code, which can be used to power code navigation functionality such as Go to definition, Find references, and Find implementations.

> "Note on the name: SCIP is pronounced the same way as “skip” and it’s a recursive acronym that stands for “SCIP Code Intelligence Protocol.” SCIP is also a purposeful nod to SICP (Structure and Interpretation of Programs), a book about analyzing programs."

- 📝 [SCIP - a better code indexing format than LSIF](https://sourcegraph.com/blog/announcing-scip)
  > :star: Here are the key points from the page:
  >  -   **Introduction of SCIP**: Sourcegraph introduces SCIP (SCIP Code Intelligence Protocol) as a new indexing format to replace LSIF for better code navigation.
  >  -   **Challenges with LSIF**: LSIF faced limitations due to its graph encoding, which relied on opaque ID numbers.
  >  -   **Benefits of SCIP**: SCIP uses human-readable string IDs, making it more efficient. It is 8x smaller and 3x faster to process compared to LSIF.
  >  -   **Compatibility and Future Plans**: Sourcegraph will continue to support LSIF but recommends using SCIP for new indexers. They are also working on new SCIP indexers for various languages.

- 📝 [scip-typescript: a new TypeScript and JavaScript indexer](https://sourcegraph.com/blog/announcing-scip-typescript)
  > :star: Here are the key points from the page:
  > -   **Introduction of SCIP**: Sourcegraph introduces SCIP, a new indexing format for code navigation, replacing LSIF due to its limitations.
  > -   **Benefits of SCIP**: SCIP uses human-readable string IDs, making it more efficient and easier to process than LSIF.
  > -   **Compatibility**: Sourcegraph will continue to support LSIF but recommends using SCIP for new indexers.
  > -   **New Indexers**: SCIP indexers for TypeScript, JavaScript, Java, Scala, and Kotlin are available, with a Python indexer coming soon

- 📝 [We're Deprecating Language Server Support](https://sourcegraph.com/blog/deprecating-lsp)

- 📝 [Precise Code Intelligence: An LLM Antihallucinogen](https://eric-fritz.com/articles/llm-antihallucinogen) 
  > Eric Fritz's (Lead author? of SCIP) blog post how SCIP can be used to reduce LLM hallucinations by providing precise code context. This could help ensure our RAG system provides accurate information.
  >
  > *Further Reading*: 
  > - https://www.datastax.com/guides/what-is-a-vector-embedding


### [`OpenCTX`](https://openctx.org/) 
Sourcegraph created/supported effort to build open standard for representing code context.  This could help with sharing code context between tools like our RAG system.
- [Docs](https://openctx.org/docs)
- [Annoucement Blog Post](https://sourcegraph.com/blog/opencodegraph)

### [`LSIF.dev`](https://lsif.dev/)
A community-driven source of knowledge for Language Server Index Format implementations. LSIF (Language Server Index Format) is a derived from [LSP (Language Server Protocol)](https://langserver.org/).
  > *"The goal of the Language Server Index Format (LSIF, pronounced like "else if") is to support rich code navigation in development tools or a Web UI without needing a local copy of the source code. The format is similar in spirit to the [Language Server Protocol](https://microsoft.github.io/language-server-protocol/) (LSP), which simplifies the integration of rich code editing capabilities into a development tool."* \
  > **Source**: [Microsoft VSCode Blog](https://code.visualstudio.com/blogs/2019/02/19/lsif)

  > :star: Here are the key points about LSIF:
  > -   **LSIF Overview**: The **Language Server Index Format (LSIF)** is a standard format for language servers or other programming tools to emit their knowledge about a code workspace.
  > -   **Functionality**: LSIF allows code viewing clients to provide features like **autocomplete, go to definition, and find references** without requiring real-time computations.
  > -   **Adoption**: Several companies, including **Sourcegraph and GitHub/Microsoft**, are supporting LSIF's growth, and it is being used to power a growing list of language intelligence tools.
  > -   **Benefits**: LSIF enables code browsers to provide **code intelligence** features using information dumped during a CI build step, enhancing efficiency and portability. Is there anything else I can assist you with? 😊

- [LSIF.dev](https://lsif.dev/) - is a community-driven site, maintained by Sourcegraph, to track development progress of LSIF indexers.
- [LangServer.org](https://langserver.org/) - is an open community working to standardize the Language Server Protocol (LSP)
- [LSP Specification](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/)


## Next Steps

1. Dive deeper into Sourcegraph's documentation and code repositories
2. Analyze the architecture of Sourcegraph's indexing system
3. Identify specific components or techniques that align with our project goals

## Resources

- [Sourcegraph GitHub Repository](https://github.com/sourcegraph/sourcegraph)
- [Sourcegraph Documentation](https://docs.sourcegraph.com/)
