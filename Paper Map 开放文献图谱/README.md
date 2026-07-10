# Paper Map 开放文献图谱工具

Paper Map 是一个轻量级的本地网页工具，用于从一篇论文出发，快速查看相关文献、前序文献和后续引用，并以关系图和表格的形式展示文献脉络。它适合用于开题、文献综述、研究方向摸底、论文影响路径梳理等场景。

本工具不需要后端服务器。打开 `paper-map.html` 后，在浏览器中输入论文标题、DOI 或 OpenAlex ID，即可通过 OpenAlex API 获取文献信息。

## 文件说明

推荐保留以下文件：

```text
paper-map.html
jcr-2026-journal-metrics.csv
README.md
```

其中：

- `paper-map.html`：主工具页面。
- `jcr-2026-journal-metrics.csv`：可选的期刊分区/影响因子参考表。
- `README.md`：使用说明。


## 主要功能

- 输入论文标题、DOI 或 OpenAlex ID，查找起点论文。
- 自动获取相关文献、前序文献、后续引用。
- 生成可点击的文献关系图。
- 在表格中展示题名、作者、年份、被引次数、来源期刊、分区、影响因子等信息。
- 支持上传期刊指标 CSV，用于匹配 JCR 分区和影响因子。
- 支持导出文献列表为 JSON 和 CSV。

## 如何使用

1. 用浏览器打开 `paper-map.html`。
2. 在 `OpenAlex API key` 输入框中填入你的 OpenAlex API key。
3. 可选：在 `期刊分区/影响因子 CSV` 处选择 `jcr-2026-journal-metrics.csv`。
4. 在论文输入框中输入论文标题、DOI 或 OpenAlex ID。
5. 点击 `生成图谱`。
6. 点击图中的节点或表格行查看文献详情。
7. 可使用 `导出 JSON` 或 `导出 CSV` 保存结果。

## OpenAlex API key 从哪里获取

OpenAlex 是开放学术知识图谱服务。它提供 REST API，用于查询论文、作者、机构、期刊来源、引用关系等数据。

官方文档入口：

- OpenAlex Developers: <https://developers.openalex.org/>
- API key 申请入口: <https://openalex.org/settings/api>

根据 OpenAlex 官方开发者文档，API key 目前是免费的，但不同请求会消耗不同额度。若请求频繁，请留意 OpenAlex 当前的认证和计费说明。

## 论文输入支持什么格式

输入框支持以下几类内容：

```text
Attention Is All You Need
10.48550/arXiv.1706.03762
https://doi.org/10.1038/s41467-023-xxxxx
W1234567890     该内容为文献的OpenAlex ID
```

推荐优先使用 DOI 或 OpenAlex ID，因为标题搜索可能存在重名或匹配偏差。

## 期刊分区/影响因子参考表

由于OpenAlex不直接提供 JCR 影响因子和 JCR 分区，因此本工具通过外部 CSV 参考表进行匹配。

当前已提供的文件：

```text
jcr-2026-journal-metrics.csv
```

该文件由 JCR 表整理而来，包含：

- `journal`：期刊名称
- `issn`：ISSN 或 eISSN
- `quartile`：JCR 分区，如 `Q1`、`Q2`、`Q3`、`Q4`
- `impact_factor`：影响因子
- `category`：JCR 学科类别
- `jcr_year`：JCR 年份

最小可识别格式如下：

```csv
journal,issn,quartile,impact_factor
Nature Communications,2041-1723,Q1,14.7
Science,0036-8075,Q1,47.3
The ISME Journal,1751-7362,Q1,10.8
```

中文列名也可以识别：

```csv
期刊,issn,分区,影响因子
Nature Communications,2041-1723,Q1,14.7
Science,0036-8075,Q1,47.3
```

## 参考表如何加载

由于浏览器安全限制，普通本地 HTML 页面不能随意自动读取电脑上的本地文件。因此需要手动选择参考表：

1. 打开 `paper-map.html`。
2. 找到左侧 `期刊分区/影响因子 CSV`。
3. 点击文件选择按钮。
4. 选择 `jcr-2026-journal-metrics.csv`。
5. 页面状态栏显示“已加载 ... 条期刊指标”后，即表示加载成功。

加载参考表后，工具会按期刊名或 ISSN 自动匹配文献的分区和影响因子。

## 分区和影响因子如何匹配

匹配逻辑如下：

1. 先使用 OpenAlex 返回的期刊名称与参考表中的 `journal` 或 `期刊` 匹配。
2. 如果期刊名未命中，再使用 OpenAlex 返回的 `ISSN/eISSN` 与参考表中的 `issn` 匹配。
3. 如果都未命中，则显示为 `未匹配`。

ISSN 匹配通常比期刊名匹配更可靠。因为期刊名可能存在大小写、缩写、标点、旧名、新名等差异。

## 数据来源和限制

本工具的文献元数据来自 OpenAlex，包括题名、作者、年份、被引次数、来源期刊、引用关系等。期刊分区和影响因子来自用户手动加载的 CSV 参考表。

需要注意：

- OpenAlex 的引用数据可能存在延迟或缺失。
- 标题搜索可能返回相似但不是同一篇的论文。
- 影响因子和 JCR 分区属于第三方指标，应以你所在机构正式获取的 JCR 数据为准。
- 如果要在论文、报告或项目中正式引用指标，请注明 JCR 年份和数据来源。




# Paper Map: Open Literature Graph Tool

Paper Map is a lightweight local web tool for exploring scholarly literature from a seed paper. Starting from a paper title, DOI, or OpenAlex ID, it retrieves related works, prior works, and citing works, then displays them as an interactive graph and a structured table.

It is useful for literature reviews, research scoping, thesis preparation, and quickly understanding the citation context around a paper.

The tool does not require a backend server. Open `paper-map.html` in a browser, enter a paper identifier, and the page will query the OpenAlex API directly.

## Files

Recommended files:

```text
paper-map.html
jcr-2026-journal-metrics.csv
README.md
```

File roles:

- `paper-map.html`: the main web tool.
- `jcr-2026-journal-metrics.csv`: optional journal metrics reference file.
- `README.md`: documentation.


## Features

- Search a seed paper by title, DOI, or OpenAlex ID.
- Retrieve related works, prior works, and citing works.
- Build an interactive paper graph.
- Show paper metadata in a table, including title, authors, year, citations, source journal, quartile, and impact factor.
- Load a journal metrics CSV to match JCR quartiles and impact factors.
- Export results as JSON or CSV.

## How to Use

1. Open `paper-map.html` in your browser.
2. Enter your OpenAlex API key in the `OpenAlex API key` field.
3. Optional: choose `jcr-2026-journal-metrics.csv` in the journal metrics file picker.
4. Enter a paper title, DOI, or OpenAlex ID.
5. Click `生成图谱`.
6. Click graph nodes or table rows to view paper details.
7. Use `导出 JSON` or `导出 CSV` to save the result.

## Where to Get an OpenAlex API Key

OpenAlex is an open scholarly knowledge graph. Its REST API provides access to works, authors, institutions, sources, citation relationships, and more.

Official links:

- OpenAlex Developers: <https://developers.openalex.org/>
- API key settings: <https://openalex.org/settings/api>

According to the official OpenAlex developer documentation, API keys are free, but API requests consume usage quota. Check OpenAlex's current authentication and pricing page if you plan to use the tool heavily.

## Supported Paper Input

The search box accepts values such as:

```text
Attention Is All You Need
10.48550/arXiv.1706.03762
https://doi.org/10.1038/s41467-023-xxxxx
W1234567890
```

DOIs and OpenAlex IDs are preferred because title search can return ambiguous results.

## Journal Metrics Reference File

OpenAlex does not provide JCR impact factors or JCR quartiles directly. This tool matches those metrics from an external CSV file.

The currently provided file is:

```text
jcr-2026-journal-metrics.csv
```

It contains:

- `journal`: journal name
- `issn`: ISSN or eISSN
- `quartile`: JCR quartile, such as `Q1`, `Q2`, `Q3`, or `Q4`
- `impact_factor`: impact factor
- `category`: JCR subject category
- `jcr_year`: JCR year

Minimum supported format:

```csv
journal,issn,quartile,impact_factor
Nature Communications,2041-1723,Q1,14.7
Science,0036-8075,Q1,47.3
The ISME Journal,1751-7362,Q1,10.8
```

## How to Load the Reference File

Because of browser security restrictions, a local HTML page cannot freely read local files from your computer. Therefore, the reference CSV must be selected manually.

1. Open `paper-map.html`.
2. Locate `期刊分区/影响因子 CSV` in the left panel.
3. Click the file chooser.
4. Select `jcr-2026-journal-metrics.csv`.
5. Wait until the status message says that journal metrics were loaded.

After loading, the tool will match journal quartiles and impact factors by journal name or ISSN.

## How Matching Works

The matching process is:

1. Match the OpenAlex source journal name against the `journal` or `期刊` column.
2. If the name does not match, try ISSN/eISSN against the `issn` column.
3. If neither matches, the journal metrics are shown as `未匹配`.

ISSN matching is generally more reliable than journal-name matching because journal names may vary by capitalization, abbreviation, punctuation, former titles, or alternate titles.

## Data Sources and Limitations

Bibliographic metadata comes from OpenAlex, including titles, authors, publication years, citation counts, source journals, and citation relationships. Journal quartiles and impact factors come from the user-loaded CSV reference file.

Limitations:

- OpenAlex citation data may be incomplete or delayed.
- Title search can return a similar but incorrect paper.
- Impact factors and JCR quartiles are third-party metrics. Use official JCR data from your institution when accuracy matters.
- For formal reports or publications, cite the JCR year and data source.
