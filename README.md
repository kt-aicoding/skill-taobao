# Taobao Shopping Skill

`taobao-shopping` is a Codex/CC skill for visible, low-frequency Taobao browser assistance. It is designed for supervised personal shopping flows such as logging in manually, searching for a product, selecting an approved SKU, and adding the item to cart without checkout.

This repository contains only the reusable skill files. It does not include cookies, browser profiles, screenshots, account data, shopping history, order data, addresses, phone numbers, payment data, or local session artifacts.

## Skill Name

Installable skill name:

```text
taobao-shopping
```

Repository name:

```text
skill-taobao
```

## Contents

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
└── scripts/
    └── pw-taobao.sh
```

## Requirements

- Node.js/npm with `npx` available on `PATH`
- Codex/CC skill runtime
- The companion Playwright skill installed at:

```text
$CODEX_HOME/skills/playwright
```

`CODEX_HOME` defaults to `~/.codex` when unset.

## Installation

Clone or copy this repository into your skills directory as `taobao-shopping`:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
git clone https://github.com/kt-aicoding/skill-taobao.git \
  "${CODEX_HOME:-$HOME/.codex}/skills/taobao-shopping"
```

Validate the skill if you have the system skill validator available:

```bash
python3 "${CODEX_HOME:-$HOME/.codex}/skills/.system/skill-creator/scripts/quick_validate.py" \
  "${CODEX_HOME:-$HOME/.codex}/skills/taobao-shopping"
```

## Usage

Ask Codex/CC to use the skill:

```text
Use $taobao-shopping to search Taobao for a product and add the approved item to cart.
```

The bundled wrapper keeps Taobao automation in a dedicated Playwright session:

```bash
export CODEX_HOME="${CODEX_HOME:-$HOME/.codex}"
export TBPW="$CODEX_HOME/skills/taobao-shopping/scripts/pw-taobao.sh"
"$TBPW" tab-list
```

## Safety Model

The skill is intentionally conservative:

- Uses a visible headed browser for login and sensitive account flows.
- Keeps a dedicated `taobao` Playwright session.
- Avoids clearing cookies, storage, or browser data.
- Avoids checkout, order submission, and payment controls.
- Stops for CAPTCHA, slider, SMS, QR, device verification, risk prompts, prescription review, real-name prompts, checkout, and payment.
- Avoids network body inspection and bulk scraping.
- Redacts or avoids account, address, phone, order, payment, balance, coupon, and logistics details.

## Low-Interruption Mode

The skill prefers element-level Playwright actions over OS-level mouse or keyboard automation. It does not intentionally move, resize, maximize, or foreground the browser window. If a site or browser action still steals focus, slow down and continue only when the user says it is acceptable.

## Regulated Goods

For prescription medicines and other regulated goods, the skill only matches product identity to user-provided prescription details such as name, brand, strength, dosage form, and quantity. It must not choose, change, or infer medical dosage, and it stops for prescription upload/review or pharmacist consultation.

## Disclaimer

This project is not affiliated with Taobao, Tmall, Alibaba, or Playwright. Use it only on accounts you own or are authorized to operate, and follow the relevant platform terms, laws, and medical/legal requirements.
