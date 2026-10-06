# yhm · 逻辑关系图

离线运行的逻辑关系图编辑器：输入逆波兰逻辑表达式，生成可选择节点的逻辑关系图，可编辑节点名称与备注，并导出 / 载入 JSON 模型。整个页面不联网、无构建步骤，直接由 GitHub Pages 托管。

在线地址：`https://<你的用户名>.github.io/yhm/`

## 功能

- 逆波兰表达式 → 逻辑图：支持与 `.`、或 `,`、非 `<`、推出 `>`、等价 `=`，以及常量 `0` / `1` 和中文变量名。
- 图在本地按有序二元决策图生成，重复分支、恒真、恒假表达式会自动简化。
- 画布交互：拖动节点、拖动空白处平移、滚轮 / 按钮缩放、适应画布、重新布局、悬停连线删除。
- 节点属性：编辑名称与备注并保存；总览小地图实时同步。
- 模型：始终导出当前图形，可载入旧版 `nodeArray` / `linkArray` 模型，保留备注与节点位置。
- 界面：自适应布局，跟随系统的浅色 / 深色外观，键盘可达（Tab 顺序、可见焦点、跳转到画布）。

## 目录结构

```
index.html      页面入口（GitHub Pages 打开的文件）
style.css       界面样式（设计变量、浅色 / 深色、响应式）
src/core.js     无 DOM 依赖：表达式解析、决策图生成、模型校验
src/app.js      画布、布局、缩放、节点属性、导入导出、错误反馈
assets/         节点图标
lib/            第三方库：JointJS / Backbone / jQuery / Lodash / Graphlib / Dagre
tests/          Node.js 原生测试
重构说明.md      本次重构的问题清单、算法说明与验证记录
.nojekyll       告诉 GitHub Pages 不要套用 Jekyll
```

## 本地预览

双击 `index.html` 即可使用（已在 Edge 中验证 `file://` 打开，图标与图形正常）。若想更接近线上环境，可在仓库根目录起一个本地服务：

```sh
python -m http.server 8000
npx serve .
```

然后打开 `http://localhost:8000/`。

## 使用

1. 输入逆波兰表达式，例如 `a b . fe >`。
2. 点击「生成逻辑图」，或按 Ctrl / ⌘ + Enter。
3. 拖动节点改变位置；拖动空白处平移；滚轮、加减按钮缩放。
4. 点击节点编辑名称和备注，点击「保存修改」。节点类型不能通过名称字段改变。
5. 「保存文件」导出当前图形；「载入文件」载入 `.json` / `.txt` 模型。

「查看 / 编辑 JSON 模型」中可用「仅解析」「文本转图」「图转文本」。保存文件始终导出当前图形；若只改了 JSON 文本，先点「文本转图」应用。

## 表达式规则

| 运算 | 输入 |
| --- | --- |
| 与 | `a b .` |
| 或 | `a b ,` |
| 非 | `a <` |
| 推出 | `a b >` |
| 等价 | `a b =` |

变量与运算符之间必须用空白分隔。支持中文变量名、单个变量和常量 `0` / `1`；变量名不能包含 `. , < > =`。重复分支、恒真和恒假表达式会自动简化，所以图形可能与原版不同。

为限制复杂输入：最多 512 个词元、64 个变量；计算中最多 4096 个决策节点、50000 次未缓存运算；载入最多 2000 个节点、6000 条连线，文件不超过 2 MB。不同变量顺序可能生成不同大小的图。

这是逻辑表达式图形编辑器，不含电路时序仿真，也不做逻辑正确性自动证明。

## 测试

需要 Node.js，无需安装依赖：

```sh
npm test
```

## 发布到 GitHub Pages

1. 新建一个 **Public** 仓库，名字用 `yhm`。
2. 把本目录下的**所有文件和文件夹**放到仓库根目录，保持原有层级（`index.html` 必须在最外层）。
3. 仓库 **Settings → Pages → Build and deployment**：Source 选 `Deploy from a branch`，Branch 选 `main`，目录选 `/ (root)`，保存。
4. 等 1~2 分钟，访问 `https://<你的用户名>.github.io/yhm/`。

注意：

- 文件名大小写敏感。GitHub Pages 运行在 Linux 上，`assets/SEL.svg` 的 `SEL` 必须保持大写。
- `.nojekyll` 已在仓库中，避免 Jekyll 处理静态文件。
- 仓库为 `yhm` 时地址是 `/yhm/` 这种项目页；若仓库改名为 `<用户名>.github.io`，地址会变成站点根目录，页面内的相对路径无需修改。
- 想换站点名字：改 `index.html` 的 `<title>`、顶栏 `.brand-name` 文本、`<meta name="description">`，以及本文件的标题。

## 许可与致谢

- 本项目代码：MIT，见 `LICENSE`。
- 重构自 [kuangdash.gitlab.io/logicsim](https://kuangdash.gitlab.io/logicsim/) 的本地副本。
- `lib/` 中的 JointJS、Backbone、jQuery、Lodash、Graphlib、Dagre 保留各自原有的版权与许可声明，未做升级。
