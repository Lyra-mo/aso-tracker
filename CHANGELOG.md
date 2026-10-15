# Changelog

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
