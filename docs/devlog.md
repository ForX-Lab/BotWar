# Development log / 開發日誌

Short notes on what we changed and why, written for players rather than
engineers. Numbers that affect play live in [`rules.md`](rules.md); dated changes
live in [`changelog.md`](changelog.md). Nothing here explains how the engine makes
its decisions.

給參賽者看的簡短紀錄：我們改了什麼、為什麼改。影響戰局的數值在
[`rules.md`](rules.md)，按日期的變更在 [`changelog.md`](changelog.md)。這裡不會
說明引擎如何做決策。

---

## 2026-11-03 — Rehearsing mainnet before mainnet / 在主網之前先演練主網

A contract that pays real money deserves a rehearsal that touches nothing real.
We generated a test variant of the production contract — same logic, rehearsal
settings — ran the whole production test suite against it, then walked the
deploy → claim → payout path on a fork and measured what it costs. A second
review round before launch turned up a handful of issues: every confirmed one is
fixed with a test, and the rest become configuration and checklists rather than
a rushed code change. No rule changed in any of this.

會支付真錢的合約，值得一場不動真格的演練。我們從生產合約生成了一份測試變體——
邏輯相同、參數為演練而設——讓整套生產測試在它身上跑過，再把部署 → 領取 → 出款
路徑在分叉上完整走一遍並實測費用。上線前的第二輪複審找出了幾個問題：成立的都
已修好並有測試；其餘的化為配置與清單，而不是趕工改代碼。這一切都不改動任何規則。

## 2026-11-03 — Joining and resting without babysitting / 不用人盯的參戰與休息

Holding a tank NFT used to mean babysitting: transfer it, then go register, then
write a strategy before the cycle. Now the wallet itself is the signal — when an
NFT arrives at a wallet with an AI ID, the site registers the agent and assigns
a random starting strategy on its own, and claiming grants counts as you being
back. The
mirror side is rest: an account with no real activity for days steps offline at
the reset, so long-idle slots stop lingering. And when an automatic assignment fails, the slots
page says so instead of pretending everything is fine.

以前持有 NFT 需要人盯著：轉入、註冊、趕在週期前寫策略。現在錢包本身就是訊號——
NFT 轉入已持有 AI ID 的錢包，網站就會自動登記註冊並隨機指派一份起手策略；領取
資助即代表你回來了。另一面是休息：長時間沒有真實活動的遊戲帳號會在重置時自動
下線，閒置槽位不再一直掛著。隨機策略指派失敗時，槽位頁會明說，而不是假裝一切正常。

## 2026-11-02 — Why the endgame stalled / 尾盤為什麼會僵住

With only a few tanks left inside a small ring, waiting was about as good as
moving, so the last turns could drift into a standoff and the poison decided who
won. We changed the AI, not the rules: in the last ring tanks engage, and a tank
that has been quiet for a few turns becomes the preferred target. Matches are now
much more likely to end on a kill, and scoring, schedules and replays are the same
as before.

場上只剩幾輛、圈又很小的時候，「等」和「動」幾乎一樣划算，於是最後幾回合可能僵住，
最後由毒霧決定勝負。我們改的是 AI，不是規則：進入最後的圈就主動接戰，而連續幾回合
沒開火的車會成為優先目標。現在比賽更常以擊殺收尾；積分、賽程與重播格式都與之前
相同。

## 2026-11-02 — Fairness you can check yourself / 自己就能核對的公平性

A hash-chained log is only worth something if you can recompute it. Every cycle now
publishes a signed commitment before the seed block is mined, so the seed cannot be
picked after the fact; an optional hourly checkpoint puts that hour's roots on
chain for anyone to compare. The *Verify fairness* button on the site does the
check in your browser instead of asking you to trust a page.

哈希鏈紀錄只有在你能自己重算時才有意義。現在每一輪都會在種子區塊產生前公開簽名
承諾，種子因此不可能被事後挑選；可選的每小時檢查點會把該小時的根寫上鏈，供任何人
比對。網站上的「驗證公平性」按鈕會在你的瀏覽器裡完成核對，而不是請你相信一個頁面。

## 2026-11-02 — Five AIs read the contract / 五個 AI 讀了一遍合約

Before the next deployment we asked five models — Gemini 3.8 Flash, Kimi K3.1 Pro,
Gemini 4 Argon, GLM 5.3 and Opus 5.5 — to review the contract source
independently. Every finding went back to the code: reproduce it, fix what is
real, and cover it with a test. It is slower than trusting one summary, and that
is the point. This is not a formal third-party audit or a bug bounty.

下一輪部署前，我們請五個模型——Gemini 3.8 Flash、Kimi K3.1 Pro、Gemini 4 Argon、
GLM 5.3、Opus 5.5——各自獨立審查合約原始碼。每一條意見都回到程式碼：重現、修掉
成立的、補上測試。這比相信一份摘要慢，而這正是重點。它不是正式第三方審計，也不是
漏洞賞金。

## 2026-11-02 — A settlement screen that keeps up / 跟得上比賽的結算畫面

Matches can end well before the turn cap, so the settlement has to work in whatever
time is left. We rebuilt it around three questions a player asks: who won
(champion first), did I go through (advanced and eliminated listed apart, your own
row called out), and how much did I score. Nothing about the result changed — the
screen reads the same replay the engine produced.

比賽可能在到達回合上限前就結束，結算只能用剩下的時間把事情說完。我們照玩家會問的
三件事重做：誰贏了（冠軍置頂）、我晉級了嗎（晉級與淘汰分開列，標出你自己）、我拿
多少分。結果本身沒有改變——畫面讀的就是同一份重播。

## 2026-11-01 — Finishing the fight / 把比賽打完

Too many matches used to reach the turn cap with two or more tanks alive, so the
poison and a ranking decided them instead of a fight. Shrinking the ring faster
would have left long-range guns nowhere to stand, so the zone timeline stayed; we
added a short sudden-death window over the last ten turns, raised the caps to 70
and 80, and let the last kill end the match. The champion is the actual last
survivor and always tops the match score.

以前太多比賽打到回合上限時還有兩輛以上存活，最後由毒霧與名次決定，而不是打出來
的。讓圈縮得更快會讓長炮無處可站，所以圈的時間軸不動；改成在最後 10 回合加一段
驟死、把上限提高到 70 與 80 回合，並讓最後一擊當下結束比賽。冠軍是全場真正的
倖存者，而且保證單場積分最高。

## 2026-11-01 — Filling the later stages with players / 讓後段賽事坐滿玩家

Our test rounds are small, and the old rule sent the top four of each group on.
With three groups that often left the next stage short of real cars, so house bots
took the free seats — you could finish fifth and be out while a bot moved on. We
changed the advancement step so the seat count grows with the number of cars
entered, and any seat a bot would fill is offered to the best real cars still
available. The minimum never goes down, so nobody who would have advanced before
loses a seat.

我們的測試輪規模不大，舊規則固定取每組前 4 名。三個小組的情況下，下一階段常常
湊不滿真人，空位就由陪練 Bot 補上——你拿第 5 名被淘汰，Bot 卻晉級了。我們改了
晉級這一步：名額隨報名車輛數增加，而任何原本會由 Bot 補上的座位，會優先給還沒
晉級的最佳真人車輛。下限不會降低，因此原本能晉級的人不會因此失去名額。

Two side effects are intended. Several cars of one agent can advance, capped by
the next stage's match count (at most 3), and cut cars are clearly marked as
benched so you know why a car did not appear.

兩個附帶效果是刻意設計的：同一 Agent 可以多車晉級，上限為下一階段場次數（最多
3 輛）；被擠掉的車會明確標記為替補，讓你知道它為什麼沒有出賽。

## 2026-11-01 — A clock that never runs backwards / 只進不退的時鐘

Watching the live show on a local network, the picture occasionally jumped back a
few turns and replayed them, worst during the acid-rain turns. The page builds the
live picture itself from the published replay plus an estimate of the server
clock; every refresh used to re-take that estimate, and a second-resolution
timestamp can point slightly backwards. A rewind looks like a seek, so the
renderer obeyed it.

在區域網路看直播時，畫面偶爾會倒退幾個回合再重播，酸雨階段最嚴重。頁面是根據
已公開的重播加上伺服器時鐘估計，自行拼出直播畫面；過去每次請求都會重新估算，
而秒級時間戳可能略微倒退。倒退在渲染器眼中等於「跳轉」，於是它就照做了。

The page now keeps a forward-only clock: small corrections are smoothed out over a
few seconds instead of jumping, stale or cached timestamps are ignored, and when a
tab wakes from sleep the stream catches up rather than replaying. No server or
engine change was needed, and this is the kind of bug we prefer to fix by removing
the possibility instead of adding a guard.

現在頁面使用只進不退的時鐘：小幅誤差在幾秒內平滑追平而不跳格，過期或快取的
時間戳會被忽略，分頁從睡眠恢復後直接追上直播而不是重播。伺服器與引擎都無需改動；
這類問題我們傾向「讓它不可能發生」，而不是加一層補丁。

## 2026-11-01 — Second balance pass / 第二輪平衡調整

The first balance pass looked at 49 different builds. It found one build that ran
away with the win rate and several matchups that were simply lost before the
first shot. The second pass gave gun classes a counter loop — brawler beats
sniper, sniper beats skirmisher, skirmisher beats brawler — and made long guns pay
extra fog damage outside the ring, so a sniper cannot simply park in the corner
and out-trade everyone.

第一輪平衡研究了 49 種 build，發現有一種勝率明顯失控，也有幾種對局在開槍前就
已經輸了。第二輪讓炮型形成循環克制——近戰克狙擊、狙擊克游擊、游擊克近戰——並讓
長炮在圈外多受毒霧傷害，狙擊手不能再躲在角落裡換贏所有人。

We also flattened the lineup and the multiplier in the same pass: at most three
tanks per entry (two NFTs plus the free white label) and a 1.750 ceiling. The
intent is that the second NFT is worth owning for its extra tank, not for a large
score bonus. Balance is never finished; we will keep publishing the study when we
re-run it.

同一輪也壓平了陣容與倍率：每次最多 3 輛（2 輛 NFT + 免費白牌），上限 1.750。
目的是讓第 2 輛 NFT 的價值來自「多一輛車」，而不是高額積分加成。平衡永遠不會
完成，之後重跑研究時我們會繼續公開。

## 2026-11-01 — One signature to compete / 簽一次名就能參賽

The most common piece of feedback was that entering took too many steps: mint,
open the agent page, build a strategy, sign, submit, repeat for every slot. Now,
after connecting or minting, the site proposes a starting strategy per slot and
your agent plays the next cycle after a single signature. You can still edit
everything by hand; the suggestion is a floor, not a ceiling.

最常見的回饋是參賽步驟太多：鑄造、打開 Agent 頁、編輯策略、簽名、提交，每個槽位
再來一次。現在連接或鑄造之後，網站會為每個槽位建議一份起手策略，只要簽一次名，
下一個週期就會自動出賽。你仍然可以全部手改；建議只是下限，不是上限。

In the same batch: Binance Wallet sign-in (QR on desktop, the App on mobile),
batch buy and batch cancel on the market, a visible market fee, and an account
deletion flow that removes identity data instead of hiding it.

同一批還包含：幣安錢包登入（電腦掃碼、手機 App）、市場批量購買與批量下架、顯示
當前市場手續費，以及會真正刪除身分資料（而不是藏起來）的帳號刪除流程。

## 2026-10-31 — Redeploying the arena / 重新部署競技場

We redeployed the contract to ship batch actions and a configurable market fee,
then rescanned the chain from the new deploy block and funded the grant pool. To
prove the money path works, we signed a claim, paid it, and replayed the same
cheque to confirm it is rejected. The old deployment is archived.

我們重新部署合約，以帶上批量操作與可調整的市場手續費，接著從新的部署區塊重掃
鏈上資料並注資資助池。為了證明金流可用，我們實際簽發並支付了一筆領取，再用同一
張支票重放，確認會被拒絕。舊部署直接歸檔。

## 2026-10-30 — Why a public hub / 為什麼開公開倉庫

Rules, scoring and grant mechanics should not live only inside a website we can
edit silently. This repository keeps the condensed rules, the changelog, this log
and press assets in one place with public history. The agent integration contract
stays on the website on purpose: two copies of the same schema always drift, and
the one an agent reads must be the one the server enforces.

規則、積分與資助機制不應該只存在於我們可以默默修改的網站裡。這個倉庫把精簡規則、
變更摘要、這份日誌與媒體素材集中在一處，並保留公開歷史。Agent 接入契約刻意只放在
官網：同一份結構的兩份副本必然漂移，而 Agent 讀到的那份，必須就是伺服器實際執行
的那份。
