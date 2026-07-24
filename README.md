# ASO 数据中心 v2.0.0

## 上传到 GitHub

将本目录中的三个文件上传到 `feature/ios-dashboard-v1` 分支根目录，并覆盖同名文件：

- `index.html`
- `favicon-32x32.png`
- `favicon-48x48.png`

不要删除原仓库的 README。先在测试分支部署验证，再合并到 `main`。

## 数据兼容

- Sensor Tower 继续使用 `aso_master_data`，原历史数据不会因页面升级而迁移或清空。
- ST 备注继续使用 `aso_notes_v110`。
- iOS 七麦使用新键 `aso_ios_qimai_snapshots_v200`。
- 同一天、同 App、同批次、同国家、同关键词、同类别会去重，并合并来源词。
- 不同日期不会互相覆盖，用于形成 14 天关键词轨迹。

## 七麦数据导入

当前版本支持直接导入插件导出的：

`七麦_14天历史_YYYY-MM-DD.json`

进入“数据管理” → “导入七麦 14 天历史”。

同时预留 URL 自动接收格式：

- `?qimai=<URL编码后的JSON>`
- `?source=qimai_ios&data=<URL编码后的JSON>`

后续插件同步版可直接使用。
