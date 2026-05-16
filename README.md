# cookable-data

**烧了吧 (Cookable)** iOS 应用的**公开数据仓**。

主 App 源码在另一个**私有仓** [`jo9900/Cookable`](https://github.com/jo9900/Cookable)。这里只放需要**对全世界可见**的两类东西：

1. **菜谱 / 食材 / 手法 JSON** —— App 在运行时通过 raw URL 拉取，可在不发版的情况下更新菜谱
2. **隐私政策 / 服务条款 markdown** —— 给 App Store 提交时填写的公开 URL

## 为什么分仓

App 源码本身是 private（看你的）；但 GitHub Pages / raw URL 给匿名访问要求 repo 是 public。把"会公开消费"的内容剥到这个独立 public 仓，主代码仓保持 private。

---

## 仓库内容

```
.
├── methods.json        # 烹饪手法清单（13 项 builtin）
├── ingredients.json    # 食材 / 调味料 / 香料 三合一（94 / 27 / 14）
├── recipes.json        # 菜谱（102 道 builtin，schema 已为 UGC 预留字段）
├── version.json        # 版本探针，App 先拉这个判断要不要拉主表
└── legal/
    ├── privacy.md      # 隐私政策
    └── terms.md        # 服务条款
```

## 给 App 用的 URL（raw）

```
https://raw.githubusercontent.com/jo9900/cookable-data/main/version.json
https://raw.githubusercontent.com/jo9900/cookable-data/main/methods.json
https://raw.githubusercontent.com/jo9900/cookable-data/main/ingredients.json
https://raw.githubusercontent.com/jo9900/cookable-data/main/recipes.json
```

启动流程：
1. App 启动 → 用 bundle 内置 / 本地缓存优先
2. 后台异步拉 `version.json`（< 1 KB）
3. 比对 `version` 字段 → 如果远程更新，再拉对应主表
4. 写入本地缓存 → AppState 触发 UI 刷新

## 给 App Store 用的 URL（blob）

提交 App Store 时填这两条：

```
隐私政策: https://github.com/jo9900/cookable-data/blob/main/legal/privacy.md
服务条款: https://github.com/jo9900/cookable-data/blob/main/legal/terms.md
```

GitHub 自带的 markdown 渲染足够用，无需开 Pages。

---

## 如何更新

不要直接在这个仓里手动改 JSON —— 它们由 App 仓里的脚本生成。

在 App 仓 `~/Desktop/others/cookable/` 下：

```bash
# 改 scripts/generate_data.py（菜谱、食材、调味料、香料、手法）
# 或 改 docs/legal/*.md（法律文档）

./scripts/push_data.sh "data: 加 5 道川菜"
```

脚本做的事：

1. 重生成 App 仓内的 `data/*.json`（被 App 内置）
2. 拷贝 JSON 到这个仓
3. 拷贝 `docs/legal/*.md` 到这个仓的 `legal/`
4. commit + push

用户下次重启 App 即拉到新数据。

## 直接编辑也行（应急）

如果就是手忙脚乱要改 typo：

```bash
cd cookable-data
vim recipes.json     # 顺手把 version 字段也 +1，否则 App 不会觉得有更新
git add recipes.json && git commit -m "fix typo" && git push
```

但**这不会同步回 App 仓的 bundle 内置数据**，下个 App 出厂版还是用旧的 fallback。建议走 `push_data.sh` 双向同步。

---

## 联系

App 维护者: `adeliaehd85512b@gmail.com`
