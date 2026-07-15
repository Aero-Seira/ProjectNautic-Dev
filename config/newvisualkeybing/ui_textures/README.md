# NewVisualKeybing — 自定义界面材质

在本文件夹放入 PNG 文件即可重绘模组界面；随后打开可视化键位界面，点击皮肤按钮切换到「自定义」即可生效。

## 材质包

材质以「材质包」为单位组织，同一时刻仅一个生效：
- 直接放在本文件夹中的散装 PNG 构成「默认（散装文件）」材质包。
- 含 PNG 的子文件夹是一个以文件夹名命名的材质包。
- 一个 `.zip` 压缩包是以文件名命名的材质包（材质可位于压缩包根目录，或位于单层包裹文件夹内；每个包可自带 `pack.json`）。
- Shift+点击皮肤按钮可切换到下一个材质包（同时会重新加载）。当前材质包会记录在配置中。

## 工作方式

- 下方每个组件读取一个与文件名完全一致的 PNG。缺失的文件回退到内置原版外观，因此可只替换想改的部分。
- 格式：带透明通道的 32 位 PNG（RGBA）。任意尺寸均可，无需 2 的幂次。
- 九宫格：`border`（四角内缩像素）保持不拉伸，边与中心随控件尺寸缩放。下表列出各槽位默认值，可在 `pack.json` 中覆盖。
- 拉伸：整图拉伸填满目标矩形。
- 平铺：按原始尺寸重复填充。
- `key` 与 `mouse_button` 会按按键状态色正片叠底着色——请画成浅灰/白以便显色；也可提供逐状态的 `key_*` 文件精确控制（不着色绘制）。
- 修改文件后，重开界面或 Shift+点击皮肤按钮即可重新加载/切换材质包。

## 组件清单

| 文件 | 缩放 | 默认边框 | 着色 | 推荐尺寸 | 用途 |
|---|---|---|---|---|---|
| `panel.png` | nine-slice | 10 px | 否 | 48×48 px | 所有面板/窗口的背景（模组列表、鼠标、详情、弹窗）。 |
| `button.png` | nine-slice | 4 px | 否 | 80×24 px | 普通（默认）工具栏/标题栏按钮。 |
| `button_hover.png` | nine-slice | 4 px | 否 | 80×24 px | 悬停/聚焦的按钮；缺失时回退到 button。 |
| `button_disabled.png` | nine-slice | 4 px | 否 | 80×24 px | 禁用的按钮；缺失时回退到 button。 |
| `editbox.png` | nine-slice | 3 px | 否 | 64×24 px | 默认文本框（搜索框、名称输入框）。 |
| `editbox_focused.png` | nine-slice | 3 px | 否 | 64×24 px | 聚焦的文本框；缺失时回退到 editbox。 |
| `tooltip.png` | nine-slice | 6 px | 否 | 32×32 px | 悬停提示框边框。 |
| `key.png` | nine-slice | 6 px | 是 | 32×32 px | 通用键帽，会按按键状态色正片叠底着色；建议用浅灰以便显色。 |
| `key_free.png` | nine-slice | 6 px | 否 | 32×32 px | 未绑定按键（覆盖该状态下着色的 key，按原色绘制）。 |
| `key_self.png` | nine-slice | 6 px | 否 | 32×32 px | 绑定到本模组的按键（不着色覆盖）。 |
| `key_other.png` | nine-slice | 6 px | 否 | 32×32 px | 绑定到其他模组/单一绑定的按键（不着色覆盖）。 |
| `key_combo.png` | nine-slice | 6 px | 否 | 32×32 px | 组合键中的按键（不着色覆盖）。 |
| `key_conflict.png` | nine-slice | 6 px | 否 | 32×32 px | 存在冲突绑定的按键（不着色覆盖）。 |
| `keyboard_chassis.png` | nine-slice | 12 px | 否 | 64×64 px | 按键所在的托盘/底板。 |
| `mouse_body.png` | stretch | — | 否 | 80×116 px | 鼠标外形；拉伸填充鼠标主体区域（保持 80×116 比例效果最佳）。 |
| `mouse_button.png` | nine-slice | 6 px | 是 | 32×32 px | 鼠标按键格，按状态着色；缺失时回退到 key。 |
| `background.png` | tile | — | 否 | 32×32 px | 全屏背景，按原始尺寸平铺（类似原版泥土菜单背景）。 |

## pack.json（可选）

可覆盖每个槽位的 `border`（九宫格边框）、`tint`（是否着色）与 `scale`（缩放模式：`nineslice` / `stretch` / `tile`）。已在同目录生成可直接编辑的模板。
