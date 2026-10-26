# 挺会过日子 · 优惠券 MCP

让 AI 帮你找外卖、打车、酒店、咖啡等日常消费优惠，返回可领取的推广链接或完整口令。

**直接连接现成的远程服务，无需下载代码、安装 Node.js 或自行部署。**

- **复制接入提示词：** [www.thgrz.cn/connect](https://www.thgrz.cn/connect)
- **MCP 服务地址：** `https://www.thgrz.cn/mcp`
- **协议：** Streamable HTTP
- **认证：** 无需登录，无需 API Key
- **优惠网站：** [www.thgrz.cn](https://www.thgrz.cn)

## 把下面这段话发给你的 AI

```text
请帮我接入「挺会过日子」优惠券 MCP。

服务名：tinghui-coupons
服务地址：https://www.thgrz.cn/mcp
协议：MCP Streamable HTTP
无需登录，无需 API Key。

请先识别你正在运行的 AI 客户端，按该客户端的官方方式配置远程 MCP。
保留其他已有配置；如果你无法自动配置，请告诉我最少必要的操作步骤。
配置完成后，实际调用 list_coupon_categories，再调用 search_coupons，
参数为 {"query":"打车","limit":3}，验证连接成功。
只有实际工具调用成功，才能报告已接入；如果需要新会话加载工具，请明确告知。

返回优惠时，原样保留推广链接的全部参数、口令大小写和符号，并说明领取步骤。
不要替换推广入口，不要声称已自动领取到账户。

同一 IP 每连续 60 秒最多 10 次 MCP POST 请求，初始化、通知和工具调用均计入。
每日约 90000 次动态请求预算用完后暂停，按北京时间每天上午 8 点恢复。
收到 429 时按 Retry-After 等待；如果是 DAILY_BUDGET_EXHAUSTED，停止重试，
告诉我仍可直接打开 https://www.thgrz.cn 查看和领取优惠。
```

也可以让 AI 读取[最新完整接入说明](https://www.thgrz.cn/connect.txt)。普通聊天工具如果不支持 MCP 或没有配置权限，不能仅靠提示词自动增加工具。

## 接入后怎么问

- “帮我找一下美团外卖优惠。”
- “有没有打车券？”
- “肯德基和麦当劳有什么优惠？”
- “帮我找一下咖啡优惠。”

AI 负责查询优惠；用户点击链接或复制口令，按平台页面提示自行领取。服务不登录外卖账户、不代领、不下单、不付款。

## 提供的工具

| 工具 | 作用 | 常用参数 |
| --- | --- | --- |
| `search_coupons` | 按关键词、平台、分类查找优惠 | `query`、`platform`、`category`、`limit`、`offset` |
| `get_coupon` | 获取某条优惠的完整领取入口与说明 | `id` |
| `list_coupon_categories` | 列出当前分类和平台 | 无 |

搜索建议使用简短关键词，例如“外卖”“打车”“咖啡”。多个词用空格分隔时，需要全部匹配。单次最多返回 30 条，默认 10 条。

## 访问限制

- 每个 IP 每连续 60 秒最多 10 次 MCP POST 请求，多个工具共享额度。
- 公司网络、代理、云端 AI 服务等可能共用出口 IP 和额度。
- 超限返回 HTTP `429`，请按响应头 `Retry-After` 等待，不要立即循环重试。
- 全站动态接口共用每日约 9 万次保守预算，可能提前暂停；北京时间每天上午 8 点恢复。
- 暂停 AI 接口时，仍可直接访问优惠网站。

## 给 AI 客户端或工具目录

- 机器可读连接信息：[connection-info](https://www.thgrz.cn/connection-info)
- 接入提示词纯文本：[connect.txt](https://www.thgrz.cn/connect.txt)
- [HTTP 配置示例](./mcp-config.json)

配置示例适用于接受 `mcpServers` 和 `type: http` 格式的客户端；不同客户端的设置格式可能不同。MCP 地址供客户端调用，直接用浏览器打开不是普通网页。

## 关于优惠

优惠信息由站点维护者手动更新，具体优惠、领取资格、有效期和费用以平台活动页面为准。部分优惠为付费券包或会员商品。

返回的是推广入口，符合平台归因规则的订单可能为本站带来 CPS 佣金。使用服务无需提供平台密码、短信验证码或 Cookie。

本仓库是远程 MCP 服务的公开接入入口和使用说明，服务运行于上述网站。发布到 GitHub 不代表已经被任何 MCP 市场、目录或 AI 客户端自动收录。

关键词：优惠券 / 外卖 / 打车 / 咖啡 / MCP / coupons / deals / Streamable HTTP
