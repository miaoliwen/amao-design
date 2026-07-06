# Warm Paper Design System (WPDS) — Agent Guide

> 🌐 语言 / Language: [中文](#) · [English](README.en.md)

一份**与具体助手无关**的设计系统规范，任何 AI 编码 agent（Claude、GPT、Copilot、WorkBuddy 等）都可以读取并据此产出视觉一致的 UI。本目录包含规范本体；当 agent 为该产品的界面做构建、样式化或审查时，应加载并遵循它。

本指南也可直接作为 **system prompt / 设计约束** 粘贴给任意 agent 使用。

---

## 这是什么

- 一套独立的设计系统规范（不绑定任何单一助手或框架）。
- 两个核心文件：本指南 + [`references/wpds-spec.md`](references/wpds-spec.md)（完整的 token 表、组件、动画、断点、a11y、反模式）。
- 目标是把「暖纸风」视觉语言变成可被 agent 重复执行的确定性指令，而非靠每次口头描述。

---

## 目录结构

```
wpds/
├── SKILL.md                 # （可选）WorkBuddy 等平台的触发元数据 + 强制规则摘要
├── references/
│   └── wpds-spec.md         # 完整设计规范（权威参考，按需读取）
└── README.md                # 本通用 agent 指南
```

> `wpds-spec.md` 是权威参考。需要某个具体数值（精确 hex、圆角、阴影、字号、断点、组件 CSS）时再读取，避免常驻占用上下文。

---

## 设计理念

温暖、专注、如纸。介于「纸质教科书」与「现代数字工具」之间的氛围。

- **暖**：以暖沙底色替代冷白，降低长时间使用的视觉疲劳。
- **静**：克制用色。珊瑚橙是全系统唯一活跃强调色，其余皆不用于装饰。
- **纸感**：圆润面板、柔和阴影、低对比分隔线，模拟纸张与卡片的实体质感。
- **暗色 = 重新打光**：暗色模式不是简单反色，而是在暖棕底色上做柔光映射，全程保留暖调。

---

## 何时应用

满足以下任一场景时，agent 应加载并套用本规范：

- 构建 / 样式化 / 审查该产品的任何前端页面、组件、原型或设计令牌输出
- 生成 Tailwind 配置、全局 CSS 或设计令牌
- 用户提到「暖纸风、暖沙色、珊瑚橙强调色、玻璃面板、bento 卡片、极光氛围光、纸张噪点、浮动字母」等关键词

---

## Agent 执行步骤

1. **读规范**：遇到上述场景时，先读 [`references/wpds-spec.md`](references/wpds-spec.md) 拿到精确 token 值。
2. **套规则**：下面「强制规则」中的每一条都必须满足，不得违反。
3. **产出**：生成的 HTML / CSS / 组件 / 配置全部使用 `warm-*` 令牌、单一珊瑚橙强调色、暖色阴影、暗色重新打光。
4. **自检**：交付前用文末「校验清单」逐条核对。

---

## 强制规则（必须严格遵守）

1. **唯一强调色** — 珊瑚橙（`coral-500 #f97316` 主 / `coral-600 #ea580c` 悬停）。禁止引入新强调色；青色 `#06b6d4` 仅用于极光氛围光与渐变文字。
2. **只用暖色板** — 所有中性色用 `warm-*`；`text-neutral-*` 仅为兼容性别名，新代码一律 `warm-*`。**禁止**纯白 `#ffffff` / 纯黑 `#000000` 作背景。
3. **暖色阴影** — 始终 `rgba(42,36,28, …)` 暖黑，不用冷调默认阴影。
4. **禁止 scoped / CSS-in-JS** — 所有样式走全局 CSS + 工具类（如 Tailwind）。
5. **暗色模式为重新打光** — 通过 `<html class="dark">` 切换；映射 `warm-50↔warm-950`、`warm-900↔warm-50`、强调色 `coral-500→coral-400`；暗色样式紧跟在亮色后，用 `html.dark .class` 书写。
6. **字体** — Geist 主体 + Geist Mono 等宽；注意自定义字重映射（勿直接依赖框架默认 `medium`/`semibold` 语义）。
7. **动画** — 全系统单一缓动 `cubic-bezier(0.16, 1, 0.3, 1)`。
8. **可访问性** — skip-link、图标按钮 `aria-label`、日志区 `role="log"` + `aria-live="polite"`、装饰元素 `aria-hidden`、双模式满足 WCAG-AA 对比度、尊重 `prefers-reduced-motion`。

完整规则、token 表与组件 CSS 见 [`references/wpds-spec.md`](references/wpds-spec.md)。

---

## 可直接粘贴的 System Prompt 片段

> 你正在为该产品构建前端 UI。请严格遵循 Warm Paper Design System (WPDS)：
> - 底色使用暖沙色板 `warm-*`，禁止纯白/纯黑背景；
> - 唯一强调色为珊瑚橙 `#f97316`（悬停 `#ea580c`），不得引入其他强调色；
> - 阴影一律用暖黑 `rgba(42,36,28,…)`；
> - 样式写在全局 CSS / 工具类中，禁止组件级 scoped 与 CSS-in-JS；
> - 暗色模式用 `<html class="dark">` 重新打光，而非反色；
> - 动画统一缓动 `cubic-bezier(0.16, 1, 0.3, 1)`；
> - 满足 WCAG-AA 对比度，装饰元素加 `aria-hidden`；
> - 需要精确数值时查阅 `references/wpds-spec.md`。

---

## 在工程中落地令牌

把 `warm-*` / `coral-*` 色板、暖色阴影、`darkMode: 'class'`、spring 缓动与极光动画写入工程的 Tailwind 配置 / 设计令牌文件，再从全局 CSS / 工具类引用。具体 token 值见 [`references/wpds-spec.md`](references/wpds-spec.md)。

---

## 校验清单（交付前自检）

- [ ] 背景是 `warm-50` / `warm-950`，无纯白纯黑
- [ ] 强调色只有珊瑚橙
- [ ] 阴影带暖黑 `rgba(42,36,28,…)`
- [ ] 暗色用 `html.dark` 重新打光，而非反色
- [ ] 动画缓动为 `cubic-bezier(0.16, 1, 0.3, 1)`
- [ ] 装饰元素已 `aria-hidden`，交互元素有 `aria-label`

---

## 反模式（明确禁止）

1. 引入新的强调色。
2. 新代码使用 `text-neutral-*`（仅兼容别名）。
3. 组件级 scoped 样式。
4. 引入新的 CSS-in-JS 方案。
5. 使用冷调默认阴影。
6. 用纯白 `#ffffff` / 纯黑 `#000000` 作背景。
7. 装饰性大段文字带语义或缺少 `aria-hidden`。

---

## 相关文件

- [`references/wpds-spec.md`](references/wpds-spec.md) — 完整设计规范（权威参考）
- `SKILL.md` —（可选）各平台的触发元数据，可忽略或按平台适配
