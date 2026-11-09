# AI线路助手

配合 **Surge** 或 **Clash Verge Rev** 使用的 macOS 小工具：

- **线路评测**：读取你客户端里已加载的节点，逐个检测 ChatGPT / Codex / OpenAI API 连通性、ProxyCheck IP 风险分、延迟和短测速，按「GPT 分高 → IP 风险低 → 速度快 → 延迟低」排序，一键选用。评测只读，不会替你切换线路。
- **Claude 守卫**：一键写入带标记的 Claude 防护规则——「Claude 专用」组固定出口（切换主线路不影响 Claude）、Claude 域名 / 付款页 / 本机 Claude 程序走专用组、拦截 STUN（WebRTC）与 Claude 的 UDP/QUIC；之后持续核对防护状态、出口 IP、国家与风险，变化时发系统通知。
- **LuLu 程序级拦截（可选）**：安装包内附 LuLu 4.5.6 官方原版镜像；已安装 LuLu 时，可经 LuLu 官方窗口补齐 Claude 组件的拒绝规则。

## 系统要求

- Apple 芯片 Mac，macOS 14 或以上
- Surge Mac 或 Clash Verge Rev（二选一，需已导入你自己的订阅）
- 可选：LuLu 4.5.5 / 4.5.6

## 下载与安装

1. 在 [Releases](../../releases) 下载最新的 `AI-Route-Helper-<版本>-AppleSilicon-macOS14.zip`。
2. 解压，把 `AI线路助手.app` 拖进「应用程序」。
3. 双击打开。程序已用 Developer ID 签名并经 Apple 公证，不需要右键打开或改安全设置。

可选：核对下载文件完整性（结果应与 Release 页给出的 SHA-256 一致）：

```bash
shasum -a 256 ~/Downloads/AI-Route-Helper-*-AppleSilicon-macOS14.zip
```

自带独立 Python / PyYAML / mihomo，不需要安装 Xcode、Homebrew 或 Python。

## 首次使用

1. 打开后按向导选择客户端（Surge 或 Clash Verge Rev），读取策略组并选主组。保存连接不会切换线路。
2. 点「一键检测全部」查看排序；右键节点可单独检测或测速。
3. 需要 Claude 防护时进入守卫页，按提示写入防护规则：
   - **Surge**：写入当前配置文件里两个带标记的区块，写入前用 `surge-cli --check` 校验并备份原配置，失败自动恢复。托管配置（managed profile）不支持写入。
   - **Clash Verge Rev**：写入「全局扩展脚本」，写入后需在 Clash Verge 里刷新订阅或重启才生效；TUN 模式和关闭 IPv6 需在 Clash Verge 中手动设置。
4. 只有使用「LuLu 自动补规则」时才需要授予辅助功能权限；通知权限用于防护变化提醒。

## 隐私

- 不收集、不上传任何数据；没有账号、统计或遥测。
- 评测与出口监测只访问固定地址：`chatgpt.com`、`codex-cloud-backend.chatgpt.com`、`api.openai.com`（未登录，预期返回 401）、`api.ipify.org`、`proxycheck.io`、`speed.cloudflare.com`。不连接 Claude，不使用 Cookie 或 API 密钥。
- 节点凭据只在本机内存和短期私有临时配置中使用；配置备份只保存在本机（目录 0700、文件 0600）。

## 限制

- 评测只覆盖可安全复制的 TCP 节点；代理链、外部提供器、UDP 类协议、关闭证书校验的节点会跳过并说明原因。
- GPT 分是未登录端点的连通比例，不代表能登录或生成；IP 风险分来自 ProxyCheck，不是账号安全评分。
- 守卫显示的是本机配置与规则匹配的核验结果，未做连接 Claude 的实测，不能据此声称「零 IP 泄露」。浏览器网页版、共享 Node CLI、IPv6/WebRTC 等仍需你自行确认。

## 第三方组件

| 组件 | 许可 | 说明 |
|---|---|---|
| [mihomo](https://github.com/MetaCubeX/mihomo) v1.19.33 | GPL-3.0 | 独立子进程，仅用于隔离评测；同版本源码随包提供 |
| [LuLu](https://github.com/objective-see/LuLu) 4.5.6 | GPL-3.0 | 可选组件，官方原版签名镜像，不静默安装；同版本源码随包提供 |
| [python-build-standalone](https://github.com/astral-sh/python-build-standalone) 3.12 | PSF | 内置运行时 |
| [PyYAML](https://github.com/yaml/pyyaml) 6.0.3 | MIT | |

许可全文与源码位于 `AI线路助手.app/Contents/Resources/licenses/`（右键 App →「显示包内容」）。

## 签名

Developer ID Application: Zhongfei Liu (X8M9HQLYKS)，已经 Apple 公证。

本仓库只用于发布安装包和说明。
