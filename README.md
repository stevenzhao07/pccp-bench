# PCCP-Bench

一个**可判定、可复跑**的测题集，只问一件事：对一次模型变更，PCCP（Predetermined Change Control Plan）该判 **go / hold / reject** 中的哪一个。

**DOI：10.5281/zenodo.24550267**（数据集归档） · concept DOI [10.5281/zenodo.24550266](https://doi.org/10.5281/zenodo.24550266)

- 规模：42 题（公开 32 / 隐藏 10，隐藏题只承诺哈希，不在归档内）
- 每题带所依据的法规原文，第三方可以在**理由**上不同意，而不是在管道上被卡住
- 参考裁判是参考实现，**不是**监管判定，**不是** FDA 背书

## 怎么验（外人可做，不用问我们）

```
curl -L -o p.zip https://zenodo.org/records/24550267/files/pccp-bench_zenodo_v0.1.0.zip
sha256sum p.zip        # 应为 1907e93a21305c3055777dda2f3c85f037501d821bba08abb37f7ac8127aaf61
unzip p.zip && cd p && python -c "import hashlib,json;m=json.load(open('MANIFEST.json'));print(all(hashlib.sha256(open(f['archive_path'],'rb').read()).hexdigest()==f['sha256'] for f in m['files']))"
```

冻结根：`2d305387f61808d8d18e2cbb8ff627dc7948a08f37f4b75e6852dc29cdc6de9d`
（14 叶子 / 131,216 字节。规则：叶子取文件 sha256 摘要的**字节**、按 path 排序、成对 `sha256(a+b)`、奇数节点与自身配对 —— 照此复算必得同一根。）

## 怎么跑

解包后 `python bench.py --submit your_adapter.py`。适配器逐题收到一个案例，返回 go/hold/reject 与理由；分数是精确匹配、宏平均召回、引用标签三项。参考适配器在公开列与掩盖列各 0.9688；空基线"一律 go"精确匹配 0.875 但宏召回只有 0.5 —— 让偷懒答案在头条数字上获胜的集合等于没测，所以两项并列报出。

## 已知分歧

42 题里 2 题（K260973-H1、K260973-N2）我们读作 **hold**、参考裁判判 **reject**。分歧公开列着，不悄悄抹平 —— 一个把自身分歧藏起来的基准，是在要求被信任而不是被检验。

## 试真台接入

私有仓里的 Assurance Instrument Test Bench（试真台）有一份 `presets/pccp-bench.json`，把冻结根、包内逐字节、版本同号、许可状态、DOI 主张、台子自跑与空基线对照各判一遍；检查器自带反照（改一字节根必须变）。

## 许可

数据 CC BY 4.0；归档内代码另注 Apache-2.0（逐文件见 `LICENSE_STATUS.md`）。

## 这里暂不放数据集

数据集字节在归档里（DOI 上面那个）。本仓只放发布流程与元数据；归档字节一旦进仓，冻结根与包内路径就要跟着改，反而多一层可能对不上的地方。
