# GameHotData

为 **BKB Bot 的游戏热点研究**积累可追溯的页面观察数据：记录游戏在哪里出现、页面展示了什么指标、何时观察，以及这些数据有哪些限制。适合发现研究候选、跟踪栏目变化和开展后续同口径对比，不是全网游戏热度榜，也不直接代表玩家规模、收入或增长率。

## 当前状态

首组指标基线于 **2026-10-23** 采集，由三份 JSON 组成：CrazyGames、Google Playground 和 Poki。旧快照已从主分支当前文件树移除，但仍保留在 Git 提交历史中；不应把旧口径与本组基线直接拼接。

- **更新计划**：外部定时任务已配置为自 **2026-10-24** 起，每天北京时间 **00:00、06:00、12:00、18:00** 采集，间隔 6 小时。
- **执行方式**：由仓库外的采集与调度流程执行，不是 GitHub Actions；本仓库当前保存数据，不包含可直接运行的采集器或调度代码。计划时间不等于实际完成时间，以文件内时间戳为准。
- **范围**：当前仅覆盖上述三个平台。Twitch 已排除；Instagram、YouTube 尚未采集。
- **封面与简介**：已为首组基线的 45 条栏目记录补充中文简介，并下载 38 张去重封面；Subway Surfers 的封面明确标记未观察到。补充元数据不刷新原有指标。
- **分析阶段**：首组基线本身不能支持增长、加速或持续走热结论；后续快照需先检查范围和指标口径是否可比。

### 最新追加批次（北京时间 2026-10-24 00:00 计划）

已按实际完成时间追加三份文件，未覆盖首组指标基线：

- [CrazyGames 20261023T160753Z](2026/10/24/crazygames_20261023T160753Z.json)：20 条栏目记录，前 5 个详情重新读取指标。首页栏目变为 `Top picks for you`，没有观察到 US 标签，不能直接与首组地区榜的位置比较。
- [Playground 20261023T161006Z](2026/10/24/playground_20261023T161006Z.json)：刷新页面后重读两栏各 10 张卡片，保留实际 DOM 顺序。2 个封面地址改变后重新下载，字节哈希与已有文件相同，因此没有重复存图。
- [Poki 20261023T160833Z](2026/10/24/poki_20261023T160833Z.json)：重新读取首页前五和详情状态，4 个详情正常、Subway Surfers 仍为 404。本轮没有观察到在线数，保存 `null`，不沿用上次值。

这轮观察发生在 UTC 16:06–16:09，完成于 16:07–16:10，即北京时间次日 00:06–00:10。目录按北京时间分区，文件名仍使用 UTC。封面/简介复用时保留原有独立时间戳；新增快照不保证栏目或统计口径与前一轮完全一致。

## 平台与数据来源

所有来源均为实际浏览到的渲染页面，不是官方统计接口或全量数据库。详情页地址保存在记录内，以下列出本组基线的入口与取样方式。

| 平台 | 平台介绍与准确入口 | 本组基线范围 |
| --- | --- | --- |
| CrazyGames | 浏览器游戏平台。[首页](https://www.crazygames.com/)与 [Hot 页面](https://www.crazygames.com/hot)提供游戏发现入口。 | 两个入口各取 DOM 顺序前 10 张游戏卡片，共 20 条；原指标采集仅额外检查首页前 5 个游戏的详情页。封面/简介补充时另查其余详情，但没有改写原指标。首页观察栏目为 `Top games in US today`，不是经独立验证的全球榜。 |
| Google Playground | `playground.google` 上的游戏创作与发现页面。本仓库只观察[首页](https://playground.google/)的通用游戏发现卡片。 | `homepage_trending_carousel` 与 `homepage_popular_played_carousel` 两个栏目各取前 10 张游戏卡片，跳过非游戏创作推广；共 20 条栏目观察、14 个独立游戏；封面与简介也来自通用发现卡片，不访问详情页。 |
| Poki | 浏览器游戏平台。本次观察[中文首页](https://poki.com/zh)的“热门免费游戏”编号列表及其详情链接。 | 保留列表全部 5 个游戏，尝试检查 5 个详情页，其中 4 个取得详情元数据；不额外补入游戏凑数。 |

这些均为**有界、非随机样本**，不覆盖平台全量目录。DOM 顺序是页面结构中的卡片顺序，不自动等于屏幕上的绝对位置或官方名次。地区、语言、登录状态、推荐机制和页面改版都可能影响结果。

### 来源中的已知限制

- **CrazyGames**：首页显示 US 区域标签；Hot 页标题提及“本月”，介绍却提及“过去 24 小时”，统计窗口存在冲突，原样保留，不替平台决定口径。分类或标签旁的数量是目录条目数，不当作单个游戏的游玩量。
- **Playground**：本次使用经授权的既有 Google 登录会话读取通用发现卡片。未存储账户身份、邮箱、浏览历史、个人资料链接、个性化推荐或登录凭据。页面仍有浏览器兼容性提示，但发现卡片可读；未来会话能否持续有效没有保证。登录状态不表示数据包含私人账户内容。
- **Playground 栏目含义**：trending 栏目的原文是“过去 24 小时内玩家数量增幅最大”，但页面未提供增幅数值。另一栏选择的是“最热门游戏”，卡片顺序也不严格按展示游玩量降序排列。两者均不据此生成官方数字排名。
- **Poki**：[Subway Surfers 详情链接](https://poki.com/zh/g/subway-surfers)在本次观察及直接复查时显示 404。保留首页观察，详情字段留空，并记录 `source_extensions.detail_page_status = "error_404_verified"`；这不是对该游戏永久下架的判断。
- **Poki 排名冲突**：首页明确编号和详情页“热门”名次分别保留。例如 Level Devil 首页编号为 1，详情页显示 `#9`，不能合成同一套排名。

## 目录与时间

```text
assets/
└── covers/
    ├── crazygames/<sha256>.avif
    ├── playground/<sha256>.webp
    └── poki/<sha256>.avif
2026/
└── 10/
    └── 23/
        ├── crazygames_20261023T152614Z.json
        ├── playground_20261023T152634Z.json
        └── poki_20261023T152804Z.json
```

- 目录格式：`YYYY/MM/DD/`，按 **Asia/Shanghai（北京时间，UTC+8）**日期分区。
- 文件名格式：`平台_YYYYMMDDTHHMMSSZ.json`，时间部分使用 **UTC**，`Z` 表示 UTC。本组文件名对应各文件的 `collection_completed_at_utc`。
- 例如 `20261023T152614Z` 是北京时间 **2026-10-23 23:26:14**。跨北京时间午夜时，目录日期与文件名中的 UTC 日期可能不同。
- `batches[].observation_window_utc` 描述批次观察窗口；记录和指标的 `observed_at_utc` 描述相应观察时间。文件完成时间不是每条数据同时测得的时间。

直接查看基线：[CrazyGames](2026/10/23/crazygames_20261023T152614Z.json) · [Playground](2026/10/23/playground_20261023T152634Z.json) · [Poki](2026/10/23/poki_20261023T152804Z.json)

## 数据格式

文件为 UTF-8 JSON，补充封面/简介后的首组基线标记 `schema_version: "2.1.0"`；相对 2.0.0 增加了 `cover`、`description` 和 `enrichment`，原指标字段保留。这是当前版本标识，**不表示三个文件具有完全一致的结构，也不是已经通过统一 JSON Schema 校验的机器契约**。读取程序应兼容可选字段、不同层级和 `null`，不要仅凭版本号假定字段必定存在。

### 共用的组织方式

| 路径 | 含义 |
| --- | --- |
| `schema_version`、`document_kind` | 版本及文档类型；CrazyGames 为 `fresh_public_ui_test_run`，另外两份为 `public_ui_sample`。 |
| `collection_completed_at_utc` | 该文件采集完成时间。 |
| `batches[]` | 同一文件内的来源或栏目批次。一个文件可以包含多个批次。 |
| `batches[].batch_id` | 本次批次标识，包含时间，不是跨期固定的栏目 ID。 |
| `batches[].source` | 平台、入口 URL、读取方式和访问上下文。 |
| `batches[].scope` | 页面范围、抽样规则、覆盖性质、记录数和详情页检查情况。 |
| `batches[].records[]` | 本批次的游戏观察记录。 |
| `records[].entity` | 原始游戏名、来源 URL、身份键及部分平台的额外身份字段。此处及下文 `records[]` 均位于 `batches[]` 内。 |
| `records[].selection_evidence` | 来源栏目、观察时间、展示位置、位置依据及平台明确给出的名次（如有）。 |
| `records[].metrics[]` | 按 `name` 区分的指标数组；不要把数组下标当作固定指标编号。 |
| `records[].game_associations[]` | 来源游戏标签及身份关联依据，未据此完成跨平台游戏身份核验。 |
| `records[].source_extensions` | 平台特有的原始元数据、详情页状态和冲突说明。 |
| `records[].cover` | 实际下载的封面路径、来源、哈希、格式、大小、尺寸与独立下载时间；缺失时明确标记。 |
| `records[].description` | 带来源与独立时间的中文事实概括，明确标记为生成摘要，不冒充官方原文。 |
| `enrichment` | 后补元数据的完成时间、范围及 `metrics_refreshed: false`，不改变原指标观察时间。 |

### 三份文件的实际差异

| 项目 | CrazyGames | Playground / Poki |
| --- | --- | --- |
| 日期分区 | 根级 `partition` | `batches[].partition` |
| 限制说明 | 根级 `limitations` | `batches[].limitations` |
| 指标语义 | 根级 `metric_semantics`；各指标含精度、时间口径等可选字段 | 各条 `metrics[]` 内有 `semantics`、`precision`、`time_basis` 等字段 |
| 指标来源与时间 | 已检查详情的指标由记录的 `source_extensions.detail_source_url` 和 `source_extensions.observed_at_utc` 溯源 | 每条指标直接含 `source_url` 与 `observed_at_utc` |
| 内容保存政策 | 根级 `data_policy` 与 `limitations` | 根级 `data_policy` |
| 身份字段 | `entity` 仅有 `type`、`title_raw`、`canonical_url`、`identity_key` | 另有 `platform_id`、`identity_basis`、`published_at_utc`；Poki 的 `platform_id` 为 `null` |

身份键也有差异：

- Playground 使用来源 URL 中观察到的游戏 ID，例如 `playground:game:1074005397800118085`。
- CrazyGames、Poki 当前使用来源 URL 作为回退身份，例如 `poki:game:url:https://poki.com/zh/g/level-devil`。`canonical_url` 在这里是记录的来源游戏地址，不意味着完成了重定向或跨语言 URL 归并。
- 不应只按游戏名合并，也不能把不同平台上相似的名字视为同一款游戏。

详情元数据没有被强行统一：CrazyGames 使用 `developer_raw`、`release_header_raw`、`last_updated_body_raw`、`breadcrumb_labels`、`tag_labels` 等；Playground 卡片保留 `genre_raw` 与 `player_mode_raw`；Poki 使用 `developer`、`release_date_raw`、`latest_update_raw`、`genre_raw` 等。日期保留原始精度，未用月份猜测具体日期。页面自身的发行日期或排名冲突存放在 `release_conflict`、`metadata_conflicts` 等相应字段中。

### 真实指标示例

以下完整指标对象摘自 Playground 第一批次的 CATacombs 记录，即 `batches[0].records[0].metrics[0]`，不是完整文件结构：

```json
{
  "name": "playground.plays",
  "value": 13700,
  "unit": "plays",
  "raw_value": "13.7K",
  "raw_text": "13.7K 次游玩",
  "semantics": "Displayed game plays count. Not unique players, concurrent users, or 24-hour change; accumulation window unspecified.",
  "availability": "observed",
  "precision": "approximate_source_abbreviation",
  "time_basis": "unspecified_by_source",
  "parsing_rule": "K × 1000; rounded source abbreviation, exact underlying count and rounding method unknown.",
  "source_url": "https://playground.google/",
  "observed_at_utc": "2026-10-23T15:26:34Z"
}
```

这里的 `13700` 只是把 `13.7K` 换算为便于处理的数值，不是已知精确计数；也不是过去 24 小时新增 13,700 次游玩。

## 封面与中文简介

首组指标基线中共有 **45 条栏目观察、39 个按平台身份去重的游戏**。封面和简介在其后单独补充，不能把其下载/生成时间当作评分、点赞或游玩量的采集时间。

| 平台 | 记录数 / 独立游戏 | 实际下载的去重封面 | 中文简介 |
| --- | --- | --- | --- |
| CrazyGames | 20 / 20 | 20 张 AVIF | 20 个游戏 |
| Google Playground | 20 / 14 | 14 张 WebP | 14 个游戏，复用于两个栏目 |
| Poki | 5 / 5 | 4 张 AVIF | 5 个游戏 |
| 合计 | 45 / 39 | 38 张，共 541,901 字节 | 45 条记录均有简介 |

### 封面字段与去重

- `source_url`：页面真实图片元素暴露的下载地址，保留原样；`source_page_url`：观察到该图片的页面。
- `repository_path`：**相对仓库根目录**的实际文件路径，例如 `assets/covers/poki/<sha256>.avif`。读取日期目录下的 JSON 时，不要把它误当作相对该 JSON 所在目录。
- `sha256`、`mime_type`、`byte_size`、`width`、`height`：按真实下载字节和完整图片解码验证。CDN URL 即使以 `.png` 结尾，也可能协商返回 AVIF；文件后缀与 MIME 以实际图片为准。
- `fetched_at_utc`：实际成功下载时间。图片观察时间如存在，单独记录为 `observed_at_utc`。
- `status: "downloaded"` 才代表仓库内有真实图片；缺失使用 `not_observed`，下载失败使用 `download_failed`，同时令路径、哈希和成功下载时间为 `null`，保留原因。不能将外链或 HTML 错误页算作已下载。
- 以文件内容 SHA-256 命名，在同平台内复用相同内容。同一游戏跨栏目重复出现时复用同一文件；未来图片改变则保存新哈希文件，不覆盖旧快照引用。

Poki 的 Subway Surfers 详情在补充时仍返回 404，首页提供了简短玩法信息，但没有观察到对应封面。已保留中文简介并明确标记缺图；没有用 Subway Surfers Match、Blast 或其他游戏图片替代。

### 简介字段与来源

`description.text` 是依据可见游戏文案或既有可追溯摘要形成的简短中文事实概括。`source_urls` 指向依据页面，`basis` 说明是卡片文案、详情文案还是既有摘要的中文转述；`language` 为 `zh-CN`，`status` 为 `generated`，`is_generated` 为 `true`。`observed_at_utc` 与 `generated_at_utc` 分别记录依据观察和生成时间。

不复制完整宣传介绍，不凭游戏名猜玩法，也不把来源声称的玩法当作亲自游玩验证。旧的 `source_extensions.gameplay_summary` 是原采集时的部分摘要，可能为英文或 `null`；统一展示请使用新增的 `description`，不要改写旧字段来伪装同时采集。

后续定时采集沿用这些字段和内容寻址规则，新增日期快照，不覆盖已有指标快照。封面或简介抓取失败不会抹掉其他已经取得的事实数据。

## 指标应该怎样读

| 平台 | 当前指标名 | 使用边界 |
| --- | --- | --- |
| CrazyGames | `crazygames.rating`、`crazygames.likes`、`crazygames.dislikes` | 仅检查过的 5 个详情页有观察值。评分标注基于最近 6 个月；赞/踩的累计窗口未知，不是游玩人数。 |
| CrazyGames | `crazygames.plays`、`crazygames.active_users` | 本组均未取得数值，保留 `null`；不能由赞/踩推导。 |
| Playground | `playground.plays`、`playground.likes` | 页面展示的游玩次数与点赞数；累计窗口未说明，不等于独立玩家、同时在线人数或 24 小时增量。 |
| Poki | `poki.homepage_rating`、`poki.rating` | 首页与详情页评分分别记录；不是热度增速。 |
| Poki | `poki.likes`、`poki.dislikes`、`poki.rating_votes` | 赞/踩缩写值与展示的评分投票总数分别保存，不要求近似赞踩之和精确等于投票总数。 |
| Poki | `poki.online_display` | 原文如 `5,000+`，保存 `value: 5000`、`comparison_operator: ">="`；“在线”的人数定义和测量窗口未知，不能宣称是精确 CCU 或活跃用户。 |
| Poki | `poki.detail_popularity_rank` | 仅代表详情页显示的名次，与首页编号独立，范围和计算方法未知。 |
| Poki | `poki.plays`、`poki.active_users` | 本组均未观察到，保留 `null`，不从评分、投票或“在线”标签推断。 |

共同规则：

1. **保留原文与精度。** `K × 1000`、`M × 1000000` 仅用于数值换算；来源舍入规则未知。`integer_as_displayed` 表示页面按整数展示，也不是后台审计结果。不同平台的评分上限未在本次观察文本中明确，不擅自归一化为百分制。
2. **空值不等于零。** `availability: "not_collected"` 表示该项未采集；`"not_observed_in_inspected_detail"` 表示检查详情时未观察到该指标。Poki 的 404 还需结合详情状态判断。缺失字段也不要补成零。
3. **展示顺序不等于官方排名。** CrazyGames 与 Playground 的 `selection_evidence.platform_rank` 为 `null`。Poki 首页明确编号 1–5，才在该字段记录数字；这仍是来源页面的编号，不是仓库独立核算的全网排名。
4. **同一游戏可以有多条记录。** Playground 的 20 条记录包含 6 个同时出现在两个栏目的游戏。统计独立游戏数时按 `entity.identity_key` 去重；分析栏目曝光时保留批次归属，不能直接删除重复观察或累加其游玩量。

## 后续如何比较

1. **先固定比较口径**：平台、入口 URL、栏目/`scope.surface`、筛选与排序、地区与语言、登录上下文、抽样规则应尽量一致。不要用含时间的 `batch_id` 直接连接跨期数据。
2. **再匹配实体与指标**：同平台按 `entity.identity_key` 匹配，指标按 `metrics[].name` 匹配，使用对应的观察时间。跨平台匹配需要另外核验身份。
3. **先检查可比性，再计算差值**：只有双方确有观察值、语义和累计窗口可比时，才讨论展示计数变化。缩写值的微小差异可能来自舍入；下界展示不能当精确值相减。当前这些指标的累计窗口多未说明，应保留这一限制。
4. **区分候选变化与真实增长**：入选/移出样本只说明在本次有界观察中的出现情况；未入选不等于归零、下架或热度消失。栏目位置变化也可能受推荐机制、地区、时间段或页面布局影响。
5. **不要混加不同含义的量**：游玩次数、点赞、投票、评分、在线标签、展示位置和详情名次不是同一种“热度”。当前不提供跨平台统一热度分数，也不能据单次样本推断留存、玩家规模或趋势。

## 采集边界与使用说明

本组数据通过云端浏览器读取页面，未使用隐藏 API，也未使用用户电脑。没有主动游玩、投票或下载游戏；访问详情页时嵌入式游戏可能自动加载。封面补充仅下载页面实际暴露的游戏缩略图，保存至仓库；记录保留事实性元数据、必要的短证据标签和自行概述的中文简介，不保存完整介绍文案、视频、游戏代码或其他游戏资源。

公开可见和能够下载不等于获得任意再发布许可，仓库未主张封面授权。平台名称、商标及原内容权利归相应权利人；本仓库不代表与 CrazyGames、Google Playground 或 Poki 存在合作、授权或背书关系。使用者应自行确认来源条款及适用的使用权限，保留来源与观察时间，不把采样数据包装为官方统计。后续文件若与本文示例不同，应优先读取文件自身的来源、范围、语义和限制说明。
