# ASO 数据中心 v2.2.0 Preview

## 新增页面

- iOS 点点
  - 相关词＋排名变动
  - 相关词＋新进榜
  - 同一关键词跨日期合并
  - 来源词、最新日期、最新排名变动、指数、排名与历史追踪
- 交叉验证
  - 七麦与点点是否同时发现同一关键词
  - 两边排名方向是否一致
  - 七麦/点点排名、变动和指数并列展示
  - 不对两个平台的指数或排名做平均

## 数据存储

- 七麦：`aso_ios_qimai_snapshots_v200`
- 点点：`aso_ios_diandian_snapshots_v220`
- ST：继续使用 `aso_master_data`

三类数据分开保存。点点插件同步来源标识为 `diandian_ios`。

## 部署

将根目录中的 `index.html`、`favicon-32x32.png`、`favicon-48x48.png` 上传到 GitHub，最终合并到 Render 绑定的 `main` 分支。
