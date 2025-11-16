# Document Repository Index

This directory contains documents processed by the Claude doc-copy skill, optimized for RAG (Retrieval Augmented Generation) systems.

## Directory Structure

```
docs/
├── index.md (this file)
└── imported/
    ├── articles/       # News articles, blog posts, essays
    ├── reports/        # Reports, whitepapers, research papers, analyses
    ├── guides/         # How-to guides, tutorials, documentation
    ├── references/     # Reference materials, specifications, API docs
    ├── other/          # Miscellaneous documents
    └── TEMPLATE.md     # Template showing document structure
```

## Document Categories

### Articles
Blog posts, news articles, opinion pieces, and essays. Generally informational content meant to educate or inform about specific topics.

### Reports
Research papers, whitepapers, analytical reports, case studies, and formal documentation of findings or investigations.

### Guides
Step-by-step tutorials, how-to guides, user manuals, and instructional content designed to teach specific skills or processes.

### References
API documentation, technical specifications, reference manuals, lookup materials, and standardized documentation.

### Other
Documents that don't fit into the above categories, including mixed-content documents, presentations, and miscellaneous materials.

## Document Naming Convention

All processed documents follow this naming pattern:
```
YYYY-MM-DD-descriptive-title.md
```

**Examples:**
- `2025-11-16-kubernetes-deployment-guide.md`
- `2025-11-16-machine-learning-research-paper.md`
- `2025-11-16-api-security-best-practices.md`

## Metadata Schema

Each document includes comprehensive YAML frontmatter with:

### Core Metadata
- Title, type, dates, source

### Content Analysis
- Summary, abstract, key topics, keywords

### Entity Extraction
- People, organizations, technologies, locations

### Classification
- Categories, audience level, primary intent

### RAG Optimization
- Chunk strategy, semantic density, related concepts

### Technical Metadata
- Word count, page count, content flags, languages

See `TEMPLATE.md` for the complete schema and structure.

## Usage

### Adding New Documents

Use the Claude doc-copy skill to process documents:

```
Use the doc-copy skill to process: /path/to/document.pdf
```

The skill will automatically:
1. Extract and convert content to markdown
2. Generate comprehensive metadata
3. Save to the appropriate category directory
4. Commit and push to Git

### Searching Documents

Documents are optimized for search through:
- **Keywords**: Searchable terms in frontmatter
- **Topics**: High-level subject categorization
- **Entities**: Named entities (people, orgs, tech)
- **Categories**: Hierarchical classification
- **Full-text**: All content is searchable

### RAG Integration

These documents are structured for optimal RAG performance:

1. **Vector Search**: Rich metadata provides multiple semantic access points
2. **Filtering**: Metadata enables pre-retrieval filtering by type, category, audience level
3. **Chunking**: Recommended strategies and hierarchical structure support intelligent chunking
4. **Cross-referencing**: Related concepts and entity extraction enable graph-based retrieval

## Statistics

<!-- This section can be updated manually or automatically -->

- **Total Documents**: 0
- **By Type**:
  - Articles: 0
  - Reports: 0
  - Guides: 0
  - References: 0
  - Other: 0

- **Last Updated**: 2025-11-16

## Document List

<!-- Documents will be listed here as they are added -->

### Recent Additions

None yet. Process your first document using the doc-copy skill!

### All Documents (Alphabetical)

<!-- Auto-generated list can go here -->

---

**Maintained by**: Claude doc-copy skill
**Repository**: ClaudeSkill
**Purpose**: RAG-optimized knowledge base
