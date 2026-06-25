---
name: deeplink大屏-style-guide
purpose: DeepLink 三块算力大屏（屏1 全国算力一张网 / 屏2 算力操作区 / 屏3 机房监控区）视觉与动效的唯一规范基准。任何 AI 编程助手或人类工程师可凭此文件单独复原全部设计风格，不依赖其他文档。
audience: AI 编程助手 + 人类工程师 + 视觉走查 + 外部合作方
version: 2.0
base-color: #0c0e13 ~ #14161b（允许深 tint 区间）
canvas: 1920×1080 设计稿 · vh-clamp 响应式 · 16:9 stage 守护
sources:
  - deeplink大屏-wt/屏1/算力大屏-屏1-全国算力一张网.html  (screen1 worktree · 最新深耕)
  - deeplink大屏-wt/屏2/算力大屏-屏2-算力操作区.html      (screen2 worktree · 最新深耕)
  - deeplink大屏-wt/屏3/算力大屏-屏3-机房监控区.html      (screen3 worktree · 最新深耕)
---

> **使用契约（Usage Contract）**
>
> 1. **复制优于推导** — 直接复制本规范中的 CSS / HTML 片段，不要根据色板与字号自由重组组件
> 2. **模式优于原语** — 原语（color / space / radius / type）只是数值，真正决定风格的是它们的组合规则；本规范将组合规则同样固化为代码
> 3. **未登记即不允许** — 遇到本规范未覆盖的组件，优先用现有模式拼装；新规则必须先在 §5 / §6 登记后再投入使用
> 4. **物理大屏部署专用** — 本规范服务于 -1L 展厅 3 块 1920×1080 物理大屏，**单一深色主题**，无主题切换、无键盘输入、无小屏断点（仅保留 1920 → 980 → 720 三级降级断点应对预览场景）
> 5. **印刷质感优先** — 即使在深色域，仍保留"低饱和、温度优先、印刷质感"立场。**禁止**滑向赛博朋克 / 霓虹发光 / 高饱和荧光的"科技感大屏"美学

---

## 目录

- §0 设计立场（7 条）
- §1 原语层 — 颜色 · 字体 · 字号 · 间距 · 圆角 · 阴影 · 时序
- §2 排版系统 — Logo · Eyebrow · 标题 · 数值 · Body
- §3 画布与响应式（vh-clamp + 16:9 stage 守护）
- §4 三屏布局骨架
- §5 通用组件解剖
- §6 大屏特有模式（10 类）
- §7 动效目录（按类别）
- §8 反模式登记表
- §9 一致性验收 Checklist
- §10 AI 协作契约
- §11 与 v1.0 的差异 · Changelog

---

## 0. 设计立场（Design Stance）

### 0.1 场域是深色印刷面板，不是发光显示器
主背景永远在 `#0c0e13` ~ `#14161b` 区间（冷中性暖偏深色，**非纯黑** `#000`，**非冷灰** `#1a1a1a`），主色 `--vermilion-6` `#7aa5db` 仅用于关键操作、活跃节点与视觉重音。**温度优于通量** —— 信息密度可以大，但不能发光。

### 0.2 几何承诺 · 2px 圆角是唯一锐角
`border-radius: 2px` 是几乎全部容器与控件的圆角值；**已登记例外仅三处**（见 §1.6），**禁止再创造新例外**。

### 0.3 字体即语义角色（Type-as-Semantics）
| 字体 | 语义角色 |
|---|---|
| Cormorant Garamond Italic | **品牌印记 · 仅 Logo 占用** |
| Noto Serif SC 600 | 中文标题、关键指标数值、机房名 |
| JetBrains Mono Uppercase | 元信息（分类 / 状态 / 计数 / 时间戳 / 坐标 / 单位） |
| Inter 400/500/600 | 控件文本、Body、辅助说明 |

> 看到字体即识别"这是什么类型的文字"，无需依赖颜色或边框辅助。

### 0.4 平面是默认 · 纵深是稀缺资源
`box-shadow` 仅出现在 §1.7 登记的两类场景（浮层投影 / focus 指示）；**唯三例外**：
- `.col.is-active::before` 流光层 `filter: drop-shadow(0 0 4px rgba(122,165,219,.5))`（屏2 进行中卡片）
- `.node-cell.flash` 单帧扩散 `box-shadow: 0 0 6px var(--vermilion-6)`（一次性，瞬间消失）
- `.tile.is-alert` 红色 inset shadow（监控告警边框）

**禁止**为大屏制造"悬浮卡片""按下凹陷"类阴影。

### 0.5 单一深色主题 · 物理大屏部署语境
本规范服务于 -1L 展厅 3 块 1920×1080 物理大屏，**始终深色**：
- 无主题切换（无 light/system 三态）
- 无 `prefers-color-scheme` 监听
- 无主题相关条件样式

### 0.6 大屏不是网页
大屏部署语境下，**禁止**：
- 鼠标交互的视觉提示（hover state · 大屏无指针）
- 键盘焦点环（`:focus-visible` 仍声明但无视觉意义）
- 滚动条（所有内容必须在 1920×1080 内排开）
- 模态 / drawer / toast / dropdown 等需要点击的浮层

**允许**：
- 自动循环的入场动效（subtle，不抢戏）
- 数据驱动的状态切换动效（节点点亮、雷达扫描、数据流、流光卡）
- 实时时钟、计数器、心跳指示
- 场景轮播（屏3 视频墙 15s 一轮）

### 0.7 信息密度的可信度是视觉一等公民
大屏上每一个数字、每一个状态都必须：
- **可识别**（来源是哪台机房 / 哪个节点）
- **可时效**（时间戳、心跳）
- **可置信度量**（正常 / 警告 / 异常）
- **可追溯**（异常告警有具体 code 与 message）

> **自检触发器**：若你产出的大屏看起来接近 ECharts demo / Ant Design Charts / 阿里 dataV / 数据可视化模板 —— 大概率违反了上述某条立场。

---

## 1. 原语层（Primitives）

### 1.1 色彩原语 · 整段复制至 `:root`

```css
:root {
  /* ── 文字色阶（冷白系）─────────────── */
  --ink:       #e8ebf0;   /* L1 · 主文字 / 冷白 */
  --ink-soft:  #b3b8c0;   /* L2 · 次级文字 */
  --ink-mute:  #828892;   /* L3 · 辅助文字 */
  --ink-faint: #555a62;   /* L4 · 弱化文字 / 仅非关键信息 */

  /* ── 表面色阶（允许在区间内选择）──────── */
  /* 推荐默认（屏3 同款）: #14161b · 偏温和，适合长时间观看 */
  /* 深耕变体（屏1/屏2）: #0c0e13 · 偏沉，强化对比，适合数据密集场景 */
  --paper:      #14161b;   /* 主背景 · 可在 #0c0e13 ~ #14161b 区间选 */
  --paper-warm: #1c1f26;   /* 悬停渗透 / 卡片底色 / 重音容器（屏2 用 #14171e）*/
  --paper-cool: #181a20;   /* 次级容器 / 中性区块（屏2 用 #10131a） */

  /* ── 分隔线（Rule）────────────── */
  --rule:      #2b2f37;    /* 实线分隔 */
  --rule-soft: #20242b;    /* 弱化分隔 / 行间分隔 */

  /* ── 主色（Brand · 朱砂蓝 · 7 阶严格保持 H≈214°）─── */
  /* 从深 tint 到高亮，色阶 6 为常规主色，各阶承担固定 UI 角色 */
  --vermilion-1: #1a2230;  /* 极弱 tint · AI 消息底 */
  --vermilion-2: #233048;  /* 禁用 / pill 底 */
  --vermilion-3: #2c4068;  /* 小面积填充 / 集群格背景 */
  --vermilion-4: #3d5d8e;  /* 装饰边线 / 地图边角十字 */
  --vermilion-5: #5b88c2;  /* 悬浮态 / 连接线 */
  --vermilion-6: #7aa5db;  /* 常规主色 · H 214° / S 56% / L 67% · AA on --paper */
  --vermilion-7: #a0c2e8;  /* 高亮激活 · pressed / flash */

  /* 兼容别名 */
  --vermilion:      var(--vermilion-6);
  --vermilion-soft: var(--vermilion-5);
  --vermilion-bg:   var(--vermilion-2);
  --vermilion-deep: var(--vermilion-7);

  /* ── 语义色（四阶完整：default · soft · bg · deep）──── */
  --success:      #a4b870;  /* 橄榄绿 · 完成 / 训练场标记 */
  --success-soft: #859554;
  --success-bg:   #1d2418;
  --success-deep: #c0d490;

  --warning:      #d4ad6a;  /* 沙金 · 待命 / 异常前兆 */
  --warning-soft: #b89253;
  --warning-bg:   #251f17;
  --warning-deep: #e8c688;

  --error:        #e57878;  /* 朱赭红 · 故障 / 异常 */
  --error-soft:   #c25858;
  --error-bg:     #2a1818;
  --error-deep:   #f29595;

  /* ── 录制 / Live 专用红（唯一非朱砂红）─── */
  --rec-red: #ec5f50;     /* 仅 .badge-live / pulse-rec / 录制状态 */

  /* ── 字体栈（Type Stack）──────── */
  --font-sans:  'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI',
                'PingFang SC', 'Noto Sans SC', sans-serif;
  --font-serif: 'Cormorant Garamond', 'Noto Serif SC', serif;
  --font-mono:  'JetBrains Mono', ui-monospace, 'SF Mono', monospace;

  /* ── 时序 token ─── */
  --t-fast: .12s;
  --t-base: .15s;
  --t-mid:  .22s;
  --t-slow: .35s;
  --ease:     cubic-bezier(.2, 0, 0, 1);
  --ease-out: cubic-bezier(.16, 1, .3, 1);

  /* ── 阴影阶梯（唯二合法）─── */
  --shadow-overlay: 0 8px 24px rgba(0,0,0,.5);
  --shadow-modal:   0 20px 60px rgba(0,0,0,.6);   /* 大屏极少用 */
  --shadow-focus:   0 0 0 2px rgba(231,234,239,.18), 0 1px 4px rgba(0,0,0,.5);
}

html { color-scheme: dark; }
```

> **严格约束**：
> - 主色 7 阶之外**不允许**私造任何变体（rgba 透明叠加、HSL 临时调整）
> - 任何"看起来像主色"的颜色都必先归类至某一阶
> - 深色基底**不允许**替换为纯黑 `#000` 或冷灰 `#1a1a1a` —— 保留 `#0c0e13 ~ #14161b` 区间的轻微暖偏

#### 1.1.1 主色 7 阶角色映射

| 色阶 | Token | 典型用途 | hex |
|---|---|---|---|
| 1 | `--vermilion-1` | 极弱 tint / 容器底 | `#1a2230` |
| 2 | `--vermilion-2` | 禁用 / `pill-bg` | `#233048` |
| 3 | `--vermilion-3` | 小面积填充 / 集群格 | `#2c4068` |
| 4 | `--vermilion-4` | 装饰边线 / 地图边角 / focus-soft | `#3d5d8e` |
| 5 | `--vermilion-5` | 连接线 / 悬浮态填充 | `#5b88c2` |
| 6 | `--vermilion-6` | **常规（主色）** · `.btn-primary` / 链接 / 重音 | `#7aa5db` |
| 7 | `--vermilion-7` | 数据高亮 / flash / pressed | `#a0c2e8` |

### 1.2 字体引入

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="preconnect" href="https://fonts.font.im">
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,500;0,600;1,400&family=Noto+Serif+SC:wght@400;500;600;700&family=JetBrains+Mono:wght@300;400;500;600&family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<!-- 国内镜像 backup · Google Fonts 不通时启用 -->
<link href="https://fonts.font.im/css2?family=Cormorant+Garamond:ital,wght@0,400;0,500;0,600;1,400&family=Noto+Serif+SC:wght@400;500;600;700&family=JetBrains+Mono:wght@300;400;500;600&family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
```

### 1.3 字号阶梯（响应式 · vh-clamp）

**v2.0 起字号全部走 `clamp(min, vh, max)` 响应式**，以 1080 视口为基准。原始字号 = vh 中位数 × 1080 / 100。

```css
:root {
  --fs-9:  clamp(8px,  0.83vh, 14px);   /* 极小元信息 · chip 内英文副标 */
  --fs-10: clamp(9px,  0.93vh, 16px);   /* pill / table th / 坐标轴标签 */
  --fs-11: clamp(10px, 1.02vh, 17px);   /* label / live badge / metric-label / 时间戳 */
  --fs-12: clamp(11px, 1.11vh, 18px);   /* section-label / table td / 副标 */
  --fs-13: clamp(12px, 1.20vh, 20px);   /* btn / 紧凑正文 / .title-s / 卡片名 */
  --fs-14: clamp(13px, 1.30vh, 22px);   /* body / 重点正文 */
  --fs-15: clamp(13px, 1.39vh, 23px);   /* alert msg */
  --fs-16: clamp(14px, 1.48vh, 24px);   /* .title-m / 卡片大标题 */
  --fs-18: clamp(15px, 1.67vh, 26px);   /* 二级 hero */
  --fs-20: clamp(16px, 1.85vh, 30px);   /* .title-l / 屏幕主标题 */
  --fs-22: clamp(17px, 2.04vh, 32px);   /* topbar 主标题 / tile 站点名 */
  --fs-24: clamp(18px, 2.22vh, 36px);   /* 次级主指标数值 */
  --fs-28: clamp(20px, 2.60vh, 42px);   /* 主指标数值 */
}
```

| 字号 token | 基准 px @1080vh | 主要用途 |
|---|---|---|
| `--fs-9` | 9 | chip 副标、监控浮层 badge |
| `--fs-10` | 10 | pill / `.label` 元信息 / table th |
| `--fs-11` | 11 | `label` / `metric-label` / `live-badge` / 时间戳 |
| `--fs-12` | 12 | section 副标 / table td / `weight-tag` |
| `--fs-13` | 13 | btn / `cluster-title` / 卡片名 / `step-marker` |
| `--fs-14` | 14 | body / `topbar-right .ts` / `chip-icon` text |
| `--fs-16` | 16 | `col-eyebrow .name` / `.title-m` |
| `--fs-20` | 20 | `.title-l` / tile mini 站点名 / 主指标次级 |
| `--fs-22` | 22 | topbar 主标题 / `tile-bl .name` / 主指标 |
| `--fs-28` | 28 | hero 数值（大屏顶部 KPI 巨数） |

> **严格约束**：
> - **禁止**直接写 `font-size: 14px` —— 全走 `var(--fs-N)`
> - **禁止**自己造 `--fs-N`（如 `--fs-17` `--fs-15`，仅在 §1.3 已登记的阶梯里选）

### 1.4 间距阶梯（响应式 · vh-clamp）

```css
:root {
  --sp-2:  clamp(2px,  0.20vh, 4px);
  --sp-3:  clamp(2px,  0.28vh, 5px);
  --sp-4:  clamp(3px,  0.37vh, 7px);
  --sp-5:  clamp(4px,  0.46vh, 8px);
  --sp-6:  clamp(5px,  0.56vh, 10px);
  --sp-8:  clamp(6px,  0.74vh, 12px);
  --sp-10: clamp(8px,  0.93vh, 16px);
  --sp-12: clamp(9px,  1.11vh, 18px);
  --sp-14: clamp(11px, 1.30vh, 22px);
  --sp-16: clamp(12px, 1.48vh, 24px);
  --sp-18: clamp(14px, 1.67vh, 27px);
  --sp-20: clamp(15px, 1.85vh, 30px);
  --sp-24: clamp(18px, 2.22vh, 36px);
  --sp-28: clamp(21px, 2.60vh, 42px);
  --sp-32: clamp(24px, 2.96vh, 48px);

  --topbar-h: clamp(60px, 7.4vh, 96px);  /* 屏1 用 7.4vh，屏2/3 用 7vh */
}
```

#### 1.4.1 间距用途映射

| 用途 | 推荐值 |
|---|---|
| 图标—文字 gap | `--sp-6` / `--sp-8` |
| 卡片内 padding · 紧凑 | `--sp-10` / `--sp-12` |
| 卡片内 padding · 标准 | `--sp-14` / `--sp-16` |
| 卡片之间 | `--sp-8` / `--sp-12` |
| 区段之间 | `--sp-20` / `--sp-24` |
| 大屏顶栏左右 padding | `--sp-24` / `--sp-32` |
| topbar 分隔线高度 | `--sp-32` |

> **禁止 5 / 7 / 9 / 11 / 13 / 15 / 18 / 22 等非阶梯值**（除非已是 token 中位）。

### 1.5 圆角阶梯（Radius Scale）

```
2px      · 几乎所有容器、控件、徽章、卡片（默认值）
4px      · col / chip-wall / chip-icon（屏2 主区块例外，已登记）
50%      · 头像 / 状态点 / 节点圆点 / pill::before
0        · 整页区块边界 / 全局分隔线
10px     · 唯一胶囊例外：.badge-live（LIVE / REC 状态徽章）
1px      · .node-cell（集群格 · 紧凑像素感）
```

> **中间值（6 / 8 / 12 / 16）一律不允许**。

### 1.6 阴影阶梯（Shadow Scale）

| Token | 用途 | 允许场景 |
|---|---|---|
| `--shadow-overlay` | 浮层投影 | 大屏极少用 · 仅设计预览 |
| `--shadow-modal` | 模态投影 | 大屏部署后**不使用** |
| `--shadow-focus` | 焦点环 | 设计预览时声明 · 大屏部署后无视觉 |

**已登记三处例外**（§0.4 重申）：
- `.col.is-active::before` 流光层 drop-shadow
- `.node-cell.flash` 单帧扩散 box-shadow（瞬间消失）
- `.tile.is-alert` inset box-shadow（红色描边）

**任何"卡片悬浮""按钮按下""节点发光"类阴影一律视为反模式**。

### 1.7 时序阶梯（Motion Tokens）

| Token | 时长 | 用途 |
|---|---|---|
| `--t-fast` | .12s | 高频小元素 |
| `--t-base` | .15s | 标准控件 |
| `--t-mid` | .22s | 列表入场：slideUp |
| `--t-slow` | .35s | 浮层 fade |

**Easing**：
- `--ease: cubic-bezier(.2, 0, 0, 1)` — 标准控件
- `--ease-out: cubic-bezier(.16, 1, .3, 1)` — 入场 / 流光

#### 1.7.1 大屏循环动效时长（不引用上述 token）

| 场景 | 时长 / 曲线 |
|---|---|
| 节点 ring 脉冲（map） | 2 ~ 2.4s · `ease-out` |
| 连接线 dash flow | 1.5 ~ 1.8s · `linear` |
| 雷达 sweep | 4s · `linear` |
| 数据流粒子 | 1.4 ~ 1.6s · `linear` |
| LIVE 心跳 / Alert flash | 1.4s · `ease-in-out` |
| 异常节点闪烁 | 1 ~ 1.5s · `ease-in-out` |
| `.col.is-active::before` 流光 | 4.4s · `linear` |
| `.node-cell.use` cell-breathe | 2.6s · `ease-in-out` infinite |
| `.node-cell.flash` cell-flash | .55s · `ease-out` 单次 |
| `.node-cell.baseline.bflash` | 1.1s · `ease-out` 单次 |
| weight-fill::after pulse | 1.2s · `ease-in-out` |
| chip-card 切换 | .25s · `--ease` |
| col / cluster-block 状态切换 | .6s ~ .8s · `--ease` |

> **严格约束**：
> - 同一屏幕**禁止**同时出现超过 4 类不同节奏的动效（视觉噪音）
> - 动效幅度 **subtle**：opacity 不低于 .4，transform 不超过 4px，无旋转 360° 之外的过激变换

### 1.8 Reset / Base

```css
* { box-sizing: border-box; margin: 0; padding: 0; }
html { color-scheme: dark; }
html, body { width: 100%; height: 100%; overflow: hidden; background: #000; }
body {
  font-family: var(--font-sans);
  color: var(--ink);
  background: var(--paper);
  font-size: var(--fs-14);
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: .01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: .01ms !important;
  }
}
```

### 1.9 图标系统

- **来源**：Lucide 风格 / `stroke-width: 1.8` / 无填充
- **尺寸阶梯**：`12 / 14 / 16 / 20`（在 SVG `width/height` 直接写，可绑 `--sp-N`）
- **着色**：`stroke: currentColor`
- **禁止**用 emoji 充当图标
- **禁止**用 `fill` 表达层级

---

## 2. 排版系统（Typographic System）

### 2.1 Logo · Cormorant Italic · ⚠ **字体写法 · 不允许图片**

> **重要规则**：当前三屏 HTML 临时用了 `<img src="DeepLink Logo.png">`，**v2.0 起正式恢复字体写法**。所有新页面、所有迭代必须用以下 CSS / HTML，不允许嵌入 logo 图片。

```css
.logo-mark {
  font-family: 'Cormorant Garamond', serif;
  font-style: italic;
  font-size: var(--fs-22);          /* 响应式 · 基准 22px */
  font-weight: 500;
  color: var(--ink);
  letter-spacing: -.02em;
  line-height: 1;
  flex-shrink: 0;
  white-space: nowrap;
}
.logo-mark em {
  color: var(--vermilion);
  font-style: normal;               /* em 标签从 italic 切回 normal */
  font-weight: 600;
}
```

```html
<div class="logo-mark">Deep<em>Link</em></div>
```

**视觉解剖**：
- `Deep` — Cormorant Italic 500，主文字色 `--ink`，倾斜
- `Link` — Cormorant Normal 600，朱砂蓝 `--vermilion-6`，**正体**（被 em 反向覆盖斜体）
- 整体 `letter-spacing: -.02em`（拉丁字母收紧 2%）

**不允许**：
- ❌ 把 logo 当图片 `<img>` 嵌入
- ❌ 用 Inter / Noto Serif / 任何 sans 字体替代 Cormorant
- ❌ 改变 Italic + Normal 的对比（Deep 必斜 · Link 必正）
- ❌ 把 `Link` 染成 success / warning / error 色

### 2.2 元信息字体三件套

> **决策树**：
> - 出现在 hero / 区段顶部、需要视觉引导 → `.eyebrow`（朱砂 + 左右连接线）
> - 区段标题、需要分隔 → `.section-label`（带底分隔线）
> - 行内辅助、徽章前后、列表前缀 → `.label`（无修饰）

```css
.label {
  font-family: var(--font-mono);
  font-size: var(--fs-11);
  font-weight: 600;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: var(--ink-faint);
}
.label--strong { color: var(--ink-mute); }

.section-label {
  font-family: var(--font-mono);
  font-size: var(--fs-12);
  letter-spacing: .2em;
  text-transform: uppercase;
  color: var(--ink-faint);
  padding-bottom: var(--sp-6);
  border-bottom: 1px solid var(--rule-soft);
  margin-bottom: var(--sp-10);
}

.eyebrow {
  font-family: var(--font-mono);
  font-size: var(--fs-12);
  letter-spacing: 0.28em;
  text-transform: uppercase;
  color: var(--vermilion);
  display: inline-flex; align-items: center; gap: var(--sp-12);
}
.eyebrow::before, .eyebrow::after {
  content: ""; width: 24px; height: 1px; background: var(--vermilion);
}
```

### 2.3 Title · Noto Serif 600

```css
.title-l { font-family: 'Noto Serif SC', serif; font-size: var(--fs-20); font-weight: 600; letter-spacing: 0.02em; }
.title-m { font-family: 'Noto Serif SC', serif; font-size: var(--fs-16); font-weight: 600; letter-spacing: 0.02em; }
.title-s { font-family: 'Noto Serif SC', serif; font-size: var(--fs-13); font-weight: 600; letter-spacing: 0.02em; }
```

> **Letter-spacing 规则**：
> - **Noto Serif SC（CJK）** 永远 `+0.02em`（中文字间需轻微开）
> - **Cormorant（拉丁）** 永远 `-0.02em`（拉丁字间需轻微收）

### 2.4 Topbar 主标题（双语并置 · 朱砂强调）

```css
.topbar-title .main {
  font-family: 'Noto Serif SC', serif;
  font-size: var(--fs-22);
  font-weight: 600;
  letter-spacing: .02em;
  color: var(--ink);
  line-height: 1.2;
}
.topbar-title .main em {
  font-family: var(--font-serif);   /* Cormorant */
  font-style: italic;
  color: var(--vermilion);
  font-weight: 600;
  margin: 0 1px;
  letter-spacing: -.02em;
}
.topbar-title .sub {
  font-family: var(--font-mono);
  font-size: var(--fs-11);
  color: var(--ink-mute);
  letter-spacing: .14em;
  text-transform: uppercase;
}
```

```html
<div class="topbar-title">
  <div class="main">深度共联<em>DeepLink</em>开放计算体系</div>
  <div class="sub">National Compute Mesh · Realtime</div>
</div>
```

### 2.5 Body · Inter

正文 `var(--fs-14) / line-height 1.6 / color: var(--ink)`。次级正文 `var(--ink-soft)`，元信息 `var(--ink-mute)`。

> **严格约束**：中英文混排时**禁止**为中文单独指定字体；让 Inter 走 fallback 到 PingFang SC / Noto Sans SC。

### 2.6 数值文本 · Tabular Nums

所有指标数值（总算力、温度、利用率、计数）**必须**使用 `font-variant-numeric: tabular-nums`：

```css
.metric-value,
.metric-num,
.ts,
.num,
[class*="num"] {
  font-variant-numeric: tabular-nums;
}
```

> **原因**：大屏数值会实时更新；非等宽数字（如 `1` 比 `9` 窄）会导致数字跳动时容器抖动。

---

## 3. 画布与响应式（Canvas & Responsive）

### 3.1 画布约定（v2.0 重大演进）

```
设计画布：1920 × 1080（16:9）
设计单位：1 设计 px = 1 物理 px @ 1920×1080 投屏
缩放策略：vh-clamp 字号/间距 + 16:9 stage 守护（自适应宽高比）
```

**v1.0 → v2.0 关键差异**：
- v1.0 用 `transform: scale()` 等比缩放（固定 1920×1080 画布）
- v2.0 用 `vh-clamp + width:100vw + height:min(100vh, 56.25vw)`（自适应 + 16:9 守护）

### 3.2 标准 Stage 容器

```html
<div class="stage screen-N">
  <!-- 所有内容 -->
</div>
```

```css
html, body { width: 100%; height: 100%; overflow: hidden; background: #000; }
body { font-family: var(--font-sans); color: var(--ink); background: var(--paper); }

.stage {
  position: fixed;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);

  /* 宽度跟随视口 */
  width: 100vw;

  /* 关键：高度 = min(视口高, 视口宽 × 9/16)
     视口 ≥ 16:9 → height = 100vh（视口短的那个，全填）
     视口 <  16:9 → height = 56.25vw（letterbox 顶底） */
  height: min(100vh, 56.25vw);

  background: var(--paper);
  overflow: hidden;
}
```

### 3.3 响应式断点（预览阶段降级）

大屏部署后视口固定 1920×1080，断点不会触发。**断点仅服务于设计预览 + 笔记本走查**。

```css
/* 中等宽屏（< 1400px）：缩小角标 padding，标题降一档 */
@media (max-width: 1400px) {
  .tile-bl { left: var(--sp-20); right: var(--sp-20); bottom: var(--sp-20); }
  .tile-bl .name { font-size: var(--fs-20); }
}

/* 窄屏（< 980px）：隐藏副标 sub */
@media (max-width: 980px) {
  .topbar-title .sub { display: none; }
  .tile-bl .name { font-size: var(--fs-16); }
}

/* 极窄屏（< 720px）：拆掉地点 sep，仅留温度 */
@media (max-width: 720px) {
  .tile-bl .cat { display: none; }
  .topbar-right .status-count { display: none; }
}
```

> **严格约束**：
> - **禁止**在新建组件里写超过 3 级断点（1400 / 980 / 720 之外不允许）
> - **禁止**通过断点改变骨架（仅允许密度降级）
> - 大屏部署后**禁止**残留 debug 浮标，删除 `.debug-info`

---

## 4. 三屏布局骨架

### 4.1 屏 1 · 全国算力一张网（地图全屏型）

```
┌──────────────────────────────────────────┐
│ Topbar var(--topbar-h)                   │
├──────────────────────────────────────────┤
│                                          │
│   Full Map (flex:1)                      │
│   ┌─ Overlay Card (左侧浮卡)             │
│   ├─ Map Chart (ECharts 全国地图)        │
│   ├─ Map Labels (DOM 标签浮层)           │
│   ├─ Map Legend (左下 · 节点类型 legend) │
│   ├─ Map Reset (右下 · 还原按钮)         │
│   └─ Map Corners (四角十字印记)          │
│                                          │
└──────────────────────────────────────────┘
```

骨架：

```css
.screen-1 { display: flex; flex-direction: column; height: 100%; }
.topbar   { flex: 0 0 var(--topbar-h); }
.full-map { flex: 1; position: relative; overflow: hidden; min-height: 0; }
```

### 4.2 屏 2 · 算力操作区（5 段网格）

```
┌────────────────────────────────────────────┐
│ Topbar  var(--topbar-h)                    │
├────────────────────────────────────────────┤
│ Main Row (70fr)                            │
│ ┌─ col-left  ────────┬─ Resource & Fault ─┐│
│ │ ├ Task Decomp 60fr │  cluster-grid-2x2  ││
│ │ └ AI Thinking 40fr │  + anomaly-row     ││
│ └────────────────────┴────────────────────┘│
├────────────────────────────────────────────┤
│ Op Row (13fr) · Operator Pipeline 算子链   │
├────────────────────────────────────────────┤
│ Chip Row (17fr) · 12 国产芯片厂商墙        │
└────────────────────────────────────────────┘
```

骨架：

```css
.screen-2 {
  display: grid;
  grid-template-rows:
    var(--topbar-h)
    minmax(420px, 70fr)   /* main-row · 3 列主区 */
    minmax(70px,  13fr)   /* op-row · operator 流水线 */
    minmax(100px, 17fr);  /* chip-row · 芯片墙 */
  height: 100%;
  min-height: 0;
}
.main-row {
  display: grid;
  grid-template-columns: minmax(560px, 1fr) minmax(360px, 32%);
  padding: var(--sp-14) var(--sp-16);
  gap: var(--sp-12);
  border-bottom: 1px solid var(--rule);
}
.col-left {
  display: grid;
  grid-template-rows: minmax(0, 60fr) minmax(0, 40fr);
  gap: var(--sp-12);
}
```

### 4.3 屏 3 · 机房监控区（2×2 视频墙）

```
┌────────────────────────────────────────────┐
│ Topbar  var(--topbar-h)                    │
├────────────────────────────────────────────┤
│ ┌─────────────┬──────────────┐             │
│ │  Tile 1     │  Tile 2      │             │
│ │  (超算)     │  (超算)      │             │
│ │             │              │             │
│ ├─────────────┼──────────────┤             │
│ │  Tile 3     │  Tile 4      │             │
│ │  (训练场)   │  (智算)      │             │
│ │             │              │             │
│ └─────────────┴──────────────┘             │
│   (4px gap · rule-soft 填缝)               │
└────────────────────────────────────────────┘
```

骨架：

```css
.screen-3 {
  display: grid;
  grid-template-rows: var(--topbar-h) 1fr;
  height: 100%;
}
.wall {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  gap: 4px;
  background: var(--rule-soft);   /* gap 显示为分隔线 */
  overflow: hidden;
  min-height: 0;
  height: 100%;
}
.tile {
  position: relative;
  background: var(--paper-cool);
  overflow: hidden;
}
```

---

## 5. 通用组件解剖（Component Anatomy）

### 5.1 Topbar · 顶栏（三屏共享）

```html
<header class="topbar">
  <div class="topbar-left">
    <div class="logo-mark">Deep<em>Link</em></div>
    <div class="topbar-divider"></div>
    <div class="topbar-title">
      <div class="main">深度共联<em>DeepLink</em>开放计算体系</div>
      <div class="sub">National Compute Mesh · Realtime</div>
    </div>
  </div>
  <div class="topbar-right">
    <span class="badge-live">实时</span>
    <span class="ts" id="ts">2026-06-30 14:01:16</span>
    <span class="status-count"><span class="n">4</span><span class="d">/</span><span>4 摄像通道</span></span>
  </div>
</header>
```

```css
.topbar {
  padding: 0 var(--sp-32);
  border-bottom: 1px solid var(--rule);
  display: flex; align-items: center; justify-content: space-between;
  background: var(--paper);
  position: relative;
  z-index: 20;
}
.topbar-left    { display: flex; align-items: center; gap: var(--sp-24); }
.topbar-divider { width: 1px; height: var(--sp-32); background: var(--rule); }
.topbar-title   { display: flex; flex-direction: column; gap: var(--sp-3); }
.topbar-right   {
  display: flex; align-items: center; gap: var(--sp-16);
  font-family: var(--font-mono);
  font-size: var(--fs-12);
  color: var(--ink-mute);
}
.topbar-right .ts {
  font-size: var(--fs-14);
  font-variant-numeric: tabular-nums;
  color: var(--ink-soft);
  letter-spacing: .04em;
}
.topbar-right .status-count {
  display: inline-flex; align-items: center; gap: var(--sp-6);
  font-family: var(--font-mono);
  font-size: var(--fs-11);
  letter-spacing: .14em;
  text-transform: uppercase;
  color: var(--ink-mute);
}
.topbar-right .status-count .n {
  color: var(--vermilion);
  font-variant-numeric: tabular-nums;
  font-size: var(--fs-13);
  font-weight: 600;
}
.topbar-right .status-count .d { color: var(--ink-faint); }
```

### 5.2 容器层级（Surface Tier）

```css
/* L1 · 平等容器 */
.card {
  background: var(--paper-cool);
  border: 1px solid var(--rule);
  border-radius: 2px;
  padding: var(--sp-14) var(--sp-16);
}

/* L2 · 视觉重音容器（朱砂左色带 · 唯一允许的重音方式） */
.card-emphasis {
  background: var(--paper-warm);
  border: 1px solid var(--rule-soft);
  border-left: 3px solid var(--vermilion);
  border-radius: 2px;
  padding: var(--sp-12) var(--sp-14);
}

/* L3 · 中性次级容器（轻量） */
.card-info {
  background: var(--paper-cool);
  border: 1px solid var(--rule-soft);
  border-radius: 2px;
  padding: var(--sp-12) var(--sp-14);
}
```

> **严格约束**：朱砂重音**只允许** `border-left: 3px solid var(--vermilion)`，不允许通过 background-tint 全填充表达。

### 5.3 徽章系统（Pill / Badge）

```css
.pill {
  display: inline-flex; align-items: center; gap: 4px;
  font-family: var(--font-mono);
  font-size: var(--fs-10);
  font-weight: 600;
  letter-spacing: .04em;
  padding: 2px var(--sp-8);
  border-radius: 2px;
  background: var(--paper-warm);
  color: var(--ink-mute);
}
.pill::before {
  content: ''; width: 5px; height: 5px; border-radius: 50%;
  background: currentColor;
}
.pill-primary { background: var(--vermilion-bg); color: var(--vermilion); }
.pill-success { background: var(--success-bg);   color: var(--success); }
.pill-warning { background: var(--warning-bg);   color: var(--warning); }
.pill-error   { background: var(--error-bg);     color: var(--error); }
.pill-neutral { color: var(--ink-mute); }
.pill-neutral::before { background: var(--ink-faint); }

/* LIVE / REC · 唯一胶囊圆角徽章（borderradius 10px） */
.badge-live {
  font-family: var(--font-mono);
  font-size: var(--fs-11);
  font-weight: 600;
  padding: var(--sp-2) var(--sp-10);
  border-radius: 10px;
  background: var(--rec-red);
  color: #fff;
  display: inline-flex; align-items: center; gap: var(--sp-4);
  letter-spacing: .04em;
}
.badge-live::before {
  content: ''; width: 6px; height: 6px; border-radius: 50%;
  background: #fff;
  animation: live-pulse 1.4s ease-in-out infinite;
}
@keyframes live-pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: .3; }
}
```

### 5.4 数据密集表（机柜状态 / 节点列表）

```css
.table-dense {
  width: 100%; border-collapse: collapse;
  font-size: var(--fs-13);
  font-variant-numeric: tabular-nums;
}
.table-dense thead th {
  background: var(--paper-cool);
  padding: var(--sp-8) var(--sp-10);
  border-bottom: 1px solid var(--rule);
  font-family: var(--font-mono);
  font-size: var(--fs-11);
  letter-spacing: .08em;
  text-transform: uppercase;
  color: var(--ink-mute);
  text-align: left;
}
.table-dense tbody td {
  padding: var(--sp-8) var(--sp-10);
  border-bottom: 1px solid var(--rule-soft);
  color: var(--ink-soft);
}
.table-dense tbody td:first-child {
  color: var(--ink);
  font-weight: 500;
}
.table-dense .num { text-align: right; }
```

### 5.5 列表行 · 状态左色带

```css
.list-item {
  display: flex; align-items: center; gap: var(--sp-12);
  padding: var(--sp-12) var(--sp-14);
  border-bottom: 1px solid var(--rule-soft);
  background: var(--paper-cool);
  border-left: 2px solid var(--rule);
  border-radius: 0 2px 2px 0;
}
.list-item.running { border-left-color: var(--vermilion); background: var(--paper-warm); }
.list-item.done    { border-left-color: var(--success);   opacity: .7; }
.list-item.queue   { border-left-color: var(--ink-faint); }
.list-item.error   { border-left-color: var(--error);     background: var(--error-bg); }
```

### 5.6 时间线 / 流程步骤指示器

```css
.flow-step {
  position: relative;
  padding: var(--sp-12) var(--sp-14) var(--sp-12) var(--sp-32);
  margin-bottom: var(--sp-8);
  background: var(--paper-cool);
  border-left: 1px solid var(--rule);
}
.flow-step::before {
  content: '';
  position: absolute;
  left: -4px; top: 18px;
  width: 7px; height: 7px;
  border-radius: 50%;
  background: var(--rule);
  border: 1px solid var(--paper);
}
.flow-step.done::before    { background: var(--success); }
.flow-step.running::before {
  background: var(--vermilion);
  animation: pulse-dot 1.4s ease-in-out infinite;
}
@keyframes pulse-dot {
  0%, 100% { box-shadow: 0 0 0 0 rgba(122,165,219,.6); }
  50%      { box-shadow: 0 0 0 4px rgba(122,165,219,0); }
}
```

### 5.7 Skeleton / Spinner

```css
.skeleton {
  background: linear-gradient(90deg, var(--paper-warm) 0%, var(--paper-cool) 50%, var(--paper-warm) 100%);
  background-size: 200% 100%;
  animation: shimmer 1.4s linear infinite;
  border-radius: 2px;
}
@keyframes shimmer { from { background-position: 200% 0; } to { background-position: -200% 0; } }

.spinner {
  width: 14px; height: 14px;
  border: 1.5px solid var(--rule);
  border-top-color: var(--vermilion);
  border-radius: 50%;
  animation: spin .8s linear infinite;
  display: inline-block;
}
@keyframes spin { to { transform: rotate(360deg); } }
```

### 5.8 滚动条（仅设计预览阶段会出现）

大屏部署后不应该有滚动条。仅在预览/笔记本走查时启用：

```css
.scroll::-webkit-scrollbar { width: 3px; height: 3px; }
.scroll::-webkit-scrollbar-thumb { background: var(--rule); border-radius: 2px; }
.scroll { scrollbar-width: thin; scrollbar-color: var(--rule) transparent; }
```

---

## 6. 大屏特有模式（Big Screen Patterns）

本节登记了 12 类大屏专属组件，全部出自三屏最新深耕版本。

### 6.1 指标卡（Metric Card）

```css
.metric {
  padding: var(--sp-14) var(--sp-16);
  background: var(--paper-cool);
  border: 1px solid var(--rule);
  border-radius: 2px;
}
.metric.emphasis {
  background: var(--paper-warm);
  border-color: var(--rule-soft);
  border-left: 3px solid var(--vermilion);
}
.metric-label {
  font-family: var(--font-mono);
  font-size: var(--fs-10);
  letter-spacing: .18em;
  text-transform: uppercase;
  color: var(--ink-faint);
  margin-bottom: var(--sp-6);
}
.metric-value {
  font-family: 'Noto Serif SC', serif;
  font-size: var(--fs-28);
  font-weight: 600;
  letter-spacing: .02em;
  color: var(--ink);
  font-variant-numeric: tabular-nums;
  line-height: 1.1;
}
.metric-value .unit {
  font-family: var(--font-mono);
  font-size: var(--fs-14);
  font-weight: 500;
  color: var(--ink-mute);
  margin-left: 4px;
}
.metric-sub {
  font-size: var(--fs-12);
  color: var(--ink-mute);
  margin-top: var(--sp-6);
}
```

> **唯一允许的字号组合**：
> - 主指标：`--fs-28` (Serif 600) + `--fs-14` (Mono unit)
> - 次级指标：`--fs-24` (Serif 600) + `--fs-12` (Mono unit)
> - 底部 KPI 条：`--fs-20` (Serif 600) + `--fs-11` (Mono unit)

### 6.2 节点 + 脉冲环（Map Nodes · 屏1 地图核心）

```css
.node {
  position: absolute;
  transform: translate(-50%, -50%);
}
.node-dot {
  width: 10px; height: 10px;
  border-radius: 50%;
  border: 2px solid var(--paper);
  position: relative;
}
.node.hub   .node-dot { background: var(--vermilion); width: 12px; height: 12px; }
.node.super .node-dot { background: var(--success); }
.node.train .node-dot { background: var(--warning); }

/* Ring · 脉冲扩散 */
.node-ring {
  position: absolute; top: 50%; left: 50%;
  width: 28px; height: 28px;
  border-radius: 50%;
  border: 1px solid currentColor;
  transform: translate(-50%, -50%);
  animation: ring 2.4s ease-out infinite;
}
.node.hub   .node-ring { color: var(--vermilion); }
.node.super .node-ring { color: var(--success); }
.node.train .node-ring { color: var(--warning); }
@keyframes ring {
  0%   { transform: translate(-50%, -50%) scale(.4);  opacity: .8; }
  100% { transform: translate(-50%, -50%) scale(1.8); opacity: 0;  }
}
```

> **严格约束**：节点类型必须用**色相**区分（vermilion / success / warning），**禁止**用形状区分，**禁止**用大小区分（仅 hub 略大）。

### 6.3 连接线（虚线流动 + 脉冲突出）

```css
/* 虚线流动 · 常规已连接 */
.conn-line {
  stroke: var(--vermilion-5);
  stroke-width: 1;
  stroke-dasharray: 4 5;
  fill: none;
  opacity: .55;
  animation: dash 1.6s linear infinite;
}
@keyframes dash { to { stroke-dashoffset: -18; } }

/* 脉冲突出 · 当前焦点链路 */
.pulse-line {
  stroke: var(--vermilion);
  stroke-width: 1.4;
  fill: none;
  stroke-dasharray: 60 1000;
  animation: pulse-line 3.2s cubic-bezier(.16,1,.3,1) infinite;
}
@keyframes pulse-line {
  0%   { opacity: 0; stroke-dashoffset: 0; }
  20%  { opacity: .9; }
  100% { opacity: 0; stroke-dashoffset: -260; }
}
```

### 6.4 集群网格 16 列（Cluster Grid · 屏2 资源区核心）

```html
<div class="cluster-grid-2x2">
  <div class="cluster-block live">
    <div class="cluster-meta">
      <span class="cluster-title">华为昇腾 · 6000 卡集群</span>
      <span class="cluster-stat">UTIL <span class="util">82%</span></span>
    </div>
    <div class="node-grid">
      <div class="node-cell baseline"></div>
      <div class="node-cell baseline bflash"></div>
      <div class="node-cell use"></div>
      <div class="node-cell use flash"></div>
      <!-- … 共 16 × N 格 -->
    </div>
    <div class="cluster-live-stat">… 当前任务实时占用</div>
  </div>
  <!-- 其他 3 个 cluster-block -->
</div>
```

```css
.cluster-grid-2x2 {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--sp-16) var(--sp-24);
  padding-bottom: var(--sp-14);
  border-bottom: 1px solid var(--rule-soft);
}
.cluster-block { display: flex; flex-direction: column; gap: var(--sp-8); }
.cluster-meta  { display: flex; justify-content: space-between; align-items: baseline; }
.cluster-title {
  font-family: var(--font-sans);
  font-size: var(--fs-13);
  font-weight: 600;
  color: var(--ink);
  letter-spacing: .02em;
  transition: color var(--t-base) var(--ease);
}
.cluster-block.alert .cluster-title { color: var(--error); }
.cluster-block.alert .cluster-title::after {
  content: '  ⚠';
  font-family: var(--font-mono);
  font-weight: 700;
  color: var(--error);
}
.cluster-stat {
  font-family: var(--font-mono);
  font-size: var(--fs-10);
  letter-spacing: .08em;
  color: var(--ink-mute);
  text-transform: uppercase;
  font-variant-numeric: tabular-nums;
}
.cluster-stat .util { color: var(--vermilion); font-weight: 600; }

/* 16 列像素方阵 */
.node-grid {
  display: grid;
  grid-template-columns: repeat(16, 1fr);
  gap: 2px;
}
.node-cell {
  aspect-ratio: 1;
  border-radius: 1px;
  background: transparent;
  border: 1px solid rgba(44, 64, 104, .35);
  transition: background .3s var(--ease), border-color .3s var(--ease);
}

/* baseline · 日常 70% 持续运行 · 不脉动 · 比 use 弱 */
.node-cell.baseline {
  background: rgba(122,165,219,.32);
  border-color: rgba(122,165,219,.45);
}
/* baseline 偶发轻闪 */
.node-cell.baseline.bflash {
  animation: cell-bflash 1.1s ease-out !important;
}
@keyframes cell-bflash {
  0%   { background: rgba(122,165,219,.65); border-color: rgba(122,165,219,.75); }
  100% { background: rgba(122,165,219,.32); border-color: rgba(122,165,219,.45); }
}

/* use · 当前任务实际占用 · 持续呼吸 */
.node-cell.use {
  background: var(--vermilion);
  border-color: var(--vermilion);
  animation: cell-pop .5s ease-out, cell-breathe 2.6s ease-in-out 1.2s infinite;
}
@keyframes cell-pop     { 0% { opacity: 0; } 100% { opacity: 1; } }
@keyframes cell-breathe { 0%, 100% { opacity: 1; } 50% { opacity: .55; } }

/* flash · 调度瞬间高亮（单次） */
.node-cell.flash {
  animation: cell-flash .55s ease-out !important;
  z-index: 2; position: relative;
}
@keyframes cell-flash {
  0%   { background: var(--vermilion-7); border-color: var(--vermilion-7); box-shadow: 0 0 6px var(--vermilion-6); }
  100% { background: var(--vermilion);   border-color: var(--vermilion);   box-shadow: 0 0 0 transparent; }
}

/* err · 故障 */
.node-cell.err { background: var(--error); border-color: var(--error); }
```

### 6.5 进行中卡片 · 四边流光（屏2 标志性视觉）

> 屏2 中三栏主区（任务分解 / AI 思考 / 资源&故障）通过 `.is-active` 标记当前激活栏，激活栏四周显示流光动效，已完成的栏回归默认。

```html
<div class="col is-active">
  <div class="col-eyebrow">
    <span class="name">
      <span class="phase-num">02</span>
      <span>调度推理</span>
      <span class="en">Scheduling</span>
    </span>
    <span class="meta">…</span>
  </div>
  <!-- 内容 -->
</div>
```

```css
.col {
  position: relative;
  display: flex; flex-direction: column;
  padding: var(--sp-14) var(--sp-16);
  background: var(--paper-cool);
  border: 1px solid var(--rule);
  border-radius: 4px;          /* ← 已登记的 4px 例外 */
  overflow: hidden;
  transition:
    opacity .8s var(--ease-out),
    filter  .8s var(--ease-out),
    transform .8s var(--ease-out),
    border-color .6s var(--ease),
    box-shadow .6s var(--ease),
    background .6s var(--ease);
}
.col.is-active { border-color: var(--vermilion-4); }
.col.is-done   { border-color: var(--rule); }
.col.dim       { opacity: .5; filter: saturate(.7); transform: scale(.995); }

/* 流光层（始终存在，靠 opacity 切换可见） */
.col::before {
  content: ''; position: absolute; inset: -1px;
  pointer-events: none; z-index: 2;
  border-radius: inherit;
  /* 4 条边 · 渐变底带 + 高光接力 */
  background:
    linear-gradient(90deg, transparent 0%, var(--vermilion-5) 12%, var(--vermilion-7) 50%, var(--vermilion-5) 88%, transparent 100%) no-repeat,
    linear-gradient(90deg, transparent 35%, var(--vermilion-7) 50%, transparent 65%) no-repeat,
    linear-gradient(180deg, transparent 0%, var(--vermilion-5) 12%, var(--vermilion-7) 50%, var(--vermilion-5) 88%, transparent 100%) no-repeat,
    linear-gradient(180deg, transparent 35%, var(--vermilion-7) 50%, transparent 65%) no-repeat,
    linear-gradient(90deg, transparent 0%, var(--vermilion-5) 12%, var(--vermilion-7) 50%, var(--vermilion-5) 88%, transparent 100%) no-repeat,
    linear-gradient(270deg, transparent 35%, var(--vermilion-7) 50%, transparent 65%) no-repeat,
    linear-gradient(180deg, transparent 0%, var(--vermilion-5) 12%, var(--vermilion-7) 50%, var(--vermilion-5) 88%, transparent 100%) no-repeat,
    linear-gradient(0deg, transparent 35%, var(--vermilion-7) 50%, transparent 65%) no-repeat;
  background-size:
    100% 2px, 40% 2px,
    2px 100%, 2px 40%,
    100% 2px, 40% 2px,
    2px 100%, 2px 40%;
  filter: drop-shadow(0 0 4px rgba(122,165,219,.5));
  opacity: 0;
  transition: opacity .85s var(--ease-out);
}
.col.is-active::before {
  opacity: 1;
  animation: col-flow 4.4s linear infinite;
}
@keyframes col-flow {
  0%      { background-position: 0% 0%,   -40% 0%, 100% 0%, 100% -40%, 0% 100%, 140% 100%, 0% 0%, 0% 140%; }
  25%     { background-position: 0% 0%,   140% 0%, 100% 0%, 100% -40%, 0% 100%, 140% 100%, 0% 0%, 0% 140%; }
  25.01%  { background-position: 0% 0%,   -40% 0%, 100% 0%, 100% -40%, 0% 100%, 140% 100%, 0% 0%, 0% 140%; }
  50%     { background-position: 0% 0%,   -40% 0%, 100% 0%, 100% 140%, 0% 100%, 140% 100%, 0% 0%, 0% 140%; }
  50.01%  { background-position: 0% 0%,   -40% 0%, 100% 0%, 100% -40%, 0% 100%, 140% 100%, 0% 0%, 0% 140%; }
  75%     { background-position: 0% 0%,   -40% 0%, 100% 0%, 100% -40%, 0% 100%, -40% 100%, 0% 0%, 0% 140%; }
  75.01%  { background-position: 0% 0%,   -40% 0%, 100% 0%, 100% -40%, 0% 100%, 140% 100%, 0% 0%, 0% 140%; }
  100%    { background-position: 0% 0%,   -40% 0%, 100% 0%, 100% -40%, 0% 100%, 140% 100%, 0% 0%, 0% -40%; }
}

/* 内部渐变叠层（is-active 时渐入） */
.col::after {
  content: '';
  position: absolute; inset: 0; z-index: 0;
  border-radius: inherit;
  background: linear-gradient(180deg,
    rgba(122,165,219,.10) 0%,
    rgba(122,165,219,.03) 40%,
    transparent 100%);
  pointer-events: none;
  opacity: 0;
  transition: opacity .8s var(--ease-out);
}
.col.is-active::after { opacity: 1; }
.col > * { position: relative; z-index: 1; }
```

### 6.6 环节序号章（Phase Num · 屏2 col 内）

```css
.col-eyebrow .name {
  font-family: 'Noto Serif SC', serif;
  font-size: var(--fs-16);
  font-weight: 600;
  color: var(--ink);
  letter-spacing: .02em;
  display: inline-flex; align-items: center; gap: var(--sp-10);
  transition: color .6s var(--ease);
}
.col.is-active .col-eyebrow .name { color: var(--vermilion); }
.col.is-done   .col-eyebrow .name { color: var(--success); }

.phase-num {
  position: relative;
  flex-shrink: 0;
  display: inline-flex; align-items: center; justify-content: center;
  width: var(--sp-24); height: var(--sp-24);
  border: 1px solid var(--rule);
  border-radius: 2px;
  font-family: var(--font-mono);
  font-size: var(--fs-11);
  font-weight: 600;
  color: var(--ink-mute);
  background: var(--paper-warm);
  transition: border-color .4s var(--ease), color .4s var(--ease), background .4s var(--ease);
}
.col.is-active .phase-num {
  border-color: var(--vermilion);
  color: var(--vermilion);
  background: var(--vermilion-bg);
  animation: phase-pulse 2s ease-in-out infinite;
}
.col.is-done .phase-num {
  border-color: var(--success);
  color: var(--success);
  background: var(--success-bg);
}
/* 外圈亮段绕中心旋转（active 时渐显） */
.phase-num::before {
  content: ''; position: absolute; inset: -3px;
  border-radius: 4px;
  background: conic-gradient(var(--vermilion-7) 0deg, transparent 90deg);
  opacity: 0;
  transition: opacity .6s var(--ease);
  animation: phase-spin 2.4s linear infinite;
  z-index: -1;
}
.col.is-active .phase-num::before { opacity: 1; }
@keyframes phase-pulse {
  0%, 100% { box-shadow: none; }
  50% { box-shadow: 0 0 0 2px rgba(122,165,219,.15); }
}
@keyframes phase-spin { to { transform: rotate(360deg); } }
```

### 6.7 决策权重条（Decision Weight Bars · 屏2 AI 思考）

```html
<div class="weight-block">
  <span class="weight-tag">WEIGHING</span>
  <div class="weight-rows">
    <div class="weight-row">
      <span class="weight-name active">利用率</span>
      <div class="weight-track"><div class="weight-fill active" style="width:82%"></div></div>
      <span class="weight-val active">.82</span>
    </div>
    <!-- 其他维度 -->
  </div>
</div>
```

```css
.weight-block {
  display: flex; align-items: center; gap: var(--sp-12);
  padding: var(--sp-6) var(--sp-12);
  background: var(--paper);
  border: 1px solid var(--rule-soft);
  border-radius: 2px;
  margin-bottom: var(--sp-10);
}
.weight-tag {
  font-family: var(--font-mono);
  font-size: var(--fs-9);
  font-weight: 600;
  color: var(--ink-faint);
  letter-spacing: .18em;
  text-transform: uppercase;
}
.weight-rows { display: flex; flex: 1; gap: var(--sp-14); }
.weight-row {
  flex: 1;
  display: flex; align-items: center; gap: var(--sp-6);
}
.weight-name {
  font-family: var(--font-sans);
  font-size: var(--fs-10);
  font-weight: 600;
  color: var(--ink-soft);
  transition: color .35s var(--ease);
}
.weight-name.active { color: var(--vermilion); }
.weight-track {
  flex: 1;
  position: relative; height: 4px;
  background: var(--rule-soft);
  border-radius: 2px;
  overflow: hidden;
  min-width: 24px;
}
.weight-fill {
  position: absolute;
  top: 0; left: 0; bottom: 0;
  background: linear-gradient(90deg, var(--ink-faint), var(--ink-mute));
  opacity: .55;
  border-radius: 2px;
  width: 0;
  transition: width .8s var(--ease-out), background .35s var(--ease), opacity .35s var(--ease);
}
.weight-fill.active {
  background: linear-gradient(90deg, var(--vermilion-4), var(--vermilion-6));
  opacity: 1;
}
.weight-fill.active::after {
  content: '';
  position: absolute;
  top: -1px; right: -3px;
  width: 12px; height: 6px;
  background: linear-gradient(90deg, transparent, var(--vermilion-7));
  filter: drop-shadow(0 0 5px var(--vermilion-6));
  animation: weight-pulse 1.2s ease-in-out infinite;
}
@keyframes weight-pulse {
  0%, 100% { opacity: 1; }
  50%      { opacity: .35; }
}
.weight-val {
  font-family: var(--font-mono);
  font-size: var(--fs-10);
  font-weight: 600;
  color: var(--ink-mute);
  font-variant-numeric: tabular-nums;
  min-width: 20px;
  text-align: right;
  transition: color .35s var(--ease);
}
.weight-val.active { color: var(--vermilion); }
```

### 6.8 芯片厂商墙（Chip Wall · 屏2 底部 15 列）

```html
<section class="row-chips">
  <span class="eyebrow">12 国产芯片</span>
  <div class="chip-wall">
    <div class="chip-card front">
      <div class="chip-head">
        <span class="chip-icon"><img src="logo/1. 华为昇腾.png" alt="华为昇腾"></span>
        <div class="text">
          <span class="brand">华为昇腾</span>
          <span class="model">Ascend 910B</span>
        </div>
      </div>
    </div>
    <!-- 12 个 chip-card -->
  </div>
</section>
```

```css
.row-chips {
  display: grid;
  grid-template-columns: minmax(96px, 8%) 1fr;
  align-items: center;
  padding: var(--sp-10) var(--sp-16);
  gap: var(--sp-14);
  overflow: hidden;
}
.chip-wall {
  display: grid;
  grid-template-columns: repeat(15, minmax(0, 1fr));
  background: var(--paper-cool);
  border: 1px solid var(--rule);
  border-radius: 4px;             /* ← 已登记的 4px 例外 */
  overflow: hidden;
}
.chip-card {
  position: relative;
  padding: var(--sp-10) var(--sp-6);
  border: none;
  border-right: 1px solid var(--rule-soft);
  border-radius: 0;
  display: flex; flex-direction: column;
  align-items: center; justify-content: flex-start;
  text-align: center;
  gap: var(--sp-6);
  transition: background .25s var(--ease);
}
.chip-card:last-child { border-right: none; }
.chip-card.front {
  background: linear-gradient(135deg, var(--success-bg) 0%, var(--paper-warm) 70%);
  border-color: var(--success);
}
.chip-icon {
  width:  clamp(36px, 4vh, 52px);
  height: clamp(36px, 4vh, 52px);
  display: inline-flex; align-items: center; justify-content: center;
  border-radius: 4px;             /* ← 已登记的 4px 例外 */
  overflow: hidden;
  color: var(--vermilion);
}
.chip-icon img {
  width: 100%; height: 100%;
  object-fit: contain;
  display: block;
  filter: brightness(1.05);
}
.chip-card.front .chip-icon {
  background: linear-gradient(135deg, var(--success-bg), var(--paper));
  color: var(--success);
}
.chip-card .brand {
  font-family: var(--font-sans);
  font-size: var(--fs-12);
  font-weight: 600;
  color: var(--ink);
  letter-spacing: .02em;
}
.chip-card .model {
  font-family: var(--font-mono);
  font-size: var(--fs-9);
  color: var(--ink-mute);
  letter-spacing: .08em;
}
```

### 6.9 Mini Sparkline 折线图

```html
<svg class="chart-svg" viewBox="0 0 320 60" preserveAspectRatio="none">
  <defs>
    <linearGradient id="grad" x1="0%" y1="0%" x2="0%" y2="100%">
      <stop offset="0%"   stop-color="var(--vermilion)" stop-opacity=".25"/>
      <stop offset="100%" stop-color="var(--vermilion)" stop-opacity="0"/>
    </linearGradient>
  </defs>
  <path d="M 0,40 L 40,32 L 80,28 …" fill="url(#grad)"/>
  <path d="M 0,40 L 40,32 L 80,28 …" fill="none" stroke="var(--vermilion)" stroke-width="1.4"/>
</svg>
```

```css
.chart-svg { width: 100%; height: 60px; display: block; }
```

> **严格约束**：
> - 填充用同色 25% → 0% 渐变（不允许彩色渐变）
> - 线条 stroke-width 1.4
> - 异常点用 3px 圆点 + error 色标注
> - **禁止**渐变发光 / glow filter

### 6.10 Alert Bar · 警示条（屏2 / 屏3 共享）

```html
<div class="tile-alert-bar">
  <span class="icon">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
      <path d="M10.29 3.86 1.82 18a2 2 0 0 0 1.71 3h16.94a2 2 0 0 0 1.71-3L13.71 3.86a2 2 0 0 0-3.42 0z"/>
      <line x1="12" y1="9" x2="12" y2="13"/>
      <line x1="12" y1="17" x2="12.01" y2="17"/>
    </svg>
  </span>
  <div class="body">
    <span class="head">WARNING · 10:24:35</span>
    <span class="sep">·</span>
    <span class="msg">d-ai-gpu-05 节点连接失败 · GPU 温度异常 · 触发实时隔离</span>
  </div>
</div>
```

```css
.tile-alert-bar {
  display: none;                       /* 默认隐藏 · is-alert 时显示 */
  position: absolute;
  left: var(--sp-32); top: var(--sp-24);
  max-width: calc(100% - var(--sp-32) * 2);
  z-index: 5;
  align-items: center;
  gap: var(--sp-10);
  padding: var(--sp-6) var(--sp-12);
  background: rgba(42, 24, 24, .92);
  border: 1px solid var(--error);
  border-left-width: 4px;
  border-radius: 2px;
  overflow: hidden;
}
.tile.is-alert .tile-alert-bar { display: inline-flex; }
.tile-alert-bar::before {
  content: '';
  position: absolute;
  left: 0; top: 0; bottom: 0;
  width: 4px;
  background: var(--error);
  animation: live-pulse 1.4s ease-in-out infinite;
}
.tile-alert-bar .icon  { color: var(--error); }
.tile-alert-bar .head  {
  font-family: var(--font-mono);
  font-size: var(--fs-10);
  font-weight: 600;
  letter-spacing: .14em;
  text-transform: uppercase;
  color: var(--error);
  white-space: nowrap;
}
.tile-alert-bar .msg {
  font-family: var(--font-sans);
  font-size: var(--fs-12);
  color: var(--ink);
  font-weight: 500;
  overflow: hidden;
  text-overflow: ellipsis;
}
.tile.is-alert {
  box-shadow: 0 0 0 1px var(--error) inset;   /* 已登记的 inset shadow 例外 */
  z-index: 1;
}
```

### 6.11 监控墙 Tile（屏3 · 视频 + 站点标识）

```html
<div class="tile">
  <video class="tile-stream" src="video/xxx.mp4" autoplay muted loop playsinline></video>
  <div class="tile-overlay"></div>
  <div class="tile-alert-bar">…（见 6.10）</div>
  <div class="tile-bl">
    <span class="cat">超算</span>
    <span class="name">国家超级计算郑州中心 · SCNet 机房</span>
    <span class="sub">
      <span class="v">郑州 · 河南</span>
      <span class="sep">·</span>
      <span class="v">22.4°C / 45%</span>
      <span class="sep">·</span>
      <span class="v">PUE 1.18</span>
    </span>
  </div>
  <div class="tile-br" id="ts-1">14:01:16.842</div>
</div>
```

```css
.tile-stream {
  position: absolute; inset: 0;
  width: 100%; height: 100%;
  object-fit: cover;
}
/* 上下黑色 letterbox 渐变 · 保证标识可读 */
.tile-overlay {
  position: absolute; inset: 0;
  background: linear-gradient(180deg,
    rgba(20,22,27,.55) 0%, transparent 18%,
    transparent 70%, rgba(20,22,27,.78) 100%);
  pointer-events: none;
}

/* 左下标识 */
.tile-bl {
  position: absolute;
  bottom: var(--sp-24); left: var(--sp-32);
  right: var(--sp-32);
  z-index: 4;
  display: flex; flex-direction: column;
  gap: var(--sp-6);
}
.tile-bl .cat {
  font-family: var(--font-mono);
  font-size: var(--fs-11);
  letter-spacing: .28em;
  text-transform: uppercase;
  color: var(--ink-mute);
  display: inline-flex; align-items: center; gap: var(--sp-10);
}
.tile-bl .cat::before {
  content: '';
  width: 18px; height: 1px;
  background: currentColor;
}
.tile-bl .name {
  font-family: 'Noto Serif SC', serif;
  font-size: var(--fs-22);
  font-weight: 600;
  color: var(--ink);
  letter-spacing: .02em;
  line-height: 1.2;
}
.tile-bl .sub {
  font-family: var(--font-mono);
  font-size: var(--fs-11);
  color: var(--ink-mute);
  letter-spacing: .08em;
  display: flex; align-items: center; gap: var(--sp-8);
  flex-wrap: wrap;
}
.tile-bl .sub .sep { color: var(--rule); }
.tile-bl .sub .v   { color: var(--ink-soft); font-variant-numeric: tabular-nums; }

/* 右下毫秒时间戳 */
.tile-br {
  position: absolute;
  bottom: var(--sp-16); right: var(--sp-32);
  z-index: 4;
  font-family: var(--font-mono);
  font-size: var(--fs-11);
  letter-spacing: .08em;
  color: var(--ink-soft);
  background: rgba(20,22,27,.7);
  padding: var(--sp-4) var(--sp-8);
  border: 1px solid var(--rule);
  border-radius: 2px;
  font-variant-numeric: tabular-nums;
}
```

### 6.12 摄像头取景框角标 / 地图边角十字

监控画面或地图四角的 L 型印记：

```css
.scene-corner,
.map-corner {
  position: absolute;
  width: 16px; height: 16px;
  pointer-events: none;
  opacity: .85;
}
.scene-corner::before, .scene-corner::after,
.map-corner::before,   .map-corner::after {
  content: '';
  position: absolute;
  background: var(--vermilion-4);    /* 地图用 vermilion-4 / 监控取景用 vermilion */
}
.scene-corner::before, .map-corner::before { width: 14px; height: 1px; }
.scene-corner::after,  .map-corner::after  { width: 1px;  height: 14px; }
.scene-corner.tl, .map-corner.tl { top: 14px; left: 14px; }
.scene-corner.tr, .map-corner.tr { top: 14px; right: 14px; }
.scene-corner.tr::before, .scene-corner.tr::after,
.map-corner.tr::before,   .map-corner.tr::after   { right: 0; }
.scene-corner.bl, .map-corner.bl { bottom: 14px; left: 14px; }
.scene-corner.bl::before, .scene-corner.bl::after,
.map-corner.bl::before,   .map-corner.bl::after   { bottom: 0; }
.scene-corner.br, .map-corner.br { bottom: 14px; right: 14px; }
.scene-corner.br::before, .scene-corner.br::after,
.map-corner.br::before,   .map-corner.br::after   { right: 0; bottom: 0; }
```

### 6.13 雷达扫描（备用 · 当前三屏未启用）

```css
.radar-sweep {
  transform-origin: center;
  animation: sweep 4s linear infinite;
}
@keyframes sweep { to { transform: rotate(360deg); } }
```

---

## 7. 动效目录（按类别 · 按节奏排序）

> 同屏同时存在动效不超过 4 类。下表按节奏从快到慢列出，便于核对节奏冲突。

| 节奏 | 关键帧 | 时长 | 用途 |
|---|---|---|---|
| **极快** | `cell-flash` | .55s ease-out 单次 | 集群格子瞬亮 |
| 极快 | `spin` | .8s linear infinite | spinner |
| **快** | `cell-bflash` | 1.1s ease-out 单次 | baseline 偶发轻闪 |
| 快 | `weight-pulse` | 1.2s ease-in-out | 决策权重条头部光晕 |
| 标准 | `live-pulse` | 1.4s ease-in-out | LIVE / Alert flash |
| 标准 | `pulse-dot` | 1.4s ease-in-out | 流程步骤当前点 |
| 标准 | `shimmer` | 1.4s linear | skeleton |
| 标准 | `flow` | 1.6s linear | 数据流粒子 |
| 标准 | `dash` | 1.6s linear | 连接线虚线流动 |
| **中** | `phase-pulse` | 2.0s ease-in-out | phase-num 章节序号脉冲 |
| 中 | `ring` | 2.4s ease-out | 节点脉冲扩散 |
| 中 | `phase-spin` | 2.4s linear | phase-num 外圈光段 |
| 中 | `cell-breathe` | 2.6s ease-in-out | use 节点持续呼吸 |
| **慢** | `pulse-line` | 3.2s ease-out | 焦点链路脉冲 |
| 慢 | `sweep` | 4s linear | 雷达扫描 |
| 慢 | `col-flow` | 4.4s linear | is-active 卡四边流光 |
| 慢 | `slideUp` / `fadeIn` | var(--t-mid) ease | 入场 |

**入场动效**：

```css
@keyframes slideUp {
  from { opacity: 0; transform: translateY(6px); }
  to   { opacity: 1; transform: none; }
}
@keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
.appear { animation: slideUp var(--t-mid) var(--ease) both; }
```

**Reduced Motion 降级**（必带）：

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: .01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: .01ms !important;
  }
}
```

---

## 8. 反模式登记表（Anti-patterns）

### 8.1 视觉反模式

| ❌ 反模式 | ✅ 规范实现 |
|---|---|
| `border-radius: 6 / 8 / 12 / 16 px` | `2px`（已登记例外：`4px` 仅 col / chip-wall / chip-icon · `10px` 仅 `.badge-live` · `1px` 仅 `.node-cell` · `50%` 仅圆点） |
| 主色随意挑（`#3b82f6` / `#2563eb` / `#0ea5e9`） | `var(--vermilion-6)` `#7aa5db` |
| 标签写成 `<h3 style="font-size:14px">` | mono uppercase 10–12 + `letter-spacing: .14-.2em` |
| 卡片 / 按钮 `box-shadow: 0 2px 8px ...` | flat 默认；shadow 仅 §1.6 三处例外 |
| 重音用 `background: rgba(vermilion, .1)` 全填充 | 仅 `border-left: 3px solid var(--vermilion)` |
| 中文单独指定 PingFang / Noto Sans | 让 Inter 走 fallback |
| `transition: all .3s ease-in-out` | 引用 §1.7 token + 显式属性 |
| Noto Serif 标题用 `font-weight: 700` | 永远 600 |
| 用 emoji 充当 icon | lucide 风格 SVG `stroke-width: 1.8` |
| Logo 用 `<img>` 图片 | 字体写法 `<div class="logo-mark">Deep<em>Link</em></div>` |

### 8.2 大屏专属反模式

| ❌ 大屏反模式 | ✅ 规范实现 |
|---|---|
| 背景用纯黑 `#000` 或冷灰 `#1a1a1a` | `#0c0e13 ~ #14161b`（保留 ~5° 暖偏） |
| 数据用 ECharts 默认色板（#5470c6 / #91cc75）| 全部映射到 `--vermilion-N` / `--success` / `--warning` / `--error` |
| 节点 / 连接线带 `box-shadow` 发光 | 仅用 `ring` 脉冲扩散，不用阴影发光 |
| 文字 / 图标 `filter: drop-shadow` 霓虹效果 | 平面 · 仅 `.col.is-active::before` 流光层允许 drop-shadow 4px |
| 用 `radial-gradient(neon)` 模拟光晕 | 仅可用同色 25% → 0% 线性渐变（折线图填充） |
| 直接写 `font-size: 14px` | 全走 `var(--fs-N)` |
| 自造 `--fs-17` `--fs-19` 等非阶梯字号 token | 仅用 §1.3 已登记的阶梯 |
| 响应式 `@media (max-width: ...)` 改变骨架 | 仅允许密度降级，三级断点（1400/980/720）已登记 |
| 旋转 360° 的装饰图案（如旋转齿轮） | 只允许雷达 sweep / phase-spin / spinner 三处 |
| 同屏 5 种以上不同节奏动效 | 控制在 ≤ 4 种 |
| 数值用 `font-variant-numeric: proportional-nums` | 必须 `tabular-nums` |
| 大段中文用 italic | placeholder / quote 之外**禁止** italic 中文 |
| 用 `text-shadow` 强化大字可读性 | 大字本身字号够大 · 不需 shadow |
| 用 `mix-blend-mode: screen` 叠加发光 | **禁止** |
| 翻牌器 / flip clock 等装饰性数字动效 | **禁止** · 数字平静更新即可 |
| 全屏粒子背景 / matrix 雨 / 星空 | **禁止** · 背景必须保持 `--paper` 纯色 |
| 显示一个"假"实时数字（每秒 +1） | 数值要么真实驱动 · 要么静态显示 · 禁止假动 |
| 屏3 监控画面里放广告 / Slogan | 监控画面只显示监控内容 |
| 留 debug 浮标 `.debug-info` 进部署版 | 必须删除 |

---

## 9. 一致性验收 Checklist

### 视觉签名
- [ ] 整屏背景为 `#0c0e13 ~ #14161b` 区间，**非**纯黑 / 冷灰
- [ ] 所有圆角为 2px（已登记例外：4px / 10px / 50% / 1px）
- [ ] 主色 `#7aa5db` 仅用于关键操作与重音
- [ ] 卡片 / 按钮无 `box-shadow`（已登记三处例外除外）
- [ ] 至少能看到一处「mono uppercase + .14-.2em letter-spacing」label

### 排版
- [ ] Logo 为字体写法 Cormorant Italic 22 / 500，em 朱砂正体 600（**不允许图片**）
- [ ] Noto Serif SC 标题永远 600
- [ ] 所有元信息为 JetBrains Mono Uppercase
- [ ] 所有数值带 `font-variant-numeric: tabular-nums`
- [ ] 中文未单独指定字体

### 画布 / 响应式
- [ ] 使用 1920×1080 设计基准 + `vh-clamp` 字号/间距 + `16:9 stage 守护`
- [ ] 字号全部走 `var(--fs-N)`，未直接写 px
- [ ] 间距全部走 `var(--sp-N)`，未直接写 px
- [ ] 仅出现 1400 / 980 / 720 三级断点（如有）
- [ ] 内容全部在 1920×1080 内排开，无主滚动条
- [ ] debug 浮标已删除

### 颜色 / 数据
- [ ] 节点类型用色相区分（vermilion / success / warning），不用形状 / 大小
- [ ] 数据图表无渐变发光 / filter glow
- [ ] success / error 不仅靠颜色区分，必附图标或文字
- [ ] `--ink-faint` 仅承载非关键信息
- [ ] ECharts 系列色已全部映射到主色 7 阶或语义色

### 动效
- [ ] 同屏不同节奏动效 ≤ 4 种
- [ ] 已实现 `prefers-reduced-motion` 降级
- [ ] 无随意 transform 旋转（仅 sweep / phase-spin / spinner 允许 360°）
- [ ] 无 box-shadow 发光、无霓虹光晕、无粒子背景

### 反模式扫描
- [ ] §8 表中每一项 ❌ 在最终代码中均不出现

---

## 10. AI 协作契约

1. **声明唯一真实来源**：动手前显式声明
   > 我会以 `deeplink大屏-style-guide.md v2.0` 为唯一参考，不依赖训练数据中的大屏设计直觉，不参考阿里 dataV / 腾讯 Hippy / ECharts 等已有大屏 demo

2. **复制而非生成**：先在 §5 / §6 找最接近模式，复制后调整 className 与文案；找不到则组合现有模式

3. **AI 输出后自检**：
   > 请逐条对照 `deeplink大屏-style-guide.md §8 反模式登记表`与`§9 Checklist`，列出本次输出中每一条违规位置（文件:行号 + 违规项 + 应改为）。仅输出清单。

4. **commit 中注明**：`conform: deeplink-v2.0`

---

## 11. 与 v1.0 的差异 · Changelog

### v2.0 (2026-06-12)

**响应式系统重构**
- 🆕 字号 / 间距全部 vh-clamp 化（`--fs-N` / `--sp-N` 走 clamp(min, vh, max)）
- 🆕 画布从「固定 1920×1080 + transform: scale()」改为「`width:100vw` + `height:min(100vh, 56.25vw)` 16:9 stage 守护」
- 🆕 三屏统一 topbar 高度 token `--topbar-h: clamp(60px, 7vh ~ 7.4vh, 96px)`
- 🆕 允许预览阶段三级断点（1400 / 980 / 720），但禁止改变骨架，仅密度降级

**Logo · 字体写法恢复（重大）**
- 🔴 **正式废止 `<img src="DeepLink Logo.png">` 写法**
- 🆕 恢复 `<div class="logo-mark">Deep<em>Link</em></div>` 字体写法（Cormorant Italic 22 / 500 + em 朱砂正体 600）
- 🆕 §2.1 升级为强制规则 · 加入反模式 §8.1

**基底色调整**
- 🆕 允许 `#0c0e13 ~ #14161b` 区间（屏1/2 偏沉 · 屏3 偏温和）
- 🆕 三屏 `--paper-warm` / `--paper-cool` 微调以匹配各自 paper 基色

**新增大屏特有模式**
- 🆕 §6.5 `.col.is-active` 四边流光卡（屏2 标志性视觉 · col-flow 关键帧）
- 🆕 §6.6 `phase-num` 环节序号章（active 时朱砂脉冲 + 外圈光段旋转）
- 🆕 §6.7 决策权重条（weight-block · 屏2 AI 思考）
- 🆕 §6.8 芯片厂商墙（15 列 grid · chip-card · chip-icon）
- 🆕 §6.11 监控墙 Tile（屏3 视频 + 站点标识 + 毫秒时间戳）
- 🆕 §6.4 集群网格 16 列升级（baseline / use / flash / err 四态 + cell-breathe / cell-flash / cell-bflash 三关键帧）

**动效目录全表**
- 🆕 §7 按节奏排序的动效目录（17 类关键帧）

**反模式扩充**
- 🆕 §8.1 加入 "Logo 用 img" / "直接写 px" / "自造非阶梯字号 token"
- 🆕 §8.2 加入 "留 debug 浮标进部署版"

**移除**
- ➖ v1.0 §1.9 `transform: scale()` 缩放策略（已被 16:9 stage 守护取代）
- ➖ v1.0 §10 与 DiscoveryOS 派生关系说明（v2.0 起作为独立规范维护）

### v1.0 (2026-06-01)

- fork 自 DiscoveryOS v2.2
- 建立单一深色主题 + 1920×1080 transform scale 基线
- 登记 10 类大屏特有模式

---

**End of Specification · DeepLink 大屏 v2.0**

> 这份规范不是某一屏的样式快照，而是它们的"行为协议"。任何外部合作方拿到这一份文档 + 三屏 HTML 源码（位置见 frontmatter `sources`），都能在不联系原作者的情况下：① 复原已有屏；② 新建第四块同构屏；③ 把任何一类组件移植到其他项目，且视觉、动效、文字层级保持 1:1 一致。
