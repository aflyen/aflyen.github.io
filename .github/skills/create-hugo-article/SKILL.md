---
name: create-hugo-article
description: 'Create new Hugo blog articles (posts or pages). Use when: writing new blog posts, creating static pages, or adding content to areflyen.no. Generates properly formatted YAML frontmatter, creates article files with correct naming conventions, and ensures consistency with existing content.'
argument-hint: 'Article type (post/page), title, date, categories/tags'
---

# Create Hugo Article

## When to Use

- Writing a new blog post for `content/posts/`
- Creating a new static page for `content/pages/`
- Need a template with correct YAML frontmatter and file naming

## Hugo Article Structure

This Hugo site uses:
- **Posts**: Dated articles in `content/posts/` (older: `.html`, newer: `.md`)
- **Pages**: Static pages in `content/pages/` (`.md` files)
- **Format**: Markdown (`.md`) for new content
- **Naming**: `YYYY-MM-DD-slug.md` (lowercase, hyphens for spaces)
- **Date**: ISO 8601 format with timezone (`YYYY-MM-DDTHH:MM:SSZ`)

## Before You Create

1. Decide: **post** (dated, archived by date) or **page** (static, e.g., About)
2. Prepare: title, content, optional categories/tags
3. Choose date: today's date or a specific publish date

## Procedure

### Step 1: Generate Filename
- Use format: `YYYY-MM-DD-article-slug.md`
- Date: today or your specified date
- Slug: lowercase title with hyphens instead of spaces
- Example: `2026-05-12-create-hugo-article-skill.md`

### Step 2: Create YAML Frontmatter
Use the appropriate template below. All required fields must be present.

### Step 3: Add Content
- Use Markdown format (`.md`)
- Include headers, lists, code blocks as needed
- Reference assets from `/assets/YYYY/MM/` if including images

### Step 4: Save File
- Posts: `content/posts/YYYY-MM-DD-slug.md`
- Pages: `content/pages/YYYY-MM-DD-slug.md`

## Templates

### Blog Post Template

```markdown
---
title: "Your Article Title Here"
description: "Brief one-line description of the post"
date: "2026-05-12T10:30:00Z"
author:
  display_name: Are Flyen
categories:
  - Category Name
tags:
  - Tag 1
  - Tag 2
draft: false
---

## Introduction

Start your post content here.

## Main Section

Add your content with proper headers and formatting.

### Subsection

Details here.

## Conclusion

Wrap up your thoughts.
```

### Static Page Template

```markdown
---
title: "Page Title"
description: "Brief description shown in meta tags"
url: /page-slug/
draft: false
---

Your page content in Markdown format.

## Sections

Organize with headers as needed.
```

## Frontmatter Fields

### Required Fields

| Field | Type | Example | Notes |
|-------|------|---------|-------|
| `title` | string | "My Article Title" | The page/post title |
| `date` | ISO 8601 | "2026-05-12T10:30:00Z" | Posts only; use `Z` for UTC timezone |
| `draft` | boolean | false | Set to `false` to publish |

### Optional Fields (Posts)

| Field | Type | Example |
|-------|------|---------|
| `description` | string | "Summary of the article" |
| `author.display_name` | string | "Are Flyen" |
| `categories` | array | `[SharePoint, Azure]` |
| `tags` | array | `[PowerShell, CSOM]` |

### Optional Fields (Pages)

| Field | Type | Example |
|-------|------|---------|
| `description` | string | "Meta description" |
| `url` | string | "/about/" |
| `preview` | string | "preview.jpg" |

## Publishing Workflow

1. **Create** the file in the correct directory with frontmatter
2. **Preview**: Run Hugo locally to verify formatting
3. **Review**: Check headers, links, and code blocks render correctly
4. **Publish**: Set `draft: false` and commit to main branch
5. **Verify**: Check the live site after deploy

## File Organization

```
content/
├── posts/
│   ├── 2024-09-06-update-your-azure-devops.md
│   ├── 2022-09-19-enable-guest-users.md
│   └── YYYY-MM-DD-slug.md  ← New posts go here
└── pages/
    ├── 2022-09-12-about.md
    └── YYYY-MM-DD-slug.md  ← New pages go here
```

## Quick Reference: Common Tasks

**Format date for now**: Use current date in format `2026-05-12T14:30:00Z`

**Create slug**: 
- Title: "How to Fix SharePoint"
- Slug: `how-to-fix-sharepoint`
- Filename: `2026-05-12-how-to-fix-sharepoint.md`

**Add code block**:
````markdown
```powershell
Get-SPSite -Limit 1
```
````

**Add image**:
```markdown
![Alt text](/assets/2026/05/image-name.png)
```

**Add reference link**:
```markdown
[Link text](https://example.com)
```

## Troubleshooting

**Post not appearing**: Check `draft: false` and date is not in future

**Frontmatter not parsing**: Verify YAML syntax (colons, quotes, indentation)

**Date format error**: Use ISO 8601 with `Z` suffix: `2026-05-12T10:30:00Z`

**Slug conflicts**: Ensure filename is unique; add extra descriptor if needed
