# Changelog

## v2.3.26（数据管理瘦身 + 清理按钮归位）

- 「本地数据量」从 9 行长表格压缩成一行四个徽章（Sensor Tower 排名 / 七麦快照 / 点点快照 / 同步日志），存储键与去重记录数收进「展开存储明细」折叠块。
- 取消「分来源清理」面板，清理入口按平台归位：ST 页工具栏新增「清空全部 ST 数据」，七麦页新增「清空全部七麦数据」，点点页新增「清空全部点点数据」；「数据管理」只保留「清空同步日志」。不再出现「ST 已按 App 分开、却又在数据管理里整块清空」的不一致。
- 「ST 关键词护栏」整块收进折叠面板（默认不展开）；面板内默认只显示一行统计（已登记清单 / 累计拒绝 / 异常关键词），登记清单与拒绝记录明细收进第二层折叠。
- 修复回收站重复显示：同一次删除会同时写入主回收站与 ST 镜像，合并展示时按 `_trashId`（或 `deletedAt + 类型 + 键`）去重，不再出现两条一模一样的「ST·关键词」。
- 修复七麦空态文案仍提示「从数据管理导入 14 天历史 JSON」（导入功能已在 v2.3.24 移除）。

## v2.3.25

- 新增脏数据识别 `isInvalidStKeyword()`：关键词若是「↑ 118.2% / ↓ 34.8% / 694.7%」这类排名变动或纯百分比，一律判定为异常（箭头字符、`±数字%`、含 % 且无字母三种规则）；像 `2048` 这种纯数字关键词仍照常保留。
- ST 接收路径（`filterStRowsByKeywordWhitelist`）在写入前先拦异常关键词：命中即拒绝并写入拒绝日志（`reason:'invalid_keyword'`），与是否登记关键词清单无关，因此未登记清单的 App 也受保护。
- 新增「关键词清单护栏」面板按钮「清理异常关键词」：预检 → 确认 → 一键移除主表里所有异常关键词行（移入 ST 回收站，可恢复），正常关键词不受影响。
- 护栏面板状态里新增「异常关键词 N 条」计数与提示（列出前 6 个异常关键词），一眼能看出本地是否还残留旧版插件写入的脏数据。
- 新增全局 `ASO_PRUNE_ST_INVALID_KEYWORDS({app,batch,dryRun})` 与 `ASO_COUNT_ST_INVALID_KEYWORDS()`。
- 其余护栏、回收站与批量删除逻辑保持 v2.3.24 不变。

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
