# Jeff 的技术站

IT 支持 / 开发 / 运维方向的个人站点。纯静态站点：无构建、无框架、无数据库，克隆下来双击就能看。

- 源码仓库：`https://github.com/EnormousKnob/personal-tech-notes`
- 线上地址：`https://personal-tech-notes.pages.dev`（Cloudflare Pages，首次部署后生效）
- 部署方式：`git push` → Cloudflare 自动拉取并发布，约 30~60 秒生效

---

## 一、文件都在哪，改什么看这里

| 文件 | 作用 | 常改的东西 |
|---|---|---|
| `index.html` | **首页**：首屏、能力域三卡、精选项目、项目清单、最近笔记、小狗同事、CTA | 自我介绍文案、项目卡片、统计数字 |
| `projects.html` | **项目页**：11 个项目按四类归档，每块带锚点（`#terminal`、`#snmp`、`#cam` 等） | 项目描述、状态标签、技术要点 |
| `notes.html` | **笔记页**：黑苹果专题 + 计划中的运维专题 | 笔记条目、计划选题 |
| `about.html` | **关于页**：工作方法、技术栈 | 履历、技能标签 |
| `404.html` | 找不到页面时的兜底页（含柯基），Cloudflare Pages 自动识别 | 提示语 |
| `style.css` | **全站样式表**，顶部 `:root` 是配色变量 | 颜色、字号、间距 |
| `site.js` | 文档页的搜索、目录高亮、图片放大、复制代码 | 一般不用动 |
| `overview.html`<br>`hardware.html`<br>`display.html`<br>`cpu.html`<br>`build.html`<br>`wifi.html`<br>`files.html` | 黑苹果 7 篇文档页 | 正文内容（`.prose` 区域） |
| `images/` | 卡通小狗 SVG + 真实照片 | 换图直接替换同名文件 |
| `search-index.json` | 文档页搜索用的索引 | 新增文档后需手动补条目 |
| `_headers` | Cloudflare 响应头（安全头 + 图片缓存） | 一般不用动 |
| `source-*` | 从 [zbook-g10-hackintosh](https://github.com/EnormousKnob/zbook-g10-hackintosh) 同步的原始文档、SSDT、config.plist、脚本 | **不要手改**，等上游更新后重新同步 |

---

## 二、日常最常做的 5 件事

### 1. 改一段文案
直接在对应 `.html` 里搜索原文关键词，替换文字即可。例如改首页标语：

```bash
# 在仓库目录下
grep -n "解决问题" index.html
```

### 2. 改配色
`style.css` 第一行就是配色变量，改这里全站生效：

```css
:root{
  --paper:#faf8f4;   /* 页面底色（米白） */
  --card:#fffdf9;    /* 卡片底色 */
  --ink:#28251f;     /* 正文深炭色 */
  --muted:#79746b;   /* 次要文字 */
  --line:#e7e1d8;    /* 分隔线 */
  --orange:#ab471e;  /* 强调色（砖橙） */
  --wash:#f4e7dc;    /* 浅橙背景 */
  --soft:#f2eee7;    /* 区块浅底 */
}
```

### 3. 新增一个项目
① 在 `projects.html` 里复制一个完整区块，改 `id`、标题、描述：

```html
<section class="proj-block" id="新锚点">
  <div class="proj-top"><span class="tag">分类</span><span class="status">开发记录</span></div>
  <h2>项目名</h2>
  <p>一两句话说明它解决什么问题。</p>
  <div class="stack-row"><span class="chip">技术1</span><span class="chip">技术2</span></div>
</section>
```

② 在 `index.html` 的「项目清单」里复制一张 `.proj` 卡片，链接指向 `projects.html#新锚点`。
③ 卡片数量建议保持偶数，否则网格末尾会空半格。

### 4. 新增一篇笔记
① 复制一份现有文档页（如 `wifi.html`）改名为新页面名，替换 `<main>` 里的正文；
② 在 `notes.html` 加一行 `.note-row` 指向它；
③ 在 `search-index.json` 补一条索引，文档页才能被搜索到；
④ 如果希望侧边栏也能进入，在相关文档页的 `.layout .sidebar` 里加一个链接。

### 5. 换小狗
`images/dog-noir.svg`（夜班·法斗）和 `images/dog-corgi.svg`（白班·柯基）是手绘矢量图，直接用文本编辑器打开就能改颜色；换成自己的图时保持同名覆盖即可，SVG 优先（不糊、体积小）。

---

## 三、本地预览

在仓库目录执行任意一条：

```bash
py -m http.server 8080        # Windows
python -m http.server 8080    # macOS / Linux
```

浏览器打开 `http://localhost:8080`。改完文件刷新即可，不需要重启服务。

> 不要直接双击 `index.html`。用 `file://` 打开时，部分浏览器会拦截 `search-index.json` 的读取，搜索功能会失效。

---

## 四、提交并发布

```bash
git add -A
git commit -m "更新：补充 XX 项目"
git push origin main
```

推送后 Cloudflare Pages 会自动构建，**约 30~60 秒**后线上更新，链接不变。

查看构建进度：Cloudflare 控制台 → Workers & Pages → 你的项目 → **Deployments**，最新一条状态变为 `Success` 即已上线。

---

## 五、首次部署 Cloudflare Pages（只需做一次）

1. 打开 <https://dash.cloudflare.com> 注册 / 登录（免费账户即可，不需要买任何东西）。
2. 左侧进入 **Workers & Pages** → **Create** → 选 **Pages** 标签 → **Connect to Git**。
3. 授权 GitHub，选择仓库 **`personal-tech-notes`**，分支选 **`main`**。
4. 构建设置按下表填写（**这个站点没有构建步骤，千万别填 `npm run build`**）：

   | 选项 | 填什么 |
   |---|---|
   | Framework preset | `None`（或 Preact 之外的任意项都不要选） |
   | Build command | **留空** |
   | Build output directory | `/` |
   | Root directory | 留空 |

5. 点 **Save and Deploy**，等 1 分钟左右，得到 `https://personal-tech-notes.pages.dev`。

此后：每次 `push` 到 `main` 自动发布正式站；向其它分支推送或发 PR 会生成**预览地址**，可以先看效果再合并。

---

## 六、绑定自己的域名（可选）

**已有域名（在 Cloudflare 托管 DNS）**：
Pages 项目 → **Custom domains** → **Set up a custom domain** → 输入 `www.你的域名` → Cloudflare 自动添加 CNAME，证书自动签发。

**域名在别家注册商**：
在域名商后台加一条 CNAME 记录，指向 `personal-tech-notes.pages.dev`，再回到 Pages 里添加该域名验证。

**关于国内访问**：Cloudflare Pages 全球 CDN 免费，国内可达性一般可用但速度不算快（通常比 GitHub Pages 稳定）。如果以后要面向国内访客追求速度，只有两条路：
- 域名备案 + 国内云厂商（阿里云 / 腾讯云）对象存储或轻量服务器，年成本百元级；
- 继续用境外节点，接受速度现状。

免费前提下，Cloudflare Pages 是性价比最高的选择。

---

## 七、出问题怎么回滚

- **回到上一版**：Deployments → 找到上一个 `Success` 的部署 → 右上角 **Rollback**，10 秒生效，源码不受影响。
- **看构建失败原因**：Deployments → 点失败的部署 → 查看 Build log。静态站几乎不会失败，常见原因是文件名大小写或提交了超大文件（单文件上限 25 MiB）。
- **改完没生效**：先确认 `git push` 成功（`git log origin/main -1`），再看 Deployments 是否在构建；浏览器按 `Ctrl+F5` 强制刷新。

---

## 八、维护节奏建议

| 频率 | 做什么 |
|---|---|
| 随手 | 解决一个新问题，就补一段到 `projects.html` 对应项目下 |
| 每月 | 检查项目状态标签是否还准确（`开发记录` / `设计+部分实现` / `探索阶段`） |
| 每季度 | 同步一次上游黑苹果仓库的 `source-*` 文件；检查外链是否失效 |
| 年度 | 复盘：删掉不打算继续做的项目，比堆着更有说服力 |

---

## 九、内容来源与许可

黑苹果相关文档、SSDT、`config.plist`、`wifi-fix.sh` 等同步自
<https://github.com/EnormousKnob/zbook-g10-hackintosh>（快照 `8d82cced`），上游许可见 `source-LICENSE`，以 `source-` 开头的文件均为原始副本，请勿手改。

其余页面文案、样式与卡通插图为本站原创。
