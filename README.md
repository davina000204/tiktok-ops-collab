# TikTok 达人样品协同运营台

正式网址：

https://davina000204.github.io/tiktok-ops-collab/

## 架构

- 前端：GitHub Pages
- 登录：Supabase Auth 邮箱一次性登录链接
- 数据库：Supabase PostgreSQL
- 权限：Row Level Security + 成员角色
- 实时同步：Supabase Realtime
- 前端密钥：仅包含 publishable key，可公开
- 服务端密钥：不进入网页、不进入代码仓库

## 角色

| 角色 | 权限 |
|---|---|
| Owner | 全部权限，管理成员、审批申请、撤销编辑、移除访问 |
| Admin | 编辑业务数据、审批申请、管理非 Owner 成员 |
| Operator | 新增和编辑业务数据，不能删除和管成员 |
| Viewer | 只读查看和导出 |

## 新成员流程

1. 打开正式网址。
2. 输入工作邮箱并发送登录链接。
3. 在同一个浏览器点击邮件中的登录链接。
4. 登录后提交“协同办公申请”和申请说明。
5. 此时无法读取任何业务数据。
6. Owner 或 Admin 在“成员权限”页面审批。
7. 审批时分配角色。
8. 成员获得角色后才能进入业务数据区。

## Owner 撤销权限

进入“成员权限”：

- 修改角色为 `viewer`：立即撤销编辑权限，保留只读。
- 点击“撤销编辑”：立即改为只读。
- 点击“移除访问”：立即移除全部访问权限，成员需要重新申请。
- Owner 账号不能被 Admin 修改或删除。

## 文件

- `index.html`：分文件版页面
- `styles.css`：页面样式
- `app.js`：页面与云端逻辑
- `config.js`：Supabase 项目地址和 publishable key
- `seed.js`：产品与记录示例数据
- `schema.sql`：Supabase 表、RLS、函数、成员审批与撤销权限
- `deploy_storage.py`：静态文件上传脚本（不包含密钥）
- `dist/index.html`：GitHub Pages 使用的单文件版本
- `auto_ingest.py`：本地 OCR 自动导入脚本
- `live-sync-extension/`：Chrome 实时同步扩展源码模板（不含真实令牌）
- `schema_live_sync.sql`：实时同步 RPC 数据库结构
- `截图并同步.command`：一键框选截图并写入云端
- `启动自动同步.command`：监听 `inbox` 文件夹
- `build_dist.py`：重新生成单文件部署版
- `previews/`：登录页、Owner 总览、成员权限页面截图

- `品牌飞书表格/`：按品牌拆分的精简审核表（8 个商家/品牌、80 条历史记录，统一保留 11 列）
- `品牌飞书分表总览.md`：Vina 账号下 8 张飞书表链接、记录数与自动同步状态
- `品牌表格映射.csv`：品牌、飞书链接、活动 ID、商品 ID 与启用状态
- `品牌表格清单.csv`：品牌记录数、本地文件和在线链接清单

- `品牌产品映射.csv`：产品 ID 到商家/品牌的权威映射，用于自动分表

- 待审核样品和视频列表统一按申请日期倒序；同一天按 TikTok 页面最新抓取顺序排列
- 达人昵称旁显示 TikTok 头像；头像地址随实时同步数据保存，加载失败时显示达人首字母
- 同一个达人申请同一个商品 PID 时只保留一条申请，状态更靠后的记录优先并自动合并非空字段
- 待审核申请按申请时间从近到远排列；若页面只有日期，则使用首次捕获时间，同日再参考 TikTok 页面顺序。

## v30 工作流增强

- 今日待办中心：待审核、待 Code、Code 待发商家、异常四条主线
- 视频批量勾选：批量复制摘要、批量复制 Code 话术、批量标记已请求
- 同步健康面板：TikTok 快照、飞书云端同步、断线补查状态
- 飞书云端函数：Supabase `feishu-sync` 每分钟自动执行，不依赖主电脑运行
- 多视频完整保存
- 商家 Code 批量一键复制（达人、产品名称、商品 ID、视频链接、vid、Code）
- 今日运营简报一键复制
- 在线 SOP：[manual.html](./manual.html)

完整流程说明见 [`工作流程SOP与使用手册.md`](./工作流程SOP与使用手册.md)。

