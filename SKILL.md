---
name: ad-jury
description: "Score a batch of ad creatives before they spend. Builds 3 to 5 buyer personas from your own anonymized sales notes, describes each ad from what is actually on the image, then runs randomized head-to-head heats where every persona picks the ad it would act on. Outputs a ranked scorecard. Triggers on: score these ads, which ad will win, run the jury on this batch, ad jury."
---

# ad-jury

Rank a batch of ads by how likely your real buyers are to stop, click, and act, before any money goes behind them. Works with Claude alone. An optional step uses Jev (a judgment model) for the picks; without it, Claude plays the jury.

Three stages. Each writes files you can read, so nothing is a black box.

## Stage 1: Personas (once, then quarterly)

Input: `personas/notes.md`, your own sales notes, call summaries, or lead-form answers, already stripped of names, emails, phone numbers, and company names. Aim for 40+ leads if you have them; 15 works.

1. Run `prompts/persona-builder.md` on the notes. It groups buyers by what actually separates them (what they use now, how established they are, the driver that made them inquire), not by demographics.
2. It writes one card per persona to `personas/cards/<id>.md` with exactly these sections: `## Who they are`, `## What they use now`, `## Top pains`, `## Hesitations`, `## How far they got with sales`. About 300 words each. 3 to 5 personas; merge anything under about 10 leads.
3. It writes `personas/personas.json`: `[{"id","name","summary","lead_count"}]`. Weight = lead_count / total.
4. Read the cards. If one describes nobody you've ever sold to, delete it and rerun.

## Stage 2: Describe the ads (per batch)

Input: `batches/<batch>/ads/*.png` (or .jpg) plus `batches/<batch>/captions.json` (`{"<file>": "first line of the caption"}`, optional).

For each image, run `prompts/describe-ad.md` with the image attached. It writes `batches/<batch>/descriptions/<file>.json`:

```json
{"id": "<file>", "hook_on_image": "exact words on the image, or (none)",
 "caption_first_line": "...", "scene": "<= 40 words: setting, who, doing what",
 "story_object": "phone | receipt | whiteboard | (none)", "format": "static | video | carousel"}
```

Describe only what is visible. No judging, no guessing results. Batches of 15 per run keep it accurate.

## Stage 3: The jury (per batch)

1. Make heats: shuffle the batch, cut into groups of 4, repeat for 50 rounds with a fixed seed (pad a short last heat with random others). 20 ads = 250 heats.
2. For each heat, run `prompts/heat.md` with the 4 descriptions labeled A to D and the persona cards (strip the `How far they got with sales` section so the jury cannot see past outcomes). One pick per persona per heat. With Jev, send the same state as one evaluation with one choice question per persona; with Claude alone, ask for a JSON of picks with a one-line reason each.
3. Score: per persona, an ad's win rate is the share of its heats it was picked in (25% is average). Overall = persona win rates weighted by persona share.
4. Write `batches/<batch>/scorecard.md`: ads ranked by overall win rate, each with its per-persona rates, the hook on image, and the single most common reason it won or lost. Flag any ad whose rank differs by more than a third between the two halves of the rounds (odd vs even) as unstable.

## Reading the scorecard

- It ranks ads against each other. It does not predict an absolute lead rate.
- Trust the top and bottom thirds more than the middle.
- An ad that wins with one persona and loses with the others is a targeting decision, not a bad ad.
- Rerun after launch: compare the ranking to cost per qualified lead at 30 days. That is the only test that matters.

## Validation, so you know what you're trusting

In one agency's backtest (58 image ads inside lead campaigns, Jan to Aug 2026, judged by four personas built from about 110 qualified leads and 140 anonymized note summaries), the top third by jury win rate produced 3.9x the qualified leads per $1,000 of the bottom third, beat 99% of 2,000 random shufflings, and ranked almost identically on odd and even rounds (Spearman 0.97). It added about 14% over simply betting on the ad's pain point. Scope: image ads in lead-objective campaigns, ranked relative to each other. Nothing is claimed for traffic campaigns or absolute rates. Run your own backtest before you trust it on your money.

## Rules
- Never put personal data (names, emails, phones, company names) into any file this skill reads or writes.
- One pre-registered backtest. If it fails, report it failed; do not retune on the same data.
