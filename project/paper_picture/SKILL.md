---
name: pdf-slide-figure-extraction
description: >-
  Extract clean, complete single-page or cropped figures from a PDF (lecture
  slides, papers, reports) and embed them into notes. Use whenever you need to
  pull a circuit diagram, waveform, Bode plot, block diagram, timing/state
  diagram, or any figure out of a PDF instead of redrawing it. Renders the
  target page to PNG with pdftoppm, crops precisely with ImageMagick, and
  visually verifies the crop is not cut off before using it.
---

# PDF / Slide Figure Extraction (截图技巧)

> 把 PDF（课堂 slides / 论文 / 报告）里的图**完整、干净地抠出来**并插进笔记的通用工作流。
> 适用于任何带「文件系统 + 终端」的 AI agent（需要 `pdftoppm` 与 ImageMagick `convert/magick`，二者来自 poppler-utils 与 ImageMagick）。

## 0. 核心理念（先理解，再动手）

所谓「截图」**不是对屏幕拍照**，而是两步：

1. 用 `pdftoppm` 把 PDF 的**某一页整页渲染**成 PNG（比例 = 纸张比例）。
2. 再用 ImageMagick **按像素坐标裁切**出需要的区域。

裁切可以是**任意宽高比、任意区域** —— 切得对不对，全看裁切框坐标对不对，跟「能不能任意比例」无关。

**优先级原则：** 只要 PDF 里有现成的图，就**优先抠原图**，不要用 mermaid / 自画图替代（原图可读性远高于重画）。只有完全无图、纯文字 / 纯公式 / 纯表格的页，才用文字 / 公式 / 表格重排。

## 1. 何时插图 / 何时不插

### ✅ 必须抠原图
- 电路图、系统框图、数据流图、反馈框图（每种新拓扑都要有图）
- 信号波形、转移特性曲线、频响 / Bode 图
- 时序图、状态机图、流水线图
- 难以用文字 / 公式描述的几何示意图

### ❌ 不要插原图（改用文字 / KaTeX / 表格重排）
- 纯文字页（标题页、Outline 页、纯 bullet 页）
- 纯公式页 → 用 KaTeX 重排
- 表格页 → 用表格重排
- 单变量推导链 → 用公式列出
- 同一张图的多张相似截图 → 只留最有代表性的一张

## 2. 工作顺序（必须按此顺序）

1. **先把 PDF 从头到尾过一遍**，给每页标记 `必插 / 可选 / 不插`。
2. **写每节内容时，需要图的位置直接抠对应那一页** —— 不要先写完文字再回头补图，更不要先用占位图兜底。
3. **写完再扫一遍**，确认每个新拓扑 / 新波形 / 新框图都对应一张图。

## 3. 整页渲染（工作流 A，默认）

把需要的几页渲染成 PNG（`-r` 是 dpi，140 适合整页阅读，单图细节用 300+）：

```bash
cd /data && mkdir -p slides && \
  for p in 3 8 12 13 14 20; do \
    pdftoppm -png -r 140 -f $p -l $p slides.pdf slides/page && \
    mv slides/page-*.png slides/slide_$p.png; \
  done
```

整页就够用时，直接拿 `slides/slide_N.png` 嵌入即可。

## 4. 精确裁切单张图（工作流 A+，防切残）⭐

当某一页里**只想抠一张图 / 一个电路框 / 一条公式**（不要整页）时，按这四步做：

1. **高 dpi 渲染目标页**：
   ```bash
   pdftoppm -png -r 300 -f N -l N in.pdf out/p
   ```
   300 dpi 够清晰；细节多可调更高。

2. **先把整页 PNG 读进来看一眼**（这一步不能省）：目的是用眼睛确认这张图的**真实边界**（左到哪、右到哪、上下含不含 caption），记下大致像素范围。用 `identify out/p-NN.png` 拿整页尺寸作为坐标基准（如 300 dpi A4 ≈ 2481×3508）。

3. **按坐标裁切 + 去白边 + 补留白**：宁可框**大一点**再靠 `-trim` 自动收紧，绝不框小切残；记得**连图题号 / caption 一起裁**。
   ```bash
   # -crop 宽x高+X偏移+Y偏移：先框够大的一条带，再 -trim 去白边、-border 补留白
   convert out/p-04.png -crop 2480x1120+0+600 +repage -trim +repage \
     -bordercolor white -border 25 out/fig.png
   identify out/fig.png
   ```

4. **裁完再看一眼复核**：确认四周内容（框、箭头、标注、文字）一个都没缺，再使用 / 嵌入。**没复核过的裁切图不许直接用。**

> 🚨 **切残是最典型的错误。** 常见原因不是「不能任意比例」，而是裁切框坐标设小 / 设偏，把图右侧或下方切掉。
> **修正口诀：先看整页定边界 → 框大一点 → `-trim` 收边 → 裁完复核。**
> 一旦发现某张图不完整，立即按本流程重抠并替换。

## 5. 截图细节

- **只截图片本身**，不要把页边框、页码、装饰元素带进来。
- 保留原图上的标注（红框、箭头、高亮）。
- 给图加 caption，例如 `![Slide N — 标题](图URL)`。

## 6. 兜底（工作流 C）

只有在**完全拿不到 PDF 文件**（只有解析出来的文本、没有源 PDF）时，才用 mermaid / KaTeX / 表格强行补图，并在交付时明确告诉用户：「拿不到源 PDF，请把 PDF 发我，我再用整页渲染 + 精确裁切补真图」。

## 7. 交付前强制自检

- [ ] 每张「必插」图：要么嵌了干净的抠图，要么留了占位 + 在结尾明确提醒补图？
- [ ] 有源 PDF 却用了 mermaid 兜底（没走渲染+裁切）？→ 算违规，必须重做。
- [ ] 每张裁切图都「先看整页 → 框大 → trim → 复核」过，确认没切残？
- [ ] 是否避免了重复 / 纯文字页的无意义截图？

## 依赖与可移植性

- 必需命令行工具：`pdftoppm`（poppler-utils）、`convert` 或 `magick`（ImageMagick）、`identify`。
- 任何能读写文件 + 执行终端命令 + 回看图片的 AI agent 都可直接套用本 skill。
- 若目标平台无法执行终端命令，则只能退化为工作流 C（文字 / 公式 / 表格重排）。
