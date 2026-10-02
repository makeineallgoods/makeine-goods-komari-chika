# 小鞠知花制品图鉴

《败犬女主太多了！》小鞠知花官方实体制品的独立 JSON 图鉴项目。

当前收录 60 条：GAGAGA SHOP 官方角色筛选 28 条，以及从ゲーマーズ、
Animega×Sofmap、AMNIBUS、二次元コスパ、キャラ印.com 官方公告整理的
首批扩展资料 32 条。所有记录均保留官方来源链接，图片已下载到项目内。

## 本地运行

```bash
python3 -m http.server 4173
```

访问 `http://localhost:4173/`。页面通过 `fetch()` 读取 JSON，不能直接双击 `index.html`。

## 数据位置

- `data/catalog.json`：本角色商品数据，唯一需要日常编辑的商品文件。
- `data/catalog.schema.json`：六个角色项目共用的结构约束。
- `data/registry/`：从 `makeine-goods-catalog/registry/` 同步的公共词表快照。
- `images/items/`：详情图。
- `images/thumbs/`：列表缩略图。

商品使用 `campaign_id`、`manufacturer_id` 和 `category_id` 引用公共词表。相同群像商品在不同角色项目中必须使用相同的 `global_product_id`。

## 重新导入

导入脚本按商品 `id` 覆盖合并，可以重复运行而不会生成重复记录：

```bash
python3 tools/import_gagaga.py /tmp/komari-gagaga.html
python3 tools/import_official_baseline.py
python3 tools/download_images.py
python3 tools/generate_thumbnails.py
```

- `import_gagaga.py`：解析已下载的 GAGAGA SHOP 小鞠角色筛选页。
- `import_official_baseline.py`：写入其他官方渠道已人工核对的结构化基线。
- `download_images.py`：仅下载本地缺失的原图，并用 Pillow 校验文件内容。
- `generate_thumbnails.py`：生成网页列表使用的 WebP 缩略图。

## 公共词表

在工作区的 `makeine-goods-catalog` 中维护活动名称、厂商名称、固定译名和角色名称，然后运行：

```bash
python3 makeine-goods-catalog/tools/sync_registries.py
python3 makeine-goods-catalog/tools/validate_character_projects.py
```

## Netlify

项目无需构建命令，发布目录填写 `.`。`netlify.toml` 已包含缓存规则。
