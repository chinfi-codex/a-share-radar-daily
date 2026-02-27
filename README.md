# A股公告雷达（按日期）交付说明

## 交付文件
- 数据管道脚本：`skills/a-share-announcement-radar/scripts/build_radar_daily.py`
- 一键批量脚本：`skills/a-share-announcement-radar/scripts/generate_radar_daily.sh`
- 静态页面：`output/radar-daily/index.html`
- 示例数据：`output/radar-daily/2026-02-26.json`

## 运行命令
### 1) 单日生成
```bash
python3 skills/a-share-announcement-radar/scripts/build_radar_daily.py \
  --date 2026-02-26 \
  --output-dir output/radar-daily
```

### 2) 最近 N 天批量生成
```bash
# 最近1天（默认）
./skills/a-share-announcement-radar/scripts/generate_radar_daily.sh 1

# 以指定结束日向前N天
./skills/a-share-announcement-radar/scripts/generate_radar_daily.sh 7 2026-02-26
```

## 页面使用
1. 打开：`output/radar-daily/index.html`
2. 选择日期并点击“加载”，默认读取同目录 `YYYY-MM-DD.json`
3. 可输入“股票代码/名称”做全局筛选（公告/调研/问答）
4. 如果浏览器限制 `file://` 读取：
   - 直接用页面“选择JSON文件”手动加载，或
   - 在 `output/radar-daily` 目录启服务：`python3 -m http.server 8000`

## 数据结构（YYYY-MM-DD.json）
- `announcement`: 公告分类（含机构调研类标签）
- `institutionalResearch`: cninfo `tabName=relation` 明细
- `interactions`: p5w `getNewSearchR` 明细
- 每个分区都包含 `ok/error/items`；抓取失败会写入 `error`，页面展示“暂无数据/抓取失败原因”。

## 已运行验证
- 日期：`2026-02-26`
- 结果：公告 912 条；机构调研 13 条；互动问答 0 条（当天过滤后为空，非报错）

## 已知限制
- p5w 接口为分页抓取后按日期过滤；若接口端当日确无数据，会显示 0 条。
- 直接双击 HTML 时，不同浏览器对本地 JSON 读取策略不同；已提供文件手动加载兜底。
