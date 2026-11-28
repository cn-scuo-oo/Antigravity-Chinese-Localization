# Antigravity 汉化 · 个人定制版

个人使用的 [Google Antigravity](https://antigravity.google/) 中文汉化补丁仓库。

> **这不是原创项目。** 核心汉化引擎全部来自开源项目
> [antigravity-cn-community/Antigravity-Chinese-Localization](https://github.com/antigravity-cn-community/Antigravity-Chinese-Localization)
> （作者 [@aabbWei](https://github.com/aabbWei) 及社区贡献者，MIT License）。
> 感谢原作者和社区的辛勤付出，本仓库的所有汉化能力都来自上游项目。

本仓库只是我把上游补丁存了一份、并在日常使用中加了点自己需要的小功能，供个人自用。

## 本仓库增加了什么

相对上游，只有两处功能改动：

1. **建议提示卡片词条**（`src/dictionary.js` 末尾）
   新对话欢迎页那三张服务端下发的建议卡片（会议简报 / 会议笔记 / 幻灯片）原本不在上游词典里，补上了翻译。

2. **输入框提示词自动汉化补丁**（`src/dom-translator.js` 中的 `__agyComposerPromptPatch`）
   点击建议卡片后，实际填入输入框的是另一句英文提示词模板（和卡片文案不一样）。这个补丁会在主世界（绕过 contextIsolation）监听输入框，当草稿整段精确匹配已知模板时，通过 Lexical 模型层 API 把它改写为中文：
   - `@提及` 药丸（Google Calendar / Drive / Slides）完整保留；
   - 改的是编辑器内部状态，所以**发送出去的消息也是中文**，所见即所发；
   - 只做整段精确匹配，绝不碰用户自己输入的内容。

另有文档（本 README）、版本追踪号与 `app.asar.ready` 就绪包随官方版本同步更新的差异。

如果 Google 以后更换建议卡片的文案，需要在 `dictionary.js` 和 `COMPOSER_PROMPT_MAP`（`dom-translator.js` 内）两处补新词条，然后重新执行汉化。

## 怎么用

需要 Node.js（≥ 18.18）。本包已测试适配官方客户端 **Antigravity v2.36.0+**（离线就绪包 `app.asar.ready` 同步生成；更老的官方版本请使用上游对应 Release）。

- **macOS / Linux**：运行 `./运行汉化.sh`，浏览器会打开 `http://127.0.0.1:3388` 管理页面；
- **Windows**：双击 `双击运行汉化.bat`。

在管理页面点"开始汉化"即可：脚本会自动退出 Antigravity → 备份官方 `app.asar`（`app.asar.bak`）→ 注入汉化引擎 → 重打包部署。之后手动启动 Antigravity 就能看到中文界面。

改过词典或补丁后，重新跑一遍同样的流程即可生效（流程是幂等的）。

> macOS 提示"已损坏"时的排错命令：`xattr -cr /Applications/Antigravity.app && codesign --force --deep --sign - /Applications/Antigravity.app`

## 命令行参数（可选）

`node localize.js` 支持白名单参数（传入未识别的参数会被拒绝）：

| 参数 | 作用 |
| :--- | :--- |
| `--now` | 跳过管理页面，直接执行完整汉化打包部署 |
| `--apply-only` | 仅对已解包目录执行静态文件汉化注入 |
| `--auto-restore` | 官方更新后的自动恢复（智能重汉化或还原官方包） |
| `--check-status` | 以 JSON 输出客户端版本与补丁匹配状态 |

不带参数运行则启动 Web 管理面板（`http://127.0.0.1:3388`）。

## 与上游保持同步

```bash
git fetch upstream
git merge upstream/main   # 有冲突时保留本仓库的三处定制
# 然后重新执行一次汉化流程
```

上游地址已配置在 `upstream` remote 中。

## License

遵循上游的 [MIT License](LICENSE)，版权归 [aabbWei](https://github.com/aabbWei) 及 [antigravity-cn-community](https://github.com/antigravity-cn-community) 贡献者所有。本仓库的少量定制部分同样以 MIT 发布。

上游的完整功能介绍、汉化覆盖范围与详细文档，请直接查看[原项目 README](https://github.com/antigravity-cn-community/Antigravity-Chinese-Localization#readme)。
