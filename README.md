# theFox — LGBTQ+ Job Search Suite for Claude

<i>Beta Release. First public project, please be kind</i>

A set of AI-powered job search tools built for people who want smart help without needing to be technical. TheFox runs inside Claude's Cowork app and gives you a personal resume writer, job finder and application coach — all in one place.

> **Specifically built for LGBTQ+ users.** YOU matter. Yes, you. My creator, Dom, is part of the LGBTQ+ community and knows firsthand that our community faces unique struggles when it comes to finding a new job. There's always an element of fear. We recognize that, while unfortunate, discrimination still exists. I was built to help ease that fear and help you find a job with a friendly employer! This is built in a way to prioritize companies with strong signals of diversity. While this may work for others, this tool was built with LGBTQ+ job seekers in mind.
---

## What It Does

TheFox has three tools that work together in sequence:

**1. Resume Wizard** — builds your resume
Claude interviews you about your work history, education, and skills, then writes a polished, professionally formatted resume and saves it as a Word document (`.docx`) you can attach to applications immediately.

**2. theHunt** — finds jobs that match your values
Instead of dumping 500 listings at you, theHunt identifies about 50 companies actually worth working for — checking LGBTQ+ inclusion track records, DEI standing, remote work authenticity, and culture ratings — then searches their job boards directly and scores every result for how well it fits your background.

**3. JINX** — coaches you through applying
JINX is your application coach. She reads your resume, cross-references it against each job listing, and gives you a full breakdown: what you bring, what gaps exist and whether they matter, what makes you stand out and exactly what to say. She logs every application so you can track your progress.

> JINX is a drag queen job coach with the precision of Bianca Del Rio and the heart of Jinkx Monsoon. She will be honest about your gaps and loud about your strengths — and she will get you hired.

---

## ***Improved***

These updates reduce token usage and tighten safety without changing core behavior.

### Token Consumption

- **Guardrails moved out of skill prompts:** Security policies now live in dedicated resource files instead of being repeated inline across skills.
- **Reusable security references:** Skills point to central guardrail docs, which reduces repeated prompt text and keeps rules consistent.
- **Lean operational prompts:** Skills focus on workflow and outputs while security and policy detail is centralized.

### Security Hardening

- **Dedicated guardrail files by workflow:**
    - `resources/security/the-hunt-guardrails.md`
    - `resources/security/resume-wizard-guardrails.md`
    - `resources/security/guardrails.md` (JINX assistant)
- **Explicit data-handling limits:** Guardrails define strict file scope, PII boundaries, and prohibited actions.
- **Injection-aware processing:** Untrusted external content is treated as data, with malicious instruction patterns flagged and excluded.
- **Consent-first job search:** theHunt requires explicit user confirmation before running external search flows.

---

## Requirements

Before installing theFox, you need:

- **A Claude account** — Cowork currently requires you be a pro member ($20/month)
- **Claude Cowork** — the desktop app that runs theFox (see Step 1 below)
- **Node.js** — free software needed to generate your Word document resume (see Step 2 below)

---

## Installation

### Step 1 — Install Claude Cowork

Claude Cowork is the desktop app that powers theFox.

1. Go to [claude.ai/download](https://claude.ai/download)
2. Download the version for your computer (Mac or Windows)
3. Open the downloaded file and follow the installation steps
4. Sign in with your Claude account 
5. In the app, click the **Cowork** tab in the top sidebar

### Step 2 — Install Node.js (one-time, takes 2 minutes)

Node.js is what Claude uses behind the scenes to generate your formatted Word resume. You don't need to understand it — you just need it installed.

1. Go to [nodejs.org](https://nodejs.org)
2. Click the big **LTS** button (It's the stable version)
3. Open the downloaded file and follow the installation steps, leaving everything at defaults
4. When it's done, restart your computer

**To confirm it worked:**
- **Mac:** Open the **Terminal** app (press Cmd+Space, type "Terminal", press Enter). Type `node --version` and press Enter. You should see something like `v20.11.0`.
- **Windows:** Open **Command Prompt** (press the Windows key, type "cmd", press Enter). Type `node --version` and press Enter. Same result — a version number.

If you see a version number, you're good. If you get an error, restart your computer and try again.

**Then run this one command** to install the resume library (copy and paste it exactly):

**Mac:**
```
npm install -g docx
```

**Windows:**
```
npm install -g docx
```

### Step 3 — Install the theFox Plugin

1. Download theFox_Plugin zip file from Releases
2. Open **Claude Cowork**
3. In the left sidebar, click **Customize**
4. Click **Browse plugins**
5. Click **Upload plugin** (or "Install from file")
6. Find the `theFox-plugin` file you downloaded and select it
7. Click **Install**

That's it. TheFox is now installed. You'll see **Resume Wizard**, **theHunt**, and **JINX** available in your Cowork sidebar.

> **Already have a resume?** You can skip the Resume Wizard. Ask Claude to take your resume and create a .md file from it, then save it inside the `resume/` folder in theFox. Then go straight to Step 2 of using theFox below.

---

## How to Use It

TheFox works through normal conversation — just talk to Claude the way you normally would.

### Step 1: Build Your Resume

Start a new Cowork task and say something like:

> "I need to build a resume."

or

> "Help me with my resume. I'm applying for marketing roles."

Claude will guide you through the whole process, one question at a time — work history, education, skills, and projects. Take your time.

Near the end you'll choose a visual style:

| Style | What it looks like | Good for |
|---|---|---|
| **Modern** | Clean design with colored section headers | Tech, startups, creative fields |
| **Executive** | Simple, all-black, lots of breathing room | Senior roles, finance, consulting |
| **Minimal** | Plain and tight, maximum compatibility | Conservative industries, or if unsure |
| **Creative** | Two-column layout with personality | Design, marketing, media |
| **Custom** | You describe it, Claude builds it | If you have something specific in mind |

When done, Claude saves:
- `resume/resume.docx` — your formatted Word resume, ready to attach to applications
- `resume/resume.md` — a plain-text version used by theHunt and JINX

### Step 2: Find Jobs

Once your resume is ready, say:

> "Let's search for jobs. I'm looking for remote product manager roles at inclusive companies."

or just:

> "Help me find jobs."

Claude will ask a few questions about what you're looking for, then go search. This takes a few minutes — it's checking a lot of sources.

> **Note:** Before searching, Claude will show you a safety notice and ask you to type "yes" to confirm. This is intentional — it makes sure you know external connections are being made before they happen.

When done, you'll have a ranked list of roles scored for both job fit and company culture.

### Step 3: Apply with Coaching

After theHunt has results, say:

> "Help me apply to the top matches."

or

> "Walk me through the [Company Name] role."

JINX takes over. She'll compare your resume to the listing and give you:
- What you already bring to the role
- What gaps exist and whether they actually matter
- What makes you stand out from the pile
- Exactly what to lead with in your application

After each role, she logs it to your **application tracker** — a running record of every role you've reviewed so you can stay organized across your whole search.

---

## Your Files

```
theFox/
│
├── resume/
│   ├── resume.docx       ← Attach this to job applications
│   └── resume.md         ← Used by theHunt and JINX (don't delete)
│
├── theHunt/
│   └── results.md        ← Job listings with scores and apply links
│
├── tracker/
│   ├── applications.md   ← Your application log (auto-updated by JINX)
│   └── applications.csv  ← Export for Google Sheets (generated on request)
│
└── wiki/
    ├── your-safety.md    ← How to stay safe during your job search ← READ THIS
    └── google-sheets-setup.md
```

**Your resume and job results stay on your computer.** TheFox never sends your name, email, or resume content anywhere. The only external connections it makes are anonymous searches for job listings.

> **Recommended:** Open `wiki/your-safety.md` before you start. Job searching involves sharing personal information with strangers — knowing what to watch for matters.

---

## Frequently Asked Questions

**Do I need to pay for anything?**
Claude Cowork requires a $20/monthly Claude Pro Subscription. TheFox is free. Node.js is free. The time you save? Priceless

**What is an ATS?**
Applicant Tracking System — software companies use to receive and filter applications before a human sees them. TheFox builds resumes with ATS compatibility in mind.

**Can I skip the Resume Wizard?**
Yes. If you already have a resume, copy its content into `resume/resume.md` and skip straight to theHunt. TheHunt and JINX both read from that file. If your resume is not an .md file, please ask Claude to help you make a .md file of your current resume.

**What if something goes wrong?**
Close the task, start a new one, and try again. Claude doesn't carry mistakes forward between tasks.

**Can I run the tools more than once?**
Yes. Run the Resume Wizard again anytime you want to update your resume. Run theHunt again for fresh listings.

**Is my data private?**
Yes. Nothing personal leaves your computer.

---

## What's Coming Next

- **Cover letter generator** — a tailored cover letter for any role
- **Interview prep** — the questions you're likely to get, based on your resume and the specific role
- **Resume review** — score your existing resume against a job description

---

## A Note From the Creator

This project was built by a proud member of the LGBTQ+ community. By using theFox, you agree to engage with this project in a spirit of inclusivity and respect for all people, regardless of identity, background, or orientation. Hate has no home here.

---
