# 微博爬虫

自动爬取指定用户的微博内容，每小时运行一次，并将数据保存到 GitHub 仓库的 `gh-pages` 分支。

## 功能特性

- 📅 **定时运行**：每小时自动爬取微博数据
- 📊 **多种格式**：支持 Markdown、CSV、JSON 格式输出
- 📁 **持久存储**：数据保存到 `gh-pages` 分支，可通过 GitHub Pages 访问
- 🔐 **安全配置**：Cookie 通过 GitHub Secrets 管理
- 🛡️ **防封禁**：内置请求延迟和会话管理

## 配置说明

### 1. 配置 GitHub Secrets

1. 登录微博，获取 Cookie
2. 进入 GitHub 仓库 → Settings → Secrets and variables → Actions
3. 添加以下 Secret：
   - `WEIBO_COOKIE`：微博 Cookie 字符串

### 2. 目标用户配置

编辑 `config.json` 文件：

```json
"user_id_list": ["6602486386"], // 要爬取的用户ID列表
"since_date": 30, // 爬取最近30天的微博
"write_mode": ["markdown", "csv", "json"], // 输出格式
"output_directory": "weibo", // 输出目录
```

### 3. 查看爬取结果

爬取的数据会自动推送到 `gh-pages` 分支：

1. 进入 GitHub 仓库 → Code → 切换到 `gh-pages` 分支
2. 查看 `weibo/` 目录下的文件
3. 也可以通过 GitHub Pages 访问（如果启用）

## 目录结构

```
weibo-crawler/
├── weibo/                    # 爬取数据输出目录
│   └── [用户昵称]/           # 用户文件夹
│       ├── [user_id].csv     # CSV 格式数据
│       ├── [user_id].json    # JSON 格式数据
│       └── [年份]-[月份]/     # Markdown 文件按月归档
│           └── [日期].md     # Markdown 格式数据
├── .github/
│   └── workflows/
│       └── weibo-crawler.yml # GitHub Actions 工作流
├── config.json               # 配置文件
└── weibo.py                  # 爬虫主程序
```

## 本地测试

1. 安装依赖：`pip install -r requirements.txt`
2. 在 `config.json` 中添加 Cookie（临时测试用）
3. 运行：`python weibo.py`
4. 查看 `weibo/` 目录生成的文件

## 注意事项

- ⚠️ **Cookie 安全**：测试完成后请从 `config.json` 中移除 Cookie
- 🔄 **Cookie 过期**：微博 Cookie 会定期过期，需要定期更新
- 📦 **数据存储**：`gh-pages` 分支会自动保存所有爬取数据
- ⏱️ **运行频率**：默认每小时运行一次，可在工作流文件中调整
- 🌐 **跨域访问**：已配置 CORS 头，允许其他项目访问 JSON 文件

## CORS 配置

为了解决跨域访问问题，项目已在 `_headers` 文件中配置了 CORS 头：

```
/*
  Access-Control-Allow-Origin: *
  Access-Control-Allow-Methods: GET, POST, OPTIONS
  Access-Control-Allow-Headers: Content-Type, Authorization
  Access-Control-Max-Age: 86400
```

这允许任何域名访问 `gh-pages` 分支中的文件，包括：
- JSON 文件：`https://weibo-api.050815.xyz/%E5%BC%82%E9%A3%A8%E5%AE%A2/6602486386.json`
- CSV 文件：`https://weibo-api.050815.xyz/%E5%BC%82%E9%A3%A8%E5%AE%A2/6602486386.csv`
- Markdown 文件：`https://weibo-api.050815.xyz/%E5%BC%82%E9%A3%A8%E5%AE%A2/2026-03/2026-03-20.md`

## 技术栈

- Python 3.11+
- GitHub Actions
- peaceiris/actions-gh-pages

## 许可证

MIT
