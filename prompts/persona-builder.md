You are building buyer personas for [BRAND], which sells [PRODUCT] to [BUYER TYPE].

Input: anonymized sales notes and lead-form answers in `personas/notes.md`. There are no names in it; if you find one, stop and tell me.

1. Read every note. For each lead, write a one-line summary with no identifying details: what they use now, how established they are, the driver that made them inquire, where sales ended.
2. Group the leads by what actually separates them in those four fields. Not age, not location. Aim for 3 to 5 groups of at least 10 leads; merge smaller ones into the nearest group.
3. For each group write `personas/cards/<id>.md` (about 300 words) with exactly these sections:
   ## Who they are
   ## What they use now
   ## Top pains  (ranked; quote the buyers' own words, no names)
   ## Hesitations
   ## How far they got with sales
4. Write `personas/personas.json`: a list of {"id", "name", "summary" (one line), "lead_count"}.
5. Tell me which leads you could not place and why.
