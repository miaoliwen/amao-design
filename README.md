# amao-design — Warm Paper Design System (WPDS)

一个用户级 WorkBuddy skill，封装了 amao 产品界面所采用的 **暖纸设计系统（Warm Paper Design System, WPDS）**。当你为 amao 构建、美化或审查任何前端 UI 时，本 skill 会自动套用统一的视觉语言，保证产出与既定规范一致。

---

## 何时会触发

满足以下任一场景时，WorkBuddy 会自动加载本 skill：

- 为 amao 构建 / 样式化 / 审查任何前端页面、组件、原型或设计令牌输出
- 生成 amao 的 Tailwind 配置、全局 CSS 或设计令牌
- 提到「暖纸风、暖沙色、珊瑚橙强调色、玻璃面板、bento 卡片、极光氛围光、纸张噪点、浮动字母」等关键词

---

## 目录结构

```
amao-design/
├── SKILL.md                 # 入口：触发说明 + 核心强制规则（精炼）
├── references/
│   └── wpds-spec.md         # 完整设计规范（全部 token 表、组件、动画、断点、a11y、反模式…）
└── README.md                # 本文件
```

> `wpds-spec.md` 是按需加载的权威参考。需要某个具体数值（精确 hex、圆角、阴影、字号、断点、组件 CSS）时再读取，避免常驻占用上下文。

---

## 设计理念

温暖、专注、如纸。介于「纸质教科书」与「现代数字工具」之间的氛围。

- **暖**：以暖沙底色替代冷白，降低长时间使用的视觉疲劳。
- **静**：克制用色。珊瑚橙是全系统唯一活跃强调色，其余皆不用于装饰。
- **纸感**：圆润面板、柔和阴影、低对比分隔线，模拟纸张与卡片的实体质感。
- **暗色 = 重新打光**：暗色模式不是简单反色，而是在暖棕底色上做柔光映射，全程保留暖调。

---

## 核心强制规则（摘要）

1. **唯一强调色** — 珊瑚橙（`coral-500 #f97316` 主 / `coral-600 #ea580c` 悬停）。禁止引入新强调色；青色 `#06b6d4` 仅用于极光氛围光与渐变文字。
2. **只用暖色板** — 所有中性色用 `warm-*`；`text-neutral-*` 仅为兼容性别名，新代码一律 `warm-*`。**禁止**纯白 `#ffffff` / 纯黑 `#000000` 作背景。
3. **暖色阴影** — 始终 `rgba(42,36,28, …)` 暖黑，不用 Tailwind 默认的冷调 `shadow-*`。
4. **禁止 scoped / CSS-in-JS** — 所有样式走全局 CSS + Tailwind 工具类。
5. **暗色模式为重新打光** — 通过 `<html class="dark">` 切换；映射 `warm-50↔warm-950`、`warm-900↔warm-50`、强调色 `coral-500→coral-400`；暗色样式紧跟在亮色后，用 `html.dark .class` 书写。
6. **字体** — Geist 主体 + Geist Mono 等宽；注意自定义字重映射（勿直接依赖 Tailwind 默认 `font-medium`/`font-semibold` 语义）。
7. **动画** — 全系统单一缓动 `cubic-bezier(0.16, 1, 0.3, 1)`。
8. **可访问性** — skip-link、图标按钮 `aria-label`、日志区 `role="log"` + `aria-live="polite"`、装饰元素 `aria-hidden`、双模式满足 WCAG-AA 对比度、尊重 `prefers-reduced-motion`。

完整规则、token 表与组件 CSS 见 [`references/wpds-spec.md`](references/wpds-spec.md)。

---

## 如何使用

### 让 WorkBuddy 直接产出
只需在对话中描述需求，例如：
> “用 amao 设计系统给我做一个设置页”
> “把这个卡片改成 WPDS 风格的玻璃面板”
> “给这个按钮加 hover 和 pressed 态”

### 在工程中落地令牌
参考配套的 `tailwind.config.js`（项目集成示例），把 `warm-*` / `coral-*` 色板、暖色阴影、`darkMode: 'class'`、spring 缓动与极光动画写入工程，再从全局 CSS / 工具类引用。

### 快速校验清单
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
5. 使用冷调默认 `shadow-*`。
6. 用纯白 `#ffffff` / 纯黑 `#000000` 作背景。
7. 装饰性大段文字带语义或缺少 `aria-hidden`。

---

## 相关文件
- [`SKILL.md`](SKILL.md) — 触发与强制规则
- [`references/wpds-spec.md`](references/wpds-spec.md) — 完整设计规范
