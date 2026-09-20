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
| Apple/apns | 苹果系统推送域名及官方连接网段（精简合并版） | [qx/Apple/apns.list](qx/Apple/apns.list) | 不提供（仅 QX） |
| AppStore | App Store 搜索与目录主机 | [qx/AppStore.list](qx/AppStore.list) | 不提供（仅 QX） |
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

## App Store（仅 QX）

qx/AppStore.list 用于 apps.apple.com 子域及 iTunes 搜索/目录接口。需创建 AppStore 策略组，并使用 force-policy=AppStore、inserted-resource=true、opt-parser=false 引用，放在通用 Apple 直连资源之前。该规则不包含 APNs，不同步 Clash。

## APNs 苹果推送（仅 QX）

[规则直链](https://raw.githubusercontent.com/Jason3u/Proxy-Rules-Collection/main/qx/Apple/apns.list)。本次仅新增规则与说明，不自动修改 QX 主配置，不提供 Clash 版本。

合并来源：[ttyyss2233 的 Apns.module](https://raw.githubusercontent.com/ttyyss2233/Tool/main/shadowrocket/mokuai/Apns.module)、[blackmatrix7 的 Apple.list](https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Apple/Apple.list)，网段核对 [Apple 官方 APNs 文档](https://support.apple.com/en-us/102266)。共 15 条：1 条推送域名后缀、5 条明确的推送别名、5 条 IPv4、4 条 IPv6。已由 Shadowrocket 语法转换为 QX 原生语法，无需资源解析器。

与原模块不同：不收录整个 akadns.net、apple.com.edgekey.net，也不使用整个 17.0.0.0/8，避免过度覆盖其他服务。blackmatrix7 的 init-p01st.push.apple.com 和 init-s01st.push.apple.com 已被 push.apple.com 后缀覆盖，不重复列出。IP 规则匹配整个网段，不限制端口，不能保证网段内每条连接都只用于推送。

如需手动启用，先在 `[policy]` 中建立 APNs 策略组；下例要求已有“台湾节点”组，可替换成已有的稳定节点/策略组：

```ini
static=APNs, 台湾节点, direct
```

在 `[filter_remote]` 中添加以下内容，并置于通用 Apple 规则之前；同时检查是否有更高优先级的本地规则或排除路由：

```ini
https://raw.githubusercontent.com/Jason3u/Proxy-Rules-Collection/main/qx/Apple/apns.list, tag=APNs, force-policy=APNs, inserted-resource=true, opt-parser=false, update-interval=86400, enabled=true
```

这只是分流规则，不会开启 iOS 的系统推送隧道接管；仅对进入 QX 的连接生效，不能保证修复 Telegram 通知。APNs 是多个应用共用的系统推送通道，不是 Telegram 专属。不要对 APNs 进行 MitM 解密。启用后应检查实际连接是否命中 APNs，再做锁屏收通知测试；可手动切换 direct 对照。未在 iPhone 上进行运行验证。
