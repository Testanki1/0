# 0 · Tanki Jump（3D 坦克迷你游戏 · 平台跳跃重构）

> [中文版](#中文版) · [English version](#english-version) · [引用来源](#引用来源--sources)

---

# 中文版

一个在浏览器里运行的 3D 迷你小游戏：用 **Three.js + Rapier（WebAssembly 物理）** 把《3D坦克》（Tanki Online）历次活动迷你游戏的场景取到本地，做成一个可自由驾驶、跳跃、计时的平台跳跃玩法。无后端、无账号、无广告，唯一持久化是 `localStorage` 里的本地最佳成绩。

## 非官方声明

> **本作品为玩家基于对游戏的热爱自发制作的同人成果，与《3D坦克》官方无从属代理关系。**
>
> *"This content is not affiliated with Tanki Online and was created by fans."*

两句均为官方 Wiki 给出的标注范例句，非本文作者自拟。游戏开始页与「？」帮助面板内也显示同一句。

## 玩法

- 摇杆 / **WASD** 移动，**SPACE** 跳跃，拖拽旋转视角，点击屏幕进入全屏。
- **逆时针**方向跳跃得分，反向跳跃扣分，双跳 **+2** 分。
- 完成 6 圈共 102 分即通关，最佳成绩保存在本地。

## 与官方原站的关系：四个场景分别对应哪里

本项目的四个场景，对应官方四个独立的活动迷你游戏网站（每年复刻一次，活动期结束后可能下线）。**国际服与国服都是官方站点**：国际服在国际服官网域名下，国服由官方运营团队在 3dtank.com 与 pages.3dtank.com 本地化运营。

| 本项目场景 | 官方活动站（国际服） | 官方活动站（国服） | 官方玩法 |
| --- | --- | --- | --- |
| `cyber` 赛博坦克 | <https://pages.tankionline.com/summer-game/> | <https://pages.3dtank.com/summer-game/> | 在悬浮平台之间移动，每步消耗 10 能量 |
| `snow` 3D坦克与雪陷阱 | <https://new-year.tankionline.com/> | 暂未提供 | 在浮冰之间移动，燃料为“姜饼”；踩到雪陷阱有 50% 概率后退两步或原地不动 |
| `space` 太空中的坦克 | <https://darkmatter.tankionline.com/> | 暂未提供 | 在星舰残骸碎片间移动，每步 10 能量；踩到放射性碎片有 50% 概率后退 2 格 |
| `halloween` 不给糖就捣蛋 | <https://pages.tankionline.com/halloween> | <https://pages.3dtank.com/halloween> | 在亡灵岛屿间移动，燃料为“棒棒糖”，每步 10 根；6 圈后抵达第 7 个检查点通关 |

**一句话概括关系**：官方原站是“**限时活动进度网页**”（登录 → 花活动货币 → 走格子/跳平台 → 领游戏内奖励）；本项目是“**把官方活动场景搬出来做成一个 3D 跳跃小游戏**”（无需登录 → 没有货币 → 靠操作打分 → 只有本地记录）。场景与官方站的对应关系，是通过比对页面资源文件名逐一确认的。

## 逐项对比：`/0/` vs 官方迷你游戏原站

| 维度 | 官方活动站（halloween / new-year / darkmatter / summer-game） | 本项目 `/0/` | 评价 |
| --- | --- | --- | --- |
| 入口 | 每年活动期开放（通常 27 天左右），由官网新闻页跳转；过期后部分会显示“网站暂不可用” | 永久在线，GitHub Pages 静态托管 | 生命周期不同，本项目不受活动期限制 |
| 登录 | **必须**用游戏昵称 + 密码登录活动网站（社交账号登录者需联系客服取密码） | 无登录、无后端、无任何账号数据 | 本项目不接触玩家凭据 |
| 玩法类型 | 棋盘/跳岛式进度玩法：花货币选择前进 1–2 格，躲避陷阱（后退 2 格 / 原地） | 实时 3D 平台跳跃：WASD 驾驶 + 跳跃 + 转视角 + 计时/圈数 | **玩法完全不同**，本项目是“操作型”而非“进度型” |
| 活动货币 | 有（能量 / 棒棒糖 / 姜饼），通过合约、金箱子、特殊任务获取，也可付费购买 | 无 | 本项目不涉及任何付费与掉落机制 |
| 奖励 | 真实发放到游戏账号：迷彩、手榴弹、改造、钥匙、皮肤（如十字军“恶魔”皮肤） | 仅 `localStorage` 里的最佳时间，无任何游戏内收益 | 本项目不产生任何游戏内利益 |
| 通关结构 | 官方多为“走满 N 圈到第 7 个检查点” | 也是 6 圈、第 102 分通关（刻意对齐官方节奏） | 数值节奏参考官方，但交互方式不同 |
| 技术栈 | React（CRA 打包，`/static/js/*.chunk.js`）+ 后端登录接口 | 单个 `index.html` + importmap 引入 `three` 与 `@dimforge/rapier3d-compat`，纯前端物理 | 本项目无构建、无后端 |
| 3D 素材 | 官方自有 CDN（`web-cdn.tankionline.com`） | 取自同一批官方活动素材（模型 / 环境图 / 天空图 / 音乐） | 素材来源同源，非原创美术 |
| 音频 | 官方原创 BGM 与音效 | 仓库内另存官方音频并播放 | 同源 |
| 多语言 | `?lang=en` 等参数切换，官方维护多语种 | 依 `navigator.language` 在中文/英文间切换 | 本项目只有中英两种，本地自行维护 |
| 多端 | 早年限 PC，近两年起支持手机浏览器 | 同时支持桌面与移动（虚拟摇杆） | 相当 |
| 官方标识 | 官方 Logo、官方域名、备案信息 | 无 Logo；标题含 “Tanki” 字样 | ⚠️ 建议标题加 `(Unofficial)` |

### 结论

1. **玩法层面不是复制品**：官方是“活动进度网页”，本项目是“操作型 3D 平台跳跃”，两者交互方式、货币体系、奖励链路完全不同，本项目也不接触任何账号数据。
2. **素材层面同源**：模型、环境图、天空图与音乐取自官方活动站使用的同一批素材。这是本项目唯一需要留意的部分。
3. 本项目为非官方同人作品，无广告、无收费、无付费墙，不在游戏内或官方社群宣传链接；如官方有任何异议，收到通知后会立即下线或删除相关内容。

---

# English version

A browser-based 3D mini-game built with **Three.js + Rapier (WebAssembly physics)**. It takes the scenes from past Tanki Online event mini-games and turns them into a free-roam parkour course: drive, jump, chase the clock. No backend, no accounts, no ads — the only thing stored is your best time, in `localStorage`.

## Unofficial notice

> *"This content is not affiliated with Tanki Online and was created by fans."*
>
> 本作品为玩家基于对游戏的热爱自发制作的同人成果，与《3D坦克》官方无从属代理关系。

Both lines are the sample wording published by the official Tanki Online wiki, in its English and Chinese editions respectively — not a paraphrase. The same notice is shown on the start screen and in the in-game help panel.

## Gameplay

- Move with the joystick or **WASD**, jump with **SPACE**, drag to look around, tap to go fullscreen.
- Counter-clockwise jumps score points; jumps in the other direction cost points; a double jump is worth **+2**.
- Finish 6 laps / 102 points to complete the course. Your best time is saved locally.

## Where the four scenes come from

Each scene is taken from a separate official event website. These sites are re-run roughly once a year and some are taken offline once the event ends. **Both the international and the Chinese sites are official** — the Chinese game is run on the official 3dtank.com domain, including its own event pages under pages.3dtank.com.

| Scene | Official site (international) | Official site (Chinese server) | Official gameplay |
| --- | --- | --- | --- |
| `cyber` CyberTank | <https://pages.tankionline.com/summer-game/> | <https://pages.3dtank.com/summer-game/> | Move between floating platforms; 10 Energy per move |
| `snow` Snow Trap | <https://new-year.tankionline.com/> | Not available | Move across ice floes fuelled by Gingerbread; a snow trap has a 50% chance of pushing you two steps back or holding you in place |
| `space` Tanks in Space | <https://darkmatter.tankionline.com/> | Not available | Move between starship fragments; 10 Energy per move; a "radioactive fragment" has a 50% chance of sending you two steps back |
| `halloween` Trick-or-Treat | <https://pages.tankionline.com/halloween> | <https://pages.3dtank.com/halloween> | Move between demonic islands fuelled by Lollipops (10 per move); reach checkpoint 7 after 6 laps |

**In short**: the official sites are limited-time event pages. You log in, spend event currency, move between platforms or islands, and collect in-game rewards. This project takes those scenes and turns them into a real-time 3D parkour game: no login, no currency, skill-based scoring, a local best time and nothing else. The scene-to-site mapping was confirmed by comparing resource filenames on each official page.

## Side by side: `/0/` vs the official mini-game sites

| Aspect | Official event site (halloween / new-year / darkmatter / summer-game) | `/0/` | Notes |
| --- | --- | --- | --- |
| Availability | Live for about 27 days per event and linked from the news post; some go offline afterwards | Always online, static hosting on GitHub Pages | Different lifespan |
| Login | Required: in-game nickname and password. Players who sign in through social accounts have to ask support for a password | None — no backend, no account data | This project never touches player credentials |
| Game type | Board-style progression: spend currency to move one or two cells forward and dodge traps that send you back or hold you in place | Real-time 3D parkour: drive, jump, orbit the camera, race the clock | A different genre — skill-based rather than progression-based |
| Event currency | Yes (Energy, Lollipops, Gingerbread), earned from contracts, gold boxes and special missions, or bought outright | None | No purchases, no drop mechanics |
| Rewards | Real items credited to the game account: paints, grenades, augments, keys, skins such as the Demonic skin for Crusader | A best time in `localStorage` | No in-game benefit at all |
| Progression | Usually "complete N laps and reach checkpoint 7" | Also 6 laps and 102 points, deliberately matching the official pace | The pacing follows the event; the interaction does not |
| Tech stack | React (Create React App build, `/static/js/*.chunk.js`) plus a login backend | A single `index.html` with an import map for `three` and `@dimforge/rapier3d-compat`; physics run entirely in the browser | No build step, no backend |
| 3D assets | Served from the game's own CDN (`web-cdn.tankionline.com`) | The same official event assets: models, environment maps, sky textures and music | Same sources, not original artwork |
| Audio | Official soundtrack and sound effects | Official audio re-hosted in this repository and played in the page | Same sources |
| Languages | Switched with `?lang=en` and similar parameters, maintained officially | Switches between Chinese and English based on `navigator.language` | Two languages, maintained here |
| Platforms | PC only in the early years; mobile browsers supported more recently | Desktop and mobile, with a virtual joystick | Comparable |
| Official branding | Official logo, official domain, ICP filing | No logo; the title contains the word "Tanki" | ⚠️ Consider adding `(Unofficial)` to the title |

### Summary

1. **It is not a gameplay clone.** The official pages are event progression boards; this is a skill-based 3D platformer. The interaction, the currency and the reward chain have nothing in common, and no account data is involved.
2. **The assets come from the same sources.** Models, environment maps, sky textures and music are the ones used by the official event sites. That is the one part of this project worth keeping an eye on.
3. This is a non-commercial fan project: no ads, no fees, no paywalls, and the link is not promoted inside the game or on official channels. If the rights holders object, the page or the related content will be removed as soon as a request arrives.

---

## 引用来源 / Sources

| 文档 | 链接 | 取用日期 |
| --- | --- | --- |
| Cybertank 2026 官方公告（活动站入口） | <https://tankionline.com/en/2026/07/cybertank-2026-mini-game/> | 2026-11-01 |
| Cybertank 官方活动站（国际服 / 国服） | <https://pages.tankionline.com/summer-game/> · <https://pages.3dtank.com/summer-game/> | 2026-11-01 |
| Tanki and The Snow Trap 官方活动站 | <https://new-year.tankionline.com/> | 2026-11-01 |
| Tanks in Space 2026 官方公告（活动站入口） | <https://tankionline.com/en/2026/04/tanks-in-space-mini-game-2026/> | 2026-11-01 |
| Tanks in Space 官方活动站 | <https://darkmatter.tankionline.com/> | 2026-11-01 |
| Trick-or-Treat 2026 官方公告（活动站入口） | <https://tankionline.com/en/2026/10/trick-or-treat-mini-game-2026/> | 2026-11-01 |
| Trick-or-Treat 官方活动站（国际服 / 国服） | <https://pages.tankionline.com/halloween> · <https://pages.3dtank.com/halloween> | 2026-11-01 |
| Fan Content Guidelines（EN，声明出处） | <https://en.tankiwiki.com/Creating_Fan_Content_Guide> | 2026-11-01 |
| 粉丝内容创作指南（中文，声明出处） | <https://wiki.3dtank.net/粉丝内容创作指南> | 2026-11-01 |

说明：

- 官方活动站为限时站点，链接可能随活动结束失效；本项目对应的素材路径以其当时的资源目录为准。
- 引文为原文摘录，版权归原权利方；本文件对官方内容的使用属于评论与说明性质。
