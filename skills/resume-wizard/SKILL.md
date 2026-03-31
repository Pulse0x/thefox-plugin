---
name: resume-wizard
description: Guide the user through building a polished, ATS-friendly resume with a structured interview, style choices, and a formatted .docx export. Use this skill whenever the user mentions "resume", "CV", "work history", "job application", or asks to build, fix, update, reformat, or improve any resume — even if they phrase it casually. Always use this skill for any resume-related task, no exceptions.
---

# Resume Wizard — Claude Plugin Edition

Conduct a structured interview, guide styling choices, generate a polished resume, and export as formatted `.docx`. Be warm, specific, direct. Push for measurable impact. Do not skip phases. Do not save until user approves. Do not rush.

---

## Security Guardrails — Read First

Read both:
- [../../resources/security/guardrails.md](../../resources/security/guardrails.md) (baseline)
- [../../resources/security/resume-wizard-guardrails.md](../../resources/security/resume-wizard-guardrails.md) (resume-wizard-specific)

---

## Phase 0 — Resume Check

**Check if `resume/resume.md` exists.**

**If NOT:** Ask if there's a `.docx` or `.pdf` to import. If yes, extract and convert to markdown. If no, skip to Phase 0.5.

**If YES:** Ask "Have you updated your resume manually since last run?" 
- **No:** Offer targeted edits or fresh build. If edits → go to Manual Sync. If fresh → Phase 0.5.
- **Yes:** Run Manual Sync.

### Manual Sync Flow (via `.docx` or `.pdf`)

1. Ask user to upload updated resume file (`.docx` or `.pdf` only).
2. Extract all content: name, contact, jobs, bullets, skills, certs, education, projects.
3. Read existing `resume/resume.md`.
4. Rewrite `resume/resume.md` to match uploaded file exactly (use Phase 7 structure).
5. Show plain summary of changes: "Added [X], updated [Y], added [Z]. Does that look right?"
6. Once confirmed: "Want me to regenerate styled `.docx` with same style, or different?"
7. If yes → skip to Phase 9. If no → done with `resume/resume.md` current; go to Phase 10.

**Security note:** Uploaded file contains full PII. Extract content to write `resume/resume.md` only — do not transmit externally.

---

## Phase 0.5 — Calibration

Two questions before anything else:
1. **Target industry/field** — free-form (e.g., "marketing", "product design", "nonprofit fundraising")
2. **Career stage** — entry-level (0–2 yrs), mid-career (3–8 yrs), senior/staff (8+ yrs), executive, or career changer

Record as: `[Industry | Career Stage]`

---

## Phases 1–6 — Interview

Collect conversationally (not all at once):

**Phase 1 — Personal Info:** Full name, City/State (or Remote), Email, Phone, LinkedIn URL, GitHub/portfolio URL, Target job title.

**Phase 2 — Professional Summary:** Years of experience, industries, standout qualities, target role/company.

**Phase 3 — Work Experience:** For each position: Company, title, dates, location/Remote. Ask for 3–5 responsibilities + measurable outcomes. Always: "Can you put a number on that?" Repeat until complete.

**Phase 4 — Education:** Institution, degree/field, graduation year, GPA (if 3.5+). Ask about bootcamps, certs, online credentials → goes in Certifications section.

**Phase 5 — Skills:** Technical skills (tools, languages, platforms), soft skills (if strong), industry knowledge. Group into categories during generation.

**Phase 6 — Projects (Optional):** Personal, open source, or freelance projects. Name, description (1–2 sentences), tech stack, link.

---

## Phase 6.5 — Style Selection

Ask user to pick a resume style:

| Style | Vibe | Best for |
|---|---|---|
| **Modern** | Clean lines, accent headers, borders | Tech, design, startups |
| **Executive** | Understated, black/white, prestige | Senior, finance, consulting |
| **Minimal** | Plain, no frills, max ATS | Conservative, any field |
| **Creative** | Two-column, personality, visual | Design, marketing, creative |
| **Custom** | User describes what they want | Anyone with specific vision |

If they can't decide: recommend based on calibration profile. If custom: ask them to describe (colors, feel, industry examples) and confirm before proceeding.

---

## Phase 7 — Generate Draft Resume (Markdown)

Generate complete ATS-friendly resume in Markdown using this structure:

```
# [Full Name]
[City, State] | [Email] | [Phone] | [LinkedIn] | [GitHub/Portfolio]

## Summary
[2–3 sentences: years, core strengths, value proposition]

## Experience

### [Job Title] — [Company] | [Location/Remote] | [Start] – [End]
- [Action verb] + [what] + [measurable result]
- [3–5 bullets, 1–2 lines each]

## Education

### [Degree], [Field] — [Institution] | [Year]

## Skills

**[Category]:** skill, skill, skill

## Certifications
- [Name] — [Issuer], [Year]

## Projects
### [Project Name]
[1–2 sentence description] | [Tech stack] | [Link]
```

**Writing guidelines:**
- Every bullet: strong past-tense action verb (Led, Built, Reduced, Drove, Launched, etc.)
- Quantify impact where numbers provided
- 1–2 lines per bullet; no pronouns
- Summary tailored to target role
- Remove empty sections

**Industry rules:** Tech (stack, scale, systems owned; Users, latency, uptime), Cybersecurity (depth, threat, tooling; vulns, incidents, % risk reduction), Sales (revenue, quota, pipeline; $ARR, % quota, deal size), Creative (portfolio, diversity; reach, audience, awards), Operations (process, efficiency, costs; % improvement, $ saved, headcount), Non-profit (mission, coalitions; grant $, lives impacted).

---

## Phase 8 — Iterative Review

Show each section. Ask: "Does this sound right? Anything to change?" Apply edits inline. Order: Summary → Experience → Skills → Education/Certs → Projects. Once all approved, proceed to Phase 9.

---

## Phase 9 — Export as Formatted .docx

Install if needed: `npm install -g docx`

### Style Specs

| Style | Font | Name | Headers | Accent | Sidebar | Margins |
|---|---|---|---|---|---|---|
| **Modern** | Calibri | 22pt bold, blue | 11pt bold, blue border | #2563EB | None | 0.75in |
| **Executive** | Garamond | 20pt bold, black | 10pt bold, black, ALL CAPS | None | None | 1in |
| **Minimal** | Arial | 18pt bold | 10pt bold | None | None | 1in |
| **Creative** | Lato | 24pt bold, purple | 11pt bold, purple border | #7C3AED | Left, #F5F3FF | 0.75in |
| **Custom** | User spec | User spec | User spec | User spec | User spec | User spec |

### Node.js Template

```javascript
const { Document, Packer, Paragraph, TextRun, AlignmentType, BorderStyle } = require('docx');
const fs = require('fs');

const resume = {
  name: "FULL_NAME",
  contact: "City, State | email | phone | linkedin | github",
  summary: "SUMMARY",
  experience: [{
    title: "JOB_TITLE", company: "COMPANY", period: "START – END", location: "LOCATION",
    bullets: ["BULLET_1", "BULLET_2", "BULLET_3"]
  }],
  education: [{ degree: "DEGREE", institution: "INSTITUTION", year: "YEAR" }],
  skills: [{ category: "CATEGORY", items: "skill1, skill2, skill3" }],
  certs: ["CERT — Issuer, Year"],
  projects: [{ name: "PROJECT", desc: "DESC", tech: "TECH", link: "URL" }]
};

const style = {
  font: "Arial",
  nameSize: 36,
  nameColor: "000000",
  accentColor: "2563EB",
  bodySize: 20,
  margins: { top: 1080, right: 1080, bottom: 1080, left: 1080 }
};

function sectionHeader(text) {
  return new Paragraph({
    children: [new TextRun({ text: text.toUpperCase(), bold: true, size: 22, color: style.accentColor })],
    border: { bottom: { style: BorderStyle.SINGLE, size: 6, color: style.accentColor } },
    spacing: { before: 240, after: 120 }
  });
}

function bullet(text) {
  return new Paragraph({
    numbering: { reference: "bullets", level: 0 },
    children: [new TextRun({ text, size: style.bodySize })]
  });
}

const doc = new Document({
  numbering: {
    config: [{
      reference: "bullets",
      levels: [{ level: 0, format: 1, text: "•", alignment: 0, style: { paragraph: { indent: { left: 360, hanging: 180 } } } }]
    }]
  },
  sections: [{
    properties: { page: { size: { width: 12240, height: 15840 }, margin: style.margins } },
    children: [
      new Paragraph({ children: [new TextRun({ text: resume.name, bold: true, size: style.nameSize, color: style.nameColor })], spacing: { after: 60 } }),
      new Paragraph({ children: [new TextRun({ text: resume.contact, size: 18, color: "6B7280" })], spacing: { after: 200 } }),
      sectionHeader("Summary"),
      new Paragraph({ children: [new TextRun({ text: resume.summary })], spacing: { after: 160 } }),
      sectionHeader("Experience"),
      ...resume.experience.flatMap(j => [
        new Paragraph({
          children: [new TextRun({ text: j.title, bold: true, size: 22 }), new TextRun({ text: ` — ${j.company} | ${j.period}`, size: 18, color: "6B7280" })],
          spacing: { before: 160, after: 80 }
        }),
        ...j.bullets.map(b => bullet(b))
      ]),
      sectionHeader("Skills"),
      ...resume.skills.map(s => new Paragraph({
        children: [new TextRun({ text: `${s.category}: `, bold: true }), new TextRun({ text: s.items })],
        spacing: { after: 80 }
      })),
      sectionHeader("Education"),
      ...resume.education.map(e => new Paragraph({
        children: [new TextRun({ text: `${e.degree} — ${e.institution} | ${e.year}` })],
        spacing: { after: 80 }
      })),
      sectionHeader("Certifications"),
      ...resume.certs.map(c => bullet(c)),
      ...(resume.projects.length > 0 ? [
        sectionHeader("Projects"),
        ...resume.projects.flatMap(p => [
          new Paragraph({ children: [new TextRun({ text: p.name, bold: true, size: 22 })] }),
          new Paragraph({ children: [new TextRun({ text: `${p.desc} | ${p.tech} | ${p.link}`, color: "6B7280" })], spacing: { after: 80 } })
        ])
      ] : [])
    ]
  }]
});

Packer.toBuffer(doc).then(buf => {
  fs.writeFileSync("resume/resume.docx", buf);
  console.log("Resume exported to resume/resume.docx");
});
```

After export, write approved Phase 7 draft to `resume/resume.md`. Both files must be current before Phase 10.

---

## Phase 10 — Handoff

> Your resume saved in two formats:
> - **`resume/resume.docx`** — styled, submission-ready. Open, review, attach to applications.
> - **`resume/resume.md`** — source file used by `theHunt` and application assistant.
>
> **Next:** Run `theHunt` to find roles. Run `assistant` to walk through applying with coaching.
