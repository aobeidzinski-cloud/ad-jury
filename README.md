# ad-jury

A Claude Code skill that ranks a batch of ads before they spend, using buyer personas built from your own anonymized sales notes and randomized head-to-head heats.

## What it does
1. Builds 3 to 5 buyer personas from your sales notes (no personal data in, none out).
2. Describes each ad from what is actually on the image.
3. Runs 50 rounds of randomized 4-ad heats; every persona picks one ad per heat.
4. Writes a scorecard: ads ranked by win rate, per persona, with the reason they won or lost.

## Install
```bash
git clone <this repo> ad-jury
cp -r ad-jury ~/.claude/skills/ad-jury
```
Open Claude Code in your project and say: "run the jury on this batch." The skill reads `SKILL.md` and asks for what it needs.

## Inputs
- `personas/notes.md`: your sales notes, call summaries, or lead-form answers with names, emails, phones, and company names removed. See `examples/notes.md`.
- `batches/<batch>/ads/`: the ad images. Optional `captions.json`.

## Example
`examples/` has invented notes, four personas, six described ads, and the scorecard they produce. Read `examples/scorecard-sample.md` first.

## Optional: Jev as the jury
If you use Jev (a judgment model), send each heat as one evaluation with one choice question per persona and read the probabilities as win rates. Without it, Claude plays the jury from `prompts/heat.md`. Both produce the same files.

## What the backtest showed
In one agency's backtest on 58 image ads inside lead campaigns (Jan to Aug 2026), the top third by jury win rate produced 3.9x the qualified leads per $1,000 of the bottom third and beat 99% of 2,000 random shufflings. It added about 14% over simply betting on each ad's pain point. It ranks ads against each other; it does not predict an absolute lead rate. Run your own backtest before trusting it with budget. Details in `SKILL.md`.

## License
MIT.
