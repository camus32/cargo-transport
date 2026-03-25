# 货物运输分配工具 - 详细部署指南

## 目录
1. [本地部署](#本地部署)
2. [GitHub Pages 部署](#github-pages-部署)
3. [常见问题](#常见问题)

---

## 本地部署

### 环境要求
- Node.js 18+ (推荐 20 LTS)
- npm 或 yarn
- Git (可选)

### 步骤 1：下载项目

#### 方式一：直接下载压缩包
1. 下载 `cargo-transport-tool.zip`
2. 解压到任意文件夹
3. 进入项目目录：`cd app`

#### 方式二：使用 Git 克隆
```bash
git clone https://github.com/你的用户名/cargo-transport.git
cd cargo-transport
```

### 步骤 2：安装依赖

```bash
# 使用 npm
npm install

# 或使用 yarn
yarn install

# 或使用 pnpm
pnpm install
```

安装过程可能需要 1-3 分钟，取决于网络速度。

### 步骤 3：启动开发服务器

```bash
# 使用 npm
npm run dev

# 或使用 yarn
yarn dev

# 或使用 pnpm
pnpm dev
```

启动成功后，你会看到类似输出：
```
  VITE v7.3.0  ready in 500 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```

### 步骤 4：访问应用

打开浏览器，访问：
```
http://localhost:5173
```

### 步骤 5：构建生产版本（可选）

```bash
npm run build
```

构建完成后，`dist` 文件夹中包含可部署的静态文件。

---

## GitHub Pages 部署

### 准备工作

1. 注册 GitHub 账号：https://github.com/signup
2. 安装 Git：https://git-scm.com/downloads

### 步骤 1：创建 GitHub 仓库

1. 登录 GitHub
2. 点击右上角 `+` → `New repository`
3. 填写信息：
   - **Repository name**: `cargo-transport` (或你喜欢的名字)
   - **Description**: 货物运输分配工具 (可选)
   - **Visibility**: Public (公开) 或 Private (私有)
   - **Initialize this repository with**: 不勾选任何选项
4. 点击 `Create repository`

### 步骤 2：准备本地项目

#### 2.1 修改配置文件

编辑 `vite.config.ts`：

```typescript
import path from "path"
import react from "@vitejs/plugin-react"
import { defineConfig } from "vite"

export default defineConfig({
  base: '/cargo-transport/',  // ← 改成你的仓库名
  plugins: [react()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
});
```

**注意**：`base` 必须与仓库名完全一致，包括大小写。

#### 2.2 初始化 Git 仓库

```bash
# 进入项目目录
cd app

# 初始化 Git 仓库
git init

# 添加所有文件
git add .

# 提交
git commit -m "Initial commit"
```

### 步骤 3：推送到 GitHub

```bash
# 添加远程仓库（将 你的用户名 替换为你的 GitHub 用户名）
git remote add origin https://github.com/你的用户名/cargo-transport.git

# 推送代码
git branch -M main
git push -u origin main
```

### 步骤 4：启用 GitHub Pages

1. 打开 GitHub 仓库页面
2. 点击 `Settings` 标签
3. 左侧菜单选择 `Pages`
4. **Source** 部分选择 `GitHub Actions`

![GitHub Pages Settings](https://docs.github.com/assets/cb-66033/images/help/pages/pages-source-settings.png)

### 步骤 5：等待自动部署

1. 点击仓库顶部的 `Actions` 标签
2. 你会看到工作流正在运行
3. 等待状态变为 ✅ 绿色对勾

![GitHub Actions](https://docs.github.com/assets/cb-28256/images/help/repository/actions-tab.png)

### 步骤 6：访问网站

部署完成后，访问地址为：
```
https://你的用户名.github.io/cargo-transport/
```

例如：
```
https://zhangsan.github.io/cargo-transport/
```

---

## 更新网站

修改代码后，重新推送即可自动部署：

```bash
# 添加修改的文件
git add .

# 提交
git commit -m "更新说明"

# 推送
git push
```

推送后，GitHub Actions 会自动重新部署，约 1-2 分钟后生效。

---

## 常见问题

### Q1: 安装依赖时卡住

**解决方案**：
```bash
# 使用淘宝镜像
npm config set registry https://registry.npmmirror.com
npm install

# 或使用官方镜像
npm config set registry https://registry.npmjs.org
npm install
```

### Q2: 本地运行报错 "Cannot find module"

**解决方案**：
```bash
# 删除 node_modules 重新安装
rm -rf node_modules
rm package-lock.json
npm install
```

### Q3: GitHub Pages 显示 404

**检查清单**：
1. 仓库是否为 Public？Private 仓库需要 GitHub Pro
2. `vite.config.ts` 中的 `base` 是否与仓库名一致？
3. Actions 是否运行成功？
4. 是否等待了 1-2 分钟？

### Q4: 页面空白或资源加载失败

**解决方案**：
1. 检查 `vite.config.ts` 的 `base` 配置
2. 确保使用相对路径 `./` 或正确的仓库路径
3. 重新构建并推送

### Q5: 如何绑定自定义域名？

1. 在仓库的 `Settings` → `Pages` 中添加自定义域名
2. 在你的域名服务商处添加 CNAME 记录指向 `你的用户名.github.io`
3. 等待 DNS 生效（通常 24 小时内）

---

## 项目结构

```
app/
├── .github/
│   └── workflows/
│       └── deploy.yml      # GitHub Actions 自动部署配置
├── src/
│   ├── components/         # React 组件
│   │   ├── AddCargoDialog.tsx
│   │   ├── CargoItem.tsx
│   │   ├── CargoList.tsx
│   │   ├── ExcelImporter.tsx
│   │   ├── ExportButton.tsx
│   │   ├── StatisticsPanel.tsx
│   │   ├── VehicleCard.tsx
│   │   └── ui/             # shadcn/ui 组件
│   ├── data/
│   │   └── cargoData.ts    # 默认货物数据
│   ├── hooks/
│   │   └── useCargoState.ts # 状态管理
│   ├── types/
│   │   └── index.ts        # TypeScript 类型定义
│   ├── App.tsx             # 主应用组件
│   ├── App.css             # 应用样式
│   └── main.tsx            # 入口文件
├── index.html              # HTML 模板
├── package.json            # 项目依赖
├── vite.config.ts          # Vite 配置
├── tailwind.config.js      # Tailwind CSS 配置
└── README.md               # 项目说明
```

---

## 技术栈

- **框架**: React 18 + TypeScript
- **构建工具**: Vite
- **样式**: Tailwind CSS
- **组件库**: shadcn/ui
- **Excel 处理**: xlsx
- **图标**: Lucide React

---

## 需要帮助？

如有问题，请：
1. 检查上述常见问题
2. 查看 GitHub Actions 日志
3. 提交 Issue 到 GitHub 仓库
