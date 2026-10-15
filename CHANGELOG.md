# Changelog

## v2.3.24

- ST 主表新增「全选当前筛选」复选框（表头）与「清除勾选」「删除选中关键词」按钮，并显示「已勾选 N 个关键词」；未勾选时两个按钮自动禁用。
- 新增 `deleteKeywordGroups(entries)` 批量删除：命中的全部历史记录作为一个 `st_keyword` / `st_keywords` 条目移入回收站（主回收站 + `aso_st_trash_v1` 双写，`deletedAt` 与 `_trashId` 一致），单行「删整词」也走同一实现。
- `deleteSingleDate`（`st_date`）与「删整词」（`st_keyword`）不再硬删除，改为移入回收站，可在回收站恢复。
- 重写「删除当前 App 数据」：按「下拉框具体值 → selectedApps[0] → 当前筛选唯一的 App」三级解析，全部走 `safeJsonParse` / `normalizeRecords` / `saveMasterData`；删除后清空残留筛选、把 `#appSelect` 复位并刷新下拉框，成功提示「已移入回收站：App（N 条记录）」，失败提示「删除失败：…」，不再静默无反应。
- `deleteAppTracking(app)` 改为委托给同一条实现，删除不存在的 `deleteCurrentAppBtn` 死绑定。
- 移除数据管理「导入与导出」面板及其隐藏 file 输入，并清理只服务于这些按钮的 `exportUnifiedData` / `importUnifiedData` / `importQimaiFile` / `exportIosExcel`；ST / 七麦 / 点点 / 交叉验证页面的 Excel 导出保持不变。
- 回收站改为独立面板：新增「刷新回收站」「恢复全部」「清空回收站」按钮与「保留 30 天 / 最多 50 条」说明；统一保留策略文案为 30 天（原先 UI 写 21 天、代码已为 30 天）。
- 重写 `renderTrashPanel()`：显示条目数 / 记录总数 / 最早删除时间，并按条目展示中文类型标签、App/批次/关键词、记录数、删除时间、剩余保留天数，以及「恢复」「详情」「永久删除」按钮（详情最多列出 20 条记录摘要、`stStatuses` 数量与 `keywords` 数组）。
- 新增 `permanentlyDeleteTrash(index)`、`restoreAllTrash()`、`clearTrashAll()`、`trashEntrySummary(entry)`：所有恢复/删除都会同步清理两个回收站存储，避免恢复后出现重复条目；清空回收站需两次确认且明确不可恢复。
- 修复 `moveToTrashRecord` 会把 ST 镜像条目复制进主回收站的问题，并让 `moveStorageKeyToTrash`、关键词清单护栏清理复用同一个 `deletedAt` 写入两个存储。

## v2.3.23

- 新增 ST 关键词清单护栏：ST 同步只接收已登记关键词清单内的排名行，清单外的关键词会被拒绝并记录，避免其他 App 的关键词行写入本 App/批次。
- 新增存储键 `aso_st_keyword_whitelist_v1`（App+批次+国家 -> 清单）与 `aso_st_rejected_keywords_v1`（被拒绝关键词，最多 500 条）。
- 数据管理新增“关键词清单护栏（Sensor Tower）”卡片：刷新清单状态、清理历史清单外关键词（先预检再确认，记录移入 ST 回收站）。
- 新增全局工具：`ASO_ST_KEYWORD_STATUS`、`ASO_SET_ST_KEYWORD_WHITELIST`、`ASO_PRUNE_ST_OUT_OF_LIST`。

## v2.2.0 Preview

- 新增 iOS 点点页面。
- 新增七麦/点点交叉验证页面。
- 看板接收器新增 `diandian_ios` 快照。
- 统一备份新增点点快照。
- 数据管理新增点点独立清理。
