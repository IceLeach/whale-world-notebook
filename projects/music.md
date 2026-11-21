# music（乐谱渲染）

## 目标

让大肥鱼创作音乐：她写 MusicXML 乐谱，插件渲染成 wav。

## 方案

方案 A：MuseScore Studio 4 命令行（`MuseScore4.exe -o out.wav in.musicxml`）。

对比过 FluidSynth + SoundFont 和纯 JS 合成：MuseScore 音质最好、MusicXML 支持最全，一步到位。

## 插件

- 包：`dsh-whale-music`（`plugins/packages/music`）
- 工具：`whale_compose`
- 参数：`musicxml` 或 `musicxml_path`、`out_path`、`unroll_repeats`、`force`、`timeout_ms`
- 设置页：MuseScore 可执行文件路径（唯一必要配置）
- 控制端口：9882
- 插件不缓存、不复制，只写 `out_path` 指定的那一个文件

## 本机

- MuseScore：`C:\Program Files\MuseScore 4\bin\MuseScore4.exe`（4.7.7）
- 输出：44.1kHz 立体声 32-bit float wav

## 已知问题

- 左右手写在同一小节但缺 `<backup>` → MuseScore 把它们串起来，时长翻倍。
- 显式写 `<sound tempo="120">` → 被解析成一半（实测 123 秒 vs 60 秒）；先用默认 120 BPM。
- 输出是 float wav，部分播放器不兼容；需要 16-bit 时得后处理。

## 试作

- 《海边奔跑》：D 大调、4/4、120 BPM、30 小节，63 秒（含混响尾音）。
- 文件：`music-assets/sea-run.wav` / `sea-run.musicxml`
- 结构：前奏 2 → A 8 → A2 8（高八度）→ B 8（海浪起伏）→ 尾声 4。

## 下一步

- 插件已随 `plugins` 仓库发布（`7e038d8`）。
- 可选：16-bit PCM 导出、乐谱预览（PDF/PNG）、给她的 MusicXML 校验工具。
