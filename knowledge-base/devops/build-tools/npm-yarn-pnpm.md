# npm、yarn、pnpm 包管理

## 1. 核心概述（官方定义）

### npm 概述
npm（Node Package Manager）是 Node.js 的默认包管理器，也是世界上最大的软件注册表。npm 由三部分组成：网站、CLI 命令行工具和注册表。

### Yarn 概述
Yarn 是由 Facebook、Google、Exponent 和 Tilde 联合开发的快速、可靠、安全的 JavaScript 包管理器，解决了 npm 早期版本的一些问题。

### pnpm 概述
pnpm（Performant npm）是一个快速、节省磁盘空间的包管理器，使用硬链接和符号链接来节省磁盘空间，安装速度非常快。

---

## 2. 核心知识点（结构化层级）

### 2.1 包管理基础

#### package.json
- name：包名
- version：版本
- description：描述
- main：入口文件
- scripts：脚本命令
- dependencies：生产依赖
- devDependencies：开发依赖
- peerDependencies：对等依赖
- engines：引擎版本

#### 语义化版本
- MAJOR：主版本号（不兼容的 API 修改）
- MINOR：次版本号（向下兼容的功能性新增）
- PATCH：补丁号（向下兼容的问题修正）
- 预发布版本和构建元数据

#### 依赖范围
- 精确版本：1.0.0
- 补丁版本更新：~1.0.0
- 次要版本更新：^1.0.0
- 大于等于：>=1.0.0
- 范围：1.0.0 - 2.0.0

### 2.2 npm 核心功能

#### 基本命令
- npm init：初始化项目
- npm install：安装依赖
- npm uninstall：卸载依赖
- npm update：更新依赖
- npm run：运行脚本

#### 依赖管理
- npm install <package>：安装生产依赖
- npm install <package> -D：安装开发依赖
- npm install <package> -g：全局安装
- npm ci：根据 lockfile 安装

#### 发布相关
- npm login：登录
- npm publish：发布包
- npm version：更新版本
- npm deprecate：弃用包

### 2.3 Yarn 核心功能

#### 基本命令
- yarn init：初始化项目
- yarn add：添加依赖
- yarn remove：移除依赖
- yarn upgrade：升级依赖
- yarn run：运行脚本

#### 依赖管理
- yarn add <package>：添加生产依赖
- yarn add <package> -D：添加开发依赖
- yarn global add：全局安装
- yarn install --frozen-lockfile：根据 lockfile 安装

#### 工作区
- yarn workspace：工作区管理
- 多包管理
- 依赖提升

### 2.4 pnpm 核心功能

#### 基本命令
- pnpm init：初始化项目
- pnpm add：添加依赖
- pnpm remove：移除依赖
- pnpm update：更新依赖
- pnpm run：运行脚本

#### 存储机制
- content-addressable store：内容寻址存储
- 硬链接和符号链接
- 节省磁盘空间
- 快速安装

#### 工作区
- pnpm workspace：工作区
- monorepo 支持
- 依赖过滤

### 2.5 lock 文件

#### package-lock.json（npm）
- 锁定依赖版本
- 保证安装一致性
- 包含完整依赖树

#### yarn.lock（Yarn）
- 锁定依赖版本
- 确定性安装
- 人类可读

#### pnpm-lock.yaml（pnpm）
- 锁定依赖版本
- 使用符号链接信息
- 高效的存储记录

### 2.6 脚本与钩子

#### npm scripts
- 生命周期脚本（preinstall、postinstall 等）
- 自定义脚本
- 脚本组合

#### npx
- 运行本地包
- 运行一次性命令
- 不安装执行

---

## 3. 官方标准语法 / API

### 3.1 package.json 示例

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "My project description",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "files": ["dist"],
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "test": "vitest",
    "lint": "eslint src",
    "format": "prettier --write src"
  },
  "keywords": ["vue", "typescript"],
  "author": "Your Name",
  "license": "MIT",
  "dependencies": {
    "vue": "^3.3.0",
    "axios": "~1.4.0",
    "lodash": ">=4.17.0 <5.0.0"
  },
  "devDependencies": {
    "vite": "^4.3.0",
    "vitest": "^0.32.0",
    "eslint": "^8.40.0",
    "prettier": "^2.8.0"
  },
  "peerDependencies": {
    "vue": "^3.0.0"
  },
  "engines": {
    "node": ">=16.0.0",
    "npm": ">=8.0.0"
  }
}
```

### 3.2 npm 常用命令

```bash
# 初始化项目
npm init
npm init -y  # 使用默认值

# 安装依赖
npm install
npm i  # 简写
npm install <package>
npm install <package>@1.0.0
npm install <package> --save-dev
npm install <package> -D

# 全局安装
npm install -g <package>

# 根据 lockfile 安装
npm ci

# 卸载依赖
npm uninstall <package>
npm un <package>

# 更新依赖
npm update
npm update <package>

# 查看信息
npm list
npm list --depth=0
npm view <package>
npm info <package>

# 运行脚本
npm run dev
npm run build

# 搜索包
npm search <keyword>

# 发布相关
npm login
npm publish
npm version patch
npm version minor
npm version major

# 清理缓存
npm cache clean --force

# npx 命令
npx <command>
npx create-vite@latest my-app
```

### 3.3 Yarn 常用命令

```bash
# 初始化项目
yarn init

# 安装依赖
yarn install
yarn

# 添加依赖
yarn add <package>
yarn add <package> --dev
yarn add <package> -D
yarn add <package>@1.0.0

# 全局安装
yarn global add <package>

# 移除依赖
yarn remove <package>

# 升级依赖
yarn upgrade
yarn upgrade <package>
yarn upgrade --latest

# 查看信息
yarn list
yarn info <package>

# 运行脚本
yarn dev
yarn build
yarn run <script>

# 工作区
yarn workspace <workspace> add <package>
yarn workspaces foreach run <script>
```

### 3.4 pnpm 常用命令

```bash
# 初始化项目
pnpm init

# 安装依赖
pnpm install
pnpm i

# 添加依赖
pnpm add <package>
pnpm add <package> -D
pnpm add <package> --save-dev
pnpm add <package>@1.0.0

# 全局安装
pnpm add -g <package>

# 根据 lockfile 安装
pnpm install --frozen-lockfile

# 移除依赖
pnpm remove <package>

# 更新依赖
pnpm update
pnpm update <package>
pnpm update --latest

# 查看信息
pnpm list
pnpm list --depth=0
pnpm view <package>

# 运行脚本
pnpm dev
pnpm build
pnpm <script>

# 工作区
pnpm --filter <package> add <dep>
pnpm -r --filter <package> run <script>

# 清理
pnpm store prune
pnpm store status
```

---

## 4. 官方示例代码（可运行）

### 4.1 完整项目示例

```json
{
  "name": "vue-app",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vue-tsc && vite build",
    "preview": "vite preview",
    "test": "vitest",
    "test:coverage": "vitest --coverage",
    "lint": "eslint . --ext .vue,.js,.jsx,.cjs,.mjs,.ts,.tsx,.cts,.mts --fix",
    "format": "prettier --write src/",
    "prepare": "husky install"
  },
  "dependencies": {
    "vue": "^3.3.4",
    "vue-router": "^4.2.2",
    "pinia": "^2.1.4",
    "axios": "^1.4.0"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^4.2.3",
    "typescript": "^5.0.2",
    "vite": "^4.3.9",
    "vitest": "^0.32.2",
    "@vue/test-utils": "^2.3.2",
    "eslint": "^8.43.0",
    "eslint-plugin-vue": "^9.14.1",
    "@typescript-eslint/eslint-plugin": "^5.59.11",
    "@typescript-eslint/parser": "^5.59.11",
    "prettier": "^2.8.8",
    "husky": "^8.0.3",
    "lint-staged": "^13.2.2"
  },
  "lint-staged": {
    "*.{js,ts,vue}": [
      "eslint --fix"
    ],
    "*.{js,ts,vue,json,css,md}": [
      "prettier --write"
    ]
  }
}
```

### 4.2 .npmrc 配置

```ini
# 国内镜像源
registry=https://registry.npmmirror.com

# 私有仓库
@myorg:registry=https://registry.myorg.com

# 代理
proxy=http://proxy.example.com:8080
https-proxy=http://proxy.example.com:8080

# 严格模式
strict-peer-dependencies=true

# 保存前缀
save-prefix=^

# 忽略脚本
ignore-scripts=false
```

### 4.3 pnpm-workspace.yaml

```yaml
packages:
  - 'packages/*'
  - 'apps/*'
```

---

## 5. 官方注意事项 / 最佳实践

### 5.1 npm 最佳实践

1. **使用 lockfile**
   - 提交 package-lock.json
   - 使用 npm ci 确保一致性
   - 定期更新依赖

2. **版本管理**
   - 遵循语义化版本
   - 合理使用依赖范围
   - 定期更新依赖

3. **脚本管理**
   - 定义清晰的脚本
   - 使用 pre/post 钩子
   - 组合常用命令

### 5.2 Yarn 最佳实践

1. **使用 Yarn v2+**
   - Plug'n'Play 模式
   - Zero-installs
   - 更好的性能

2. **工作区**
   - 使用 workspaces 管理 monorepo
   - 共享依赖
   - 统一管理

3. **确定性安装**
   - 使用 --frozen-lockfile
   - 确保构建可复现

### 5.3 pnpm 最佳实践

1. **利用存储优势**
   - 节省磁盘空间
   - 快速安装
   - 严格的依赖树

2. **工作区管理**
   - 使用 --filter 过滤
   - monorepo 支持
   - 依赖提升控制

3. **性能优化**
   - 使用 pnpm store
   - 定期清理未使用的包
   - 利用硬链接

### 5.4 通用最佳实践

1. **安全考虑**
   - 定期审计依赖：npm audit
   - 使用 npm audit fix
   - 关注安全公告

2. **镜像源**
   - 国内使用淘宝镜像
   - 配置 .npmrc
   - 使用 nrm 管理镜像源

3. **依赖清理**
   - 定期清理 node_modules
   - 使用 depcheck 检查未使用的依赖
   - 移除不用的依赖

---

## 6. 官方文档参考链接

- [npm 官方文档](https://docs.npmjs.com/)
- [Yarn 官方文档](https://yarnpkg.com/getting-started)
- [pnpm 官方文档](https://pnpm.io/)
- [package.json 参考](https://docs.npmjs.com/cli/v9/configuring-npm/package-json)
- [语义化版本规范](https://semver.org/lang/zh-CN/)
