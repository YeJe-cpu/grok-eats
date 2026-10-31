<p align="right"><a href="README.md">中文</a> · <strong>English</strong></p>

# grok-eats · Let Grok Bot order Meituan takeout for you

**Tell Grok Bot “something high-protein for dinner”. It opens Meituan takeout in its cloud browser, reads your recent orders, picks three options, applies the biggest coupon, and takes the order to checkout. All you do is pay on your phone.**

<p align="center">
  <img src="assets/12-dinner-three-candidates.png" width="49%" alt="Grok Bot picks three dinner options from recent orders">
  <img src="assets/13-dinner-quote-after-max-redpacket.png" width="49%" alt="Quote after applying the biggest coupon: ¥24.6">
</p>

<p align="center"><em>Tested with Grok Bot on 2026-10-31: a ¥14 Americano and a ¥24.6 dinner (after coupons) on Meituan. Grok Bot submitted both orders; a person paid on the phone.</em></p>

> **Not just Grok Bot.** This is a skill for agents, so any agent that can drive a browser can use it, including personal agents like Muse, Claude Code with a browser, and OpenClaw. It has been tested on Meituan. Ele.me's H5 site should work the same way, but hasn't been tested yet.

---

## Why

Meituan, China's largest food-delivery platform, offers individual users only two official AI capabilities: **claiming coupons** and **errand running**. There is no consumer API for ordering takeout.

That leaves one option: let Grok Bot drive Meituan's mobile web site (H5) in its cloud browser. The hard part is in the details:

- Coupons are **not applied automatically**: ¥41.6 without them, ¥24.6 with them.
- “Order again” **drops items**: an order of 5 dishes came back as 2 in the cart.
- Agents **invent addresses** when the one you mention isn't in your address book.
- Login may trigger an **identity check** that asks for the birthday on your ID.

This repo turns what Grok Bot learned, order by order, into an **agent skill**: a step-by-step playbook plus four hard rules, so the agent doesn't waste time on detours or overstep.

## Four hard rules

1. **Never touch payment.** Stop at checkout. Never enter a payment password.
2. **Never submit without permission.** Read the order back, and submit only after the user explicitly says “submit”.
3. **Never invent addresses.** Use only addresses already saved in the address book.
4. **Never ask for sensitive data in plain text.** Use a secure form, or let the user type it in.

## Usage

**Grok Bot**: send it `SKILL.md` and `references/` (or add them to its skills), then say “log me in to Meituan takeout”.

**Other agents** (Muse, Claude Code + browser, OpenClaw…): put the files in the agent's skills folder and use it the same way.

- Optional: copy `profile.example.md` to `profile.md` and fill in your budget and preferences.
- Recommended: install Meituan's official coupon skill, [meituan-coupon](https://clawhub.ai/meituan-openplatform/skills/meituan-coupon).

The skill itself is written in Chinese, since the Meituan UI is in Chinese.

## Limits and risks

- Use it only on **your own account, at low frequency**. No multiple accounts, no bulk ordering, no coupon farming.
- Meituan does not officially support H5 automation. A page redesign or tighter risk controls may break it. **Use at your own risk.**
- So far the evidence is one account, one day, two orders.
- Calorie estimates are for reference only and are not medical advice.
- This project is not affiliated with or endorsed by xAI or Meituan.

## Credits

- Field-tested by **Grok Bot** in its cloud browser. **Claude Code** reviewed, organized and redacted the material.
- Meituan's official [coupon skill](https://clawhub.ai/meituan-openplatform/skills/meituan-coupon).
- [Sanotsu/china-food-composition-data](https://github.com/Sanotsu/china-food-composition-data) for Chinese food-composition data.

## License

[MIT](LICENSE)
