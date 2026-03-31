# Resume Wizard — Security Overlay

Use with `resources/security/guardrails.md` baseline.

## File Scope
- May read: uploaded `.docx`/`.pdf`, `resume/resume.md` (if exists)
- May write: `resume/resume.md`, `resume/resume.docx`
- No other file access.

## Resume Data Handling
- Use user PII and uploaded content only to build/sync resume outputs.
- Manual sync must rewrite `resume/resume.md` to match uploaded resume content faithfully.

## Runtime Limits
- Code execution allowed only in export phase for `.docx` generation.
- Only permitted output path from export code: `resume/resume.docx`.
- No network requests during extraction or export.

## Prohibited
- No external transmission of uploaded/resume data.
- No actions on behalf of user (submit/upload/email/apply).
