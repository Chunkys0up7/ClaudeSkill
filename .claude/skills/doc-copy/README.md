# Doc Copy Skill

Convert any content into RAG-optimized markdown files with rich metadata and automatic GitHub storage.

## Features

- 🎯 **Trigger Keywords**: Use "kthis" or "krepo" for quick invocation
- 📝 **Multiple Content Types**: Files, pasted text, URLs, code, conversations
- 🤖 **Rich Metadata**: 20+ metadata fields optimized for RAG
- 🔄 **GitHub Integration**: Automatic commit and push via MCP
- 📁 **Multi-Repository**: Create and manage multiple knowledge repos
- ⚡ **Fully Automated**: End-to-end processing in claude.ai

## Setup Requirements

### GitHub MCP Server (Required for Full Automation)

This skill requires the **GitHub MCP server** for full automation. Choose one option:

#### Option 1: Remote GitHub MCP Server (Recommended)

✨ **Public Preview** - No local installation needed!

1. Open Claude Desktop settings
2. Enable Remote GitHub MCP server
3. Authenticate with GitHub
4. Restart Claude Desktop

#### Option 2: Local GitHub MCP Server

Add to `claude_desktop_config.json`:

**macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_your_token_here"
      }
    }
  }
}
```

**Generate GitHub Token:**
1. Go to https://github.com/settings/tokens
2. Click "Generate new token (classic)"
3. Select scopes: `repo`, `workflow`, `write:packages`
4. Copy token and add to config above
5. Restart Claude Desktop

## Installation

### Upload to Claude.ai

1. Download `doc-copy-skill.zip`
2. Go to https://claude.ai
3. Settings → Capabilities → Skills
4. Click "Upload custom Skill"
5. Select the ZIP file
6. Enable the skill

### Verify Installation

In any Claude conversation, say:
```
krepo list
```

If the skill is working, it will show your knowledge repositories.

## Usage

### Quick Save (kthis)

```
kthis

Machine learning is a subset of artificial intelligence...
```

Claude will:
1. Convert to markdown
2. Generate rich metadata
3. Save to GitHub automatically
4. Provide completion report with GitHub link

### Repository Management (krepo)

```
krepo create work "Work-related documents"
krepo switch work
krepo list
krepo current
```

### Natural Language

```
Save this article to my knowledge repo:
[paste content]
```

```
Process this URL: https://example.com/article
```

```
Save our conversation about Python
```

## What Gets Generated

Every document includes:

### YAML Frontmatter (20+ fields)
- Core: title, doc_type, source_type, dates, author
- Analysis: summary, abstract, key_topics, keywords, entities
- RAG: chunk_strategy, semantic_density, related_concepts
- Technical: word_count, has_code, has_tables, languages

### Structured Markdown
- Table of contents (for long docs)
- Executive summary
- Well-formatted content
- Code blocks with syntax highlighting
- Markdown tables
- Image descriptions

### Example Output

```markdown
---
title: "Understanding Neural Networks"
doc_type: "article"
source_type: "url"
source_url: "https://example.com/neural-networks"
processed_date: "2025-11-16"

summary: "Comprehensive guide to neural network architecture and training"
key_topics:
  - neural networks
  - deep learning
  - backpropagation
keywords:
  - AI
  - machine learning
  - training
  - models

chunk_strategy: "by_section"
semantic_density: "high"
audience_level: "intermediate"
has_code: true
word_count: 2500
---

# Understanding Neural Networks

## Summary
[Executive summary]

## Introduction
[Content converted to clean markdown]
...
```

## File Organization

Documents are organized by type:

```
docs/imported/
├── articles/        # Blog posts, articles, essays
├── reports/         # Research, analysis, data reports
├── guides/          # Tutorials, how-tos
├── references/      # API docs, code examples
└── other/           # Conversations, mixed content
```

**Multiple repositories:**
```
krepos/
├── work/docs/imported/
├── personal/docs/imported/
└── research/docs/imported/
```

## Benefits for RAG Systems

1. **Rich Metadata**: 20+ fields enable precise filtering and retrieval
2. **Semantic Chunking**: Section markers help with intelligent chunking
3. **Multiple Access Points**: Keywords, topics, entities for varied retrieval
4. **Contextual Summaries**: Abstracts that stand alone
5. **Cross-References**: Related concepts for knowledge graphs
6. **Type Classification**: Audience level, intent, density ratings

## Troubleshooting

### Skill doesn't trigger
- Check that skill is enabled in Settings
- Try explicit: "Use the doc-copy skill"

### GitHub operations fail
- Verify GitHub MCP server is configured
- Check GitHub token permissions
- Restart Claude Desktop
- Check token hasn't expired

### No completion report
- MCP server might not be connected
- Skill will fall back to providing markdown for manual save

## Support

- GitHub Issues: https://github.com/Chunkys0up7/ClaudeSkill/issues
- Documentation: See `Skill.md` for full workflow details
- MCP Setup: https://github.com/github/github-mcp-server

## License

MIT
