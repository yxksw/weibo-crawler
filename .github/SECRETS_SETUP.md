# GitHub Secrets 配置说明

## 必要的环境变量

### WEIBO_COOKIE

微博 Cookie 是运行爬虫所必需的。请在 GitHub 仓库中配置 Secrets。

## 配置步骤

### 1. 获取微博 Cookie

1. 打开浏览器，访问 [微博](https://weibo.com)
2. 登录你的微博账号
3. 按 `F12` 打开开发者工具
4. 切换到 **Network**（网络）标签
5. 刷新页面
6. 点击任意请求，查看 **Request Headers**（请求头）
7. 找到 `Cookie` 字段，复制完整的 Cookie 值

Cookie 格式示例：
```
SCF=xxx; SUB=xxx; SUBP=xxx; SSOLoginState=xxx; ALF=xxx; _T_WM=xxx
```

### 2. 在 GitHub 中配置 Secrets

1. 打开你的 GitHub 仓库
2. 点击 **Settings**（设置）标签
3. 在左侧菜单中选择 **Secrets and variables** → **Actions**
4. 点击 **New repository secret**（新建仓库密钥）
5. 填写以下信息：
   - **Name**: `WEIBO_COOKIE`
   - **Value**: 粘贴你复制的 Cookie 值
6. 点击 **Add secret**（添加密钥）

### 3. 验证配置

配置完成后，你可以：

1. 点击 **Actions** 标签
2. 选择 **Weibo Crawler Hourly** 工作流
3. 点击 **Run workflow** 手动触发一次运行
4. 查看运行日志，确认爬虫是否正常工作

## 注意事项

- ⚠️ **不要将 Cookie 提交到代码仓库**：Cookie 包含敏感信息，必须通过 Secrets 管理
- 🔐 **定期更新 Cookie**：微博 Cookie 会过期，建议定期更新
- 📝 **Cookie 格式**：确保复制完整的 Cookie 字符串，包含所有字段
- 🛡️ **安全提示**：不要与他人分享你的 Cookie

## 工作流说明

工作流文件：`.github/workflows/weibo-crawler.yml`

- **触发时间**：每小时运行一次（UTC 时间）
- **运行环境**：Ubuntu latest
- **Python 版本**：3.11
- **数据保留**：爬取的数据保留 7 天

## 时区调整

如果需要调整爬取时间（默认为 UTC 时间），可以修改工作流文件中的 cron 表达式：

```yaml
# 当前配置：每小时整点（UTC 时间）
- cron: '0 * * * *'

# 北京时间（UTC+8）：每小时整点
# 例如：北京时间 8:00, 9:00, 10:00...
- cron: '0 * * * *'  # GitHub Actions 的 schedule 使用 UTC 时间

# 如果想在北京时间的整点运行（如 8:00, 9:00），需要减去 8 小时
# 例如：北京时间 8:00 = UTC 时间 0:00
- cron: '0 0 * * *'  # 每天 UTC 0:00（北京时间 8:00）
```

Cron 表达式格式：`分钟 小时 日期 月份 星期`

常用示例：
- `0 * * * *` - 每小时整点
- `0 0 * * *` - 每天午夜（UTC）
- `0 8 * * *` - 每天早上 8 点（UTC）
- `*/30 * * * *` - 每 30 分钟
