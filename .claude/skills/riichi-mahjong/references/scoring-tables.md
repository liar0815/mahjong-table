# Fu × Han Point Tables (verified)

All base-point and payment figures below are derived from the standard
formula, not memorized, so they're safe to trust:

```
base = fu × 2^(han + 2)
```

If `base >= 2000`, the hand is scored as a flat **mangan** instead (base is
capped at 2000) — see the fixed table further down.

Payments are `base × multiplier`, then **rounded up** to the next 100
(`切り上げ`, always round up, never down or nearest):

| Situation | Payer(s) | Multiplier |
|---|---|---|
| Ron, non-dealer wins | loser (discarder) | ×4 |
| Ron, dealer wins | loser (discarder) | ×6 |
| Tsumo, non-dealer wins | dealer | ×2 |
| Tsumo, non-dealer wins | each non-dealer | ×1 |
| Tsumo, dealer wins | each of the 3 non-dealers | ×2 |

## 1–4 han table (fu 20–110)

`ron(non-dealer) / ron(dealer)` and `tsumo non-dealer-win (each/dealer) / tsumo dealer-win (each, ×3)`.
Entries reaching `base ≥ 2000` are mangan regardless of the fu/han shown
(this is the standard cutoff, distinct from the optional kiriage-mangan
house rule below).

| fu | 1 han | 2 han | 3 han | 4 han |
|---|---|---|---|---|
| 20 | 700 / 1000 · tsumo 400-200/1200 | 1300 / 2000 · tsumo 700-400/2100 | 2600 / 3900 · tsumo 1300-700/3900 | 5200 / 7700 · tsumo 2600-1300/7800 |
| 25 (chiitoi) | 800 / 1200 · tsumo 400-200/1200 | 1600 / 2400 · tsumo 800-400/2400 | 3200 / 4800 · tsumo 1600-800/4800 | 6400 / 9600 · tsumo 3200-1600/9600 |
| 30 | 1000 / 1500 · tsumo 500-300/1500 | 2000 / 2900 · tsumo 1000-500/3000 | 3900 / 5800 · tsumo 2000-1000/6000 | 7700 / 11600 · tsumo 3900-2000/11700 |
| 40 | 1300 / 2000 · tsumo 700-400/2100 | 2600 / 3900 · tsumo 1300-700/3900 | 5200 / 7700 · tsumo 2600-1300/7800 | **MANGAN** |
| 50 | 1600 / 2400 · tsumo 800-400/2400 | 3200 / 4800 · tsumo 1600-800/4800 | 6400 / 9600 · tsumo 3200-1600/9600 | **MANGAN** |
| 60 | 2000 / 2900 · tsumo 1000-500/3000 | 3900 / 5800 · tsumo 2000-1000/6000 | 7700 / 11600 · tsumo 3900-2000/11700 | **MANGAN** |
| 70 | 2300 / 3400 · tsumo 1200-600/3600 | 4500 / 6800 · tsumo 2300-1200/6900 | **MANGAN** | **MANGAN** |
| 80 | 2600 / 3900 · tsumo 1300-700/3900 | 5200 / 7700 · tsumo 2600-1300/7800 | **MANGAN** | **MANGAN** |
| 90 | 2900 / 4400 · tsumo 1500-800/4500 | 5800 / 8700 · tsumo 2900-1500/8700 | **MANGAN** | **MANGAN** |
| 100 | 3200 / 4800 · tsumo 1600-800/4800 | 6400 / 9600 · tsumo 3200-1600/9600 | **MANGAN** | **MANGAN** |
| 110 | 3600 / 5300 · tsumo 1800-900/5400 | 7100 / 10600 · tsumo 3600-1800/10800 | **MANGAN** | **MANGAN** |

Notes:
- 20fu ron practically never occurs (pinfu ron is scored at 30fu; 20fu ron
  would need a yaku with 0 fu-value ron wait, which doesn't happen in
  practice) — the row is included for completeness/tsumo only.
- 25fu is fixed chiitoitsu (七対子) fu; there's no fu variation for it.
- `tsumo X-Y/Z` reads as "each non-dealer pays X, dealer pays Y, total Z"
  for a non-dealer tsumo win; dealer-win tsumo total is `each × 3`.

## Mangan and above (fixed, independent of fu)

| Grade | Han | Ron (non-dealer) | Ron (dealer) | Tsumo (non-dealer win) | Tsumo (dealer win) |
|---|---|---|---|---|---|
| 満貫 Mangan | 5 (or capped 3-4han/high-fu) | 8000 | 12000 | 2000/4000 (each/dealer, total 8000) | 4000 all (total 12000) |
| 跳満 Haneman | 6–7 | 12000 | 18000 | 3000/6000 (total 12000) | 6000 all (total 18000) |
| 倍満 Baiman | 8–10 | 16000 | 24000 | 4000/8000 (total 16000) | 8000 all (total 24000) |
| 三倍満 Sanbaiman | 11–12 | 24000 | 36000 | 6000/12000 (total 24000) | 12000 all (total 36000) |
| 役満 Yakuman | 13+ / yakuman hand | 32000 | 48000 | 8000/16000 (total 32000) | 16000 all (total 48000) |

Double yakuman (rule-dependent — confirm before implementing) doubles all
the yakuman figures above (64000/96000/etc.).

**Kiriage mangan (切り上げ満貫, optional house rule)**: 4han30fu (base 1920)
and 3han60fu (base 1920) are rounded up to a full mangan (8000/12000)
instead of their computed 7700/11600 values. Off by default in most rule
sets — ask before enabling.

## Honba (本場) and riichi sticks (供託)

Not currently tracked by this app. If added:

- **Honba**: +300 points to the winner per honba, paid by the loser on ron;
  on tsumo, +100 per honba from *each* player. Honba count increments on
  dealer repeat (renchan) or exhaustive draw, resets on a non-dealer win.
- **Riichi sticks**: each riichi declaration puts 1000 points of the
  declarer's own score into a table pot (供託), deducted immediately from
  their score. The pot is swept entirely by whoever wins the hand the
  sticks are cleared on (in addition to normal scoring); uncollected sticks
  carry over to the next hand.

## Ryuukyoku (流局, exhaustive draw) tenpai payments

Not currently implemented. Standard rule: tenpai (ready) players split
3000 points total from noten (not-ready) players.

| Tenpai players | Each tenpai player receives | Each noten player pays |
|---|---|---|
| 1 | +3000 | −1000 |
| 2 | +1500 | −1500 |
| 3 | +1000 | −3000 |
| 0 or 4 | no exchange | no exchange |
