# Quick Start Guide - Claude Doc-Copy Skill

Get started with the doc-copy skill in 5 minutes!

## Step 1: Verify Installation

Ensure the skill is properly installed:

```bash
ls -la .claude/skills/doc-copy.md
```

You should see the skill file. If not, ensure you've cloned the repository correctly.

## Step 2: Activate the Skill

In Claude Code, invoke the skill:

```
Use the doc-copy skill
```

or

```
/skill doc-copy
```

## Step 3: Process Your First Document

### Example 1: Process a PDF

```
Process this PDF: ~/Documents/my-research-paper.pdf
```

Claude will:
1. ✅ Read and extract all content from the PDF
2. ✅ Describe any images, charts, or diagrams
3. ✅ Generate comprehensive metadata
4. ✅ Create a RAG-optimized markdown file
5. ✅ Save to `docs/imported/reports/YYYY-MM-DD-my-research-paper.md`
6. ✅ Commit and push to Git

### Example 2: Process an Image

```
Process this screenshot: ~/Pictures/architecture-diagram.png
```

Claude will:
1. ✅ Analyze the visual content in detail
2. ✅ Extract any visible text
3. ✅ Describe the diagram structure
4. ✅ Generate metadata
5. ✅ Save to `docs/imported/other/YYYY-MM-DD-architecture-diagram.md`

### Example 3: Process a CSV File

```
Process this data: ~/Data/sales-report.csv
```

Claude will:
1. ✅ Convert CSV to markdown table
2. ✅ Analyze columns and data types
3. ✅ Provide statistical summary
4. ✅ Generate metadata
5. ✅ Save to `docs/imported/reports/YYYY-MM-DD-sales-report.md`

## Step 4: Review the Output

Navigate to the `docs/imported/` directory and check your processed document:

```bash
cat docs/imported/reports/2025-11-16-my-research-paper.md
```

You'll see:
- **YAML frontmatter** with rich metadata
- **Executive summary** with key insights
- **Structured content** with proper markdown formatting
- **Image descriptions** for all visuals
- **Processing notes** documenting the conversion

## Step 5: Use in Your RAG System

The processed documents are now ready for:

### Vector Database Upload
```python
# Example: Load into a vector database
import chromadb

client = chromadb.Client()
collection = client.create_collection("documents")

# The rich metadata enables powerful filtering
collection.add(
    documents=[content],
    metadatas=[yaml_frontmatter],
    ids=[doc_id]
)
```

### Semantic Search
The comprehensive keywords and topics enable accurate retrieval:
- Full-text search across content
- Metadata filtering (type, category, audience)
- Entity-based search (people, organizations, tech)
- Topic-based clustering

### Knowledge Graph
Use the extracted entities and relationships:
- People mentioned in documents
- Organizations discussed
- Technologies referenced
- Related concepts

## Common Use Cases

### Use Case 1: Building a Knowledge Base

Process all your documentation:

```
Process all PDFs in ~/Documents/company-docs/
```

Result: Searchable, categorized knowledge base optimized for RAG.

### Use Case 2: Research Paper Collection

Convert academic papers to searchable format:

```
Process this research paper: ~/Papers/ml-research.pdf
```

Result: Papers with abstracts, keywords, and citations properly extracted.

### Use Case 3: Converting Legacy Documents

Modernize old documents:

```
Process this scanned document image: ~/Archives/old-manual.jpg
```

Result: OCR-like text extraction with visual descriptions.

### Use Case 4: Data Documentation

Document your datasets:

```
Process this CSV and explain its structure: ~/Data/customer-data.csv
```

Result: Data dictionary with column descriptions and statistics.

## Tips for Best Results

### 1. High-Quality Source Documents
- Use clear, high-resolution PDFs
- Ensure images are legible
- Provide well-structured source files

### 2. Provide Context
```
Process this API security guide for developers: ~/Docs/api-guide.pdf
```
Context helps generate more accurate metadata.

### 3. Batch Processing
```
Process all documents in this directory: ~/Documents/batch/
```
Process multiple related documents together for consistency.

### 4. Review and Refine
After processing, you can ask Claude to:
```
Update the metadata for docs/imported/guides/2025-11-16-api-guide.md to add more keywords related to authentication
```

### 5. Organize by Project
Use the category directories effectively:
- `articles/` - Blog posts, news, updates
- `reports/` - Research, analysis, whitepapers
- `guides/` - Tutorials, how-tos, documentation
- `references/` - API docs, specs, manuals
- `other/` - Everything else

## Customization

### Modify Metadata Fields

Edit `.claude/skills/doc-copy.md` to add custom metadata fields:

```yaml
# Add custom fields
project: "Project Name"
version: "1.0"
status: "draft|reviewed|published"
```

### Adjust Chunking Strategy

Request specific chunking in your prompt:

```
Process this document and optimize for paragraph-level chunking: ~/Doc/file.pdf
```

### Custom Categories

Request custom categorization:

```
Process this as a 'specification' document: ~/Specs/api-spec.pdf
```

## Troubleshooting

### Problem: Document Not Reading Correctly

**Solution**: Try converting to PDF first or provide a different format.

### Problem: Metadata Not Accurate

**Solution**: Provide more context in your request:
```
Process this advanced machine learning research paper for expert audience: ~/Papers/paper.pdf
```

### Problem: Git Push Failing

**Solution**: The skill retries automatically. If it continues to fail:
1. Check your Git credentials
2. Verify you're on the correct branch
3. Check network connectivity

### Problem: Images Not Described Well

**Solution**: Request focus on visuals:
```
Process this document and provide detailed descriptions of all diagrams: ~/Docs/architecture.pdf
```

## Next Steps

1. **Process your first batch** of documents
2. **Integrate with your RAG system** using the generated markdown files
3. **Customize the skill** to fit your specific needs
4. **Build automation** to process documents automatically
5. **Share your processed knowledge base** with your team

## Example Workflow: Complete Document Pipeline

```bash
# Step 1: Collect documents
mkdir ~/Documents/to-process

# Step 2: In Claude Code, process them
"Use the doc-copy skill to process all PDFs in ~/Documents/to-process"

# Step 3: Review the output
ls docs/imported/*/

# Step 4: Load into your RAG system
# (Use your preferred vector database loader)

# Step 5: Query your knowledge base
# (Use your RAG application)
```

## Support

- **Documentation**: See `README.md` for full details
- **Template**: Check `docs/imported/TEMPLATE.md` for structure reference
- **Skill Source**: Review `.claude/skills/doc-copy.md` for implementation
- **Issues**: Report problems in the GitHub repository

---

**Ready to start?** Just say:

```
Use the doc-copy skill to process: /path/to/your/document
```

Happy document processing! 🚀
