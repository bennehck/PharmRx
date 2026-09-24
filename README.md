# PharmRx

Training tools for MPharm dispensing practicals and integrated OSCEs, built for an action research project on dispensing practice (Levels 5 and 6). Everything runs from **one self-contained HTML file**: no server, no accounts and no installation. It works in Edge, Chrome, Firefox or Safari, and can be shared through Microsoft Teams, SharePoint or a VLE.

> **Download:** [`dist/PharmRx.html`](dist/PharmRx.html) is the tutor copy. Keep it private because it holds the answer keys.

## What it does

### Dispensing practicals
- **Random training prescriptions.** Prescriptions are HS21-style, with Level 5 = 1 item and Level 6 = 2 items. Difficulty follows a six-session plan. Seeded errors include dose, frequency, brand, allergy, renal, controlled drug (CD) quantity/dose/date and legal problems (signature, date, DOB).
- **Prescriber queries.** Students can query legal issues. A simulated prescriber replies and amends the prescription, and marking uses the prescription as amended.
- **BSO coding.** Answers are Chemcode / quantity in the CodeBook unit. A non-dispensed item is coded 88888/1; if no items are dispensed, the form is not submitted. Rules come from the BSO CodeBook (September 2026), the BSO *Common Coding Issues* guide (03/25) and the *Coding and Endorsing* guidance.
- **CD record.** Includes a running balance, with 500 brought forward.
- **Automatic marking.** Covers decision, BSO code and quantity, CD status and balance, plus tutor rubric scores.
- **Analytics.** Detection rates by error type with 95% Wilson confidence intervals and reliability flags. Also a decision confusion matrix, progress across sessions, and printable individual and class feedback.

### OSCE stations (linked patient journeys)
Seven cases across **community pharmacy, hospital pharmacy and GP practice**. Each case follows one patient through linked stations:
- responding to symptoms
- medication history
- medicines reconciliation
- medicines optimisation
- prescriber communication (SBAR)
- **independent prescribing**
- dispensing (BSO)
- counselling

Other features:
- Case variants can be randomised per student.
- In practice mode students get feedback after each station; in assessment mode stations lock once submitted.
- For a live OSCE, you can print actor briefs, examiner mark sheets and model answers, and mark on a tablet.
- Station analytics.

### Student pages and records
- The tutor file generates a **student page** (`Student_<batch>.html` or `OSCE_<id>.html`) with no answers, library data or keys.
- Students download it from a Teams assignment and open it in a browser. When they finish, they hand in a small JSON answer file.
- Work is saved in the browser. In Edge and Chrome it can also be auto-saved to a linked OneDrive/SharePoint file.
- Tutors import the answer files, either individually or as the `.zip` downloaded from Teams.
- Accessibility: status colours are safe for colour-blind users (blue/orange, always paired with ✓/✗ and text), and pages support dark mode and phones.

## Quick start (tutor)
1. Open `dist/PharmRx.html` in Edge or Chrome. On the Home tab, link a OneDrive backup file.
2. **Dispensing:**
   1. In Settings, choose the level, session and date, then **Generate new batch**.
   2. **Download student page** and attach it to a Teams assignment.
   3. When students have handed in, **import** the answer files, then use **Marking** and **Analytics**.
3. **OSCE:**
   1. Go to **OSCE stations**, then **Create circuit**.
   2. **Download student page**. For a live OSCE, print the packs instead.
   3. Import the results.
4. **Help & self-test** runs a built-in end-to-end check (26 checks).

The Help tab has step-by-step Teams instructions and a printable student handout.

## Repository layout
```
src/        template.html, engine.js (dispensing), osce_engine.js, ui_*.js (UI, concatenated in order)
data/       data_lib.json (regimens, interactions, patients, plan), data_codebook.json (BSO CodeBook subset), data_osce.json (generated)
scripts/    build.py (builds dist/PharmRx.html), build_osce_data.py (authoring source for the OSCE case library)
tests/      engine/OSCE unit tests (Node) and Playwright end-to-end browser tests
dist/       PharmRx.html - the built tutor copy
excel/      earlier Excel/VBA version (Dispensing_Rx_Tool.xlsm, template, modDispensing.bas)
```

## Build and test
```bash
python3 scripts/build_osce_data.py   # only after editing OSCE cases
python3 scripts/build.py             # -> dist/PharmRx.html
node tests/engine.test.js            # dispensing engine (5,760 generated prescriptions)
node tests/osce.test.js              # every OSCE station and variant: model answer scores 100%
npm install && npm run test:browser  # Playwright end-to-end tests (Chromium)
```

## Important notes
- All patients and prescribers are **fictional**. Doses, error rules, interactions, OSCE keys and counselling points follow NICE/CKS, BNF and MHRA guidance, but **must be checked by a pharmacist on the module team before use**. The legal rules follow the Misuse of Drugs Regulations (NI) 2002 and the Human Medicines Regulations 2012.
- Keys and prescriber replies in student pages are encoded, not encrypted. Use the live mode for summative OSCEs.
- The BSO CodeBook data is a subset of the September 2026 CodeBook published by the Business Services Organisation (NI). Update `data/data_codebook.json` when a new CodeBook is released.
