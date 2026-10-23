# 历史快照更正

此目录保存追加式更正记录。原始采集快照保持不变，更正记录不是新的采集批次，也不能计入新的游戏、热度或增长数据。

## Playground 栏目来源更正（2026-10-24）

记录：[playground_section_provenance_20261023T231822Z.json](2026/10/24/playground_section_provenance_20261023T231822Z.json)

适用原始快照：
- `2026/10/24/playground_20261023T161006Z.json`
- `2026/10/24/playground_20261023T220934Z.json`

两份快照的 trending 栏目中，CATacombs、Coastal Cruiser、Midnight Pursuit、Grumpy Knight、Westbound Frontier 和 Oregon Quest 3D 的位置分别为 1、2、3、4、5、7。这些游戏也出现于 popular 栏目。旧组装逻辑按游戏 ID 复用整条记录，导致后者的栏目说明覆盖了前者的模板。

更正仅涉及上述 12 条栏目成员记录的 4 个 `selection_evidence` 字段，共 48 个值：
- `section_name_raw`：改为 `热门游戏`。
- `section_definition_raw`：改为 `过去 24 小时内玩家数量增幅最大`。
- `ranking_method`：改为相同的 trending 栏目定义。
- `note`：改为 trending 栏目说明，明确页面未提供增长数量，游玩及点赞计数不得解释为增长值。

依据为两次采集当时保存的页面 DOM 提取结果，包括栏目标题、定义、所选 popular 标签和带 `ref=trending_games` 的卡片链接，并由未受影响的同批次 trending 记录及原始基线佐证。没有使用后来的页面推断历史状态。

指标数值、显示位置、观察时间、采集完成时间、批次范围、游戏身份、封面与简介均不改变。所有 popular 栏目记录与 15:26 基线也不改变。`created_at_utc` 只是更正记录的生成时间，不是重新采集时间。

## 读取方式

1. 以 `snapshot_path` 找到原文件，先验证其字节级 `original_sha256`。记录还包含已核实提交的链接与 Git blob SHA。
2. 在内存副本上事务性应用目标的 RFC 6902 JSON Patch。所有 `test` 必须通过，再接受更正视图；任一失败都应整体停止。
3. `patch` 只允许所列字段的 `replace`，不更新原始文件。保留更正 ID 与原始快照路径，以便追溯。
4. 如将结果序列化为 UTF-8、双空格缩进、不转义 Unicode、末尾单个换行，可核对 `corrected_view_sha256`。
5. 不要把更正视图作为新快照追加到时间序列，也不要对已更正视图再次套用此补丁。未经 overlay 处理的消费者仍会看到原文件中的旧字段。

## 防止复发

组装时只按游戏身份复用实体元数据。栏目上下文来自本轮实际保存的页面证据，并独立生成每条成员记录；图片来源及下载时间按实际图片 URL 保留，不按游戏 ID 合并。验证必须逐条对照批次表面、栏目定义、位置、卡片成员关系与本轮指标证据；仅校验 JSON 格式和数量不够。
