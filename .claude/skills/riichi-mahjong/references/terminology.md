# Terminology Glossary (日本語 ↔ English)

For keeping UI copy, commit messages, and variable naming consistent with
the Japanese-first style already used across this codebase (see
`CLAUDE.md`'s scoring section and inline comments in `app.js`).

## Scoring terms used in this project

| Japanese | Reading | English | Where used |
|---|---|---|---|
| 素点 | そてん souten | raw table score | seat score entered by the user |
| 持ち点 | もちてん mochiten | current chip count | synonym for 素点 mid-game |
| 配給原点 / 初期点 | はいきゅうげんてん / しょきてん | starting points | `settings.kichi` |
| 返し点 | かえしてん kaeshiten | break-even point | `kichi + oka/4`, in `tableResult` |
| オカ | oka | top-place bonus pool | `settings.oka`, added to rank 0 only |
| ウマ | uma | rank-based fixed bonus/penalty | `settings.uma`, `UMA_TABLE` |
| 五捨六入 | ごしゃろくにゅう gosha-rokunyuu | round to nearest 1000 (≤500 down, ≥600 up) | `gosha()` |
| 着順 | ちゃくじゅん chakujun | finishing rank (1st–4th) | `rank[]` |
| 折半 | せっぱん seppan | splitting a tie evenly | `tieScore === "split"` |
| 上家 | かみちゃ kamicha | player to the left (seat order priority) | `tieScore === "kamicha"` |
| 飛び | とび tobi | "busting" — going below 0 and ending the game early | not implemented |
| 半荘 | はんちゃん hanchan | a full match, East + South rounds | tournament format context |
| 東風戦 | とんぷうせん tonpuusen | East-only match (shorter format) | tournament format context |

## General riichi mahjong terms

| Japanese | Reading | English |
|---|---|---|
| 立直 | リーチ riichi | declare ready + closed hand, betting 1000 pts |
| 自摸 | つも tsumo | winning by self-draw |
| 栄和 / ロン | ロン ron | winning off another player's discard |
| 副露 / 鳴き | ふろ / なき furo/naki | calling (pon/chi/kan), opens the hand |
| 門前 | めんぜん menzen | closed hand (no calls made) |
| 役 | やく yaku | a scoring hand pattern/condition |
| 飜 / 翻 | はん han | han — the yaku "point" unit, doubles the base score each step |
| 符 | ふ fu | fu — the base-point unit from wait/wait-shape/triplet bonuses |
| 流局 | りゅうきょく ryuukyoku | exhaustive draw (wall runs out, no winner) |
| 聴牌 / 不聴 | テンパイ / ノーテン tenpai/noten | ready-to-win / not ready, at a draw |
| 本場 | ほんば honba | repeat-hand counter, adds bonus points |
| 供託 | きょうたく kyoutaku | pot of riichi sticks on the table |
| 親 | おや oya | dealer |
| 子 | こ ko | non-dealer |
| 連荘 | れんちゃん renchan | dealer keeps their seat after winning or the hand is a draw |
| 東南西北 | とん・なん・しゃー・ぺー | East/South/West/North (seat winds) |
| 断幺九 | タンヤオ tanyao | all-simples yaku |
| 平和 | ピンフ pinfu | all-sequences yaku, no fu bonus |
| 対々和 | トイトイ toitoi | all-triplets yaku |
| 役満 | やくまん yakuman | the highest scoring tier, flat max payout |
