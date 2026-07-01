---
name: riichi-mahjong
description: Reference knowledge base for Japanese riichi mahjong (リーチ麻雀) scoring rules and terminology, written specifically for this tournament-manager project. Use this whenever touching scoring math in app.js (tableResult, gosha, kaeshi/uma/oka/settings), whenever adding or debugging scoring-related features (honba, riichi sticks, tobi/negative-score handling, ryuukyoku noten payments, a han/fu-to-score calculator, yaku validation), or whenever the user asks anything about riichi mahjong rules, terminology, uma/oka variants, han/fu, or point tables — even in Japanese or without using the English words. Always check this before changing the scoring formulas.
---

# Riichi Mahjong Scoring & Rules Reference

Domain knowledge for Japanese competitive riichi mahjong (4-player, 東南戦/半荘 by
default), written to support this project's scoring code and to answer rules
questions accurately without hallucinating numbers.

## What this project actually implements today

This app does **not** calculate han/fu/yaku. Users type in each seat's final
raw table score (素点) by hand; the app converts that into tournament points.
Everything lives in `app.js`:

- `settings` (`app.js:65`) — `{kichi, oka, uma, tie, tieScore}`, persisted to
  localStorage/Firestore.
- `tableResult(r,t,tbl)` (`app.js:210`) — per-table conversion from raw score
  → tournament points for one round.
- `gosha(n)` (`app.js:697`) — 五捨六入 rounding to the nearest 1000
  (symmetric: `≤500` truncates, `≥600` rounds away from zero).
- Overall standings tie-breaking (`tie`: `same`/`top`/`avg`) lives around
  `app.js:520` and `app.js:586`.

Formulas actually used, in order:

1. **返し点 (kaeshi)** = `kichi + oka/4` — the raw-score break-even point.
   E.g. `25000 + 20000/4 = 30000`.
2. **素点差** = raw score − kaeshi.
3. **五捨六入** — round the 素点差 to the nearest 1000 via `gosha()`.
4. **+ ウマ** — add `UMA_TABLE[settings.uma][rank]` (in 1000-pt units;
   options are `5-10`, `10-20`, `10-30`, `20-30`, each `[1着,2着,3着,4着]`,
   e.g. `10-20` → `[20,10,-10,-20]`, `app.js:52`).
5. **+ オカ (1st place only)** — `oka/1000` added only to rank 0.
6. **Zero-sum correction** — after rounding, absorb any ±1pt table-wide
   rounding drift into the least-constrained seat (`app.js:258`) so every
   table's four scores always sum to exactly 0.

Ties within one table (`tieScore` setting):
- `split` (default) — tied seats share the average of the ranks they occupy
  (e.g. two people tied for 1st/2nd both get `(uma[0]+uma[1])/2 + oka`).
- `kamicha` — ties are broken by seat order (lower index = closer to dealer
  wins the tie), each gets the exact rank's uma/oka.

4th-seat auto-fill: if exactly 3 of 4 seats have a value, the 4th is derived
as `kichi*4 − sum(other three)` since all four raw scores must sum to the
table's starting total (`app.js:219-225`).

**Not implemented** (flag these as new features, not bugs, if asked):
honba (本場) counters, riichi-stick (供託) carryover, tobi (飛び／即終了),
ryuukyoku (流局) tenpai/noten payments, and any han/fu/yaku detection.

## When extending scoring features

If the user asks to add any of the "not implemented" items above, or a
han/fu point calculator, read the relevant reference file first — don't
derive point tables from memory, they're easy to get subtly wrong (the
100-point rounding direction, mangan cutoffs, and dealer/non-dealer
multipliers all have sharp edges):

- `references/scoring-tables.md` — verified fu×han point tables, mangan+
  fixed tables, honba/riichi-stick adjustments, ryuukyoku noten payments.
- `references/yaku.md` — standard yaku list with han values (closed/open),
  for building a yaku picker or validator.
- `references/terminology.md` — Japanese↔English glossary for UI copy,
  commit messages, and variable naming consistency with the existing
  Japanese-first codebase style.

## Rule-variant traps to ask about, don't assume

Riichi mahjong has many table-rule variants that change the numbers. Before
implementing anything number-sensitive, confirm which variant the user
wants — do not silently pick one:

- **Starting points**: 25000 is this project's default (`settings.kichi`),
  but 30000-start or 20000-start rules exist.
- **Uma**: this project already supports 4 presets (`5-10`/`10-20`/`10-30`/
  `20-30`); don't assume a "standard" uma beyond what's in `UMA_TABLE`.
- **Kiriage mangan (切り上げ満貫)**: whether 3han70fu+/4han30fu+ (base 1920,
  just under the 2000 mangan cutoff) round up to a full mangan. Optional
  rule, off by default in most sets.
- **Kuitan (喰いタン)**: whether an open-hand tanyao is allowed as a yaku.
  Doesn't affect this app's scoring math but matters for any yaku feature.
- **Double yakuman**: whether certain yakuman (e.g. 国士無双十三面待ち,
  suuankou tanki) score double. Ruleset-dependent.
- **Tobi (飛び)**: whether a negative running total ends the game
  immediately. Not implemented here — flag as new behavior, not a bug.
