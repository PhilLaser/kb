# Repository roadmap

## Target tree

```text
.
|-- README.md
|-- SUMMARY.md
|-- .gitbook.yaml
|-- CONTRIBUTING.md
|-- STYLE-GUIDE.md
|-- CHANGELOG.md
|-- _templates/
|-- assets/
|-- 00-start-here/
|-- 01-foundations/
|-- 02-windows/
|-- 03-linux/
|-- 04-networking/
|-- 05-active-directory/
|-- 06-azure-and-entra/
|-- 07-audit-playbooks/
|-- 08-offensive-techniques/
|-- 09-tool-reference/
|-- 10-malware-development/
|-- 11-reporting/
|-- 12-labs/
|-- 13-reference/
|-- windows/                 # legacy; migrate page-by-page
|-- linux/                   # legacy; migrate page-by-page
`-- maldev/                  # legacy; migrate only after core stabilizes
```

## Migration plan

1. Establish standards, templates, navigation, source registry, and QA checks.
2. Build canonical foundations used by multiple audits.
3. Build the four priority audit playbooks and their evidence model.
4. Map each legacy page to a canonical destination; record redirects and conflicts.
5. Rewrite or merge pages individually with source verification and link checks.
6. Move malware-development material only after the core structure is stable.
7. Remove a legacy path only in an explicitly reviewed migration batch.

## Assessment findings

- The current repository is heavily weighted toward AD abuse notes.
- Root documentation, GitBook configuration, contribution rules, and templates were absent.
- Linux and malware sections contain placeholder naming.
- Navigation is explicit but reflects the old taxonomy.
- At least one page embeds a complete saved HTML document with binary font data; it needs later evidence extraction, licensing review, and replacement by a normal citation.
- Existing command accuracy, references, cleanup steps, and operational-risk labels require page-by-page review.
