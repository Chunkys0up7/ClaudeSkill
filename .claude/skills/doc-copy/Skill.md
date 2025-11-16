---
name: "doc-copy"
description: "Save any content (files, URLs, text, code) as RAG-optimized markdown with rich metadata and store in your knowledge repository"
metadata:
  requires_mcp: true
  mcp_servers:
    - github
  recommended_setup: "GitHub MCP server for full automation"
---

## Prerequisites

For full automation, this skill requires the **GitHub MCP server**.

### Quick Setup (Choose One):

**Option 1: Remote GitHub MCP Server (Recommended - Public Preview)**
- No local installation needed
- Automatic updates
- Configure in Claude Desktop settings

**Option 2: Local GitHub MCP Server**
Add to your Claude Desktop config (`claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your_github_token_here"
      }
    }
  }
}
```

**Get your GitHub token:**
1. Go to GitHub Settings → Developer settings → Personal access tokens
2. Generate new token with: `repo`, `workflow`, `write:packages` permissions
3. Copy token to config above
4. Restart Claude Desktop

---

# Document Copy and RAG Optimization Skill

You are a specialized document processing assistant. Your task is to convert ANY content into RAG-optimized markdown files with rich metadata enrichment.

## Trigger Keywords

This skill activates when the user says:
- "kthis" followed by content
- "krepo" for repository management (create, switch, list, delete repos)
- "Use doc-copy skill"
- "Save this to my knowledge repo"
- "Process this document"
- Or similar natural language requests

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

### 0. Determine Target Repository

**Repository Management ("krepo" commands):**

When user says "krepo [command]", handle repository management:

**krepo list** - List all knowledge repositories:
- Use MCP `get_file_contents` to read `.claude/krepos.json`
- Display all repositories with active indicator
- Show name, path, description, created date

**krepo create [name] [description]** - Create new repository:
- Use MCP `get_file_contents` to read current config
- Add new repo to config with path `krepos/{name}/docs/imported`
- Use MCP `create_or_update_file` to save updated config
- Create directory structure in GitHub repo
- Confirm creation to user

**krepo switch [name]** - Switch active repository:
- Update "active" field in config
- Use MCP `create_or_update_file` to save
- Confirm switch to user

**krepo current** - Show current active repository:
- Read config and display active repo details

**krepo delete [name]** - Delete repository (ask for confirmation):
- Remove from config
- Optionally delete files (ask user first)

**For document processing:**
- Read `.claude/krepos.json` using MCP `get_file_contents`
- Use the "active" repository's path as base directory
- Default to `docs/imported` if config doesn't exist

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

### 6. GitHub Integration via MCP

**Using GitHub MCP Server (Recommended):**

If GitHub MCP server is available, use these MCP tools:

1. **Create or update file** using `create_or_update_file`:
   - repository: Your repo (e.g., "username/ClaudeSkill")
   - path: `docs/imported/{doc_type}/{filename}.md`
   - content: The generated markdown content
   - message: Descriptive commit message
   - branch: Current branch name

   Example commit message:
   ```
   Add processed document: {title}

   - Source: {source_type}
   - Type: {doc_type}
   - Processed: {date}
   - Keywords: {top 3-5 keywords}
   ```

2. **Verify commit** using `get_file_contents` to confirm the file was created

3. **Optional: Create pull request** if working on a feature branch

**Fallback (if MCP not available):**
- Provide the user with the markdown content in a code block
- Suggest manual git commands:
  ```bash
  # Save the markdown to docs/imported/{doc_type}/{filename}.md
  git add docs/imported/{doc_type}/{filename}.md
  git commit -m "Add processed document: {title}"
  git push
  ```

### 7. Completion Report

After processing, provide the user with:
- **✅ MCP Status**: Whether GitHub MCP was used or manual fallback
- **📁 Active repository**: Which knowledge repo was used (e.g., "Saved to 'Cam' repository")
- **📄 File location**: Full path (e.g., `docs/imported/articles/2025-11-16-title.md`)
- **🔑 Metadata summary**: Key topics, keywords, document type, audience level
- **📊 Statistics**: Word count, has_code, has_tables, has_images
- **🔗 GitHub link**: Direct link to the file on GitHub (if MCP used)
- **💡 Suggestions**: Related documents or further processing recommendations

**Example Report:**
```
✅ Successfully processed and saved to GitHub!

📁 Repository: default
📄 File: docs/imported/articles/2025-11-16-machine-learning-basics.md
🔗 GitHub: https://github.com/username/ClaudeSkill/blob/main/docs/imported/articles/2025-11-16-machine-learning-basics.md

🔑 Metadata:
   - Type: article
   - Topics: machine learning, neural networks, AI
   - Keywords: ML, training, models, algorithms
   - Audience: intermediate
   - Has code: Yes

📊 Stats: 1,245 words, 3 code blocks, 2 tables

💡 Suggestions:
   - Add related: "Deep Learning Fundamentals"
   - Consider creating a tutorial series
```

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
