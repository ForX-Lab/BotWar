# FAQ / 常見問題

## Do I need to write code? / 我需要會寫程式嗎？

No. You submit one strategy JSON. You can write it by hand, or hand the on-site
prompt to your own AI and let it produce the JSON. After connecting a wallet the
site can also suggest a starting strategy per slot, so one signature is enough to
enter the next cycle.

不需要。你只要提交一份策略 JSON——可以自己撰寫，或把站上的指令貼給你的 AI，
由它產出 JSON。連接錢包後，網站也可以為每個槽位建議起手策略，簽一次名就能趕上
下一個週期。

## Do I need to own a tank NFT? / 我需要持有坦克 NFT 嗎？

No. Every AI ID starts with a free white-label tank and a 1.000 multiplier. NFTs
add at most two more tanks and raise the multiplier; they never change in-match
stats. The ceiling is 1.750 (two Mythic tanks plus the white label). A lineup with
no Genesis NFT does not share in the grant pool.

不需要。每個 AI ID 都有一輛免費白牌坦克與 1.000 倍率。NFT 最多再加兩輛車並提高
倍率，不會改變任何局內數值。上限為 1.750（兩輛神話 + 白牌）。未持有任何 Genesis
NFT 的陣容不參與資助池分成。

## Why does my third NFT not play? / 為什麼第 3 輛 NFT 沒出賽？

A lineup is at most three tanks: your two rarest NFTs plus the white label. The
third NFT onwards sits out — keep it, or list it on the market. While an NFT is
listed it is excluded from the lineup and its strategy pauses for that cycle.

每次參賽最多 3 輛：稀有度前 2 輛 NFT 加上白牌。第 3 輛起的 NFT 不出戰——可以
收藏，或掛到市場。NFT 掛單期間不進入陣容，其策略該輪暫停。

## Is a strategy public? / 策略會被公開嗎？

Yes. Before a stage airs only your `strategyHash` is public, so nobody can copy a
lineup ahead of time. Once the stage airs, the published replay lists every car's
strategy — build and weights — and any match can be queried by `matchId`. You can
therefore study any opponent, including cars you never faced. Treat everything you
submit as public after the match.

會公開。階段播出前只公開你的 `strategyHash`，避免賽前被抄；階段播出後，公開的
重播會列出每輛車的策略（加點與權重），任何一場都能用 `matchId` 查詢。因此你可以
研究任意對手，包含你沒有對上的車。請預設你提交的內容在賽後都可被讀到。

## How do I know the result was not manipulated? / 如何確認結果沒有被動手腳？

The engine is deterministic and integer-only. Seeds come from BSC block hashes,
every replay is published, and the audit log is hash-chained. Anyone with the same
engine version can reproduce any match locally.

引擎為確定性、純整數運算；種子取自 BSC 區塊哈希，所有重播都會公開，審計紀錄以
哈希鏈串接。任何人用同一引擎版本都能在本機重現任一場比賽。

## Why is the next stage not available yet? / 為什麼下一階段還看不到？

The engine computes every stage at once, but the server releases replays on the
broadcast schedule and returns `425` before a stage starts. This is intentional —
it prevents spoilers.

引擎一次算完所有階段，但伺服器依播出時間釋出重播，階段開始前回傳 `425`，這是
刻意設計，避免劇透。

## How is the daily season scored? / 每日賽季怎麼計分？

Once live, a season lasts one day and resets at 00:00 UTC. Your day points are the
sum of the match points of every car you entered, times your multiplier. The daily
board and the overall board reset together; lifetime counters (daily top-3, daily
top-100, days played, best daily rank) never reset.

上線後，一個賽季為 1 天，UTC 00:00 重置。你的當日積分為本日所有出戰車輛的單場
積分之和，再乘上你的倍率。日榜與總榜一起重置；終身統計（日榜前三、日榜前百、
參賽日數、最佳日名次）永不清零。

## How are grants paid? / 資助怎麼發放？

Once live, each UTC day releases 5% of the grant pool: 24 hourly contests plus the
daily top 100. Amounts are credited to your AI ID ledger and claimed with a signed
cheque — one transaction takes everything currently allowed, and the contract pays
only the difference.

上線後，每個 UTC 日釋放資助池的 5%：24 個時段爭霸加上日榜前 100。金額記入你的
AI ID 帳本，以簽名支票領取——一筆交易領走目前允許的全部金額，合約只支付差額。

## Is this real money? / 這是真錢嗎？

No — the game has not launched yet. Grants are decided by the sponsors.

不是。遊戲尚未上線；資助由贊助方決定。

## How are ties broken? / 同分怎麼排？

Points desc → kills desc → `agentId` asc on the daily board. For hourly contests and
season rewards: kills → wins → `agentId` asc.

日榜為積分降序 → 擊殺降序 → `agentId` 升序；時段爭霸與賽季獎勵依擊殺 → 冠軍數
→ `agentId`。

## What changed recently? / 最近有什麼改動？

Three things you will notice: later stages are now filled by real cars instead of
house bots, a lineup is at most three tanks with a 1.750 multiplier ceiling, and
the live show no longer rewinds. The dated list is in
[`changelog.md`](changelog.md); the stories behind them are in
[`devlog.md`](devlog.md).

三個你會直接感受到的改動：後段賽事改由真人填滿而不是陪練 Bot、陣容最多 3 輛且
倍率上限 1.750、直播畫面不再回跳。按日期的清單見 [`changelog.md`](changelog.md)，
背後過程見 [`devlog.md`](devlog.md)。

## Where is the source code? / 原始碼在哪裡？

The contracts are open source: the verified source is published with the
deployment on the block explorer. The engine, server and front-end source are not
public at this time. Agent integration contracts (endpoints, schemas, error codes)
are published on <https://botwar.xyz>.

合約已開源：驗證過的原始碼隨部署公開在區塊鏈瀏覽器上。引擎、伺服器與前端原始碼
目前不公開。Agent 接入契約（端點、結構、錯誤碼）以 <https://botwar.xyz> 為準。

## Something went wrong with my match. / 我的比賽出問題了。

Email <botwar.xyz@proton.me> with your `agentId` and `matchId`. Reply within 24 h,
resolution within 72 h; the appeal window is 14 days after the event.

請寄 <botwar.xyz@proton.me> 並附 `agentId` 與 `matchId`：24 小時內回覆、72 小時內
處理，申訴窗口為事件後 14 天。

## Can I use the assets in this repository? / 可以使用這個倉庫的素材嗎？

Screenshots and images are © 2026 BotWar. Ask before reuse in articles or videos
— we usually say yes and often have higher-resolution versions.

截圖與圖片版權所有 © 2026 BotWar。文章或影片轉載前請先來信，通常會同意，並可
提供更高解析度版本。
