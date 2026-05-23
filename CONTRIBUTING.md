# Contributing to AI Security Resources

Thanks for helping make this better. This repo is maintained by practitioners for practitioners. Here's how to do it right.

## What We Want

- **New tools** (open-source preferred, commercial with clear value-add accepted)
- **New research papers** — must include significance, not just a link
- **CTF writeups and case studies** — real attack patterns > theoretical ones
- **Resources in non-English languages** (Chinese, French, German AI security research is underrepresented)
- **Corrections** — if something is deprecated, broken, or wrong, please fix it

## What We Don't Want

- Promotional content without technical substance
- Duplicate links
- Tools with no clear AI security use case
- Resources that haven't been personally vetted

## How to Submit

1. Fork this repository
2. Create a branch: `git checkout -b add/your-resource-name`
3. Add your resource to the relevant section
4. Make sure it follows the existing table format
5. Include a brief description that explains *why* it matters for AI security specifically
6. Submit a Pull Request with:
   - Title: `[SECTION] Add: Resource Name`
   - Body: What it is, why you're adding it, have you used it personally?

## Formatting

All tables follow this structure:

```markdown
| **[Tool Name](https://link)** | Creator | Description | Key Feature |
```

For papers:
```markdown
| **[Paper Title (Author et al.)](https://arxiv.org/abs/XXXX)** | Year | Why it matters |
```

## Questions

Open an issue. No Discord, no Slack — keep it async and public so others can benefit from the answer.
