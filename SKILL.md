---
name: cn-ecommerce-search
description: >
  Search products across 8 Chinese e-commerce platforms: Taobao, Tmall, JD, PDD, 1688, AliExpress, Douyin, XHS. Zero-config — no API keys needed. Powered by Shopme unified product database. Use when the user asks to find products, get product info by URL or ID, search Chinese suppliers, or compare prices.

license: MIT
metadata:
  author: shopme
  version: "2.0.0"
  mcp-server: "@shopmeagent/cn-ecommerce-search-mcp"
version: 2.0.0
display_name: "中国电商搜索"
display_name_en: "Cn Ecommerce Search"
description_zh: "8 大中国电商平台商品搜索与跨平台商品信息查询，免 API Key"
description_en: "Search products across 8 Chinese e-commerce platforms — zero-config"
visibility: "public"
icon: "https://codebuddy-platform-1258344699.cos.accelerate.myqcloud.com/public/45edac6b-2078-4678-89f3-6f9800cf5e5f/avatar/skill/au_f2e4539e-205.png"
---

# Chinese E-commerce Product Search

Search and retrieve product information across 8 Chinese e-commerce platforms via the Shopme unified product database.

**Zero-config — no API keys required.**

## When to Use

- User asks to find a product on any Chinese e-commerce platform
- User shares a product link and wants details
- User needs to search Chinese suppliers for a product
- User asks about prices on Chinese platforms
- User provides a product URL from Taobao, JD, PDD, 1688, etc.
- User wants to compare products across platforms

## 归属角色：运营总监（沈运达 / douyin-ecom-ops-team-lead）

本技能已绑定至「电商运营专家团」的**运营总监**岗位（Agent ID：`douyin-ecom-ops-team-lead`），
是主理人在选品、定价与供应链决策时的**默认市场取证工具**。

### 运营总监的触发场景

| 场景 | 典型问法 | 用途 |
|------|----------|------|
| 选品立项 | "这个品在淘宝/抖音卖多少钱" | 新赛道开推前摸底价格带与热销款 |
| 供应链摸底 | "找一下 1688 上的同源供应商" | 反查工厂价，判断毛利空间 |
| 竞品比价 | "对比小红书和抖音的同款" | 到手价、销量线索、卖点横向对比 |
| 货盘定价 | "该定什么价" | 为抖店运营的价格体系提供市场依据 |
| 素材取证 | "这个品真实卖点是什么" | 给视频策划 / 设计提供价格锚点与卖点 |

### 调用纪律

- 运营总监**只做取证与裁决**——不得代替抖店运营出价格体系、不得代替设计出视觉方案
- 比价结论必须标注**平台、币种（默认 CNY）、是否含运费**
- 引用 1688 价格时必须注明是**工厂/批发价**，通常比淘宝同款低 30–70%
- 输出「建议零售价区间 / 毛利区间」的区间值，而非单一数字
- 平台 code 用表格里的英文标识（如 `ali1688`、`douyin`、`xhs`）

### 团队协作中的位置

在 Workflow E（全岗评估）里，运营总监应**先**用本技能补齐《评估输入包》中的
**市场价与竞品信息**，再把完整输入包下发给 9 名成员，
避免成员在信息缺口上各自假设价格与卖点。

## MCP Server Setup

```json
{
  "mcpServers": {
    "cn-ecommerce-search": {
      "command": "npx",
      "args": ["-y", "@shopmeagent/cn-ecommerce-search-mcp"]
    }
  }
}
```

No environment variables required. Optional:

| Variable | Default | Description |
|----------|---------|-------------|
| `SHOPME_API_BASE` | `https://api.shopmeagent.com` | Override API endpoint (e.g. `http://localhost:8000` for local dev) |

## Available Tools

### search_products

Search products by keyword across platforms.

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `keyword` | string | **Yes** | — | Search term (Chinese or English) |
| `platform` | string | No | all | Filter by platform (see table below) |
| `sort_by` | string | No | `relevance` | `relevance`, `price_asc`, `price_desc`, `sales_desc`, `created_at` |
| `page` | number | No | 1 | Page number |
| `limit` | number | No | 10 | Items per page (max 50) |

### get_product_detail

Get detailed info about a specific product by ID or URL.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `product_id` | string | One of two | Product ID (recommended, faster) |
| `url` | string | One of two | Product URL |
| `platform` | string | No | Platform hint to speed up lookup |

### parse_product_link

Parse a product URL to identify the platform and product ID. Runs locally, no API call.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `url` | string | **Yes** | Product URL or text containing one |

## Supported Platforms

| Platform | Code | Strengths | Price Range | Typical Buyer |
|----------|------|-----------|-------------|---------------|
| **淘宝 Taobao** | `taobao` | Largest selection, consumer goods | ¥ Low-Mid | End consumers |
| **天猫 Tmall** | `tmall` | Brand flagship stores, higher quality | ¥ Mid-High | Quality-focused |
| **京东 JD** | `jd` | Fast logistics, electronics, appliances | ¥ Mid-High | Quality + speed |
| **拼多多 PDD** | `pdd` | Group-buy deals, lowest prices | ¥ Lowest | Price-sensitive |
| **1688** | `ali1688` | Wholesale/factory direct, bulk pricing | ¥ Lowest (bulk) | Resellers, businesses |
| **速卖通 AliExpress** | `aliexpress` | International shipping, buyer protection | $ Mid | International buyers |
| **抖音 Douyin** | `douyin` | Live-commerce, trending products | ¥ Low-Mid | Trend followers |
| **小红书 XHS** | `xhs` | Community picks, beauty/lifestyle | ¥ Mid | Young women, lifestyle |

## Supported URL Formats

- `item.taobao.com/item.htm?id=123456`
- `detail.tmall.com/item.htm?id=123456`
- `detail.1688.com/offer/123456.html`
- `item.jd.com/123456.html`
- `mobile.yangkeduo.com/goods.html?goods_id=123456`
- `aliexpress.com/item/123456.html`
- `haohuo.jinritemai.com/...?id=123456`
- `mall.xiaohongshu.com/goods-detail/xxx`
- Short links: `e.tb.cn/xxx`, `m.tb.cn/xxx`

## Price Understanding Guide

- All platforms except AliExpress return prices in **CNY (¥)**. Rough conversion: 1 USD ≈ 7.2 CNY
- AliExpress prices in the database are also stored as CNY
- 1688 prices are factory/wholesale, often 30-70% lower than Taobao for the same product
- Always consider shipping costs when comparing prices

## Search Tips

1. **Chinese keywords** get more results on domestic platforms (Taobao, JD, PDD, 1688)
2. **English keywords** are auto-expanded with synonyms and word variants
3. Sort by `sales_desc` to find popular/trusted products (best on XHS)
4. Use `platform` filter to narrow results to a specific platform
5. Use `get_product_detail` with a URL directly — no need to parse first
