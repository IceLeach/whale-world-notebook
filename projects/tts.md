# TTS（大肥鱼的声音）

> 调研于 2026-11-15。目标：本地训练一个属于大肥鱼的 TTS 模型，通过插件让她能说话、也能给视频配音。

## 结论先行

- **主方案：GPT-SoVITS（v2Pro）微调**。8GB 显存可训练（batch 1–2），中文强，自带人声分离 / 切片 / ASR 工具链，适合「固定角色音色」。
- **备选：IndexTTS2 零样本**。5 秒参考音频即可试音，情感控制强，但推理慢、显存约 5.5GB，更适合快速验证音色。
- 两者可以并存：先用 IndexTTS2 快速试音，定下音色后再用 GPT-SoVITS 微调出稳定模型。

## 本机硬件

| 项 | 值 |
|---|---|
| GPU | RTX 3060 Ti，8GB VRAM |
| 内存 | 16GB |
| 系统 Python | 3.9（需要另建 3.10 环境） |
| ffmpeg | 无独立安装；bili-explore 自带 `ffmpeg-static` |
| torch / uv / conda / yt-dlp | 都没有 |

8GB 显存对 GPT-SoVITS：训练可跑但 batch 小、时间偏长；推理很轻松。

## 方案对比

| 方案 | 克隆方式 | 中文 | 情感控制 | 显存/训练 | 适合 |
|---|---|---|---|---|---|
| **GPT-SoVITS v2Pro/v4** | 1 分钟微调 / 5 秒零样本 | 强 | 弱（靠参考音频） | 训练 8GB 可跑 | 固定角色音色 |
| IndexTTS2 | 3–5 秒零样本 | 强 | 强（情感预设/指令） | 推理约 5.5GB，训练要求高 | 短视频配音、试音 |
| CosyVoice 3 | 零样本/少样本 | 强 | 中 | 流式、工业级，显存要求高 | 产品化、实时交互 |
| F5-TTS / Fish Speech | 零样本 | 中 | 中 | 中等 | 多语言、快速原型 |
| Ming-3.0-tts | 克隆 + 声音设计 | 强 | 中（14 种副语言标签） | 0.5B 小模型，MIT | 轻量本地部署 |

推荐 GPT-SoVITS 的理由：中文社区生态最完整、工具链自带（UVR5、slicer、ASR）、8GB 能训、推理快（4060Ti RTF 约 0.028）。

## 素材链路（B 站 → 训练集）

```
B 站视频
  → 下载 DASH 音频（bili-explore 的 playurl + ffmpeg-static，或 yt-dlp）
  → 转 16k/44.1k 单声道 wav
  → 人声分离（UVR5 / BS-Roformer，去掉 BGM 和伴奏）
  → 切片（audio_slicer，2–10 秒一段）
  → ASR 转写（FunASR 中文 / faster-whisper）
  → 人工校对（多音字、错字、语气词）
  → GPT-SoVITS 微调（SoVITS + GPT）
  → 推理服务（api_v2.py）
```

### bili-explore 能做什么

- `playurl` 匿名可拿 DASH 音视频流（已在其设计文档中确认）。
- `asr` 命令内部会「取最高码率音频 → ffmpeg 转 16k 单声道 → 转写」，**但不保存音频**。
- `read` 只给字幕和画面帧，不下载视频。
- 结论：**音频流拿得到，差的只是「落盘」这一步**。可以给 `asr` 加一个 `--save-audio`，或单独写一个小脚本复用它的 playurl 逻辑。

### 素材挑选标准

- 人声为主、背景音乐少（UVR5 分离效果才好）。
- 总时长 5–30 分钟，切出 1–10 分钟干净语音即可。
- 音质稳定、无混响、无多人抢话。
- 注意版权：B 站二创素材，训练自用；公开发布要谨慎。

## 插件方案（dsh-whale-tts）

```
大肥鱼 / 视频工具
      ↓ 调用
dsh-whale-tts 插件（注册 whale_speak 工具）
      ↓ HTTP
本地 GPT-SoVITS api_v2.py（127.0.0.1:9880）
      ↓
wav 文件（工作区内）
```

- 工具 `whale_speak`：`text`、`reference`（参考音频）、`emotion`（可选）、`speed`（可选）。
- 输出：wav 路径 + 时长。
- 大肥鱼可以直接「说话」，也可以把 wav 交给视频工具配音。
- 插件只管调用，不训练；训练是离线流程。

## 插件与本机配置

插件 `dsh-whale-tts` 在 `plugins/packages/tts`，已安装到 desktop profile（`file:` 本地目录）。

本机训练的模型与参考素材：

| 项 | 值 |
|---|---|
| SoVITS | `GPT-SoVITS-v5-20261006/SoVITS_weights_v4/deepseek-tts-v2_e12_s540_l32.pth` |
| GPT | `GPT-SoVITS-v5-20261006/GPT_weights_v4/deepseek-tts-v2-e18.ckpt` |
| 参考音频 | `tts-assets/V2/24_BV1ctpD6iE51.wav_0010490112_0010621184_(Vocals).wav` |
| 参考文本 | 请派兵或资金援助，白饭城愿以长期贸易和资金回报。 |
| 设备 / 版本 | cuda / v4 |
| 推理端口 / 控制端口 | 9880 / 9881 |
| Python | `GPT-SoVITS-v5-20261006/runtime/python.exe` |

注意：插件包的 `cordis.patch.yml` 已改成不带默认配置（开源用），本机重装插件后要在设置页重新填上面这些值，否则服务起不来。

发布状态：已随 `plugins` monorepo 推送到 GitHub（`dsh-whale-world-plugins`，`cbf91a6`）；插件文档已通用化。

## 风险

1. **8GB 训练可能 OOM**：需要 batch 1–2、fp16、短切片；不行就上云训练。
2. **素材版权**：B 站二创，训练自用可以，公开发布要确认授权。
3. **Python 环境**：系统是 3.9，GPT-SoVITS 要 3.10；需要 uv/conda 或独立 Python。
4. **模型下载**：HuggingFace 在国内可能慢，用 ModelScope / HF-Mirror。
5. **训练时间**：短数据集 20–40 分钟起步，长数据集更久。

## 待验证（→）

- [ ] bili-explore 的 playurl + ffmpeg-static 能否稳定落盘一段音频。
- [ ] 本机能否装起 GPT-SoVITS（Python 3.10 + torch CUDA + 依赖）。
- [ ] UVR5 模型能否下载（国内网络）。
- [ ] 8GB 显存微调一次的实际耗时与 OOM 情况。
- [ ] IndexTTS2 零样本在本机的推理速度与显存占用。

## 相关

- 素材来源：B 站
- 参考项目：https://github.com/RVC-Boss/GPT-SoVITS
- 备选：IndexTTS2、CosyVoice 3、F5-TTS、Fish Speech、Ming-3.0-tts
