# bigNBniyu_editable_vector

将科研机制图、医学示意图及信息图重建为平滑、低节点、按元素分组的可编辑矢量图的 Codex skill。

A Codex skill for faithful, smooth, low-node scientific vector reconstruction with editable text and semantic object groups.

## 核心方法

- 按参考图的布局和科学对象重建，不直接交付自动描摹产生的大量碎片。
- 使用少量 Bézier 曲线、原生形状和渐变，保留重要轮廓与内部结构。
- 元素命名分组，文字尽量保留为可直接输入修改的文本。
- 核对文字、箭头和面板顺序，检查整图及 400–800% 局部。
- 局部修订仅修改指定对象及必要的相邻路径。

默认交付可编辑 SVG、预览 PNG、编辑说明和 ZIP；环境支持时同时提供矢量 PDF。原图模糊纹理、字体或遮挡部分可能需要近似，不能保证像素级 1:1。

完整工作流见 [SKILL.md](SKILL.md)。这是工作方法与验收指令，不是独立运行的图像转换软件；实际绘制与验证需要运行它的助手具备图像查看、文件编写及 SVG 渲染能力。

## 安装

仓库及展示名为 `bigNBniyu_editable_vector`。为遵循 Codex skill 命名格式，内部标识和安装目录使用 `bignbniyu-editable-vector`。

将本仓库下载后，把含有 `SKILL.md` 和 `agents/` 的目录命名为 `bignbniyu-editable-vector`，放进 Codex 的 skills 目录。默认位置是 `~/.codex/skills/`；如果设置了 `CODEX_HOME`，则使用其下的 `skills/`。

也可以让 Codex 安装：

```text
安装这个 GitHub skill：https://github.com/hejzgw711/bigNBniyu_editable_vector
安装目录名使用 bignbniyu-editable-vector。
```

## 使用

附上参考图片，再发送：

```text
使用 $bignbniyu-editable-vector 把这张图重建成可编辑矢量图。
保留原布局和全部文字，曲线平滑、节点适量，每个有意义的元素分别命名分组。
交付 SVG、预览图和打包文件；条件允许时同时导出矢量 PDF。
```

本仓库仅包含 skill 和使用说明，不包含用户原始图片、科研数据或聊天记录。
