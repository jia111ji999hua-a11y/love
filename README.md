# 二手车库存 + 以租代购履约管理（uni-app + 微信云开发）

## 功能
- 用户端：在售车辆列表、搜索、详情（多图/车况/价格/分期方案）、手机号留资、隐私协议页。
- 管理端：管理员登录、车辆新增/上下架/删除、客户合同录入、履约状态、还款台账查询入口、CSV 导出（Excel 可直接打开）。
- 云数据库分为 `vehicles`、`contracts`、`repayments`、`leads` 四类集合；车辆、合同、还款通过 `vehicleId`/`contractId` 关联。

## 本地部署
1. 安装 HBuilderX（建议最新版）和微信开发者工具，导入本项目。
2. 在微信公众平台创建小程序，复制 AppID 到 `manifest.json` 的 `mp-weixin.appid`。
3. 在微信开发者工具开通云开发，创建环境，把 `App.vue` 的 `env` 替换为环境 ID。
4. 创建数据库集合：`vehicles`、`contracts`、`repayments`、`leads`、`config`。将 `database.rules.json` 中规则按控制台格式配置；在 `config/admin` 建文档 `{ "openids": ["管理员微信 openid"] }`。
5. 右键 `cloudfunctions/api` 上传并部署云函数。在云函数环境变量设置 `ADMIN_OPENIDS`（逗号分隔）和 `ADMIN_CODE`（管理员登录口令），不要写入代码或提交仓库。
6. 首次运行：用管理员微信登录后台，录入车辆。车辆图片可先在云存储上传后，将 fileID 数组写入 `images`；生产版建议增加云存储上传组件。
7. 微信开发者工具执行“预览/上传”，按审核要求填写隐私保护指引和用户隐私协议。

## 数据字段建议
- `vehicles`: brand, series, year, mileage, color, condition, images[], price, plans[], status(on/off), createdAt。
- `contracts`: vehicleId, customerName, phone, idNo, signDate, contractNo, term, deposit, monthlyPayment, status。
- `repayments`: contractId, period, dueDate, dueAmount, paidDate, paidAmount。
- `leads`: vehicleId, name, phone, createdAt。

## 安全与审核
管理员权限必须通过云端 openid 规则校验；客户端不保存管理员密钥。分期页面使用“月供/期数/保证金”事实描述，合同以双方签署内容为准，避免承诺固定收益或夸大利率。身份证号等敏感信息仅向授权管理员展示并按法规保存。隐私协议页面需在微信后台同步配置；上线前补充客服、注销/删除数据渠道及真实主体信息。

## 当前实现边界
后台已提供核心录入和状态管理骨架；还款明细录入、图片上传 UI、精细筛选可在此基础上继续扩展，数据结构已预留。
