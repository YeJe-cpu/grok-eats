<p align="right"><a href="README.md">中文</a> · <strong>English</strong></p>

# waimai-agent · Let your agent order Meituan takeout

**Say “something high-protein for dinner”. The agent reads your recent orders, picks three options, applies the biggest coupon, and pushes the order to the checkout page. You just pay on your phone.**

<p align="center">
  <img src="assets/12-dinner-three-candidates.png" width="49%" alt="Three dinner options picked from recent orders">
  <img src="assets/13-dinner-quote-after-max-redpacket.png" width="49%" alt="Quote after applying the biggest coupon: ¥24.6">
</p>

<p align="center"><em>Tested on 2026-10-31: a ¥14 Americano and a ¥24.6 dinner (after coupons). The agent submitted both orders; a human paid on the phone.</em></p>

---

## Why

Meituan (China's largest food-delivery platform) only gives individual users two official AI capabilities: **claiming coupons** and **errand running**. There is no consumer-facing API for ordering takeout.

That leaves one option: a browser agent driving the mobile web version (H5). The hard part is the details:

- Coupons are **not applied automatically**: ¥41.6 without them, ¥24.6 with them.
- “Order again” **drops items**: an order of 5 dishes came back as 2 in the cart.
- Agents **invent addresses** when the one you mention isn't in your address book.
- Login may trigger an **identity check** that asks for the birthday on your ID.

This repo turns those lessons into an **agent skill**: a step-by-step playbook plus four hard rules, so the agent doesn't wander and doesn't overstep.

## Four hard rules

1. **Never touch payment.** Stop at the checkout page; never enter a payment password.
2. **Never submit without an explicit “submit”** from the user, after reading back the order.
3. **Never invent addresses.** Only use saved addresses.
4. **Never ask for sensitive data in plain text.** Use a secure form, or let the user type it.

## Usage

For agents that can drive a browser (cloud-desktop bots, Claude Code + browser, OpenClaw, etc.).

1. Drop this repo into your agent's skills folder, or hand it `SKILL.md` and `references/`.
2. Optional: copy `profile.example.md` to `profile.md` with your budget and preferences.
3. Recommended: install Meituan's official coupon skill, [meituan-coupon](https://clawhub.ai/meituan-openplatform/skills/meituan-coupon).
4. Tell your agent: “log me in to Meituan takeout”.

The skill itself is written in Chinese, because the Meituan UI is in Chinese.

## Limits and risks

- Use it only on **your own account, at low frequency**. No multi-account use, bulk ordering, or coupon farming.
- H5 automation is not an officially supported use. Page redesigns or stricter risk controls may break it. **Use at your own risk.**
- So far the evidence is one account, one day, two orders.
- Calorie estimates are for reference only, not medical advice.
- This project is not affiliated with or endorsed by Meituan.

## Credits

- Field-tested by **Grok Bot** in a cloud-desktop browser; reviewed, organized and redacted with **Claude Code**.
- Meituan's official [coupon skill](https://clawhub.ai/meituan-openplatform/skills/meituan-coupon).
- [Sanotsu/china-food-composition-data](https://github.com/Sanotsu/china-food-composition-data) for Chinese food-composition data.

## License

[MIT](LICENSE)
