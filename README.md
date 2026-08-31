# Quantumult-X-Rules

Quantumult X 自建分流规则，供个人 QX 配置的 `[filter_remote]` 引用。

主要用于补充 blackmatrix7 等规则库暂未收录、覆盖不完整，或需要单独策略组控制的平台规则。

## 规则列表

| 文件 | 说明 | 策略组 |
|---|---|---|
| `X.list` | X（Twitter）分流，基于 blackmatrix7 的 Twitter.list 调整并移除 grok.com | X平台 |
| `Binance.list` | Binance 币安分流，参考 blackmatrix7 并补充官方域名、备用域名及生态域名 | Binance |
| `OKX.list` | OKX 欧易分流，补充 blackmatrix7 缺失的 CDN / 链上相关域名 | OKX |
| `Bybit.list` | Bybit 交易所分流，包含全球站、备用站及 API 相关域名 | Bybit |
| `Bitget.list` | Bitget 交易所分流，包含海外站、中文区及备用域名 | Bitget |
| `Gate.list` | Gate 交易所分流，补充官方及备用域名 | Gate |
| `Cornix.list` | Cornix 自动交易平台分流，覆盖官网、Web Dashboard、API 及帮助中心 | Cornix |

## 引用地址（jsDelivr CDN）

```text
https://cdn.jsdelivr.net/gh/Jason3u/Quantumult-X-Rules@main/X.list
https://cdn.jsdelivr.net/gh/Jason3u/Quantumult-X-Rules@main/Binance.list
https://cdn.jsdelivr.net/gh/Jason3u/Quantumult-X-Rules@main/OKX.list
https://cdn.jsdelivr.net/gh/Jason3u/Quantumult-X-Rules@main/Bybit.list
https://cdn.jsdelivr.net/gh/Jason3u/Quantumult-X-Rules@main/Bitget.list
https://cdn.jsdelivr.net/gh/Jason3u/Quantumult-X-Rules@main/Gate.list
https://cdn.jsdelivr.net/gh/Jason3u/Quantumult-X-Rules@main/Cornix.list