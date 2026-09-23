# 中国电商搜索（cn-ecommerce-search）

> 一个技能，八个平台。给运营决策提供**真实的市场价与竞品证据**，而不是拍脑袋。

基于 Shopme 统一商品库，在淘宝、天猫、京东、拼多多、1688、速卖通、抖音、小红书
8 个中国电商平台检索商品、按 URL / ID 取详情、解析商品链接。**零配置，不需要 API Key。**

- 技能名：`cn-ecommerce-search`
- 版本：v2.0.0-custom.2（本仓库为二次定制版：追加「运营总监」身份绑定 + 免费赞助）
- 原作者：shopme ｜ 许可：MIT
- 定制：沈艳朝 / Emperor（2026-09-23）
- 归属角色：**运营总监（沈运达 / `douyin-ecom-ops-team-lead`）**

> ⚠️ GitHub 仓库名不支持中文，「中国电商搜索」作为项目标题与描述保留，
> 仓库 slug 使用 `cn-ecommerce-search`（与技能 ID 一致）。

---

## 一、安装

### 方式 1：放到 WorkBuddy 技能目录（推荐）

```bash
git clone https://github.com/ITemperor/cn-ecommerce-search.git \
  "C:\Users\Administrator\.workbuddy\skills\cn-ecommerce-search"
```

放好后**重启 WorkBuddy** 即可在技能列表中看到。

### 方式 2：MCP Server 直连

技能底层是 MCP 服务，也可单独接入：

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

无需环境变量。可选：

| 变量 | 默认值 | 说明 |
|---|---|---|
| `SHOPME_API_BASE` | `https://api.shopmeagent.com` | 覆盖 API 端点（本地调试可指 `http://localhost:8000`） |

---

## 二、三个工具

| 工具 | 作用 | 关键参数 |
|---|---|---|
| `search_products` | 按关键词跨平台搜索 | `keyword`（必填）、`platform`、`sort_by`、`page`、`limit`(≤50) |
| `get_product_detail` | 按 ID 或 URL 取商品详情 | `product_id` 或 `url`（二选一）、`platform`（可选加速） |
| `parse_product_link` | 解析链接识别平台与商品 ID（本地执行，不调 API） | `url`（必填） |

`sort_by` 可选：`relevance` / `price_asc` / `price_desc` / `sales_desc` / `created_at`。

---

## 三、支持的 8 个平台

| 平台 | Code | 特点 | 价格带 | 典型买家 |
|---|---|---|---|---|
| 淘宝 | `taobao` | 品类最全 | ¥ 低-中 | 终端消费者 |
| 天猫 | `tmall` | 品牌旗舰，品质较高 | ¥ 中-高 | 品质导向 |
| 京东 | `jd` | 物流快，3C 家电强 | ¥ 中-高 | 品质 + 时效 |
| 拼多多 | `pdd` | 拼团低价 | ¥ 最低 | 价格敏感 |
| 1688 | `ali1688` | 工厂直供，批发价 | ¥ 最低（批量） | 商家 / 分销 |
| 速卖通 | `aliexpress` | 跨境，买家保障 | $ 中 | 海外买家 |
| 抖音 | `douyin` | 直播电商，爆款风向 | ¥ 低-中 | 追 trending |
| 小红书 | `xhs` | 社区种草，美妆生活方式 | ¥ 中 | 年轻女性 |

### 支持的 URL 格式

```
item.taobao.com/item.htm?id=123456
detail.tmall.com/item.htm?id=123456
detail.1688.com/offer/123456.html
item.jd.com/123456.html
mobile.yangkeduo.com/goods.html?goods_id=123456
aliexpress.com/item/123456.html
haohuo.jinritemai.com/...?id=123456
mall.xiaohongshu.com/goods-detail/xxx
短链：e.tb.cn/xxx 、 m.tb.cn/xxx
```

---

## 四、运营总监身份绑定（本仓库新增）

本技能已绑定至「电商运营专家团」的**运营总监**岗位，是主理人在选品、定价与
供应链决策时的**默认市场取证工具**。

### 什么时候用

| 场景 | 典型问法 | 产出 |
|---|---|---|
| 选品立项 | "这个品在淘宝/抖音卖多少钱" | 价格带 + 热销款 |
| 供应链摸底 | "找一下 1688 上的同源供应商" | 工厂价 → 毛利空间 |
| 竞品比价 | "对比小红书和抖音的同款" | 到手价 / 销量线索 / 卖点对比 |
| 货盘定价 | "该定什么价" | 给抖店运营的价格依据 |
| 素材取证 | "这个品真实卖点是什么" | 给策划 / 设计的价格锚点与卖点 |

### 调用纪律

- **只做取证与裁决**：不代替抖店运营出价格体系，不代替设计出视觉方案
- 结论必须标注**平台、币种（默认 CNY ¥）、是否含运费**
- 引用 1688 报价时必须注明是**工厂 / 批发价**（通常比淘宝同款低 30–70%）
- 输出**价格区间与毛利区间**，不给出单一数字
- 平台标识用英文 code（`taobao` / `tmall` / `jd` / `pdd` / `ali1688` / `aliexpress` / `douyin` / `xhs`）

### 在「全岗评估」中的位置

运营总监应**先**用本技能补齐《评估输入包》的**市场价与竞品信息**，
再把完整输入包下发给 9 名成员，避免成员在信息缺口上各自假设价格与卖点。

---

## 五、价格理解要点

- 除速卖通外，所有平台价格均为 **CNY（¥）**；速卖通在库中也存为 CNY（≈ 1 USD = 7.2 CNY）
- **1688 是工厂/批发价**，通常比淘宝同款低 30–70%，对比时必须换算到同一口径
- 比价时**务必考虑运费**，否则跨平台结论会失真

## 六、搜索技巧

1. 国内平台（淘宝 / 京东 / 拼多多 / 1688）用**中文关键词**结果更多
2. 英文关键词会自动做同义词与词形扩展
3. 用 `sales_desc` 排序找爆款（小红书上效果最好）
4. 用 `platform` 过滤锁定单一平台
5. `get_product_detail` 可直接吃 URL，无需先 `parse_product_link`

---

## 七、目录结构

```
cn-ecommerce-search/
├── README.md    使用文档（本文件）
├── SKILL.md     技能本体（含运营总监身份绑定章节）
├── icon.png     技能图标
├── assets/
│   └── sponsor-qr.png  免费赞助二维码
├── assets/
│   └── sponsor-qr.png  免费赞助二维码
├── assets/
│   └── sponsor-qr.png  免费赞助二维码
└── LICENSE      MIT
```

## 八、版本记录

| 版本 | 日期 | 变更 |
|---|---|---|
| v2.0.0 | — | 原版：8 平台搜索 / 详情 / 链接解析，免 API Key |
| v2.0.0-custom.1 | 2026-09-23 | 追加「归属角色：运营总监（沈运达）」章节：触发场景表、调用纪律、在全岗评估中的位置；同步写入运营总监角色定义与专家团名册 |
| v2.0.0-custom.2 | 2026-09-23 | README 底部新增「免费赞助」二维码（`assets/sponsor-qr.png`）；目录结构补 `assets/` |

---

## 九、免费赞助

这套东西是白送的：**不收费、不锁功能、不塞广告**。如果它帮你省了时间、或者多赚了钱，
可以扫码请 Emperor 喝杯茶 —— 完全自愿，不打赏也照样用、照样更新。

<p align="center">
  <img src="assets/sponsor-qr.png" alt="免费赞助 · Emperor、| 说事-不闲聊" width="280">
</p>

<p align="center"><sub>扫码可备注一句你在做什么类目，方便后续针对性更新</sub></p>
