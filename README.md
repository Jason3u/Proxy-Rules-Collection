# Proxy Rules Collection

个人代理分流规则合集，同时提供 **Quantumult X（QX）** 和 **Clash / Mihomo（Android、Windows、macOS）** 格式。

## 目录结构

```text
.
├── qx/       # Quantumult X filter_remote 格式（.list）
└── clash/    # Clash / Mihomo rule-provider 格式（.yaml）
```

除标注「仅 QX」的规则外，两种格式内容对应，区别只在客户端要求的语法和文件结构。不要把 `qx/` 文件直接当作 Clash 规则集导入，也不要把 `clash/` YAML 直接填入 QX 的 `filter_remote`。

## 规则列表

| 规则 | 覆盖内容 | QX 文件 | Clash 文件 |
| --- | --- | --- | --- |
| APNs | 苹果推送代理测试，完整转换指定 Shadowrocket 模块 | [`qx/APNs.list`](qx/APNs.list) | 不提供（仅 QX） |
| X | X / Twitter 及相关域名与 IP | [`qx/X.list`](qx/X.list) | [`clash/X.yaml`](clash/X.yaml) |
| Binance | 币安及生态域名 | [`qx/Binance.list`](qx/Binance.list) | [`clash/Binance.yaml`](clash/Binance.yaml) |
| OKX | OKX、OKEX、OKLink 及 CDN | [`qx/OKX.list`](qx/OKX.list) | [`clash/OKX.yaml`](clash/OKX.yaml) |
| Bybit | Bybit 全球站、备用站及 API | [`qx/Bybit.list`](qx/Bybit.list) | [`clash/Bybit.yaml`](clash/Bybit.yaml) |
| Bitget | Bitget 海外站、中文区及备用域名 | [`qx/Bitget.list`](qx/Bitget.list) | [`clash/Bitget.yaml`](clash/Bitget.yaml) |
| Gate | Gate.io、Gate.com 及备用域名 | [`qx/Gate.list`](qx/Gate.list) | [`clash/Gate.yaml`](clash/Gate.yaml) |
| Telegram | 官网/分享链接/Telegraph/贴纸与媒体 CDN 及官方 DC IP 段（App 直连 IP 不查 DNS，IP 规则是接管核心流量的唯一手段） | [`qx/Telegram.list`](qx/Telegram.list) | [`clash/Telegram.yaml`](clash/Telegram.yaml) |
| Cornix | 官网、Dashboard、API、WebSocket 及第三方运行时依赖（intercom/country.is/mixpanel/hotjar/sentry/whop 等） | [`qx/Cornix.list`](qx/Cornix.list) | [`clash/Cornix.yaml`](clash/Cornix.yaml) |
| Fomo | 官方网站、应用及永续合约相关域名 | [`qx/Fomo.list`](qx/Fomo.list) | [`clash/Fomo.yaml`](clash/Fomo.yaml) |
| TradingView | 官网、中文站、图表数据与 API | [`qx/TradingView.list`](qx/TradingView.list) | [`clash/TradingView.yaml`](clash/TradingView.yaml) |

## Quantumult X 使用方法

在 QX 配置的 `[filter_remote]` 段引用 `qx/` 下的文件。策略组名称应与 QX 配置中的名称一致。

```ini
https://cdn.jsdelivr.net/gh/Jason3u/Proxy-Rules-Collection@main/qx/X.list, tag=X 规则, force-policy=X, enabled=true
https://cdn.jsdelivr.net/gh/Jason3u/Proxy-Rules-Collection@main/qx/Binance.list, tag=Binance 规则, force-policy=Binance, enabled=true
```

也可以把 `https://raw.githubusercontent.com/Jason3u/Proxy-Rules-Collection/main/qx/` 替换为 CDN 前缀。

## Clash / Mihomo 使用方法

Clash Android（如 Mihomo、Clash Meta for Android、部分 Clash Verge 衍生客户端）通常支持 YAML rule-provider。将 `clash/` 下的 URL 添加到配置的 `rule-providers`，再在 `rules` 中用 `RULE-SET` 引用：

```yaml
rule-providers:
  x:
    type: http
    behavior: classical
    format: yaml
    url: https://cdn.jsdelivr.net/gh/Jason3u/Proxy-Rules-Collection@main/clash/X.yaml
    path: ./ruleset/x.yaml
    interval: 86400

rules:
  - RULE-SET,x,你的代理策略组
```

其他规则只需替换名称和 URL，例如 `binance`、`okx`、`bybit`、`bitget`、`gate`、`cornix`。规则文件使用 `payload` 字段和 Mihomo 的 `DOMAIN-SUFFIX`、`DOMAIN-KEYWORD`、`IP-CIDR` 语法。

> 如果 Android 客户端不接受 `format: yaml` 或 `behavior: classical`，请升级到支持 Mihomo / Clash Meta 内核的版本；不同客户端的配置界面名称可能不同。

## 规则优先级

将 `RULE-SET` 放在通用兜底规则（例如 `MATCH`）之前。如果同一域名还命中了更具体的规则，应把更具体的规则放在前面。

## 更新与缓存

规则更新后，客户端需要手动更新远程规则或等待 `interval` 到期。jsDelivr 可能存在短暂缓存；需要立即验证时可改用 GitHub Raw 地址。

## 安全提示

- 规则仓库不包含订阅链接、账号密码或访问 token。
- 请不要把真实订阅地址提交到公开仓库。
- 修改规则后先在客户端校验语法，再启用配置。

## 免责声明

规则仅用于个人网络分流和测试。请遵守所在地区法律法规、服务条款及目标平台的使用政策。

## APNs 苹果推送测试（仅 QX）

来源：[ttyyss2233/Tool 的 Apns.module](https://raw.githubusercontent.com/ttyyss2233/Tool/main/shadowrocket/mokuai/Apns.module)。保留全部 12 条规则，转换为 QX 原生分流语法，无需资源解析器，不提供 Clash 副本。

在 `[policy]` 中添加：

```ini
static=APNs固定节点, resource-tag-regex=.+, server-tag-regex=.+
static=APNs, APNs固定节点, direct
```

将下列引用放在 `[filter_remote]` 最前面，先于 Apple 直连规则：

```ini
https://raw.githubusercontent.com/Jason3u/Proxy-Rules-Collection/main/qx/APNs.list, tag=APNs, force-policy=APNs, inserted-resource=true, opt-parser=false, update-interval=3600, enabled=true
```

在 APNs固定节点 中选择一个稳定的具体节点。此配置只是路由规则，不能开启 iOS 的 APNs 隧道接管；生效与否需在设备网络活动中验证。APNs 是多个应用共享的系统推送通道，不是 Telegram 专属通道。请勿对 APNs 做 MitM 解密。原模块的 akadns.net 和 apple.com.edgekey.net 匹配较宽，可能影响非推送流量，保留它们是为了复现原教程。可切换 APNs → direct 做对照测试。
