# qtcloud-data cli scope 版本检查报告

> 生成日期: 2026-07-31 | 检查对象: [quanttide/qtcloud-data](https://github.com/quanttide/qtcloud-data) `src/cli` | 检查方式: git tag / CHANGELOG / GitHub Release / 配置文件四维比对

## 摘要

cli scope 当前版本 0.2.0，配置文件（Cargo.toml / Cargo.lock）与 CHANGELOG 一致，已具备发布条件。历史版本存在 CHANGELOG 滞后与 Release 断档问题。

| 维度 | 状态 |
|---|---|
| 配置文件版本 | ✅ `Cargo.toml` = `Cargo.lock` = 0.2.0 |
| 最新 tag | cli/v0.1.16（0.2.0 未打 tag） |
| CHANGELOG | ⚠️ v0.1.1 ~ v0.1.16 共 16 个版本缺条目 |
| GitHub Release | ⚠️ 10 个 tag 缺 Release（含完整断档 v0.1.13） |
| 发布审计 | ✅ `release audit -v cli/v0.2.0` 7/7 通过 |

## 版本矩阵

### 配置文件

| 文件 | 版本 |
|---|---|
| `src/cli/Cargo.toml` | 0.2.0 |
| `src/cli/Cargo.lock` | 0.2.0 |

### tag / CHANGELOG / GitHub Release 三向对比

| 版本 | tag | CHANGELOG | GH Release | 说明 |
|---|---|---|---|---|
| 0.0.1 ~ 0.0.5 | ✅ | ✅ | ✅ | 完整 |
| 0.1.0-alpha.1 | ✅ | ✅ | ✅ Pre-release | 完整 |
| 0.1.0-beta.1 | ✅ | ✅ | ❌ | 无 Release |
| 0.1.0-rc.1 | ✅ | ❌ | ❌ | 无 CHANGELOG |
| 0.1.0-rc.2 | ✅ | ❌ | ❌ | 无 CHANGELOG |
| 0.1.0 | ✅ | ✅ | ❌ | 无 Release |
| 0.1.1 ~ 0.1.6 | ✅ | ❌ | ❌ | 无 CHANGELOG 无 Release |
| 0.1.7 ~ 0.1.12 | ✅ | ❌ | ✅ | 无 CHANGELOG |
| 0.1.13 | ✅ | ❌ | ❌ | 完整断档 |
| 0.1.14 ~ 0.1.16 | ✅ | ❌ | ✅ | 无 CHANGELOG |
| **0.2.0（当前）** | ❌ 未打 tag | ✅ | ❌ | 待发布 |

## 问题清单

### 1. CHANGELOG 严重滞后（16 个版本缺条目）

v0.1.1 ~ v0.1.16 均无 CHANGELOG 记录，`[0.1.0]` 之后直接跳到 `[0.2.0]`。发布流程中"tag 触发 Release"的自动化依赖 CHANGELOG 条目，缺条目导致后续发布历史不完整。

**建议**：
- 补录历史摘要（每条 1-2 行），或
- 明确"历史缺口不回填"策略，README 中已声明 v0.1.16 为 crates.io 发布版

### 2. GitHub Release 断档（10 个 tag 缺 Release）

v0.1.0、v0.1.0-beta.1、v0.1.1 ~ v0.1.6、v0.1.13 无对应 Release。其中 **v0.1.13 是完整断档**（tag 有、CHANGELOG 无、Release 无），为发布流水线遗漏。

**建议**：历史断档不可补发（Release 不可移动），跳过即可，v0.1.14+ 已覆盖。

### 3. 本地 tag 拉取不完整（已修复）

首次检查时本地缺 `cli/v0.1.1`、`cli/v0.1.11` 两个 tag（远程存在），`git fetch origin tag <name>` 补齐后共 26 个 cli tag。

**建议**：检查前先 `git fetch --tags`，避免误判。

### 4. 版本自动检测与手动升版冲突（关联 qtcloud-devops #19）

`qtcloud-devops release audit` 不带 `-v` 时按"最新 tag + patch+1"推断（v0.1.16 → v0.1.17），不读取 Cargo.toml 实际版本（0.2.0），产生误报。显式 `-v cli/v0.2.0` 审计 7/7 通过。

**建议**：手动升 minor/major 时必须显式 `-v` 指定版本。

## 结论与建议

1. **cli/v0.2.0 具备发布条件**：配置 + CHANGELOG 一致，`release audit` 7/7 通过，建议尽快打 tag 发布
2. **历史 CHANGELOG 缺口**：建议补录或明确不回填策略
3. **发布流程改进**：建议在 CI 中增加"tag 必须对应 CHANGELOG 条目"的检查，避免再次断档

## 关联

- [qtcloud-devops issue #19](https://github.com/quanttide/qtcloud-devops/issues/19)：release audit 自动检测版本忽略配置文件实际版本
