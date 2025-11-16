# Claude Document Processing Skill

A comprehensive Claude Skill for converting ANY content into RAG-optimized markdown files with rich metadata enrichment.

## Overview

This skill enables Claude to process ANY type of content - uploaded files, pasted text, URLs, generated content, code snippets, or conversations - and convert them into well-structured, searchable markdown files optimized for Retrieval Augmented Generation (RAG) systems. The skill automatically enriches all content with comprehensive metadata, making it perfect for vector databases, knowledge bases, and semantic search systems.

## Features

### Universal Content Support
- **Uploaded Files**: PDF, images (PNG, JPG), CSV, text files, Jupyter notebooks, DOCX
- **Pasted Content**: Articles, documentation, text copied into chat
- **Web Content**: URLs, blog posts, online articles, documentation pages
- **Generated Content**: Claude-generated code, responses, analysis
- **Code Snippets**: Code blocks, scripts, configurations, functions
- **Chat Conversations**: Saved discussions, Q&A exchanges, learning sessions
- **Mixed Content**: Any combination of the above

### Intelligent Conversion
- Preserves document structure and hierarchy
- Converts tables to markdown format
- Describes images, charts, and diagrams in detail
- Maintains code blocks with syntax highlighting
- Extracts and formats lists, emphasis, and formatting

### Rich Metadata Enrichment
Every processed document includes:
- **Core metadata**: Title, type, dates, source
- **Content analysis**: Summary, abstract, key topics
- **Keywords & entities**: Searchable terms, people, organizations, technologies
- **Classification**: Categories, audience level, intent
- **RAG optimization**: Chunking strategy, semantic density, related concepts
- **Technical metadata**: Word count, page count, content flags

### RAG Optimization
Documents are structured for optimal retrieval:
- YAML frontmatter with comprehensive metadata
- Hierarchical heading structure for semantic chunking
- Multiple keyword and topic access points
- Entity extraction for relationship mapping
- Cross-reference suggestions
- Chunk strategy recommendations

### Automated Git Workflow
- Organizes files into categorized directories
- Uses consistent naming conventions
- Creates descriptive commit messages
- Automatically pushes to repository
- Retry logic for network failures

## Installation

1. Clone this repository to your local machine
2. Ensure the `.claude/skills/` directory exists
3. The `doc-copy.md` skill file should be in `.claude/skills/doc-copy.md`

## Usage

### Quick Start with /kthis

The fastest way to save content is using the `/kthis` command:

```
/kthis

[Then paste content, provide file path, URL, etc.]
```

This automatically activates the doc-copy skill and processes whatever you provide next.

### Alternative: Full Skill Invocation

You can also invoke the skill directly:

```
/skill doc-copy
```

or say:

```
Use the doc-copy skill to process this document
```

### Managing Multiple Knowledge Repositories

Create and manage separate knowledge repositories with `/krepo`:

**Create a new repository:**
```
/krepo create Cam
```

**List all repositories:**
```
/krepo list
```

**Switch between repositories:**
```
/krepo switch Cam
```

**Show current repository:**
```
/krepo current
```

All content saved with `/kthis` will go to the currently active repository.

### Processing Content

Once the skill is active, provide ANY type of content to process:

**Option 1: Uploaded Files**
```
Process this PDF: /path/to/document.pdf
Process this image: ~/screenshots/architecture-diagram.png
Process this data: ~/data/sales-report.csv
```

**Option 2: Pasted/Copied Content**
```
Save this article I found:

[Paste your content here - can be text, code, documentation, etc.]
```

**Option 3: Web URLs**
```
Save this blog post: https://example.com/great-article
Process this documentation: https://docs.example.com/api-guide
```

**Option 4: Generated Content**
```
Save the code you just generated
Save your previous response as a guide
Document the solution we just created
```

**Option 5: Code Snippets**
```
Save this Python function:
```python
def process_data(data):
    return data.transform()
```
```

**Option 6: Conversations**
```
Save our conversation about microservices architecture
Document this Q&A session
```

### Example Workflows

**Workflow 1: Uploaded PDF**
```
User: Use the doc-copy skill
User: Process this PDF: ~/Downloads/api-security-guide.pdf

Claude:
1. Reads the PDF file
2. Extracts all content (text, tables, images)
3. Describes visual elements in detail
4. Generates comprehensive metadata
5. Creates structured markdown with YAML frontmatter
6. Saves to docs/imported/guides/2025-11-16-api-security-guide.md
7. Commits and pushes to Git
8. Provides completion report
```

**Workflow 2: Pasted Article**
```
User: Use the doc-copy skill
User: Save this article:
[User pastes long article about machine learning]

Claude:
1. Analyzes the pasted content
2. Generates appropriate title and metadata
3. Structures content with proper headers
4. Saves to docs/imported/articles/2025-11-16-machine-learning-intro.md
5. Commits and pushes to Git
```

**Workflow 3: Web URL**
```
User: Use the doc-copy skill
User: Save this: https://react.dev/learn/hooks

Claude:
1. Fetches content from URL
2. Converts HTML to markdown
3. Extracts author, date, title
4. Generates metadata
5. Saves to docs/imported/guides/2025-11-16-react-hooks-guide.md
6. Commits and pushes to Git
```

**Workflow 4: Generated Code**
```
User: [After Claude generates some code]
User: Use the doc-copy skill
User: Save that database migration script

Claude:
1. Takes the previously generated code
2. Adds documentation and usage examples
3. Generates metadata
4. Saves to docs/imported/references/2025-11-16-database-migration.md
5. Commits and pushes to Git
```

## Output Structure

### Default Repository

By default, processed documents are saved to:
```
docs/
├── imported/
│   ├── articles/        # News articles, blog posts, essays
│   ├── reports/         # Reports, whitepapers, analyses
│   ├── guides/          # How-to guides, tutorials, documentation
│   ├── references/      # Reference materials, specifications, APIs
│   └── other/           # Miscellaneous documents
└── index.md            # Catalog of all processed documents (optional)
```

### Multiple Repositories

When you create additional repositories with `/krepo create NAME`, the structure is:
```
ClaudeSkill/
├── docs/imported/       # Default repository
├── krepos/
│   ├── Cam/
│   │   ├── README.md
│   │   └── docs/imported/
│   │       ├── articles/
│   │       ├── reports/
│   │       ├── guides/
│   │       ├── references/
│   │       └── other/
│   └── Work/
│       ├── README.md
│       └── docs/imported/
│           ├── articles/
│           ├── reports/
│           ├── guides/
│           ├── references/
│           └── other/
└── .claude/
    └── krepos.json      # Repository configuration
```

Each repository is completely independent, allowing you to organize content by project, person, or topic.

### File Naming Convention
- Format: `YYYY-MM-DD-descriptive-title.md`
- Example: `2025-11-16-kubernetes-deployment-best-practices.md`

## Output Format

Each processed document includes:

### YAML Frontmatter (Metadata)
```yaml
---
title: "Document Title"
doc_type: "guide"
created_date: "2025-11-16"
processed_date: "2025-11-16"
source: "original-file.pdf"
summary: "Brief document summary"
abstract: |
  Detailed multi-line abstract
key_topics:
  - Topic 1
  - Topic 2
keywords:
  - keyword1
  - keyword2
entities:
  people: ["Person Name"]
  organizations: ["Org Name"]
  technologies: ["Tech Stack"]
categories:
  - Category
primary_intent: "educate"
audience_level: "intermediate"
chunk_strategy: "by_section"
semantic_density: "high"
word_count: 5000
has_code: true
has_tables: true
has_images: true
---
```

### Markdown Content
- Executive summary
- Table of contents (for long documents)
- Well-structured hierarchical content
- Formatted tables, code blocks, and lists
- Detailed image descriptions
- Appendices and references
- Processing notes

## RAG Integration

The generated markdown files are optimized for RAG systems:

### For Vector Databases
- Rich metadata for filtering and pre-retrieval
- Multiple semantic access points (title, summary, abstract, keywords)
- Entity extraction for relationship mapping

### For Semantic Search
- Comprehensive keyword coverage
- Natural language summaries and abstracts
- Topic and category classification

### For Chunking Strategies
- Recommended chunk strategy in metadata
- Hierarchical headers for section-based chunking
- Semantic density ratings for optimal chunk size

### For Knowledge Graphs
- Entity extraction (people, orgs, tech, locations)
- Related concepts and cross-references
- Category hierarchies

## Best Practices

1. **Document Quality**: Provide high-quality source documents for best results
2. **File Paths**: Use absolute paths when referencing documents
3. **Batch Processing**: Process related documents together for consistency
4. **Review Output**: Review generated metadata for accuracy
5. **Categorization**: Documents are auto-categorized, but you can adjust if needed
6. **Git Branch**: Ensure you're on the correct branch before processing

## Metadata Fields Reference

### Core Metadata
- `title`: Document title (extracted or generated)
- `doc_type`: article, report, guide, reference, tutorial, code, conversation, other
- `source_type`: file, pasted, url, generated, conversation
- `source`: Original filename, URL, or "user-provided"/"generated"
- `source_url`: Original URL if from web
- `author`: Original author if known
- `created_date`: Original content date
- `processed_date`: Date of conversion

### Content Analysis
- `summary`: 2-3 sentence summary
- `abstract`: 100-200 word detailed abstract
- `key_topics`: 5-10 main topics
- `keywords`: 10-15 searchable keywords
- `entities`: People, organizations, technologies, locations

### Classification
- `categories`: Hierarchical taxonomy
- `primary_intent`: educate, inform, reference, guide
- `audience_level`: beginner, intermediate, advanced, expert

### RAG Optimization
- `chunk_strategy`: by_section, by_page, by_topic
- `semantic_density`: low, medium, high
- `related_concepts`: Related topics for cross-referencing

### Technical Metadata
- `word_count`: Approximate word count
- `page_count`: Number of pages
- `has_code`: Boolean for code presence
- `has_tables`: Boolean for tables presence
- `has_images`: Boolean for images presence
- `languages`: Programming or human languages

## Troubleshooting

### Document Can't Be Read
- Verify file path is correct
- Ensure file format is supported
- Try converting to PDF or TXT first

### Incomplete Conversion
- Check processing notes in output
- Some complex layouts may require manual adjustment
- Images in PDFs are described, not extracted

### Git Push Failures
- Skill retries up to 4 times with exponential backoff
- Check network connectivity
- Verify Git credentials and branch permissions

### Metadata Inaccuracy
- Review and manually adjust YAML frontmatter
- Provide more context when processing
- Use domain-specific terminology in document

## Advanced Usage

### Batch Processing
```
Process all PDFs in this directory: /path/to/pdfs/
```

### Custom Categorization
```
Process this document as a 'tutorial' type: /path/to/doc.pdf
```

### Specific Metadata Focus
```
Process this document and focus on extracting technical specifications and API references
```

## Contributing

To improve this skill:
1. Fork the repository
2. Modify `.claude/skills/doc-copy.md`
3. Test with various document types
4. Submit pull request with improvements

## Version History

- **v1.0** (2025-11-16): Initial release
  - Multi-format document support
  - Comprehensive metadata enrichment
  - RAG optimization
  - Automated Git workflow

## License

This skill is part of the ClaudeSkill repository. Refer to repository license for usage terms.

## Support

For issues, questions, or suggestions:
- Open an issue in the GitHub repository
- Refer to Claude Code documentation
- Review the skill source in `.claude/skills/doc-copy.md`

## Acknowledgments

Built for Claude Code to enable seamless document processing and RAG optimization for knowledge management systems.
