Create a scheduled task that runs every weekday at [06:00] [my time zone]. Each run should:
1. Read job_search_config.md.
2. Search the sources for postings from the last 7 days in my locations and role families.
3. Skip jobs already on the dashboard (same link, or same title and company).
4. Log titles containing intern, student, trainee or praktik as "not applying" without fully scoring them.
5. Read and score every other new job, add a salary estimate, and save it to the dashboard as "new" or "not applying".
6. Never change jobs I have marked "considering" or "applied".
7. Update the goal counters and end with a short summary: counts per status and the top 3 new jobs.
