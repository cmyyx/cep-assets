# cep-assets

[CEP (CEP Endfield Planner)](https://github.com/cmyyx/CEP) 的副仓库：存放体积大、且会被整体重生成的静态产物，
避免主仓库历史被它们反复重写。

## 当前内容

**仓库根目录 = 主仓 `public/game-i18n/` 的内容**，在主仓以子模块挂载：

```
cmyyx/CEP
└── public/game-i18n   <- submodule -> 本仓库根目录
```

- 内容：AKEData `TableCfg/I18nTextTable_*.json` 的导出（15 语言 × 按 8 MiB 分块的 JSON + `manifest.json`），
  供站点 **工具 → 游戏文案查询**（`/tools/game-i18n`）按需 fetch `/game-i18n/<locale>/<chunk>.json`。
- 生成方：主仓 `scripts/lib/export-game-i18n.ts`，由 `scripts/sync-game-data.ts --update` 调用；
  CI（`.github/workflows/sync-game-data.yml`）在同步流程里提交到本仓库，并在主仓 PR 里更新子模块指针。

## 约定

- **不要手工修改**：所有文件都由同步流程重生成，任何本地改动都会在下次同步被覆盖。
- 每次同步本仓库会产生一次全量重写（分块边界会平移），因此本仓库历史增长很快 —— 它不承载代码，
  需要瘦身时可以直接重写历史或重建仓库，主仓库不受影响。
- 消费方只有 CEP 主仓库；主仓的 `game-i18n-hash-manifest.ts`（构建时生成）负责给这些文件加内容指纹。
