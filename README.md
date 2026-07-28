# rankdone.zip — Update Instructions

Ye zip sirf **naye/updated files** hai. Apne existing repo me in files ko
inhi paths par copy-paste/overwrite karo (folder structure same rakhi hai:
`Qsbropeedu-main/...`).

## MODIFIED files (existing files overwrite karo):
- app.py
- templates/play.html
- static/js/quiz.js
- static/css/style.css

## NEW files (add karo):
- templates/quiz_ended.html
- templates/owner_dashboard.html
- templates/owner_scorecard.html
- templates/owner_top5.html
- templates/owner_update.html
- static/js/owner_dashboard.js
- static/js/owner_scorecard.js
- static/js/owner_top5.js
- static/js/owner_update.js

Baaki sab files (index.html, edit.html, edit.js, main.js, generated.html,
generated.js, admin-deter.js, utils/, Dockerfile, requirements.txt) **untouched**
hai — is zip me include nahi ki gayi, kyunki unme koi change nahi kiya.

---

## Naya kya add hua (summary)

1. **Instructions Screen** — jab bhi koi quiz link (`/play?v=...`) khole,
   sabse pehle Title + Total Questions (top-left) + Total Marks (top-right) +
   Test name + marking-scheme guide + motivation line dikhta hai, saath me
   2 buttons: **START TEST** (seedha name-input wale flow me le jaata hai,
   jaisa pehle tha) aur **OWNER DASHBOARD**.

2. **Owner Dashboard login** — Confirm popup → Owner Nick Name popup →
   Owner Key popup. Dono values `app.py` me hardcoded hai:
   - `OWNER_DASH_NICKNAME = "MSBrOHU68@YaAR"`
   - `OWNER_DASH_KEY = "ToXic#ViPxMSvBRO!9Qx7"`
   Wrong value par silently instructions page par wapas.

3. **Owner Dashboard (`/owner/<quiz_id>`)** — 6 buttons:
   - Students Score card List (`/owner/<id>/scorecard`) — live table
     (index, true/dense Rank, Name, Marks, Percentage), auto-refresh har
     2.5 second, + "Download list" (.txt).
   - View Top 5 Students list (`/owner/<id>/top5`) — index, Name, Rank,
     Marks, live + download.
   - View Original Link — popup + copy button.
   - Delete Quiz Link — confirm popup → Confirmation Key popup
     (`DELETE_CONFIRM_KEY = "Confirm#ViPxMSvBRO$6Kz2"`, hardcoded in
     app.py). Permanently deletes quiz + images + attempts from MongoDB;
     future visits to that link show a **"QUIZ ENDED 🧐"** page.
   - UPDATE QUIZ (`/owner/<id>/update`) — same editing UI/UX as the
     existing Edit Panel, but edits the LIVE quiz directly (questions,
     answers, images, numbering). "Update Now" saves + shows
     "Updated database ✅" + returns to dashboard. "Back to menu" cancels.
   - Back — returns to the instructions screen.

4. **Live Rank for students** — after submitting, the result modal now
   also shows the student's own true/dense rank (recomputed against every
   attempt on that quiz at the moment of submission — reattempts get a
   fresh rank too).

Sab kuchh MongoDB collections ka use karta hai jo already hai
(`quizzes`, `attempts`, `question_images`) + ek chhoti nayi collection
`deleted_quizzes` (sirf tombstone ke liye — quiz_id + deleted_at, koi
question/answer data nahi — taaki deleted link par "QUIZ ENDED" dikha
sakein).

Current upload → edit → submit & generate flow bilkul as-it-is hai,
kuchh bhi touch nahi hua usme.
