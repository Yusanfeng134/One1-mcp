---
name: tro-risk-check
display_name: 被投诉 / TRO 风险排查
display_name_en: Complaint & TRO Risk Check
description: 排查被投诉或被 TRO / Schedule A 起诉的风险。用户收到侵权投诉、商标号，或问"XX 品牌会不会告我"时使用：按商标号或品牌查清对方商标详情，判断对方是否为高频维权方，给出应对方向。
description_zh: 按商标号或品牌查对方商标，判断是否 TRO 高频维权方
description_en: Look up the complainant's trademark and check whether they are a frequent TRO enforcer
version: 1.0.0
author: One1
---

# 被投诉 / TRO 风险排查

帮卖家弄清楚：投诉自己的是谁、对方的商标是什么、对方是不是经常批量起诉卖家（TRO / Schedule A）。

## 什么时候用

- "我收到亚马逊侵权投诉，里面有个商标号 XXXX，怎么回事？"
- "XX 品牌是不是经常告卖家？我卖的东西和它沾边"
- "听说有品牌在批量冻结店铺，我会不会被波及？"

## 需要从用户那里拿到

- **商标号**（投诉通知里通常有），或**品牌 / 权利人名称**
- **市场**：两位国家代码，默认 `US`
- 用户自己的**品牌名或商品标题**（可选，用来比对）

## 步骤

1. 有商标号：调用 `trademark_lookup_by_serial(serial=…, country=…)`，拿到商标名、权利人、法律状态。
2. 对权利人调用 `trademark_owner_profile(owner=…, country=…)`：是否为高频维权方、名下有效商标数量、主要类目。
3. 需要看对方商标布局时，调用 `trademark_owner_search(owner=…, live_only=true, country=…)`。
4. 用户给了自己的标题时，调用 `listing_scan(text=…, country=…)`，找出和对方商标相关的词。
5. 汇总输出。

## 判读规则（必须遵守）

- 商标状态为无效（Dead）时要明确说明，这可能是申诉的依据，但让用户咨询律师确认。
- `is_enforcer=true` 表示命中维权方名录：对方常用 Schedule A 批量起诉并申请 TRO 冻结账户，提示用户尽快处理、保留证据、咨询律师。
- 不承诺"一定能申诉成功"或"不会被冻结"，不提供法律意见。

## 输出格式

```
## 结论
一句话：对方是谁、风险多高、最该先做什么。

## 对方商标
商标名 · 注册号 · 权利人 · 状态 · 类目

## 对方是不是高频维权方
是 / 否，依据……

## 你的商品里相关的词（如有）
- 词 → 对应对方的哪个商标

## 建议的应对方向
1. 例如：立即下架或修改含风险词的标题
2. 保留进货凭证、销售记录
3. 涉及 TRO 起诉或冻结账户时，尽快咨询专业律师

---
以上结果仅供参考，不构成法律意见。数据由 Dataify 提供 · https://www.dataify.com/
```
