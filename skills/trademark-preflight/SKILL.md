---
name: trademark-preflight
display_name: 上架前商标风险自查
display_name_en: Pre-listing Trademark Check
description: 跨境卖家上架前的商标风险自查。用户问"这个品牌名能不能用""标题有没有侵权""上架前帮我查一下"时使用：依次查数据覆盖度、品牌名撞标、标题侵权词、相关权利人是否为 TRO 高频维权方，输出带结论和建议的自查报告。
description_zh: 上架前一次查清品牌名撞标、标题侵权词和 TRO 维权方风险
description_en: One-stop pre-listing check of brand name conflicts, infringing title words and TRO enforcers
version: 1.0.0
author: One1
---

# 上架前商标风险自查

帮跨境卖家在上架前一次性查清：品牌名能不能用、商品标题有没有侵权词、涉及的品牌是不是爱起诉的维权方。

## 什么时候用

- "我想用 XX 当品牌名，能不能用？"
- "帮我看看这个亚马逊/Temu/TikTok 商品标题有没有侵权风险"
- "我要在美国和欧洲卖 XX，上架前帮我查一下"

## 需要从用户那里拿到

- **品牌名**（必需，除非只查标题）
- **商品标题或文案**（可选）
- **目标市场**：两位国家代码，默认 `US`；用户说"欧洲"时问清具体国家，或逐个查
- **商品类目**（可选）：能推断出尼斯分类就带上，例如服装鞋帽 `025`、电子产品 `009`、玩具 `028`、家居 `021`

## 步骤

1. **先查覆盖度**：对每个目标市场调用 `coverage_info(country)`。记下 `scope` 和 `data_as_of`。
2. **品牌名撞标**：对每个目标市场调用 `trademark_text_search(query=品牌名, country=…, nice_class=…, limit=10)`。
3. **标题侵权词**（用户给了标题时）：调用 `listing_scan(text=标题, country=…, nice_class=…)`。
4. **维权方画像**：从第 2、3 步的高风险结果里取出权利人（最多 3 个），调用 `trademark_owner_profile(owner=…, country=…)`。
5. 汇总输出报告。

## 判读规则（必须遵守）

- `trademark_text_search` 返回的 `count` 是**最近邻条数，恒等于 limit，不是冲突数**。判断有没有冲突只看 `has_conflicts` 和 `conflict_count`（风险为「高」的条数）。
- 某个市场 `coverage_info` 的 `clearance_ok` 为 false（即 `scope` 不是 `full`），或 `data_as_of` 明显过旧时，这个市场**"没查到冲突"不代表可以注册**，报告里必须明确写出这一点，建议委托当地专业机构全量检索。
- `trademark_owner_profile` 返回 `is_enforcer=true`（TRO / Schedule A 高频维权方）时，放在报告最前面重点提示，建议规避。
- 不要替用户下"可以注册"或"一定侵权"的法律结论。

## 输出格式

```
## 结论
一句话：整体风险 高 / 中 / 低，最该先处理的一件事。

## 品牌名「XX」
| 市场 | 数据覆盖 | 高风险冲突 | 最相近的已注册商标（权利人 · 状态） |

## 标题风险词（如有）
- 高风险：词 → 对应商标 · 权利人（TRO 维权方则标注）
- 中风险：…

## 需要特别注意的权利人（如有）
- XX：TRO 高频维权方，建议规避

## 建议
2~4 条可执行的改法，例如换名方向、删改标题里的哪个词。

---
以上结果仅供参考，不构成法律意见。数据由 Dataify 提供 · https://www.dataify.com/
```
