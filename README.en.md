# Antigravity Chinese Localization — Personal Custom Build

A personal-use repo of the Chinese localization patch for [Google Antigravity](https://antigravity.google/).

> **This is not an original project.** All core localization work comes from the open-source
> [antigravity-cn-community/Antigravity-Chinese-Localization](https://github.com/antigravity-cn-community/Antigravity-Chinese-Localization)
> project (by [@aabbWei](https://github.com/aabbWei) and community contributors, MIT License).
> Full credit goes to the upstream author and community.

This repo is a personal backup of the upstream patch plus a few customizations I added for my own daily use:

1. **Suggestion card dictionary entries** (`src/dictionary.js`) — translations for the three server-driven suggestion cards on the welcome page, which upstream's dictionary does not cover yet.
2. **Composer prompt auto-translation patch** (`__agyComposerPromptPatch` in `src/dom-translator.js`) — clicking a suggestion card inserts a different English prompt template into the input box. This patch (running in the main world via `webFrame.executeJavaScript` to bypass contextIsolation) rewrites such drafts to Chinese through Lexical's model-level API when the whole text exactly matches a known template. Mention pills are preserved, and since the editor state itself is rewritten, **the sent message is Chinese too**. User-typed content is never touched.
3. **Misc** (`.gitignore`) — exclude the generated `extracted/` working directory.

See the [Chinese README](README.md) for usage details, and the [upstream README](https://github.com/antigravity-cn-community/Antigravity-Chinese-Localization#readme) for the full feature documentation. Licensed under the upstream [MIT License](LICENSE).
