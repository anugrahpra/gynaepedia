# M-CHAT-R/F Web Tool — Build Specification

> Build spec for adding an autism screening tool to the Gynae Pedia clinic website.
> All open questions have been resolved; this document is **build-ready**.

---

## 1. Executive Summary

Add an **Assessments** section to the Gynae Pedia clinic website (https://gynaepedia.co.in), starting with the **M-CHAT-R™ autism screening tool**. The tool collects responses from parents of toddlers aged 16–30 months, scores them, generates a PDF report, and emails the result to Dr. Ashish Prakash. No data is stored; no copy is sent to the parent. The framework should be extensible — additional assessments (paediatric or gynaecological) will be added later.

---

## 2. About M-CHAT-R/F

**M-CHAT-R/F** = Modified Checklist for Autism in Toddlers, Revised, with Follow-Up. Authored by **Diana Robins, Deborah Fein, and Marianne Barton (© 2009)**. The most widely-used screening tool worldwide for early identification of autism spectrum disorder.

**Target age:** Children **16 to 30 months**.
**Format:** 20 yes/no questions answered by parent/caregiver. Takes ~5 minutes.
**Purpose:** A screening tool only — never a diagnosis. Identifies children who should receive a diagnostic evaluation.

**Mandatory copyright notice (must appear on the tool page):**
> © 2009 Diana Robins, Deborah Fein, & Marianne Barton.
> The M-CHAT-R/F is free to use for clinical, research, and educational purposes.
> The questions must be presented in their exact wording without modification.

Official source: https://mchatscreen.com/

---

## 3. The 20 M-CHAT-R Questions (Verbatim — Do NOT Modify Wording)

For each question, **"Risk Answer"** is the answer that scores 1 failure point. For all items except 2, 5, and 12, **"No" = fail**. For items **2, 5, and 12**, **"Yes" = fail**.

| # | Question | Risk Answer |
|---|---|---|
| 1 | If you point at something across the room, does your child look at it? *(For example, if you point at a toy or an animal, does your child look at the toy or animal?)* | No |
| 2 | Have you ever wondered if your child might be deaf? | **Yes** |
| 3 | Does your child play pretend or make-believe? *(For example, pretend to drink from an empty cup, pretend to talk on a phone, or pretend to feed a doll or stuffed animal?)* | No |
| 4 | Does your child like climbing on things? *(For example, furniture, playground equipment, or stairs)* | No |
| 5 | Does your child make unusual finger movements near his or her eyes? *(For example, wiggling his or her fingers close to his or her eyes?)* | **Yes** |
| 6 | Does your child point with one finger to ask for something or to get help? *(For example, pointing to a snack or toy that is out of reach)* | No |
| 7 | Does your child point with one finger to show you something interesting? *(For example, pointing to an airplane in the sky or a big truck in the road)* | No |
| 8 | Is your child interested in other children? *(For example, does your child watch other children, smile at them, or go to them?)* | No |
| 9 | Does your child show you things by bringing them to you or holding them up for you to see — not to get help, but just to share? *(For example, showing you a flower or a stuffed animal)* | No |
| 10 | Does your child respond when you call his or her name? *(For example, does he or she look up, talk or babble, or stop what he or she is doing when you call his or her name?)* | No |
| 11 | When you smile at your child, does he or she smile back at you? | No |
| 12 | Does your child get upset by everyday noises? *(For example, does your child scream or cry to noise such as a vacuum cleaner or loud music?)* | **Yes** |
| 13 | Does your child walk? | No |
| 14 | Does your child look you in the eye when you are talking to him or her, playing with him or her, or dressing him or her? | No |
| 15 | Does your child try to copy what you do? *(For example, wave bye-bye, clap, or make a funny noise when you do)* | No |
| 16 | If you turn your head to look at something, does your child look around to see what you are looking at? | No |
| 17 | Does your child try to get you to watch him or her? *(For example, does your child look at you for praise, or say "look" or "watch me"?)* | No |
| 18 | Does your child understand when you tell him or her to do something? *(For example, if you don't point, can your child understand "put the book on the chair" or "bring me the blanket"?)* | No |
| 19 | If something new happens, does your child look at your face to see how you feel about it? *(For example, if he or she hears a strange or funny noise, or sees a new toy, will he or she look at your face?)* | No |
| 20 | Does your child like movement activities? *(For example, being swung or bounced on your knee)* | No |

---

## 4. Scoring & Decision Framework

### Step 1 — Compute total score
Sum failed items. Range: **0–20**.

### Step 2 — Apply decision rule

| Score | Risk Level | Recommended Action |
|---|---|---|
| **0–2** | **LOW** | No further action. If child is under 24 months, re-screen after their 2nd birthday. |
| **3–7** | **MEDIUM** | Administer M-CHAT-R/F structured follow-up interview on failed items. |
| **8–20** | **HIGH** | Bypass follow-up. Refer immediately for diagnostic evaluation and early intervention. |

### Step 3 — Web tool behaviour
**The web tool stops after the initial 20 questions.** The clinician (Dr. Ashish Prakash) performs the M-CHAT-R/F follow-up interview during the in-person consultation. The parent is told:
> "Thank you. Dr. Ashish will follow up with you during your consultation."

This is shown to every parent regardless of risk level. **Do NOT display the raw score, risk category, or any interpretation to the parent.** The clinician interprets and communicates.

---

## 5. User Flow (Parent-Facing)

### Page A — Assessments landing (`/assessments`)
Lists available screening tools. Currently only one card:
- **M-CHAT-R™ Autism Screening** (16–30 months)
  - Short description (~2 sentences)
  - Estimated time (~5 min)
  - "Start screening →" button → goes to `/assessments/mchat-r`

Build this as a flexible grid so additional assessments can be dropped in later (peads + gynae). Each card uses the same visual pattern.

### Page B — M-CHAT-R tool (`/assessments/mchat-r`)

**Section 1 — Introduction**
- Title: "M-CHAT-R™ — Autism Screening for Toddlers"
- 2–3 sentences explaining what this is, who it's for
- Disclaimer block (highlighted):
  > "This screening is primarily designed for children between **16 and 30 months** of age. Results outside this range may not be reliable. Please continue only if your child is within or near this age range."
- Important note:
  > "This is a screening tool, not a diagnosis. Your responses will be sent to Dr. Ashish Prakash, who will discuss the results with you during your consultation."
- Copyright notice (small, footer-style on this page)
- "Begin screening →" button

**Section 2 — Parent & child intake form**
Required fields:
- Parent name
- Parent phone number (with country code default +91)
- Child first name (or initials)
- Child date of birth (date picker)
- Child sex (optional: Male / Female / Prefer not to say)

Compute child age in months from DOB. Display: "Your child is X months old."

If child is **outside 16–30 months**, show a non-blocking warning:
> "Note: The M-CHAT-R is validated for children aged 16–30 months. Your responses will still be sent to the clinic, but interpretation may be limited. You may want to call us at +91 9560517234 to discuss alternative screening options."

Allow them to proceed regardless.

"Continue to questions →" button.

**Section 3 — The 20 questions**
- Present all 20 on one scrolling page (best for completion rates on mobile)
- Each question:
  - Question number badge
  - Question text (with example clarifications in parentheses)
  - Two large radio buttons / pill buttons: **Yes** / **No**
- Persistent progress indicator at top showing X / 20 answered
- Cannot submit until all 20 are answered
- Show validation error if user tries to submit incomplete

**Section 4 — Anti-spam check**
- A simple "I am not a robot" checkbox (no CAPTCHA — just a single self-attestation checkbox)
- Optional: also include a hidden honeypot field

**Section 5 — Submit**
- "Submit screening" button
- During submission: spinner; disable button to prevent double-submit

**Section 6 — Confirmation screen**
Shown after successful submission. Do NOT show score, risk, or interpretation.
> ### Thank you.
> Your responses have been sent to Dr. Ashish Prakash.
> **Dr. Ashish will follow up with you during your consultation** to discuss the results.
> If you'd like to speak to the clinic sooner, call or WhatsApp **+91 9560517234**.

Buttons: "Return to homepage" / "WhatsApp the clinic"

---

## 6. Doctor-Facing Email + PDF

### Email
- **From:** Gynae Pedia Website (via Web3Forms)
- **To:** `ashprakashiphone@gmail.com`
- **Subject:** `M-CHAT-R Screening — [Child first name] — [RISK LEVEL] — Score X/20`
  - Example: `M-CHAT-R Screening — Aarav — MEDIUM RISK — Score 5/20`
- **Body:** Plain-text version of the same info that's in the PDF (see PDF template below) — so the doctor can read it even if attachment download fails.

### PDF Attachment
Generated client-side using **jsPDF** (or pdf-lib). Filename: `mchat-r-[child-first-name]-[YYYY-MM-DD].pdf`.

**PDF layout (single page if possible, two pages OK):**

```
═══════════════════════════════════════════════════════
                    GYNAE PEDIA
            R-12/28, Raj Nagar, Ghaziabad
          +91 9560517234 · gynaepedia.co.in
═══════════════════════════════════════════════════════

M-CHAT-R™ AUTISM SCREENING — RESULT

┌─────────────────────────────────────────────────────┐
│ RISK LEVEL:       MEDIUM (3–7 failed items)         │
│ TOTAL SCORE:      5 / 20                            │
│ RECOMMENDED:      Conduct M-CHAT-R/F follow-up      │
│                   interview on failed items.        │
└─────────────────────────────────────────────────────┘

CHILD
  Name:              Aarav S.
  Date of birth:     12 Aug 2024
  Age:               21 months
  Sex:               Male

PARENT / CAREGIVER
  Name:              Priya Sharma
  Phone:             +91 98765 43210

SUBMITTED
  Date / time:       20 May 2026, 14:32 IST
  Source:            gynaepedia.co.in/assessments/mchat-r

──────────────────────────────────────────────────────
FAILED ITEMS (5)
──────────────────────────────────────────────────────
  Q3.  Does your child play pretend or make-believe?
       → Answered: No
  Q7.  Does your child point with one finger to show
       you something interesting?
       → Answered: No
  Q9.  Does your child show you things by bringing
       them to you...?
       → Answered: No
  Q14. Does your child look you in the eye when you
       are talking to him or her...?
       → Answered: No
  Q16. If you turn your head to look at something,
       does your child look around to see what you
       are looking at?
       → Answered: No

──────────────────────────────────────────────────────
ALL RESPONSES
──────────────────────────────────────────────────────
  Q1.  …                                  Yes  [pass]
  Q2.  Have you ever wondered if ...      No   [pass]
  Q3.  Does your child play pretend?     No   [FAIL]
  …
  Q20. Does your child like movement?     Yes  [pass]

──────────────────────────────────────────────────────
INTERPRETATION GUIDE
──────────────────────────────────────────────────────
  0–2:  LOW RISK — No further action. If <24 months,
        re-screen after age 2.
  3–7:  MEDIUM RISK — Follow-up interview required
        before referral.
  8–20: HIGH RISK — Refer immediately for diagnostic
        evaluation.

This screen indicates: MEDIUM RISK.

A medium-risk score requires the M-CHAT-R/F structured
follow-up interview before referral. If, after the
follow-up interview, the score remains ≥ 2, refer for
a diagnostic evaluation and early-intervention services.

──────────────────────────────────────────────────────
This screening was completed on the Gynae Pedia website.
© 2009 Diana Robins, Deborah Fein & Marianne Barton.
M-CHAT-R™ is used here with permission for clinical use.
──────────────────────────────────────────────────────
```

### Sending the email + PDF via Web3Forms
Web3Forms supports file attachments via `multipart/form-data`. The frontend code should:
1. Build the FormData object with all form fields plus the email body text
2. Generate the PDF with jsPDF, get the Blob
3. Append the Blob to FormData as `attachment` (or whatever field name Web3Forms uses)
4. POST to `https://api.web3forms.com/submit`

**Use the existing Web3Forms key** (currently going to `anugrahp@gmail.com`): `67a377ba-0a7d-41ef-881b-680883091d76` — UNLESS a separate key is created for assessments. Recommend asking the user before launch whether to create a new key targeted at `ashprakashiphone@gmail.com`. *(Note: per the user, the existing key going to `anugrahp@gmail.com` is acceptable for the time being on the callback form. For this assessment, since the result must reach Dr. Ashish directly, the cleanest fix is to create a NEW Web3Forms key with `ashprakashiphone@gmail.com` and use that key only for the assessment submission.)*

---

## 7. Technical Implementation Notes

### Project context
- **Repo:** `/Users/anugrah/Documents/Claude/Projects/gynaepedia/` (worktree at `.claude/worktrees/brave-wright-73d3e6/`)
- **Hosting:** Vercel, auto-deploys on push to `main`
- **Domain:** `gynaepedia.co.in`
- **Git workflow:** edit in worktree → commit → merge to main → `git push`
- **Site type:** Static HTML/CSS/JS, no build step, no backend
- **Vercel config:** `vercel.json` already has `cleanUrls: true` and `trailingSlash: false` — URLs without `.html` extension work

### File structure
Create these new files in the project root:
- `assessments.html` — landing page, served at `/assessments`
- `mchat-r.html` — the screening tool, served at `/mchat-r` (simpler than nested folder)

If you want true nesting (`/assessments/mchat-r`), create:
- `assessments/index.html`
- `assessments/mchat-r.html`

Both work with Vercel's cleanUrls. Recommend the flat layout for simplicity.

### Robots / sitemap
- This tool is **NOT** to be indexed by Google. Add `<meta name="robots" content="noindex, nofollow" />` to both new pages.
- Do **NOT** add these URLs to `sitemap.xml`.

### Design system (must match the rest of the site)
The new pages must look identical to the rest of `gynaepedia.co.in`. Use exactly the same:

**Color tokens:**
```css
--ivory:        #f5efe4;
--ivory-deep:   #ece4d3;
--paper:        #faf6ee;
--ink:          #1a1d1a;
--ink-soft:     #3a3d39;
--forest:       #2d4a3e;
--forest-deep:  #1f3329;
--terracotta:   #c2502c;
--terracotta-soft: #e08966;
--mustard:      #c89c3e;
--rule:         #d8cdb6;
--rule-strong:  #2d4a3e;
```

**Fonts (load from Google Fonts):**
- Fraunces (serif, for headings) — weights 300–700
- Inter Tight (sans, for body) — weights 300–700
- DM Mono (mono, for labels/eyebrows) — weights 400, 500

Font URL:
```
https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,300;9..144,400;9..144,500;9..144,600;9..144,700&family=Inter+Tight:wght@300;400;500;600;700&family=DM+Mono:wght@400;500&display=swap
```

**Paper-grain noise overlay:** Same `body::before` SVG noise that's used on all existing pages.

**Layout:** Same `max-width: 1280px`, gutter `clamp(20px, 4vw, 56px)`, body font 17px.

### Nav and footer (must be present and identical to existing pages)
**Top nav** (copy from `dr-ashish-prakash-pediatrician-ghaziabad.html` lines ~415–435):
- Logo (`gynaepedia-icon.jpg`) linking to `/`
- Links: Home, Dr. Ashish Prakash, Dr. Neera Bhan, Services, Reviews, **Assessments** (new), FAQ, Visit
- WhatsApp Us CTA button

**Mobile bottom nav:** Same as existing 5-tab bottom nav — but add an "Assessments" item. Options:
- Replace one of the existing 5 tabs (e.g. swap out "Reviews" for "Assess" on screening pages only)
- Or accept that 6 tabs is too many for mobile and use the top hamburger instead

Decide based on whichever looks cleaner.

**Footer:** Exact same forest-deep footer as existing pages.

**Floating "Call clinic" button:** Same as existing doctor pages — fixed bottom-right on mobile, hidden on desktop, with padding-bottom on footer to prevent overlap.

### Existing nav update (sitewide)
Add **"Assessments"** link to the top nav on **all existing pages**:
- `index.html` (homepage)
- `dr-ashish-prakash-pediatrician-ghaziabad.html`
- `dr-neera-bhan-gynaecologist-ghaziabad.html`

Place it between "Reviews" and "FAQ" (or wherever makes most sense visually).

### Accessibility
- All form inputs have `<label for="...">`
- Radio buttons grouped with `<fieldset>` and `<legend>` containing the question
- Submit button has clear focus state
- Color contrast on text ≥ 4.5:1 on the ivory background
- Form errors announced via `aria-live="polite"` region
- Keyboard navigation works for all radio buttons (arrow keys to move within a group, Tab between groups)

### Performance
- Inline CSS in the page `<style>` block (matches existing pattern)
- Lazy-load nothing in the assessment (it's all above-fold form content)
- jsPDF is ~300 KB — load it via CDN with `<script defer>` so it doesn't block initial render

### State management
- Plain vanilla JS, no framework
- Form state held in a single object or simple variables
- Persist nothing to localStorage (per user: no data storage)
- After successful submit, clear the form

---

## 8. Implementation Order (Recommended)

1. **Create `/assessments` landing page** with the M-CHAT-R card. Verify it matches site design and is reachable from updated nav.
2. **Add "Assessments" link to top nav on all 3 existing pages** (`index.html`, both doctor pages).
3. **Build `/mchat-r` page skeleton** — intro section, age disclaimer, intake form, all 20 questions, anti-spam check, submit button.
4. **Wire up the scoring logic** (JS): compute fail count, determine risk level. Verify with sample inputs.
5. **Build the PDF generator** (jsPDF). Test that the layout looks clean.
6. **Wire up Web3Forms submission** with PDF attachment. Test that the email arrives with the PDF.
7. **Build confirmation screen.** Verify the "Dr. Ashish will follow up" message displays correctly.
8. **Mobile QA:** Test on a real phone. Forms must be tappable, no horizontal scroll, modal/confirm screens look good.
9. **Verify `noindex` meta tag is on both new pages**, sitemap is unchanged.

---

## 9. Acceptance Criteria

- [ ] `/assessments` page loads, shows the M-CHAT-R card, matches site design
- [ ] `/mchat-r` page loads, all 20 questions present in exact wording
- [ ] Items 2, 5, 12 score "Yes" as failure; all other items score "No" as failure
- [ ] Total score range is correctly 0–20
- [ ] Risk level computed correctly: 0–2 LOW, 3–7 MEDIUM, 8–20 HIGH
- [ ] Form cannot be submitted with any unanswered question
- [ ] Age disclaimer shows for children outside 16–30 months but does not block submission
- [ ] On submit: email arrives at `ashprakashiphone@gmail.com` with subject line `M-CHAT-R Screening — [Name] — [Risk] — Score X/20`
- [ ] PDF attachment is present and shows all 20 questions, all responses, failed items highlighted, and the interpretation guide
- [ ] Parent sees "Dr. Ashish will follow up" screen — NOT their score or risk
- [ ] Tool is reachable from top nav on homepage and both doctor pages
- [ ] `noindex` meta tag is present on both new pages
- [ ] Sitemap does NOT include the new URLs
- [ ] Page is mobile-responsive and looks identical in style to the rest of the site
- [ ] Copyright notice "© 2009 Diana Robins, Deborah Fein & Marianne Barton" is visible on the tool page

---

## 10. Out of Scope (Do NOT Build)

- Hindi translation (English only for now)
- Parent receives a copy of the result (no)
- Storing responses in a database, Google Sheet, or anywhere else (no)
- Showing the parent their score or interpretation (no)
- The clinician follow-up interview (M-CHAT-R/F flowcharts) — done in person
- Multi-child sessions (one child per session)
- Privacy policy page (ignored for now)
- Indexing in Google / sitemap (no)

---

## 11. Future Considerations (Not in This Build)

- Additional paediatric assessments: re-screening M-CHAT-R, ASQ, PEDS
- Additional gynaecological assessments: EPDS (Edinburgh Postnatal Depression Scale), PCOS self-screen, menopause symptom tracker
- Hindi versions
- Aggregate dashboard for the clinic to track patterns over time
- Reminder system to re-screen at 24 months for low-risk under-2s

Design the `/assessments` landing page so that adding a new card later requires only a single HTML block, not a redesign.

---

**End of build specification.**
