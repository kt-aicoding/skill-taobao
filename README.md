# Taobao Shopping Skill

`taobao-shopping` 是一个面向 Codex/CC 的淘宝可见浏览器自动化 skill，用于低频、人工监督的个人购物辅助流程：手动登录、搜索商品、选择已确认 SKU、加入购物车，但不下单、不付款。

## 一句话安装

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills" && git clone https://github.com/kt-aicoding/skill-taobao.git "${CODEX_HOME:-$HOME/.codex}/skills/taobao-shopping"
```

## Skill 名称

- 安装后的 skill 名称：`taobao-shopping`
- GitHub 仓库名：`skill-taobao`
- 默认 Playwright 会话名：`taobao`

## 适用场景

- 打开淘宝可见浏览器会话。
- 引导用户手动完成登录、扫码、短信或滑块验证。
- 搜索商品并整理少量候选。
- 打开用户确认的商品页。
- 选择明确 SKU、数量 1，并加入购物车。
- 在用户要求时检查是否已加购。

## 不做什么

- 不自动下单、提交订单或付款。
- 不绕过验证码、滑块、短信、二维码、设备验证或风控。
- 不批量抓取、翻页扫货、刷接口或高频操作。
- 不清除 cookie、localStorage、sessionStorage 或浏览器数据。
- 不读取、保存或输出密码、短信码、cookie、token、地址、手机号、订单、支付、余额、优惠券、物流等隐私信息。

## 文件结构

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
└── scripts/
    └── pw-taobao.sh
```

## 依赖

- Node.js/npm，并确保 `npx` 可用。
- Codex/CC skill runtime。
- Playwright skill 已安装到：

```text
$CODEX_HOME/skills/playwright
```

未设置 `CODEX_HOME` 时，默认使用 `~/.codex`。

## 验证安装

如果本机有系统 skill 校验脚本，可以执行：

```bash
python3 "${CODEX_HOME:-$HOME/.codex}/skills/.system/skill-creator/scripts/quick_validate.py" \
  "${CODEX_HOME:-$HOME/.codex}/skills/taobao-shopping"
```

预期输出：

```text
Skill is valid!
```

## 使用方式

在 Codex/CC 中直接点名使用：

```text
Use $taobao-shopping to search Taobao for a product and add the approved item to cart.
```

也可以手动调用封装脚本确认会话：

```bash
export CODEX_HOME="${CODEX_HOME:-$HOME/.codex}"
export TBPW="$CODEX_HOME/skills/taobao-shopping/scripts/pw-taobao.sh"
"$TBPW" tab-list
```

## 安全设计

这个 skill 默认保守执行：

- 使用可见 headed 浏览器处理登录和敏感账户页面。
- 固定使用专用 `taobao` Playwright 会话，避免误操作其他页面。
- 已登录后优先页内搜索和导航，减少直接跳转导致的登录态重新校验。
- 每次关键操作之间保留等待时间，降低风控风险。
- 遇到验证码、滑块、短信、二维码、设备验证、实名、处方审核、药师咨询、结算或支付提示时立即停手。
- 只汇报任务相关商品信息，不复述隐私账户信息。

## 低打扰模式

skill 优先使用 Playwright 的元素级操作，例如 `click`、`fill`、`press`、`tab-select` 和 `snapshot`，避免系统级鼠标键盘自动化。默认不主动移动、缩放、最大化、置顶或关闭浏览器窗口。

如果浏览器或网站行为仍然抢占焦点，应放慢操作，并在用户确认可继续后再执行。更稳妥的做法是把淘宝窗口放到单独的桌面/Space。

## 处方药和受监管商品

处方药、医疗器械等受监管商品只能按用户提供的处方或明确规格匹配商品名、品牌、规格、剂型和数量。skill 不应选择、推断或改变医疗剂量。遇到处方上传、处方审核、实名验证、药师咨询、结算或付款提示时必须停止。

## 数据脱敏说明

本仓库只包含可复用的 skill 文件，不包含：

- 浏览器 profile、cookie、token 或登录态。
- 截图、Playwright 快照、console 日志或下载文件。
- 账号、地址、手机号、订单、支付、余额、优惠券或物流信息。
- 个人购物记录或会话记录。

## 免责声明

本项目与淘宝、天猫、阿里巴巴或 Playwright 官方无关联。请只在你拥有或被授权操作的账号上使用，并遵守平台规则、适用法律以及医疗/处方药相关要求。
