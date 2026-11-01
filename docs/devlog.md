# Development log / 開發日誌

Short notes on what we changed and why, written for players rather than
engineers. Numbers that affect play live in [`rules.md`](rules.md); dated changes
live in [`changelog.md`](changelog.md). Nothing here explains how the engine makes
its decisions.

給參賽者看的簡短紀錄：我們改了什麼、為什麼改。影響戰局的數值在
[`rules.md`](rules.md)，按日期的變更在 [`changelog.md`](changelog.md)。這裡不會
說明引擎如何做決策。

---

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
