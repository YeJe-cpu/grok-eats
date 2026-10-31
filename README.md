<p align="right"><strong>中文</strong> · <a href="README.en.md">English</a></p>

# waimai-agent · 让 Agent 帮你点美团外卖

**一句话说“晚饭吃点高蛋白的”，Agent 读你最近的订单、挑好三个选项、勾上最大的红包，把订单推到收银台。你只需要在手机上付款。**

<p align="center">
  <img src="assets/12-dinner-three-candidates.png" width="49%" alt="Agent 根据近期订单给出三个晚饭候选">
  <img src="assets/13-dinner-quote-after-max-redpacket.png" width="49%" alt="勾上最大红包后报价 24.6 元">
</p>
<p align="center">
  <img src="assets/09-diet-brief-3days-kcal-range.png" width="49%" alt="近 3 天外卖热量简报，给区间不给假精确">
  <img src="assets/03-coffee-address-from-book-confirm.png" width="49%" alt="从地址簿切地址，确认后才提交">
</p>

<p align="center"><em>2026-10-31 实测：一杯 ¥14 的美式，一顿券后 ¥24.6 的晚饭。都是 Agent 提交、人在手机上付款。</em></p>

---

## 为什么要做这个

美团给个人开放的 AI 能力只有两样：**领券**和**跑腿**。外卖下单没有给个人用的接口。跑腿“帮买”能用，但要额外付跑腿费，也用不了外卖的满减和红包。

剩下的路是让 Agent 用浏览器去点 H5 网页版。真去做了才发现，坑全在细节里：

- 红包**不会自动勾**。不勾报 ¥41.6，勾上是 ¥24.6。
- 「再来一单」**会漏菜**。原单 5 样，进购物车只剩 2 样。
- 地址 Agent 会**自己编**。地址簿里没有“XX 健身房”，它想去地图上新建一个。
- 登录时可能**弹出身份核实**，要填实名证件上的生日。

这个仓库把这些坑写成一份 **Agent skill**：一套操作规程加四条硬规则，让 Agent 别走弯路，也别越界。

## 它能做什么

| 你说 | Agent 做 |
|---|---|
| “帮我登录美团” | 短信验证码登录；遇到身份核实交给你，不在聊天里要你的生日 |
| “晚饭吃什么？” | 读近 3～7 天订单，避开今天吃过的，给出 2～3 个「再来一单」候选（选择题） |
| “我最近吃得怎么样？” | 热量简报：每一顿给区间和依据档位，不编精确数字 |
| “就第二个” | 组车 → 勾最大红包 → 读回菜品、券、合计、地址 → 等你说“提交” |
| “提交” | 提交订单，停在收银台。你打开手机美团 →「待支付」付款 |
| 每天早上 | 配合美团官方领券 skill，自动领一次红包 |

## 四条硬规则

1. **不碰支付**：提交后就停，不输任何支付密码。
2. **不擅自提交**：读回确认页给你看，你明确说“提交”才点。
3. **不编地址**：只用地址簿里已有的地址。
4. **不明文要敏感信息**：证件信息走加密表单，没有加密表单就由你自己填。

## 怎么用

适用于**能操作浏览器**的 Agent（云电脑类 Bot、Claude Code + 浏览器、OpenClaw 等）。

1. 把这个仓库放进你的 Agent 的 skills 目录，或者直接把 `SKILL.md` 和 `references/` 发给它。
2. （可选）复制 `profile.example.md` 为 `profile.md`，写上预算、忌口和偏好；也可以让 Agent 第一次登录后帮你起草。
3. （推荐）安装美团官方的领券 skill：[美团红包助手 meituan-coupon](https://clawhub.ai/meituan-openplatform/skills/meituan-coupon)。
4. 对 Agent 说：“帮我登录美团外卖”。

```
waimai-agent/
├── SKILL.md              主流程 + 四条硬规则 + 检查清单
├── profile.example.md    个人外卖档案模板
└── references/
    ├── login.md          短信登录、身份核实
    ├── address.md        只用地址簿
    ├── recommend.md      读历史订单、优先「再来一单」
    ├── checkout.md       勾最大红包、补齐漏菜、提交即停
    ├── nutrition.md      热量估算分 A～E 五档
    ├── coupons.md        配合官方领券 skill
    └── routines.md       每日例行（可选，默认关闭）
```

## 实测过程

<table>
  <tr>
    <td width="33%"><img src="assets/01-login-sms-code-via-chat.png" alt="聊天里传手机号和验证码"><br><sub>① 聊天里发手机号和验证码，Agent 来填</sub></td>
    <td width="33%"><img src="assets/02-login-identity-check-secure-form.png" alt="身份核实走加密表单"><br><sub>② 弹出身份核实，走加密表单</sub></td>
    <td width="33%"><img src="assets/06-coupon-official-meituan-skill.png" alt="官方 skill 领券"><br><sub>③ 官方 skill 一次领 10 张券</sub></td>
  </tr>
  <tr>
    <td><img src="assets/08-order-history-recent.png" alt="读近期订单"><br><sub>④ 读近期订单</sub></td>
    <td><img src="assets/14-dinner-submit-stop-at-cashier.png" alt="提交后停在收银台"><br><sub>⑤ 说“提交”后提交，停在收银台</sub></td>
    <td><img src="assets/05-coffee-delivered.jpg" alt="咖啡送到"><br><sub>⑥ 咖啡送到</sub></td>
  </tr>
</table>

16 张实测截图都在 [`assets/`](assets/)，已经打码，文件名就是步骤说明。

## 边界与风险（请先读）

- 只适合**你自己的账号、低频使用**。不做多账号、不批量下单、不刷券。
- H5 自动化不是美团官方支持的用法。页面改版、风控升级都可能让它失效，账号也可能被要求额外验证。**请自行承担使用风险。**
- 目前的证据来自一个账号、一天、两单。登录能保持多久、多久会触发风控，都还没有数据。欢迎提 issue 补充你的情况。
- 热量估算只是参考，不是医学或营养建议。
- 本项目与美团无关，没有官方背书。

## 致谢

- 实测由 **Grok Bot** 在云电脑浏览器里完成，大部分坑是它一单一单踩出来的；**Claude Code** 负责把关、整理和脱敏。
- [美团开放平台](https://clawhub.ai/meituan-openplatform/skills/meituan-coupon) 的官方领券 skill：领券这件事，官方已经做得很好。
- [Sanotsu/china-food-composition-data](https://github.com/Sanotsu/china-food-composition-data)：中国食物成分表数据。

我不是外卖重度用户，但点外卖很挑：要高蛋白、要便宜、不想一项项去选。这个 skill 最初就是为我自己写的。如果它刚好也帮你省了几块钱、少点了几下屏幕，那就很好。觉得哪里不对，直接提 issue。

## License

[MIT](LICENSE)
