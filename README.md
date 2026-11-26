# PCCP-Bench

一个**可判定、可复跑**的测题集，只问一个问题：对一次模型变更，PCCP（Predetermined Change Control Plan）该判 **go / hold / reject** 中的哪一个。

- 规模：42 题（公开 32 / 隐藏 10，隐藏题只承诺哈希，不在本仓）
- 每题都带所依据的法规原文，第三方可以在**理由**上不同意，而不是在管道上被卡住
- 参考裁判在本仓流程里跑，**不是**监管判定，**不是** FDA 背书

## 状态（2026-11-26）

**尚未发布，无 DOI。** 仓库先把发布流程立起来，数据集字节随 tag `v0.1.0` 一起进仓并冻结。

冻结根：`2d305387f61808d8d18e2cbb8ff627dc7948a08f37f4b75e6852dc29cdc6de9d`
（14 叶子 / 131,216 字节；规则：叶子 = sha256(path + NUL + file_sha256)，成对哈希，奇数节点与自身配对）

## 怎么发

`.github/workflows/zenodo-new-record.yml` —— Actions 手动触发，`version=0.1.0`：

1. 先 `dry_run=true`，停在草稿让人看
2. 再 `dry_run=false`，publish 后 log 打印 record DOI 与 concept DOI

流程里有九道闸，最要紧的一道：**上传前复算 Merkle 根，和常量比对，不一致就不传** —— 冻结集合改过一个字节，DOI 就指向了别的东西。

需要一条仓库 secret `ZENODO_TOKEN`（Zenodo personal access token，勾 `deposit:write` 与 `deposit:actions`）。GitHub 的 secret 按设计不可读回，只能由仓库所有者在 Settings 里贴一次。

## 现在缺什么

- **数据集 14 个叶子还没进仓**（字节在另一台机器上）。`MANIFEST.json` 不在仓里时，流程第一步就会红，不会带着半个集合去发布 —— 这是有意的失败方向。
- 元数据 `.zenodo.json` 里 license 写的是 `cc-by-4.0`（数据集），代码另注 Apache-2.0。

## 已知分歧

42 题里 2 题（K260973-H1、K260973-N2）我们的读法是 **hold**，参考裁判给 **reject**。分歧公开列着，不悄悄改平 —— 一个把自身分歧藏起来的基准，是在要求被信任而不是被检验。

## 引用

发布后再填。在此之前请引用冻结根，不要引用本仓 URL 当作数据集 DOI。
