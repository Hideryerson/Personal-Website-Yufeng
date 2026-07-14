# Yufeng Wu 个人网站 · Design System

> 概念:一张唱片的两面。Side A = Research(学术),Side B = Design(设计)。
> 参考:Cargo CD tracklist 模板 · 唱片/CD 包装设计 · Laura Devendorf 的学术-创作平衡。
> 原则:极简、白底黑字、排版驱动;全站唯一的"玩具"是那张透明 CD。

## 1. 色彩 Tokens

| Token | 值 | 用途 |
|---|---|---|
| `--bg` | `#ffffff` | 页面底色(纯白,专辑封套) |
| `--ink` | `#111111` | 正文与标题墨色 |
| `--dim` | `#9a9a9a` | 次要信息、悬停态、meta 行 |
| `--faint` | `#c9c9c9` | 年份括号、最弱层级 |
| `--line` | `#e4e4e4` | 顶栏/底栏分隔线 |

规则:不引入任何品牌色。虹彩只存在于 CD 盘面(粉 `rgba(255,196,222)`、蓝 `rgba(180,208,255)`、黄 `rgba(255,236,186)` 的低透明度扇区)。

## 2. 字体

全站 **Helvetica Neue** 一族,靠字重和大小写区分层级(零字体授权成本):

| 角色 | 规格 |
|---|---|
| 大标题(SIDE A: RESEARCH、子页 H1) | HN 700,全大写,字间距 `-.01em` |
| 小标题(NEWS 等)与 tabs | HN 700,全大写,`.85rem`,字间距 `.01em` |
| 曲目列表 / 姓名行 | HN 700,sentence case,`--xl`,字间距 `-.03em` |
| 首页介绍语 | HN 400,`--xl` |
| 正文 / 引用 | HN 400,`.92–.95rem` |
| meta 行 | HN 400,`.85rem`,`--dim` |

- `--xl: clamp(1.25rem, 2.6vw, 1.9rem)`(v7 缩小一号,避免长标题拥挤)。

## 3. 布局

- **首页**:两栏 grid(`1fr 1fr`,gap 2.5rem)。每栏 = `Disc 1`(小字)→ `SIDE A: RESEARCH`(标题)→ 曲目列表,连续排列不留断层。
- 左下角固定:`Yufeng Wu | 吴御风`(700)+ 一段介绍(400),同 `--xl` 字号。
- 顶栏:左 `Yufeng Wu`(点击回主页)/ 右 `About`(CV 已并入 About 页),下方 1px 分隔线;底栏对称,左侧带图标的链接(Google Scholar / LinkedIn / Email),右侧版权。
- **子页**:内容列 `max-width: 780px` 居中。
- 断点 `700px`:首页单栏,CD 缩至 74vw 并下移,卡片网格转单栏,底栏纵排。

## 4. CD(全站唯一的互动装置)

结构(5 层,均为圆形,尺寸 `min(560px, 72vw)`):
1. `.disc-glass` — `backdrop-filter: blur(2px) saturate(140%)`,做透过文字的折射感;mask 挖出中心孔(7%)
2. `.disc-tint` — 银灰底 + 两三个虹彩扇区(conic-gradient)+ 双反光点;数据区 mask 到 92%
3. `.disc-streaks` — 极细放射纹(repeating-conic,若隐若现)
4. `.disc-rings` — 内圈结构线:中心孔边缘、卡环高光、数据区起始环
5. `.disc-rim` — 92–99.5% 的半透明塑料包边 + 外缘深色轮廓线

行为:
- 默认居中,`prefers-reduced-motion` 允许时以 16s/圈 旋转
- 可拖拽(Pointer Events),拖动位移驱动 `hue-rotate`,虹彩随位置变色
- 中心孔真镂空;整碟半透明,压住文字仍可读

## 5. 组件

- **曲目行**:HN 700 `--xl`,年份 `(YYYY)` 用 `--faint`;hover 变 `--dim`。每条是链接。
- **View all ↗**:小字 `.8rem`,`--dim`,hover 变墨色。
- **标题下划线动画**:`background: linear-gradient(currentColor,currentColor) no-repeat left bottom / 0 2px`,hover 时 `background-size: 100% 2px`,0.35s。tabs 激活态同款(1.5px)。
- **Tabs**:Side A = PUBLICATIONS(默认,含 News / Selected Projects / Publication List)/ TEACHING / SERVICE / AWARDS;Side B = PROJECTS(默认)/ EXPERIENCE / AWARDS。JS 按 `.sub` 容器分组,两组互不干扰;pane id 前缀 `tab-a-` / `tab-b-`。
- **引用条目 `.cite`**:ACM 全名格式(全名 + 年份 + 标题 + 斜体 venue + DOI 全链接),本人名字加粗。Selected Projects 卡片与 Publication List 使用同一格式。
- **卡片 `.card`**:16:10 灰渐变占位图 + `.cite` 引用文本。用于 Selected Projects 和 Teaching。
- **子页 bio `.bio`**:第三人称介绍段,`.95rem`/1.65 行高,置于 H1 与 tabs 之间;Side A 学术版,Side B 设计师版。
- **Awards 分侧**:学术奖项(VC Conference Fund、HGSA、Pratt Circle、Presidential Scholarship)在 Side A;设计奖项(AAA、腾讯小程序、互联网+、武田)在 Side B。
- **项目 `.proj`(Side B)**:3:2 大图 + 标题 + meta + 一句话;整卡是 `<a>` 链接到 `#/design/<slug>` 详情页,悬停标题划线。

## 5.5 Case Study 详情页(v13 新增)

每个项目一个 `<section class="page" id="page-project-<slug>">`,容器 `.case`(max-width 1080px 居中)。

- **头部**:`.case-back`(← Side B: Design)→ `.case-eyebrow`(类别 · 场合,大写小字灰)→ `h1`(clamp 2–3.4rem)→ `.case-sub` 副标题 → `.case-meta`(上下细线夹住的自适应栅格:Date / Context / Client / Role / Collaborators,格子按需增删)
- **内容块**(自由组合,实现图文交替):
  - `.cs-hero`:16:9 通栏大图
  - `.cs-text`:居中 42em 文字块,可带 h3 小标题(大写)
  - `.cs-split`:图 3 : 文 2 双栏,加 `.rev` 左右翻转 —— 交替使用这两个就是 Wix 参考的节奏
  - `.cs-duo`:两图并排
  - `.cs-caption`:图注(.78rem 灰)
- **图片**:`.cs-img` + 比例类 `r169` / `r43` / `r1x1`,内放 `<img>`(未放图时显示渐变占位)
- **尾部**:`.cs-next` —— 左 "← All projects",右 "Next: 下一项目 →",三个项目首尾成环
- **路由**:`routes` 注册 `'/design/<slug>': 'project-<slug>'`
- **移动端**:双栏块全部堆叠(图在上文在下),间距收紧
- **加新项目三步**:① 复制任一 `page-project-*` section 改内容;② `routes` 加一行;③ Projects tab 加一张 `<a class="proj">` 卡片,顺手更新相邻项目的 Next 链接

## 6. 内容维护

- 加论文:首页曲目(若入选精选)+ Side A → Publication List 加一条 `.cite`(+ News 一条,第一人称)。
- 加设计项目:Side B 加一个 `.proj` 块。
- 占位图都用灰渐变 `div.ph`,有真图后替换为 `<img>`(放 `assets/images/`)。
- CV PDF 放 `assets/YufengWu-CV.pdf`。
- 后续计划:拆分为多页面文件(index / research / design / about / cv)+ `publications.js` 数据文件驱动;部署 GitHub Pages。

## 7. 语气 & 文案

- 隐喻点到为止:Disc 1/2、Side A/B、"Released by Yufeng Wu"、About 页的 "Liner notes"——仅此而已,学术内容零隐喻。
- News 用第一人称,论文标题必须写全。
- 介绍语:"Hi, I am an HCI researcher and designer, making technologies that listen, remember, and care — technology for good that supports people's wellbeing, connection, and everyday lives."
