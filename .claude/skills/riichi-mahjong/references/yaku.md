# Standard Yaku (役) List

Han values below follow the common Japanese ruleset (used by most
tournament/online clients, e.g. Tenhou/MahjongSoul default rules). Some
clubs vary a han or disallow specific yaku (noted inline) — confirm the
house rule before hard-coding any of these into a validator.

## 1 han

| Yaku | Closed only? | Notes |
|---|---|---|
| 立直 Riichi | Yes | Declared on a closed tenpai hand, costs 1000 pts (see riichi sticks) |
| 門前清自摸和 Menzen tsumo | Yes | Closed hand, won by self-draw |
| 平和 Pinfu | Yes | All sequences, non-yakuhai pair, two-sided wait, fixed 30fu (20fu on tsumo) |
| 一発 Ippatsu | Yes | Win within one go-around of declaring riichi, no calls in between |
| 断幺九 Tanyao | No | All simples (no terminals/honors); "kuitan" variant allows this open |
| 役牌 Yakuhai | No | 1 han per triplet of: seat wind, round wind, or dragon (haku/hatsu/chun) |
| 一盃口 Iipeiko | Yes | Two identical sequences |
| 海底摸月 Haitei | No | Tsumo on the last drawable tile |
| 河底撈魚 Houtei | No | Ron on the last discard |
| 槍槓 Chankan | No | Ron on a tile used for another player's kan (robbing a kan) |
| 嶺上開花 Rinshan kaihou | No | Tsumo on a replacement tile after a kan |

## 2 han

| Yaku | Closed only? | Notes |
|---|---|---|
| 七対子 Chiitoitsu | Yes | Seven pairs, fixed 25 fu |
| 三色同順 Sanshoku doujun | No (1 han if open) | Same sequence in all 3 suits; open hand = 1 han |
| 一気通貫 Ittsuu | No (1 han if open) | 1-9 straight in one suit; open hand = 1 han |
| 対々和 Toitoi | No | All triplets |
| 三暗刻 Sanankou | No | Three concealed triplets |
| 三色同刻 Sanshoku doukou | No | Same triplet in all 3 suits |
| 混老頭 Honroutou | No | All terminals and honors |
| 小三元 Shousangen | No | Two dragon triplets + dragon pair |
| 双立直 Double riichi | Yes | Riichi declared on the very first discard |
| 混全帯幺九 Chanta | No (1 han if open) | Every set contains a terminal or honor; open = 1 han |

## 3 han

| Yaku | Closed only? | Notes |
|---|---|---|
| 混一色 Honitsu | No (2 han if open) | One suit + honors; open hand = 2 han |
| 純全帯幺九 Junchan | No (2 han if open) | Every set contains a terminal (no honors); open = 2 han |

## 6 han

| Yaku | Closed only? | Notes |
|---|---|---|
| 清一色 Chinitsu | No (5 han if open) | One suit only, no honors; open hand = 5 han |

## Yakuman (役満, 13 han equivalent — flat mangan×8 payment)

| Yaku | Notes |
|---|---|
| 国士無双 Kokushi musou | 13 distinct terminals/honors + one pair among them; 13-sided wait variant sometimes scores double yakuman |
| 四暗刻 Suuankou | Four concealed triplets; tanki (pair) wait variant sometimes scores double yakuman |
| 大三元 Daisangen | All three dragon triplets |
| 小四喜 Shousuushi | Three wind triplets + wind pair |
| 大四喜 Daisuushi | Four wind triplets (some rules score this as double yakuman) |
| 字一色 Tsuuiisou | All honor tiles |
| 緑一色 Ryuuiisou | All green tiles (2/3/4/6/8 sou + green dragon) |
| 清老頭 Chinroutou | All terminals (no honors, no simples) |
| 九蓮宝燈 Chuuren poutou | Pure single-suit 1112345678999 shape + one more of that suit; the exact 9-sided wait variant sometimes scores double |
| 天和 Tenhou | Dealer wins with the initial hand, before any discard |
| 地和 Chiihou | Non-dealer tsumo before their first discard, with no calls in between |
| 四槓子 Suukantsu | Four kans by one player |

Whether the "double yakuman" variants above pay double is a house-rule
choice — always confirm rather than assuming.

## Dora (ドラ, not a yaku)

Dora tiles add +1 han each but cannot by themselves make a hand valid —
a hand needs at least one yaku from the tables above before dora can be
added. Types: 表ドラ (indicator-based, revealed at start), 裏ドラ
(ura-dora, riichi-only, revealed at reveal), 赤ドラ (aka dora, specific
red 5-tiles worth +1 han each on top of their normal value). Not
implemented in this app.
