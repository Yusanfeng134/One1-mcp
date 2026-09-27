<p align="center">
  <img src="assets/icon.png" width="96" alt="One1">
</p>

<h1 align="center">One1 Trademark Risk Check — MCP Server</h1>

<p align="center">
  Pre-listing trademark checks for cross-border e-commerce sellers, right inside your AI assistant.<br>
  Data provided by <a href="https://www.dataify.com/">Dataify</a>.
</p>

<p align="center">
  <a href="https://registry.modelcontextprotocol.io/v0.1/servers/cn.one1%2Ftrademark/versions/latest">Official MCP Registry: <code>cn.one1/trademark</code></a> ·
  <a href="https://one1.cn">one1.cn</a> ·
  <a href="#中文说明">中文说明</a>
</p>

---

Ask your AI assistant things like:

- *"Is the brand name **Lumora** already registered in the US?"*
- *"Does this Amazon title contain infringing trademarks: Wireless Earbuds Compatible with AirPods"*
- *"Is this logo similar to any registered trademark?"* (image URL)
- *"Does **YETI** frequently file TRO / Schedule A lawsuits against sellers?"*
- *"I got an infringement complaint citing US serial **78414956** — who is it?"*

One1 searches tens of millions of trademark records across dozens of jurisdictions — roughly 32.8M text marks and 57.5M figurative marks — plus a curated list of frequent TRO / Schedule A enforcers.

## Connect

This is a **hosted remote server**. There is nothing to install.

| | |
| --- | --- |
| Endpoint | `https://one1.cn/mcp` |
| Transport | Streamable HTTP |
| Auth | **OAuth 2.1** (auto-discovered, dynamic client registration + PKCE) — or an API key |

`initialize` and `tools/list` work without credentials. Calling a tool requires authorization; compliant clients open the One1 sign-in page automatically.

### Any MCP client (OAuth)

```json
{
  "mcpServers": {
    "one1": {
      "type": "streamable-http",
      "url": "https://one1.cn/mcp"
    }
  }
}
```

### With an API key instead

Create a key at [one1.cn/developers](https://one1.cn/developers/), then:

```json
{
  "mcpServers": {
    "one1": {
      "type": "streamable-http",
      "url": "https://one1.cn/mcp",
      "headers": { "X-Api-Key": "YOUR_ONE1_API_KEY" }
    }
  }
}
```

### Claude Code

```bash
claude mcp add --transport http one1 https://one1.cn/mcp
```

More client examples: [`examples/`](examples/).

## Tools

| Tool | What it does |
| --- | --- |
| `trademark_text_search` | Brand name / word vs. registered trademarks (phonetic, visual and semantic similarity) |
| `listing_scan` | Finds trademark-infringing words in a product title or listing copy, grouped by risk |
| `graphic_trademark_search` | Reverse image search for similar registered figurative marks (public image URL) |
| `trademark_lookup_by_serial` | Trademark details, legal status and risk by serial number |
| `trademark_owner_search` | Trademarks held by an owner / applicant |
| `trademark_owner_profile` | Whether an owner is a frequent TRO / Schedule A enforcer |
| `coverage_info` | Data coverage and freshness per jurisdiction (free) |

All tools accept `country` (ISO 3166-1 alpha-2, default `US`).

**Reading results correctly**

- `trademark_text_search.count` is the number of nearest neighbours returned (always equals `limit`), **not** the number of conflicts. Use `has_conflicts` / `conflict_count`.
- Call `coverage_info` first. Where `clearance_ok` is `false`, "no match" does **not** mean a mark is registrable.
- Results are for reference only and do not constitute legal advice.

## Skills

[`skills/`](skills/) contains ready-made workflows (Agent Skills format) that tell an assistant how to chain the tools:

| Skill | Use it when |
| --- | --- |
| [`trademark-preflight`](skills/trademark-preflight/SKILL.md) | Checking a brand name and listing title before going live |
| [`logo-conflict-check`](skills/logo-conflict-check/SKILL.md) | Checking whether a logo looks like a registered mark |
| [`tro-risk-check`](skills/tro-risk-check/SKILL.md) | Handling an infringement complaint or a TRO worry |

## Pricing

Usage is metered in API points on your One1 account. The free plan includes a monthly allowance; `coverage_info` is always free. See [one1.cn](https://one1.cn).

## Links

- Website: <https://one1.cn>
- Developer docs & API keys: <https://one1.cn/developers/>
- Data: [Dataify](https://www.dataify.com/)
- Privacy: <https://one1.cn/privacy/> · Terms: <https://one1.cn/terms/>

---

## 中文说明

**One1 商标风险检测 MCP 服务**：跨境卖家上架前，在 AI 助手里一句话查品牌名、Logo、商品标题有没有撞已注册商标，以及对方是不是 TRO / Schedule A 高频维权方。数据由 [Dataify](https://www.dataify.com/) 提供。

- **地址**：`https://one1.cn/mcp`（Streamable HTTP，托管服务，无需安装）
- **授权**：OAuth 2.1，客户端会自动弹出 One1 登录授权；也可以在 [one1.cn/developers](https://one1.cn/developers/) 创建 API Key，放在请求头 `X-Api-Key`
- **计费**：按 One1 账户的 API 点计费，免费档每月赠送额度，查询数据覆盖度免费
- **说明**：结果仅供参考，不构成法律意见；数据覆盖不完整的国家，"查不到"不代表可以注册

可以这样问：

- 帮我查 Lumora 这个品牌名在美国有没有被注册
- 看看这个亚马逊标题有没有侵权词：……
- 这个 logo 会不会和已注册商标撞：（图片链接）
- YETI 是不是经常发 TRO 起诉卖家
- 收到侵权投诉，商标号 78414956，帮我查一下对方
