---
name: logo-conflict-check
display_name: Logo 撞标检查
display_name_en: Logo Conflict Check
description: 检查 Logo、图形标识或产品图案是否与已注册图形商标相似。用户提供图片公网链接并问"这个 logo 会不会撞标""这个图案能不能用"时使用：以图搜图找视觉相似的注册商标，结合权利人维权画像给出风险判断。
description_zh: 以图搜图查 Logo、图案是否撞已注册图形商标
description_en: Reverse image search for registered figurative marks similar to your logo
version: 1.0.0
author: One1
---

# Logo 撞标检查

用以图搜图找出和用户 Logo / 图案视觉相似的已注册图形商标，并判断风险。

## 什么时候用

- "这个 logo 会不会和别人的商标撞？"
- "我设计了一个图标准备印在产品上，能用吗？"
- "这个产品图案有没有侵权风险？"

## 需要从用户那里拿到

- **图片的公网 URL**（PNG / JPG）。用户只上传了本地图片、没有链接时，请用户提供一个公网可访问的图片地址（如图床、商品页图片链接）。
- **目标市场**：两位国家代码，默认 `US`
- **商品类目**（可选）：尼斯分类，如服装 `025`、电子 `009`

## 步骤

1. 调用 `coverage_info(country)`，确认该市场**图形库是否可搜**（`graphic_served`）。不可搜时如实告诉用户，不要改用其它市场的结果冒充。
2. 调用 `graphic_trademark_search(image_url=…, country=…, nice_class=…, limit=10)`。
3. 取视觉相似度最高、风险为「高」的结果里的权利人（最多 3 个），调用 `trademark_owner_profile(owner=…, country=…)`。
4. 汇总输出。

## 判读规则（必须遵守）

- 以 `risk_level` 和 `visual_similarity` 共同判断：相似度高且同类目、权利人有效（Live）的，列为重点。
- 权利人 `is_enforcer=true`（TRO / Schedule A 高频维权方）时重点提示，建议修改设计。
- "没找到相似图形"只代表在 One1 已收录的图形库里没找到，不代表可以注册或不会侵权。
- 不下法律结论。

## 输出格式

```
## 结论
一句话：风险 高 / 中 / 低。

## 最相似的已注册图形商标
| 商标名 | 相似度 | 风险 | 权利人 | 状态 |

## 需要特别注意（如有）
- 权利人 XX 是 TRO 高频维权方……

## 建议
例如：调整哪个元素（形状、构图、配色）以拉开差异。

---
以上结果仅供参考，不构成法律意见。数据由 Dataify 提供 · https://www.dataify.com/
```
