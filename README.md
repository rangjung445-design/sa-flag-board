# SA Counsellor Flag Board

A small web page that reads the Situational Analysis survey file, flags students who may need help, lets a counsellor track follow-ups, and gives supervisors counts without any student IDs. Uses only the synthetic dataset.

## How to run it
**One command** (from inside this folder):
```
python3 -m http.server 8000
```
Then open http://localhost:8000 in a browser.
No Python? Double-click `index.html` and choose `SA_sample_dataset_SYNTHETIC.csv` when asked.
**Live link:** _add your GitHub Pages link here_

## What works
- AC-01 to AC-05 (all Must items): student list, flags with reasons, Urgent pinned on top in red, follow-up status, supervisor view with "<5" for small counts.
- AC-06 filter by school, class, flag, and search by ID.
- AC-07 follow-up status is saved in the browser and survives a refresh.
- AC-08 one extra rule (below). AC-09 mobile-friendly layout, dark mode, can be deployed on GitHub Pages.

## What does not work yet
- Statuses are saved only in one browser on one computer. Two counsellors cannot share them yet.
- No login. In real use, only the school's counsellor should see the counsellor view.
- Free-text comments are shown but not used for flagging.

## Rules and assumptions
- Baseline rules applied in order; the first match sets the flag. All matching reasons are shown.
- **Data cleaning:** answers are tidied ("often" → "Often"). A blank or out-of-range answer counts as missing, never as a risk, and is shown as "Missing answer: q1 (check paper form)" (e.g. S-010).
- **Extra rule (AC-08):** if a student answers "Prefer not to say" to the self-harm question, they are flagged **High**, not None. The baseline rules ignore this answer, so 9 students would have been invisible. A student who avoids this question may be the one who most needs a quiet check-in. It can be switched off with `USE_EXTRA_RULE = false` in `index.html`.
- Supervisor view: any count from 1 to 4 is shown as "<5"; zero is shown as 0.

## Gaps I noticed in the brief
- S-023 wrote "Sometimes I don't want to go home" but is flagged None. Comments need a human or keyword check.
- "<5" alone can sometimes be worked out from the totals (e.g. 12 students, 8 None). A real system should also hide totals or merge small classes.

## How I used AI tools
Tool: Claude (Anthropic), used to explain the brief, write the code, and draft this README. I checked the flag results by hand against the rules.
- **Prompt that worked:** _"Build the prototype as a single file that opens in a browser, make it simple so I can understand it."_ It produced a one-file app with each step labelled, so I can explain and change it.
- **Prompt that failed:** _"I did not understand this task and don't know where to begin."_ It gave me a plan, not working code. I learned to ask for a specific output.

## What I would build next
1. Shared, secure storage so statuses and notes sync across counsellors, with login.
2. A comment check that marks worrying phrases for the counsellor to read.
3. A notes field and date for each follow-up, and a chart on the supervisor view.
