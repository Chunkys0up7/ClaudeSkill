# Quick Start Guide - Claude Doc-Copy Skill

Get started with the doc-copy skill in 5 minutes!

## Step 1: Verify Installation

Ensure the skill is properly installed:

```bash
ls -la .claude/skills/doc-copy.md
ls -la .claude/commands/kthis.md
ls -la .claude/commands/krepo.md
```

You should see all three files. If not, ensure you've cloned the repository correctly.

## Step 2: Save Your First Content (Fastest Method)

The quickest way to save content:

```
/kthis

[Then provide your content - paste text, file path, URL, etc.]
```

That's it! The `/kthis` command activates the skill and processes your content in one step.

**Example:**
```
/kthis

https://example.com/great-article
```

## Step 2 Alternative: Full Skill Invocation

You can also use the traditional method:

```
Use the doc-copy skill
```

or

```
/skill doc-copy
```

## Step 3 (Optional): Create Separate Knowledge Repositories

Want to organize content by project or person? Create separate repos:

**Create a new repository:**
```
/krepo create Cam
```

**Switch between repositories:**
```
/krepo switch Cam
```

**List all repositories:**
```
/krepo list
```

Now when you use `/kthis`, content will be saved to the "Cam" repository!

## Step 4: Content Examples

Here's what you can save with `/kthis`:

The skill handles ANY type of content - not just files! Here are examples:

### Example 1: Process a PDF (Uploaded File)

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

### Example 2: Process Pasted Content

```
Save this article I found:

[Paste any content here - article, documentation, text, etc.]
```

Claude will:
1. ✅ Analyze the pasted content
2. ✅ Generate appropriate title
3. ✅ Structure with proper headers
4. ✅ Generate comprehensive metadata
5. ✅ Save to `docs/imported/articles/YYYY-MM-DD-[generated-title].md`

### Example 3: Process a Web URL

```
Save this blog post: https://example.com/great-article
```

Claude will:
1. ✅ Fetch content from the URL
2. ✅ Convert HTML to markdown
3. ✅ Extract title, author, date
4. ✅ Generate metadata (includes source URL)
5. ✅ Save to `docs/imported/articles/YYYY-MM-DD-great-article.md`

### Example 4: Process Generated Content

```
Save the code you just generated
```

or

```
Document the solution we just created
```

Claude will:
1. ✅ Take the previously generated content
2. ✅ Add documentation and context
3. ✅ Structure appropriately
4. ✅ Generate metadata
5. ✅ Save to appropriate directory

### Example 5: Process a Code Snippet

```
Save this Python function:
```python
def analyze_data(df):
    return df.groupby('category').sum()
```
```

Claude will:
1. ✅ Identify the programming language
2. ✅ Add documentation and usage examples
3. ✅ Generate metadata (has_code: true)
4. ✅ Save to `docs/imported/references/YYYY-MM-DD-python-data-analysis.md`

### Example 6: Process a Conversation

```
Save our conversation about Docker best practices
```

Claude will:
1. ✅ Structure conversation as Q&A
2. ✅ Extract key insights and learnings
3. ✅ Organize by topics discussed
4. ✅ Generate metadata
5. ✅ Save to `docs/imported/other/YYYY-MM-DD-docker-best-practices-discussion.md`

### Example 7: Process an Image (Screenshot/Diagram)

```
Process this screenshot: ~/Pictures/architecture-diagram.png
```

Claude will:
1. ✅ Analyze the visual content in detail
2. ✅ Extract any visible text
3. ✅ Describe the diagram structure
4. ✅ Generate metadata
5. ✅ Save to `docs/imported/other/YYYY-MM-DD-architecture-diagram.md`

### Example 8: Process a CSV File

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

### Use Case 1: Building a Knowledge Base from Any Source

Process documentation from multiple sources:

```
Process all PDFs in ~/Documents/company-docs/
Save this API guide: https://docs.example.com/api
Save this internal document: [paste content]
```

Result: Unified, searchable knowledge base from files, URLs, and pasted content.

### Use Case 2: Capturing Web Content

Save valuable online content:

```
Save this tutorial: https://realpython.com/python-decorators/
Save this Stack Overflow answer: https://stackoverflow.com/...
Save this blog post: https://example.com/post
```

Result: Permanent, searchable copies with full metadata.

### Use Case 3: Documenting Generated Solutions

Preserve Claude's generated work:

```
[After Claude generates code or solutions]
Save that database migration script
Document the architecture you designed
Save your explanation of the algorithm
```

Result: Well-documented, searchable reference materials.

### Use Case 4: Code Library

Build a searchable code snippet library:

```
Save this utility function: [paste code]
Save this configuration: [paste config]
Document this pattern we just created
```

Result: Organized code reference with documentation.

### Use Case 5: Learning Journal

Document your learning conversations:

```
Save our discussion about React hooks
Document this debugging session
Save this Q&A about SQL optimization
```

Result: Searchable learning archive with key insights extracted.

### Use Case 6: Research Paper Collection

Convert academic papers:

```
Process this research paper: ~/Papers/ml-research.pdf
```

Result: Papers with abstracts, keywords, and citations properly extracted.

### Use Case 7: Converting Legacy Documents

Modernize old documents:

```
Process this scanned document image: ~/Archives/old-manual.jpg
```

Result: OCR-like text extraction with visual descriptions.

### Use Case 8: Multi-Source Documentation Projects

Combine content from everywhere:

```
# From files
Process ~/docs/spec.pdf

# From web
Save https://competitor.com/feature-docs

# From conversations
Save our brainstorming session

# From generated content
Save the implementation plan you created
```

Result: Comprehensive documentation from diverse sources.

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

### 5. Organize by Project or Person
Use multiple repositories for better organization:
```
/krepo create Work
/krepo create Personal
/krepo create Research
```

Then switch between them:
```
/krepo switch Work
/kthis
[Add work-related content]

/krepo switch Personal
/kthis
[Add personal content]
```

Each repo has the same category structure:
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

## Example Workflow: Complete Multi-Repo Pipeline

**Scenario:** You want separate knowledge bases for Work and Personal content.

```
# Step 1: Set up repositories
/krepo create Work
/krepo create Personal

# Step 2: Add work content
/krepo switch Work

/kthis
~/Documents/work/api-spec.pdf

/kthis
https://company-blog.com/new-feature

/kthis
[Paste meeting notes...]

# Step 3: Add personal content
/krepo switch Personal

/kthis
~/Downloads/recipe.pdf

/kthis
https://interesting-blog.com/article

# Step 4: Check what you've saved
/krepo list

# Step 5: Review the outputs
```bash
ls krepos/Work/docs/imported/*/
ls krepos/Personal/docs/imported/*/
```

# Step 6: Load into your RAG system
# Each repo can feed a different vector database or namespace

# Step 7: Query your knowledge bases separately or together
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
