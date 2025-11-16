---
# Document Metadata (YAML Frontmatter)
title: "Document Title Goes Here"
doc_type: "article|report|guide|reference|tutorial|other"
created_date: "YYYY-MM-DD"
processed_date: "YYYY-MM-DD"
source: "original-filename.ext"

# Content Analysis
summary: "A concise 2-3 sentence summary of the document that captures its essence and main purpose."
abstract: |
  A more detailed abstract of 100-200 words that covers the key points,
  findings, and conclusions of the document. This should provide enough
  context for someone to understand the document's value without reading
  the full content.

# Keywords and Topics
key_topics:
  - Topic 1
  - Topic 2
  - Topic 3
  - Topic 4
  - Topic 5

keywords:
  - keyword1
  - keyword2
  - keyword3
  - keyword4
  - keyword5
  - keyword6
  - keyword7
  - keyword8
  - keyword9
  - keyword10

entities:
  people:
    - "Person Name"
  organizations:
    - "Organization Name"
  technologies:
    - "Technology or Framework"
  locations:
    - "Location Name"

# Categories and Classification
categories:
  - Primary Category
  - Secondary Category
  - Tertiary Category

primary_intent: "educate|inform|reference|guide|persuade|analyze"
audience_level: "beginner|intermediate|advanced|expert"

# RAG Optimization
chunk_strategy: "by_section|by_page|by_topic|by_paragraph"
semantic_density: "low|medium|high"
related_concepts:
  - Related Concept 1
  - Related Concept 2
  - Related Concept 3

# Technical Metadata
word_count: 0
page_count: 0
has_code: false
has_tables: false
has_images: false
languages: []
---

# Document Title

> **Document Type**: [Article/Report/Guide/etc.]
> **Last Updated**: YYYY-MM-DD
> **Source**: Original filename

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Introduction](#introduction)
3. [Main Content](#main-content)
4. [Conclusion](#conclusion)
5. [References](#references)
6. [Appendix](#appendix)

## Executive Summary

A comprehensive summary that provides:
- The main purpose and scope of the document
- Key findings or main points
- Important conclusions or recommendations
- Target audience and use cases

This section should be substantial enough for readers to understand the document's value at a glance.

## Introduction

### Background

Context and background information that sets the stage for the document content.

### Purpose and Scope

Clear statement of what the document covers and what it aims to achieve.

### Intended Audience

Description of who should read this document and what prerequisite knowledge is expected.

## Main Content

### Section 1: [Section Title]

Content goes here with proper structure and formatting.

#### Subsection 1.1

More detailed content with hierarchical organization.

**Key Points:**
- Important point 1
- Important point 2
- Important point 3

### Section 2: [Section Title]

#### Code Examples

If the document contains code, format it properly:

```python
def example_function():
    """
    Example code with proper syntax highlighting
    """
    return "Hello, World!"
```

#### Tables

When presenting data, use well-formatted markdown tables:

| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Data A   | Data B   | Data C   |
| Value 1  | Value 2  | Value 3  |

### Section 3: Visual Content

**Figure 1: [Image Title]**

*Detailed description of the image, chart, or diagram:*
- What the visual represents
- Key elements and their meaning
- Data presented (if applicable)
- Notable patterns or insights
- Colors, shapes, and layout details
- Any text or labels visible

### Section 4: Lists and Enumerations

**Ordered Lists** (for sequential steps):

1. First step or point
2. Second step or point
3. Third step or point

**Unordered Lists** (for non-sequential items):

- Important concept A
  - Sub-point A1
  - Sub-point A2
- Important concept B
- Important concept C

**Definition Lists**:

**Term 1**
: Definition or explanation of term 1

**Term 2**
: Definition or explanation of term 2

## Conclusion

### Summary

Recap of the main points covered in the document.

### Key Takeaways

- Takeaway 1: Brief description
- Takeaway 2: Brief description
- Takeaway 3: Brief description

### Recommendations

If applicable, actionable recommendations based on the document content.

### Next Steps

Suggested actions or further reading for the audience.

## References

### Citations

1. Reference 1: [Title, Author, Year, URL]
2. Reference 2: [Title, Author, Year, URL]

### Further Reading

- Additional resource 1
- Additional resource 2
- Additional resource 3

### Related Documents

Links to related documents in this knowledge base:
- [Document Title 1](../path/to/doc1.md)
- [Document Title 2](../path/to/doc2.md)

## Appendix

### Appendix A: Additional Data

Supplementary information, raw data, or detailed tables.

### Appendix B: Glossary

**Term 1**: Definition
**Term 2**: Definition
**Term 3**: Definition

### Appendix C: Document Processing Notes

- **Original Format**: PDF/Image/CSV/DOCX/etc.
- **Processing Date**: YYYY-MM-DD
- **Processing Tool**: Claude Code doc-copy skill v1.0
- **Conversion Notes**:
  - Any special considerations during conversion
  - Limitations or areas requiring manual review
  - Quality of source document
  - Special handling of complex elements

### Appendix D: Change Log

| Date | Version | Changes |
|------|---------|---------|
| YYYY-MM-DD | 1.0 | Initial document processing |

---

**Document Hash**: [Optional: Git commit hash]
**Processing Status**: Complete
**Quality Score**: [Optional: Self-assessed quality rating]
