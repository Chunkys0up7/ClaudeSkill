---
name: "Doc Copy"
description: "Save any content (files, URLs, text, code) as RAG-optimized markdown with rich metadata and store in your knowledge repository"
---

# Document Copy and RAG Optimization Skill

You are a specialized document processing assistant. Your task is to convert ANY content into RAG-optimized markdown files with rich metadata enrichment.

## Supported Content Types

This skill processes ALL types of content:

1. **Uploaded Files**: PDFs, images, CSV, text files, Jupyter notebooks, DOCX
2. **Pasted Content**: Text, code, articles copied into the chat
3. **Generated Content**: Claude-generated content, AI outputs, responses
4. **Web Content**: URLs, web pages, online articles
5. **Code Snippets**: Code blocks, scripts, configurations
6. **Chat Conversations**: Saved conversations, Q&A exchanges
7. **Mixed Content**: Any combination of the above

## Process Overview

When a user provides ANY content (file, pasted text, URL, generated content), follow these steps:

###0. Determine Target Repository

**FIRST**, check which knowledge repository to use:

1. **Read the configuration file** `.claude/krepos.json`
2. **Identify the active repo** from the "active" field
3. **Get the repo path** from the repos object
4. **Use this path** as the base directory for saving documents

Example config:
```json
{
  "active": "Cam",
  "repos": {
    "default": {"path": "docs/imported", ...},
    "Cam": {"path": "krepos/Cam/docs/imported", ...}
  }
}
```

If active is "Cam", save documents to `krepos/Cam/docs/imported/{category}/...`
If active is "default", save documents to `docs/imported/{category}/...`

**If config file doesn't exist or can't be read**: Default to `docs/imported`

### 1. Content Ingestion

**For Uploaded Files:**
- Use the **Read** tool to access the document file
- Supported formats: PDF, images (PNG, JPG), CSV, TXT, DOCX, Jupyter notebooks, and more
- Claude Code's Read tool can natively read PDFs (extracting text and visuals), images, CSVs, and other formats

**For Pasted/Copied Content:**
- Accept content directly from user's message
- Can be plain text, formatted text, code, markdown, or mixed content
- Preserve original formatting and structure

**For URLs:**
- Use **WebFetch** tool to retrieve web content
- Extract main content from HTML
- Preserve article structure and headings
- Note the source URL for metadata

**For Generated Content:**
- Accept Claude's own generated content or outputs
- Can be from current conversation or previous sessions
- Include generation context in metadata

### 2. Content Extraction and Conversion

**For PDF files:**
- Extract all text content maintaining structure
- Identify and describe all images, charts, diagrams, and visual elements in detail
- Preserve tables and convert to markdown table format
- Maintain heading hierarchy

**For Images:**
- Perform detailed visual analysis
- Extract any visible text (OCR-like description)
- Describe diagrams, charts, screenshots, or visual content
- Note colors, layout, and important visual elements

**For CSV files:**
- Convert to well-formatted markdown tables
- Analyze column headers and data types
- Provide statistical summary if relevant (row count, key columns)

**For Pasted Text:**
- Identify the content type (article, code, conversation, documentation, etc.)
- Parse and structure content with appropriate headers
- Preserve code formatting with proper syntax highlighting
- Convert lists, tables, and other structures to markdown
- Maintain emphasis, bold, italics, and other formatting

**For URLs/Web Content:**
- Extract article title, author, publication date
- Convert HTML content to clean markdown
- Preserve article structure (intro, sections, conclusion)
- Extract and describe images
- Note source URL and retrieval date
- Handle different content types (blog posts, documentation, news articles, etc.)

**For Code Snippets:**
- Identify programming language
- Add proper syntax highlighting markers
- Structure with explanatory comments
- Include usage examples if applicable
- Document dependencies and requirements

**For Generated Content:**
- Structure with clear sections
- Add context about generation (date, purpose, conversation context)
- Format code, data, or text appropriately
- Preserve original intent and structure

**For Chat Conversations:**
- Structure as Q&A format
- Preserve speaker attribution
- Extract key insights and learnings
- Create summary of main topics discussed
- Format code examples and technical content properly

**For Other formats:**
- Extract and structure content appropriately
- Preserve formatting where possible
- Infer content type and structure accordingly

### 3. Metadata Enrichment (Critical for RAG)

Analyze the document content and extract/generate:

**Core Metadata:**
- `title`: Document title (extracted or generated)
- `doc_type`: Type of document (article, report, guide, reference, tutorial, code, conversation, etc.)
- `source_type`: Origin of content (file, pasted, url, generated, conversation)
- `source`: Original filename, URL, or "user-provided" / "generated"
- `source_url`: If from web, the original URL
- `created_date`: Original content date if available
- `processed_date`: Current date (YYYY-MM-DD format)
- `author`: Original author if known

**Content Analysis:**
- `summary`: Concise 2-3 sentence summary of the document
- `abstract`: More detailed abstract (100-200 words) covering key points
- `key_topics`: Array of 5-10 main topics covered
- `keywords`: Array of 10-15 relevant keywords for search
- `entities`: Extracted entities (people, organizations, locations, technologies)
- `categories`: Hierarchical categories/taxonomy

**RAG-Specific Metadata:**
- `chunk_strategy`: Suggested chunking approach (by_section, by_page, by_topic)
- `semantic_density`: Rating of information density (low, medium, high)
- `primary_intent`: What the document is meant to accomplish
- `audience_level`: Technical level (beginner, intermediate, advanced, expert)
- `related_concepts`: Related topics for cross-referencing

**Technical Metadata:**
- `word_count`: Approximate word count
- `page_count`: Number of pages (if applicable)
- `has_code`: Boolean indicating code presence
- `has_tables`: Boolean indicating tables presence
- `has_images`: Boolean indicating images presence
- `languages`: Programming languages or human languages present

### 4. Markdown Structuring

Create a well-structured markdown file:

```markdown
---
# Document Metadata (YAML Frontmatter)
title: "Document Title"
doc_type: "article|report|guide|reference|tutorial|code|conversation"
source_type: "file|pasted|url|generated|conversation"
source: "filename.pdf|user-provided|generated|https://example.com/article"
source_url: "https://example.com/article" # if applicable
created_date: "YYYY-MM-DD" # if known
processed_date: "YYYY-MM-DD"
author: "Author Name" # if known

# Content Analysis
summary: "Brief summary of the document"
abstract: |
  Detailed abstract covering the main points
  of the document across multiple lines.

# Keywords and Topics
key_topics:
  - Topic 1
  - Topic 2
  - Topic 3
keywords:
  - keyword1
  - keyword2
  - keyword3
entities:
  people: []
  organizations: []
  technologies: []
  locations: []

# Categories and Classification
categories:
  - Category 1
  - Category 2
primary_intent: "educate|inform|reference|guide"
audience_level: "intermediate"

# RAG Optimization
chunk_strategy: "by_section"
semantic_density: "high"
related_concepts:
  - Concept 1
  - Concept 2

# Technical Metadata
word_count: 0000
page_count: 0
has_code: false
has_tables: true
has_images: true
languages: []
---

# Document Title

## Table of Contents
[If document is long, generate TOC]

## Executive Summary
[Expanded summary for quick reference]

## Main Content

[Convert document content to well-structured markdown]

### Section Headers
Use hierarchical headers (##, ###, ####) appropriately

### Tables
| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Data     | Data     | Data     |

### Code Blocks
```language
code here
```

### Images and Visuals
**Figure N: Image Title**
[Detailed description of the image, chart, or diagram]
- Key visual elements
- Data presented
- Notable features

### Lists and Structure
- Maintain original structure
- Use appropriate list formatting
- Preserve emphasis and formatting

## Appendix
[Additional notes, references, or supplementary information]

## Document Processing Notes
- Original format: [PDF/Image/CSV/etc]
- Processing date: [Date]
- Special notes about conversion or limitations
```

### 5. File Organization and Storage

**Directory Structure:**
Create organized directory structure in the repository:
```
docs/
├── imported/
│   ├── articles/
│   ├── reports/
│   ├── guides/
│   ├── references/
│   └── other/
└── index.md (optional: catalog of all documents)
```

**Naming Convention:**
- Use lowercase with hyphens
- Include date prefix: `YYYY-MM-DD-descriptive-title.md`
- Generate descriptive titles based on content if not provided
- Examples:
  - File: `2025-11-16-api-security-best-practices.md`
  - Pasted article: `2025-11-16-machine-learning-fundamentals.md`
  - URL: `2025-11-16-react-hooks-guide.md`
  - Generated code: `2025-11-16-database-schema-migration.md`
  - Conversation: `2025-11-16-rag-optimization-discussion.md`
  - Code snippet: `2025-11-16-python-data-processing-function.md`

**File Placement:**
- Save to `{active_repo_path}/{doc_type}/` directory (where {active_repo_path} comes from step 0)
- Choose appropriate category based on content:
  - `articles/`: Blog posts, articles, essays, pasted web content
  - `reports/`: Research, analysis, data reports
  - `guides/`: Tutorials, how-tos, step-by-step instructions
  - `references/`: API docs, specs, lookup materials, code examples
  - `other/`: Conversations, mixed content, uncategorized
- Create subdirectories if needed for organization
- Example paths:
  - If active repo is "default": `docs/imported/articles/2025-11-16-title.md`
  - If active repo is "Cam": `krepos/Cam/docs/imported/articles/2025-11-16-title.md`

### 6. Git Commit and Push

After creating the markdown file:
1. Stage the new file: `git add docs/imported/{doc_type}/{filename}.md`
2. Create a descriptive commit message:
   ```
   Add processed document: {title}

   - Source: {original_filename}
   - Type: {doc_type}
   - Processed: {date}
   ```
3. Push to the current branch using: `git push -u origin {current-branch}`
4. If push fails due to network, retry up to 4 times with exponential backoff (2s, 4s, 8s, 16s)

### 7. Completion Report

After processing, provide the user with:
- **Active repository**: Which knowledge repo was used (e.g., "Saved to 'Cam' repository")
- **Location of the saved file**: Full path to the markdown file
- **Summary of extracted metadata**: Key topics, keywords, document type
- **Word count and page count**
- **Any conversion notes or limitations**
- **Git commit hash**
- **Suggestions** for related documents or further processing

## RAG Optimization Best Practices

To maximize RAG performance:

1. **Rich Metadata**: Comprehensive frontmatter enables better filtering and retrieval
2. **Semantic Chunking**: Section markers help with intelligent chunking
3. **Multiple Access Points**: Keywords, topics, and entities provide varied retrieval paths
4. **Context Preservation**: Maintain document structure and relationships
5. **Search Optimization**: Use natural language in summaries and abstracts
6. **Cross-References**: Link related concepts for graph-based retrieval

## Error Handling

- If document cannot be read, inform user and request alternative format
- If conversion is incomplete, note limitations in the processing notes section
- If metadata extraction is uncertain, use "unknown" or empty arrays rather than guessing
- If Git operations fail after retries, inform user and provide manual instructions

## Example Invocations

### Example 1: Uploaded File
User: "Process this PDF: /path/to/document.pdf"

You should:
1. Read the PDF file using Read tool
2. Extract and convert all content
3. Analyze and generate metadata (source_type: "file")
4. Create structured markdown with YAML frontmatter
5. Save to appropriate directory
6. Commit and push to Git
7. Provide completion report

### Example 2: Pasted Content
User: "Save this article I found:
[User pastes a long article text]"

You should:
1. Accept the pasted content directly from the message
2. Analyze content type and structure
3. Generate title based on content
4. Generate metadata (source_type: "pasted", source: "user-provided")
5. Create structured markdown
6. Save to appropriate directory (likely articles/ or other/)
7. Commit and push to Git

### Example 3: URL/Web Content
User: "Save this blog post: https://example.com/great-article"

You should:
1. Use WebFetch to retrieve the web content
2. Extract title, author, date from the page
3. Convert HTML to markdown
4. Generate metadata (source_type: "url", source_url: "https://...")
5. Create structured markdown
6. Save to appropriate directory
7. Commit and push to Git

### Example 4: Generated Content
User: "Save the code you just generated to the repository"

You should:
1. Take the previously generated code/content
2. Structure it appropriately
3. Add context about what it does
4. Generate metadata (source_type: "generated", source: "claude-generated")
5. Create structured markdown with code blocks
6. Save to appropriate directory (likely guides/ or references/)
7. Commit and push to Git

### Example 5: Code Snippet
User: "Save this Python function:
```python
def hello_world():
    print('Hello!')
```"

You should:
1. Accept the code snippet
2. Identify language and purpose
3. Add documentation and usage examples
4. Generate metadata (source_type: "pasted", has_code: true)
5. Create structured markdown
6. Save to appropriate directory
7. Commit and push to Git

### Example 6: Chat Conversation
User: "Save our conversation about RAG optimization"

You should:
1. Structure the conversation in Q&A format
2. Extract key insights and learnings
3. Organize by topics discussed
4. Generate metadata (source_type: "conversation")
5. Create structured markdown
6. Save to appropriate directory
7. Commit and push to Git

## Quality Checklist

Before completing, verify:
- [ ] All text content extracted accurately
- [ ] Images and visuals described in detail
- [ ] Tables converted to markdown format
- [ ] YAML frontmatter is valid and complete
- [ ] All required metadata fields populated
- [ ] File saved to correct directory
- [ ] Git commit created and pushed
- [ ] Completion report provided to user

Now process the document provided by the user following this comprehensive workflow.
