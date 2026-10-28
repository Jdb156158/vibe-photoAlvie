# 制作步骤

1. **选型与确认**：从 Lucian 同类型控件中选出 061、282、292、349、389，先展示截图。用户要求真实图片后，以自然风景摄影替换渐变占位图，并核对操作前后效果。
2. **分镜与节奏**：沿用既有 Bento 布局。0–2 秒同时展示五种，2–17 秒依次讲解、每种 3 秒，17–20 秒总结。章节切点落在 120 BPM、2/4 拍的小节线上。详见 [STORYBOARD.md](STORYBOARD.md)。
3. **照片处理**：使用本地真实摄影；按展示框 cover 裁切，避免拉伸。像素转场的每个网格共享同一张图的裁切坐标，完成后精确拼接。
4. **影片实现**：`film/index.html` 用 Canvas 和单一时间轴，暴露 `draw(t)`；字体、照片加载完后才设置 `window.ready`。内容状态完全由时间计算，支持倒序与任意跳转。
5. **两轮静帧修订**：九个关键时刻检查构图、文字、裁切；改进开头五种效果的辨识度，调整标签对比度，并提前关闭归位以留出阅读停顿。
6. **音频合成**：`film/make_audio.py` 用 Python/numpy 合成 120 BPM 配乐、点击和呼啸等音效。第一拍 0 秒，20 秒包含 20 个完整 2/4 小节；18 秒落终止和弦，片尾淡出。
7. **逐帧导出**：Playwright 以每秒 60 帧调用 `draw(t)`，导出 1200 张 JPG；ffmpeg 合成 H.264 Main / AAC LC MP4，使用 yuv420p、BT.709、faststart、每秒关键帧。
8. **交互网页**：以相同照片、配色和 Bento 信息层级制作真实交互，不播放动画冒充操作。支持触屏、键盘、减少动态效果；切换控件时清理手势和在途转场。
9. **发布**：上传静态网页、影片、源码 ZIP 和 Markdown；GitHub Pages 从 `main` 分支根目录发布，`.nojekyll` 保证静态资源直接提供。完成后在线复查交互和媒体播放。

## 本地复现

仓库根目录：

```sh
python3 -m http.server 4860
```

浏览 `http://localhost:4860/`。

重新渲染影片（需 Node.js、Python/numpy、Chrome、ffmpeg）：

```sh
cd film
npm install
python3 -m pip install numpy
python3 make_audio.py
npm run stills
npm run render
sh encode.sh
```

可用 `CHROME_PATH` 自定义 Chrome，`FRAME_DIR` 自定义帧输出目录。脚本里的默认路径面向 macOS。

影片由项目自带 Python 音轨生成器制作；如用 huashu-motion 打开工作台可编辑预览，但运行通用 `om sound/render` 会改写音轨，复现成片应使用以上步骤。

[完整 AI 提示词](PROMPTS.md) · [影片检查](FILM-QA.md) · [网页检查](QA.md)
