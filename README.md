# 包子漫画爬虫与GUI搜索工具

## 项目简介
本项目为逆向包子漫画APP制作的爬虫工具，用于爬取包子漫画APP中的漫画
本项目包含两个主要工具：
- **baozimh_gui_search.py**：基于Tkinter的包子漫画搜索与详情解析GUI工具。
- **baozimh_crawler.py**：多线程高效下载包子漫画章节图片的爬虫脚本。

适用于需要批量下载包子漫画图片、快速检索漫画信息、获取章节目录等场景。

---

## 主要功能

### 1. GUI搜索与详情解析（`baozimh_gui_search.py`）
- 支持关键词搜索包子漫画，展示结果列表。
- 选中漫画后自动解析详情页，显示标题、作者、简介、章节数、封面等。
- 可一键复制爬虫所需字段（如BASE_URL、COVER_URL、章节范围等）。
- 支持停止搜索、异常提示、界面友好。

### 2. 漫画批量下载爬虫（`baozimh_crawler.py`）
- 支持多线程下载指定漫画的所有章节图片。
- 支持断点续爬，自动记录已完成章节。
- 自动下载封面。
- 失败图片自动记录日志，便于后续补爬。
- 代理与UA可自定义。

---

## 环境依赖
- Python 3.7 及以上
- **浏览器**：支持 Firefox 或 Chromium（推荐使用 Firefox）
- **WebDriver**：geckodriver（Firefox）或 chromedriver（Chromium）

安装依赖：
```bash
pip install -r requirements.txt
```

`requirements.txt`内容包括：
- requests
- beautifulsoup4
- selenium
- chromedriver-autoinstaller（可选，如果使用 Chromium）
- Pillow
- xpinyin
- tk

**WebDriver 说明**：
- 如果使用 Firefox：需要下载 [geckodriver](https://github.com/mozilla/geckodriver/releases)，放在项目目录下
- 如果使用 Chromium：chromedriver-autoinstaller 会自动管理

---

## 使用说明

### 1. 启动GUI搜索工具
```bash
python baozimh_gui_search.py
```
- 输入关键词，点击"搜索"
- 选择结果，自动解析详情
- 可复制爬虫字段，便于后续下载

### 2. 使用爬虫批量下载漫画

**配置步骤**：
1. 打开 `baozimh_crawler.py` 文件，找到 "配置区域" 部分
2. 设置以下参数（文件中有详细注释说明）：
   - `PROXY`：代理地址（如需代理），例如 `"127.0.0.1:7895"`，不使用代理请留空
   - `BASE_URL`：章节图片基础链接
     - 获取方法：访问漫画介绍页，从URL中提取漫画拼音和作者拼音，拼接成 BASE_URL
     - 示例：`https://app.baozimh.com/baozimhapp/comic/chapter/mangapinyin-authorpinyin/`
   - `CHAPTER_START`：起始章节编号（通常为 0）
   - `CHAPTER_END`：结束章节编号（最新章节号 - 1）
   - `OUTPUT_DIR`：图片保存目录名
   - `COVER_URL`：封面图片链接（可选）

**运行爬虫**：
```bash
python baozimh_crawler.py
```

**功能说明**：
- ✓ 多线程下载：每章节使用 16 个线程并发下载图片
- ✓ 自动重试：失败的图片会自动重试 5 次，重试间隔递增
- ✓ 断点续爬：进度保存在 `baozimh_crawler_jindu.json`，下次运行自动跳过已完成章节
- ✓ 失败记录：下载失败的图片记录在 `baozimh_crawler_failed.log`
- ✓ 进度显示：显示每章节找到的图片数量和下载成功率

**检查完整性**：
```bash
python check_missing.py
```
此脚本会检查每个章节的图片编号是否连续，找出缺失的图片

---

## 注意事项

**配置检查**：
- 运行前请确保正确配置 `BASE_URL`、`CHAPTER_END`、`OUTPUT_DIR` 等参数
- 如果 `BASE_URL` 未配置，程序会提示错误并退出

**代理设置**：
- 如需使用代理，请正确设置 `PROXY` 变量（例如 `"127.0.0.1:7895"`）
- 不使用代理请将 `PROXY` 设置为空字符串 `""`
- 代理配置会同时应用于 Selenium（浏览器）和 requests（图片下载）

**浏览器与 WebDriver**：
- 默认使用 Firefox 浏览器，需要下载 geckodriver 并放在项目目录
- 如果需要使用 Chromium，请修改 `get_driver()` 函数中的浏览器配置
- WebDriver 版本需与浏览器版本匹配

**性能调优**：
- `THREADS`：每章节图片下载线程数，建议值 16-32
- `TIMEOUT`：图片下载超时时间（秒），默认 30
- 如遇频繁超时，可适当降低 `THREADS` 或增加 `TIMEOUT`

**免责声明**：
- **仅供学习交流，请勿用于商业用途**
- 请遵守相关法律法规和平台使用条款

---

## 目录结构
```
├── baozimh_gui_search.py      # GUI搜索与详情解析工具
├── baozimh_crawler.py         # 多线程漫画图片下载爬虫
├── requirements.txt           # 依赖包列表
├── README.md                  # 项目说明文档
```

---

## 致谢
- 本项目仅供学习与技术交流。
- 感谢包子漫画平台。
- 感谢[selenium](https://www.selenium.dev)
- 感谢[xpinyin](https://github.com/lxneng/xpinyin)
