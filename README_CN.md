# Bolt.DIY 部署指南

本文档介绍如何通过 GitHub Actions 将项目部署到 Cloudflare Pages。

## 前置要求

- GitHub 账号
- Cloudflare 账号
- 项目代码已推送到 GitHub 仓库

## 部署步骤

### 1. 配置 Cloudflare

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. 记录你的 Account ID
   - 可以在 URL 中找到：`https://dash.cloudflare.com/{account-id}`
   - 或在右侧边栏查看

3. 创建 API Token
   - 访问 [API Tokens 页面](https://dash.cloudflare.com/profile/api-tokens)
   - 点击 "Create Token"
   - 选择 "Create Custom Token"
   - 设置以下权限：
     ```
     Account.Cloudflare Pages: Edit
     User.Workers Scripts: Edit
     ```
   - 创建并保存生成的 Token

### 2. 配置 GitHub Secrets

1. 在你的 GitHub 仓库中，进入 Settings > Secrets and variables > Actions
2. 添加以下 secrets：
   - `CLOUDFLARE_API_TOKEN`: 你创建的 API Token
   - `CLOUDFLARE_ACCOUNT_ID`: 你的 Cloudflare Account ID

### 3. 配置 GitHub Actions

项目已包含 GitHub Actions 配置文件：`.github/workflows/cloudflare-pages.yaml`

我来帮你创建部署文档：

```markdown:README_CN.md
# Bolt.DIY 部署指南

本文档介绍如何通过 GitHub Actions 将项目部署到 Cloudflare Pages。

## 前置要求

- GitHub 账号
- Cloudflare 账号
- 项目代码已推送到 GitHub 仓库

## 部署步骤

### 1. 配置 Cloudflare

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. 记录你的 Account ID
   - 可以在 URL 中找到：`https://dash.cloudflare.com/{account-id}`
   - 或在右侧边栏查看

3. 创建 API Token
   - 访问 [API Tokens 页面](https://dash.cloudflare.com/profile/api-tokens)
   - 点击 "Create Token"
   - 选择 "Create Custom Token"
   - 设置以下权限：
     ```
     Account.Cloudflare Pages: Edit
     User.Workers Scripts: Edit
     ```
   - 创建并保存生成的 Token

### 2. 配置 GitHub Secrets

1. 在你的 GitHub 仓库中，进入 Settings > Secrets and variables > Actions
2. 添加以下 secrets：
   - `CLOUDFLARE_API_TOKEN`: 你创建的 API Token
   - `CLOUDFLARE_ACCOUNT_ID`: 你的 Cloudflare Account ID

### 3. 配置 GitHub Actions

项目已包含 GitHub Actions 配置文件：`.github/workflows/cloudflare-pages.yaml`

```yaml
name: Deploy to Cloudflare Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      deployments: write
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Setup pnpm
        uses: pnpm/action-setup@v3
        with:
          version: 9.4.0

      - name: Install Dependencies
        run: pnpm install

      - name: Build
        run: pnpm run build

      - name: Publish to Cloudflare Pages
        uses: cloudflare/pages-action@v1
        with:
          apiToken: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          accountId: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
          projectName: bolt-diy
          directory: build/client
          gitHubToken: ${{ secrets.GITHUB_TOKEN }}
```

### 4. 触发部署

部署会在以下情况自动触发：
- 推送代码到 main 分支
- 在 GitHub Actions 页面手动触发 workflow

### 5. 查看部署状态

1. 在 GitHub 仓库中点击 "Actions" 标签页
2. 查看最新的 workflow 运行状态
3. 部署成功后，可以在 Cloudflare Pages 仪表板中查看你的站点

## 环境变量

项目使用以下环境变量：
- `NODE_VERSION`: "20"
- `NPM_VERSION`: "10"
- `VITE_LOG_LEVEL`: "debug"
- `DEFAULT_NUM_CTX`: "32768"

## 常见问题

1. 部署失败
   - 检查 GitHub Secrets 是否正确配置
   - 确认 API Token 权限是否足够
   - 查看 GitHub Actions 日志了解详细错误信息

2. 站点无法访问
   - 确认 Cloudflare Pages 项目是否创建成功
   - 检查域名配置是否正确
   - 验证构建输出目录是否正确 (`build/client`)

## 相关链接

- [Cloudflare Pages 文档](https://developers.cloudflare.com/pages)
- [GitHub Actions 文档](https://docs.github.com/cn/actions)
- [项目 GitHub 仓库](https://github.com/playWithAIGC/bolt.diy)


