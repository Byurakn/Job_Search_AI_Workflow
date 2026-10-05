# How to Build Your Own AI Job Search System

*By Byurakn Ishkhanyan · October 2026*

Follow these three steps to turn your LinkedIn profile into a job search system run with Claude. Claude learns who you are, searches and scores jobs for you every weekday, and helps you tailor a CV and cover letter for each job worth applying to. Each step has prompts you can copy and paste. Change anything in \[square brackets\] to your own details.

This guide is based on a working setup used for a real job search in Denmark and Sweden in 2026. Expect about 2 to 3 hours to set it up, spread over a few sessions.

## Before you start

You need:

- **A Claude plan with Projects** (Pro, Max, Team or Enterprise). The Project is Claude's shared memory: everything saved there is available in every new chat.
- **Your LinkedIn profile as a PDF.** On your profile, click More → Save to PDF.
- **Past CVs and cover letters** (optional, but they help a lot). Even rejected applications show Claude how you describe your work.
- **A Google account** (optional), if you want CV templates stored in Google Drive.

To set up the Project:

1. In Claude, create a new Project, for example "Job Search".
2. Upload your LinkedIn PDF and past CVs and cover letters to the Project's files.
3. Paste this into the Project instructions:

```
In this project, my LinkedIn profile and past CVs are used to build CVs for job applications and to match jobs against my profile. There is a workflow for each job application. Never invent experience, numbers or skills: only use what is in this Project or what I tell you directly. Save important decisions and documents back to the Project so future chats can find them.
```

Start every chat for this system inside that Project.

All prompts in this guide are also saved as separate files in the [`prompts/`](prompts/) folder.

## The three steps at a glance

![The three steps: build Steps 1 and 2 once, then run Step 3 for each job](images/three-steps.png)

Steps 1 and 2 are set up once. Step 3 runs for each job you apply to, and what you learn there goes back into your baseline.

## Step 1: Career coach training

In this step Claude acts as your career coach and learns who you are. By the end you have a **career baseline document** (your values, skills and evidence) and **one CV template per type of job you can do**. Every later step reads from these, so take your time here. Plan for 1 to 2 hours.

### 1.1 Find your core values

This step uses Robert Glazer's six-question method: you find values by reflecting on real past decisions, not by picking nice words from a list. Claude then tests each value to check it is real. Start a new chat in the Project and paste:

```
Act as my career coach. Using Robert Glazer's six-question process for finding core values, help me find my values from real decisions I have made in my career and life, not from a list of words. Ask me one question at a time and wait for my answer.

For each value we find, test it with this Core Validator:
1. Can I use it to make a decision?
2. Does the opposite of it strike a nerve?
3. Is it a phrase, not a single word?
4. Can I rate myself on how well I live it?

For each value that passes, write: a one-sentence statement in my own voice, 2 to 3 pieces of evidence from my answers, what happens when it is violated, and a screening question I can use to judge a job against it.

When we are done, save everything to the Project as Career_Baseline.md.
```

Answer honestly and with real examples, for example a job you left and why. Expect 4 to 6 values. Rank them: the top one or two will weigh most when scoring jobs. If Claude spots a pattern in how you behave (for example, you leave quietly instead of raising problems), keep it in the doc. It is useful for interviews.

### 1.2 Build a skills inventory

In the same chat or a new one, paste:

```
Using my LinkedIn profile and all the CVs and cover letters in this Project, build a skills inventory and add it to Career_Baseline.md. Sort skills into tiers by how deep they really are, not how often they appear on my CVs:
- Strong / core: I use these confidently and could be tested on them.
- Real but less frequent: I have used them and can show it if asked.
- Lighter touch: used a few times, not core expertise.

For soft skills that are often over-claimed (resilience, adaptability, collaboration, leadership), ask me for one concrete example each before you add it. If my CVs and LinkedIn disagree, ask me which is correct. Do not add anything I have not confirmed.
```

This tiering stops Claude overselling skills you have barely used, and stops it hiding real strengths.

The baseline is a living document. Whenever a job ad asks for something and you remember relevant experience, tell Claude and ask it to add the evidence to Career\_Baseline.md.

### 1.3 Define your job categories

Most people qualify for more than one kind of role. Defining them up front lets the dashboard search for all of them, and gives each a CV template. Paste:

```
Based on Career_Baseline.md, my LinkedIn profile and my past CVs, define 3 to 5 job categories I realistically qualify for. For each category give: a name, 4 to 6 typical job titles, and the search keywords (in [English and the local language, e.g. Danish]) that would find those jobs. Explain briefly why I fit each one. Save it to the Project as Job_Categories.md.
```

Check the list. Remove categories you don't want, even if you qualify.

### 1.4 Build one CV template per category

Paste:

```
For each category in Job_Categories.md, build a CV template as a .docx file. Use this structure, in this order:
Name → one-line punch sentence → contact details → summary (short, motivated paragraph) → key skills → key results (quantified) → experience (reverse chronological, quantified bullets; condense older roles into "Earlier experience") → education → IT tools → languages → outside work.

Use only facts from Career_Baseline.md, my LinkedIn and my past CVs. No invented numbers. Emphasise the experience that matters most for each category. Then save a short note to the Project listing the templates and what makes each one different.
```

Optional extras:

- **Second language:** "Translate all templates into \[Danish\]. Keep every number exactly as in the English version, and translate section headings and language levels."
- **Google Drive:** connect Google Drive in Claude's settings, then ask: "Upload all templates to my Google Drive folder \[link\]."

Claude may not be able to save .docx files into the Project itself. If so, it gives them to you in the chat to download, and saves a short .md note to the Project instead. That is fine.

## Step 2: Dashboard creation

In this step you build a **live dashboard** that tracks every job Claude finds, with a score for each, and you set up **scheduled runs** so Claude searches for you every weekday morning. By the end you have one page that shows which jobs to apply for, which you've applied to, and why the rest were skipped. Plan for about 1 hour.

### 2.1 Write the search and scoring rules

First write down the rules in one config document. Every scheduled run reads it, so changing the rules later means editing one document. Paste, filling in the brackets:

```
Create a config document for an automated daily job search and save it to the Project as job_search_config.md. Include:

1. Candidate summary: a short summary from Career_Baseline.md, including my top values.
2. Locations in scope: [e.g. Greater Copenhagen; Skåne, Sweden; remote roles from Nordic employers]. Anything requiring relocation is out of scope.
3. Role families and keywords: use the categories and keywords in Job_Categories.md. Read each ad's real requirements; don't trust the title alone.
4. Sources to search: [e.g. LinkedIn, Indeed, Jobindex, company career pages for X, Y, Z]. Note which ones you can actually read.
5. Scoring rubric: score each job 1 to 10 from the hiring manager's point of view on these weighted criteria:
   - Role & domain fit 15%
   - Hard-skills match 15%
   - Seniority match 10%
   - Industry fit 10%
   - Values fit (based on my top values) 10%
   - Visible near-term impact 10%
   - Compensation adequacy 10%
   - Competitive positioning (realistic odds of an interview) 10%
   - Location & work mode 5%
   - Language requirements 5%
   Overall = weighted average, 1 decimal.
6. Apply gate: recommend applying only if Overall is [8.0] or higher. Below that, log the reason. For jobs at or above the gate, add a monthly salary estimate with a confidence level, and say which CV changes could raise the score.
7. Goal: [10] applications per month, at least [2] per week.
```

Adjust the weights to what matters to you. For example, if values fit is your main reason for leaving jobs, give it more weight. Note that Claude refuses to search LinkedIn. You might need a workaround, such as a Claude extension on Chrome. This tutorial doesn't cover that part.

### 2.2 Build the dashboard

Paste:

```
Build a job search dashboard as a published artifact with its own database, following job_search_config.md. Store each job as a record with: title, company, location, work mode, link, source, date found, the 10 scores, overall score, salary estimate and confidence, status (new / considering / applied / not applying), rationale (required when not applying), applied date and last updated.

Show these sections in this order:
1. Monthly goal header: applications this month against the monthly goal and this week against the weekly minimum, with a progress bar.
2. Active candidates (new and considering), sorted by overall score.
3. Applied, most recent first.
4. Not applying, with the rationale visible.

Each row shows title, company, location, work mode, a link to the ad, overall score, salary estimate, date and a status I can change myself.
```

Open the dashboard and try changing a job's status to check it saves. Pin it in your sidebar so it's easy to find.

### 2.3 Schedule the daily search

Paste:

```
Create a scheduled task that runs every weekday at [06:00] [my time zone]. Each run should:
1. Read job_search_config.md.
2. Search the sources for postings from the last 7 days in my locations and role families.
3. Skip jobs already on the dashboard (same link, or same title and company).
4. Log titles containing intern, student, trainee or praktik as "not applying" without fully scoring them.
5. Read and score every other new job, add a salary estimate, and save it to the dashboard as "new" or "not applying".
6. Never change jobs I have marked "considering" or "applied".
7. Update the goal counters and end with a short summary: counts per status and the top 3 new jobs.
```

### 2.4 Add a Friday review

Paste:

```
Create a second scheduled task every Friday at [15:00] [my time zone] that reviews the week's jobs on the dashboard and:
- looks for patterns in "not applying" reasons (e.g. always low on salary or seniority) and suggests keyword, source or role-family changes;
- checks whether enough jobs pass the apply gate, and whether scores seem too harsh or too generous compared with jobs I overrode;
- checks pace against my monthly goal;
- proposes changes to job_search_config.md and asks me before changing anything.
```

### 2.5 Add jobs you find yourself

Automated runs can't log into LinkedIn, and many job sites don't load properly for them. When you find a job yourself, paste it into any chat in the Project:

```
Here's a job I found: [paste the ad text or link]. Check it isn't already on the dashboard, score it with job_search_config.md, and add it with source "manual". Give me the score and a one-line verdict.
```

What to expect from sources:

- **Sweden:** Arbetsförmedlingen's free JobTech API works well. Ask Claude to use it.
- **Denmark** (Jobindex, company career sites): many pages load their listings with JavaScript, so automated runs see an empty page. Claude finds these jobs through web search instead, which catches fewer.
- **LinkedIn:** often the best source, but only through manual intake (2.5).

## Step 3: Fit assessment, CV tailoring and cover letter

In this step you save a fixed workflow once, then run it for every job you consider. Claude works through it one step at a time and **stops for your approval after each step**, so you stay in control and nothing is invented. Plan for 15 minutes to save the workflow, then 45 to 90 minutes per application.

### 3.1 Save the workflow (once)

Paste this whole prompt. Edit the parts in brackets first.

```
Save the following to the Project as Job_Application_Workflow.md. When I paste a job ad and say "Run the workflow", follow it step by step and stop for my approval after every step marked STOP. If you are missing information, ask me a specific question instead of guessing.

GROUND RULE: Nothing goes into a CV or cover letter unless it is in this Project (Career_Baseline.md, past CVs and letters) or I tell you in the chat. No invented achievements, numbers or claims. Mark all changes in red. Avoid generic AI phrasing.

STEP 0 – Intake. Read the ad. Load Career_Baseline.md, the matching CV template, past tailored CVs and job_search_config.md. Note the company, contact person, deadline and salary if given. Confirm the role in one line and go on.

STEP 1 – Fit assessment. Score the job with the rubric in job_search_config.md. Give a clear recommendation (apply / borderline / don't apply), the overall score, the 2–3 strongest and 2–3 weakest criteria, and any real gaps between the ad and my profile. Ask if I have relevant experience that isn't in the Project yet, and if I have a contact at the company. STOP.

STEP 2 – CV first draft. Start from the matching CV template. Format: [Scandinavian] reverse-chronological, in the language of the ad. Sections: job title + one-line selling sentence + short tailored summary; top 3 key results tied to the ad; 3–4 core skills with a short "why it matters"; experience trimmed to what fits the ad, quantified where real numbers exist; education; languages; outside work. Use the ad's key terms naturally for ATS (applicant tracking systems), standard headings, no tables or text boxes. Build it as a .docx. STOP.

STEP 3 – Skills section. Add a separate, keyword-dense Skills section in 2–4 clusters with short headings that fit the ad. Each bullet at most one line, no explanations, no overlap with Core skills, only documented skills. STOP.

STEP 4 – Hiring manager review. Re-read the CV as the hiring manager or recruiter for this ad. Find missing keywords that are true of me, repeated or generic phrasing, overlap between sections, overclaiming and underselling. Present edits as a table: Section | Old text | New text | Why. Every addition must be offset by a cut of the same size; name the cut. Flag honest gaps instead of hiding them. STOP, then apply only the edits I approve.

STEP 5 – Format and page fit. Ask for the target length if I haven't said. Adjust font size, margins and spacing so the last page is 60–90% full, then check every page. STOP and ask me to confirm the page count in Word.

STEP 6 – Cover letter. Default length: 3/4 of an A4 page, in the language of the ad. Tone: warm, personal, first person, in my voice. No em dashes. Structure: (1) why the company and role fit me, on values and content; (2) "By hiring me, you will gain a [type of hire] who will:" + 3–4 concrete, preferably quantified bullets; (3) "The value I bring:" + 3–4 bullets on how I work, built on 4 of my core values, written naturally; (4) a closing motivation specific to this employer. If there is a known gap, address it honestly in one short paragraph. Address it to the named contact, or ask me who. Match the CV's fonts and colours. STOP.

STEP 7 – Final polish. Read the CV and letter together. Flag anything generic, templated or repetitive and suggest a specific replacement. Check there are no em dashes. Present both final files. Then save a short note to the Project: [Company]_application_note.md with the score, key content decisions and follow-ups.
```

This workflow came from many rounds of feedback. If Claude keeps doing something you dislike (a phrase, a format, a tone), ask it to add a rule to Job\_Application\_Workflow.md so it doesn't happen again.

### 3.2 Run it for each job

When the dashboard (or your own browsing) shows a job worth a closer look, start a **new chat** in the Project and paste:

```
[paste the full job ad, or a link to it]

Run the workflow.
```

Then go through it step by step:

1. **Fit assessment:** read the weak points honestly. Say "no-go" to stop, or "go" to continue. You can apply below the gate if you have a good reason (a referral, a strong personal fit). Tell Claude, and it will note the override on the dashboard.
2. **CV draft, skills, review:** red text shows what's new. Check every red line is true. When Claude asks about a gap, add the evidence if you have it; the baseline gets better with every application.
3. **Format:** open the .docx in Word and check the page count yourself. Claude's preview uses a slightly different font.
4. **Cover letter:** read it aloud. If it doesn't sound like you, tell Claude which sentences to change and why.
5. **After applying:** say "I applied today" so Claude marks the job as applied on the dashboard and saves the application note.

Use a separate chat for each application. The Project carries everything over, and a fresh chat keeps Claude focused on one job.

## Keeping it running

The system improves when you feed results back in. Three habits make the biggest difference.

### Track your outcomes

Keep your own spreadsheet of applications with these columns: Company, Job title, Date applied, Decision date, Outcome (Rejection / Ghost / Interview / Waitlisted / blank = pending), Cover letter (Yes/No), Effort (1–5), Referral (Yes/No), Contacted before applying (Yes/No), Perceived match (Yes/No). Every two weeks, paste it into a chat:

```
Here is my updated application tracker: [paste or attach]. Analyse it and save the result to the Project as Conversion_Analysis.md: interview rate overall and by each column, how long rejections take, which pending applications are worth a follow-up, and whether any change in my approach (new CV layout, ATS tailoring) shows an effect. Say when a sample is too small to be sure.
```

In the original setup, after 124 applications, two things predicted an interview: rating the job a good match yourself (15% interview rate vs 0%), and contacting someone at the company before applying (roughly 2 to 4 times more interviews). Contacting people may help you more than sending more applications.

### Watch the scores

The same job can get different scores in different chats. In the original setup, one job scored 5.6, 7.1 and 7.6, depending on how strictly the ad was read. If scores seem too harsh, tell Claude:

```
Add a calibration rule to job_search_config.md: when a re-score is lower than an earlier score for the same job with no new negative evidence, keep the earlier score and note the difference. Review in the Friday check whether scoring is too harsh compared with jobs I chose to apply to anyway.
```

### Keep the baseline growing

Each fit assessment brings up questions such as "Do you have experience with X?". When you do, make sure Claude adds it to Career\_Baseline.md. After a few applications the baseline will hold far more than your LinkedIn profile, and CVs get faster and stronger.

### Files you will end up with

| File | Created in | What it does |
| --- | --- | --- |
| Career\_Baseline.md | Step 1 | Values, tiered skills and evidence. The single source of truth for CV content |
| Job\_Categories.md + CV templates | Step 1 | The roles you target, with one CV template each |
| job\_search\_config.md | Step 2 | Locations, sources, keywords, rubric, apply gate, goal |
| Job search dashboard | Step 2 | Every job found, its score and status |
| Job\_Application\_Workflow.md | Step 3 | The 7-step process with approval gates |
| \[Company\]\_application\_note.md | Step 3 | Score, decisions and follow-ups per application |
| Conversion\_Analysis.md | Ongoing | What is working, based on your outcomes |
